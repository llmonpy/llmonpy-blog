---
title: LLM applications should be as reliable as a conventional IT systems
---
[LLMonPy is a python LLM pipeline framework](https://github.com/llmonpy/llmonpy/tree/main) that I hope will make it easy to build AI systems that generate "good
enough" responses 99.9% of the time. The market potential for LLMs will expand exponentially if 
they achieve a level of reliability comparable to traditional IT systems, significantly surpassing the current standards 
of most AI technologies. To achieve this level of reliability and expand the market potential, LLMonPy incorporates several key principles and features:

1. **Evaluation is the foundation of progress:** Obviously, a system can not get better if you can not measure the quality
   of its output.  To be scalable, the evaluations must be implemented with LLMs as judges.  However, to be useful the
   evaluations must reflect human preferences.  LLMonPy makes it easy to create juries of LLMs that rank responses. It
   will also have tools that let developers adjust the LLMs' judgements, so they make better decisions in the future.
2. **Evaluations can generate data that can be used to improve future responses:** The juries that LLMonPy uses to rank responses 
   generate a lot of Question, Best Answer, Worse Answer (QBaWa) data that can be used to fine-tune LLMs.  The fine-tuned
   LLMs can be smaller, faster, cheaper models that, thanks to their fine-tuning, generate better responses than larger
   more general models.
3. **LLMonPypelines assume abundance:**  In other words, they make a LOT of LLM requests, 90%+ for evaluation of generated
   responses.  For example, a LLMonPy "Refinement Cycle" uses a group (4+) of LLMs to generate a response to a request, then
   another group of LLMs(usually 5) to rank the responses.  It repeats this process, but uses the best responses
   from the first round as examples of good responses.  So, few-shot prompting, but where the examples are for this specific
   request.  I have found it can make a big difference in the quality of the responses. If you ran a cycle with 9 
   generators and 5 judges for 3 cycles, you would make 567 LLM requests -- 540 of them for evaluation.  You would also
   generate a lot of QBaWa data that could be used to simplify the pipeline in the future.
   [Check out an example of a Refinement Cycle here.](https://github.com/llmonpy/llmonpy/blob/main/src/samples/test_cycle.py)
   LLMonPy tracks the cost of responding to each request.  My recent tests of cycles like the one above have cost about $0.13. 
   Given the pace that prices are going down, I expect the cost for this RefinementCycle will drop to less than $0.01 per request in 12 months.


I am updating the repo several times a week now.  If this project sounds interesting to you, [please watch the repo.](https://github.com/llmonpy/llmonpy/tree/main) 

