---
title: LLM as Judge needs multiple LLMs to work reliably
---
Using LLMs as judges of LLM outputs is essential for a scalable system, but I have noticed that many people only use
one LLM to judge responses.  This is a mistake.  LLMs are not perfect and their responses can vary even when they are
given the same or similar input.  

It was surprisingly difficult to reliably evaluate responses for the [Needle in a Needle stack](https://github.com/llmonpy/needle-in-a-needlestack)
benchmark. The evaluation template was pretty simple:
```
I would like you to evaluate the answer to a question about a limerick.  The limerick is:

$limerick_text

The question is:

$question_text

$icl_text

The generated answer to the question is:

$generated_answer_text

You need to determine if the generated answer passes or fails.  Pass means the answer has the same meaning as the good 
answer.  It does not matter if the generated answer is more or less concise than the good answer.  Does the generated
answer pass or fail? Please answer with an "aaa" if the generated answer passes or "bbb" if it fails. Do not provide 
an explanation, only reply with "$pass_answer" or "$fail_answer".
```

Where "$icl_text" is one or more examples of good answers.  It is essential to have accurate evaluation of a benchmark.
I needed to use 5 LLMs to judge each response to achieve reliable evaluation of NIAN responses.  I created a tool, **dissent**,
to help me understand how the evaluators were doing.  Dissent reports how often each model disagreed with the majority 
of models evaluating a response.  Here are the results from a recent test of 400 responses to the NIAN benchmark:

```
claude-3-haiku-20240307 % wrong: 7%
gpt-3.5-turbo-0125 % wrong: 16%
gpt-4o % wrong: 5%
gemini-1.5-flash % wrong: 3%
claude-3-5-sonnet-20240620 % wrong: 2%
```
These results can vary quite a bit too.  I've seen GPT-3.5 dissent over 40% of the time and Haiku as high as 25%.  
Gemini-flash did well in this test, but it can also dissent over 10% of the time. [There is more detail on NIAN evaluation
here.](https://nian.llmonpy.ai/methodology)

### LLMonPy makes it easy to use multiple LLMs to judge responses
[LLMonPy](https://github.com/llmonpy/llmonpy) makes it very easy to use multiple LLMs to judge the output of multiple
LLMs in a tourney.  For example:
```python
        ordered_output_list = judge_output(response_list, judge_list, thread_pool, recorder)
```
Given a response_list of 9 outputs from LLMs and a list of 5 judges, this function will run a tourney of 56 one on one
contests between the responses and return a list ordered by the number of victories a response had.  LLMonPy runs the 
280 judging requests in parallel, so it does not take long.  Cheap, small models can handle comparisons like this, so it
does not cost very much to run the tourney.  Also, the tourney results are saved by the recorder, so they 
can be used to fine-tune the LLMs in the future.  

Tourneys are quite simple with LLMonPy too:
```python
        client_list = [GPT4o, MISTRAL_LARGE, GEMINI_PRO, GEMINI_FLASH, ANTHROPIC_SONNET, MISTRAL_7B, ANTHROPIC_HAIKU,
                       ANTHROPIC_OPUS]
        judge_client_list = [MISTRAL_LARGE, GEMINI_FLASH, MISTRAL_7B, MISTRAL_8X22B, ANTHROPIC_HAIKU]
        generators = create_prompt_steps(SamplePrompt(), client_list, [0.0])
        judge_list = create_prompt_steps(SamplePrompt().JudgePrompt(), judge_client_list)
        tournament = LLMonPyTournament(generators, judge_list)
        result_list, _ = do_llmonpy_step(tournament, recorder)
```
This example creates a tournament with 8 generators and 5 judges. The above example is only using one temperature, but
you could use more by changing the [0.0] to include more temperatures (ex:[0.0, 0.1, 0.2, 0.3, 0.4, 0.5]).  With six
temperatures, you would have 48 generators.  The cost of running the above tournament with one temperature is about
$0.01 (LLMonPy tracks the cost of every request).

LLMonPy is still in the early stages of development, but I am updating the repo several times a week now.  If LLMonPy
sounds interesting to you, [please watch the repo.](https://github.com/llmonpy/llmonpy) 

