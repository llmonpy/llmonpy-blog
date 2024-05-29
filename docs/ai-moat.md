---
title: How Startups Will Build a Moat
---
# How Startups Will Build a Moat
*by [Tom Burns](mailto:public@llmonpy.ai)* <br><br>
Climbing a learning curve sooner  or faster is a proven path for a company to build a moat to defend itself from 
copycats.  The steps on the learning curve for a LLM application are examples of good responses to requests.  Startups
that climb the curve the fastest will have the best request-flow and then get the most value from the requests by using
them as the basis for hundreds or thousands of experiments to improve the responses.  Every market segment that uses AI is
likely to be a winner-take-all competition where the fastest to climb the learning curve will win. This post explains how
the winners will win. 

## What is an AI pipeline?
An example of an AI pipeline is a system that generates an employment contract for a specific position. 
Employment contracts are mostly the same, but there are many small differences that matter to companies.  The pipeline
could look something like this:

1. Does the request include all the required information?
2. Identify the contract template that is closest to the request.
3. Indentify the differences between the request and the template.
4. Generate a contract that includes the differences.
5. Provide contract for guided review that includes explanations of what to check.

Each of these steps are probably also pipelines.  For example, step 1 could be:

1. Loop through each required piece of information and ask the LLM if is present in the request. So if there are 10 required pieces of information, there would be 10 prompts to the LLM.  Each prompt would be something like: Does this {contract request} include {information}? Where contract request is this specific request for a contract and information could be "company name", "employee name", "start date", etc.  The LLM would respond with a yes or no.  The responses would be used to determine if the request is complete.
2. Assemble the responses from the LLM and request the user provide any missing information.

The idea is to break the pipeline down into small steps to get better, more
consistent responses from the LLM.  The key to climbing the learning curve is to generate examples of good responses to
requests.  You can use the good responses to provide examples to the LLM in a prompt, which has been shown to make a
dramatic difference in the quality and consistency of the responses.  When you have enough good responses, you can use
them to fine tune a small model to generate responses faster, cheaper and with higher quality than a generalist large model.
The most useful form of good responses is Question, Best Answer, Worse Answer -- **QBaWa** for short. You can use the best
answers for examples in a prompt and both best and worse answers to fine tune a model using the latest techniques that
improve quality and lower costs of training.  

## Lifecycle of an AI pipeline
When developing an AI pipeline that you intend to scale, the goal is to increase quality while reducing cost per request.
To be successful scaling an AI pipeline, you must focus your efforts on automating evaluations of each step.  You can't get
better if you do not have good evaluations.  The evaluations have to be automated to scale.  Fortunately, you can use
the same data -- **QBaWa** -- to automate evaluations that you use to improve the responses to requests.  People frequently
want to focus on the responses to requests, but it is actually the evaluations that will drive improvements in the
pipeline.  Focusing on evaluations will also help you understand how to make a better prompt. 

### Tournaments and automated evaluation to generate QBaWa
An easy way to generate **QBaWa** is to use a tournament.  A tournament is a contest between multiple prompts and LLMs where
the responses are judged by other LLMs.  Each round of a tournament produces the best answer and the worst answer -- so, 
**QBaWa**. You will not be able to use all of the **QBaWa** you generate, some will be too low quality.  You will want
to choose responses that meet the requirements as a "Best" answer for the prompt. Use automated evaluation based on
responses that meet the minimum standard to determine if a response should be added to the **QBaWa&** dataset.  

### Experiments to generate QBaWa
If you record the inputs & outputs of each step of a pipeline, you can replay the requests using different prompts and
LLMs.  With an automated evaluation system, you can compare the responses to the original responses and to each other to
decide which responses are the best and worst.  This is a great way to generate **QBaWa** and get the most value out
of your request-flow.  

### The stages of an AI pipeline are:
1. **Bootstrap:** Flesh out the steps of the pipeline.  Use tournaments to generate most responses and use humans as 
   judges to build up a QBaWa dataset for each tournament that you can use for evaluation. You will need as many example
   requests as you can get to improve the scope of your **QBaWa**.  You can run automated evaluations parallel to the 
   human evaluation.  Once the automated evaluations almost always agree with the human evaluations, you can switch to 
   fully automated evaluation with occasional checks by humans.  Humans should focus on judging evaluations where judges 
   disagree.  The evaluations with discord might reveal areas you could improve your prompt, or identify an LLM that is
   not doing a good enough job.  Once the evaluations are good enough, you can start running experiments and massively 
   ramp up your **QBawa** generation.
2. **Alpha:** Move to alpha when you have extracted all the **QBaWa** you can from your example requests.  The goal of this
   stage is to get more request-flow diversity to improve your responses and to get enough **QBaWa** to justify fine-tuning
   a small model.  Using a fine-tuned small model will allow you to generate responses faster, cheaper and with higher
   quality.   Obviously, lowering cost per request is a key before moving to the greater request-flow of the beta stage.
3. **Beta:** -- Move to the beta stage when you are confident the pipeline responses are high quality ,and you can generate
   the responses at reasonable costs.  The goal of this stage is to get as many requests as possible to improve the range
   of your **QBaWa** and further reduce the cost per request with more fine-tuning and pipeline simplification.  For example,
   with fine-tuning you might not need as many LLM judges to evaluate responses.  You might also reduce some tourney steps
   to a single prompt and a fine-tuned LLM.  
4. **Production:** -- Move to the production stage with the cost and quality of the AI pipeline meet your targets.  The
   goal of this stage is to ramp up request-flow and use the request-flow to continuously improve cost and quality.  By
   this stage, you could automate the cost optimization of the pipeline by creating AI pipelines that try different
   approaches to reduce cost, then use the recordings of the responses to your request-flow to evaluate the results. Using
   this approach, you could be sure that you are maintaining or improving quality while reducing costs.

## The Moat: The QBaWa Flywheel
QBaWa a proven technique to build a moat.  Everytime you do a Google search, Google is generating **QBaWa**.  Whatever links
you clicked on are best answers, while the links you ignored are worse answers.  Google uses this data to improve their
search results and ensure that users prefer Google to other search engines.  The **QBaWa** flywheel is a virtuous cycle
where more and better **QBaWa** produces better responses to requests, which produces more requests.  Google has been using this
model for decades.  Once a company has built a lead on the learning curve, it is very hard for competitors to catch up.

## LLMonPy
LLMonPy is a framework that creates huge amounts of **QBaWa** through the use of tournaments, automated evaluations and
experiments. I will release a first, quite rough version in a couple of weeks.  You can [follow me on Twitter](https://x.com/sftombu)
to keep up with my progress.  

One thing that I noticed while working on LLMonPy and [Needle in a Needlestack](https://nian.llmonpy.ai/)
is that there has been a huge increase in rate limits and a decrease in costs.  A year ago, you would be lucky to get 10
requests a minute.  Now, you can easily get 1000s of requests and millions of tokens per minute.  Because they require so many
requests, LLMonPy and the ideas in this post would be useless without these improvements. 

## Conclusions
If I am right, there will be a furious land grab for the most valuable request-flows.  To climb the learning curve faster,
companies will conduct thousands of experiments on their request-flows to generate **QBaWa** faster than their competitors.
It will be a great time to be a Nvida shareholder (I am not a Nvida shareholder except of index funds). 

## References
* [Mastering LLMs: A Conference For Developers & Data Scientists](https://maven.com/parlance-labs/fine-tuning?cohortSlug=): Great conference that helped cystralize my thinking.
* [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685): Introduced the idea of using LLMs as judges. Picking between 2 responses is an easier application of LLM judges.
* [More Agents is All You Need](https://arxiv.org/html/2402.05120v1): Using more LLMs to improve your responses.
* [LoRA Land: 310 Fine-tuned LLMs that Rival GPT-4, A Technical Report](https://arxiv.org/abs/2405.00732): Small fine-tuned models can outperform larger models.
* [Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/abs/2305.18290): A more efficient way to fine-tune a model.
* [Reference-free Monolithic Preference Optimization with Odds Ratio](https://arxiv.org/html/2403.07691v1) : An even more efficient way to fine-tune a model.
* [DSPy: A System for Automated Prompt Optimization](https://github.com/stanfordnlp/dspy): A system for automated prompt optimization.