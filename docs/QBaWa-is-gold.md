
What is QBaWa?  It is a funny sounding acronym for Question - Best Answer - Worse Answer. QBaWa is the data that LLMs 
need to generate better, more consistent results.  You can use QBaWa to provide examples of good answers for prompts or 
to fine tune a model -- the most effective ways to improve quality and reduce costs of AI pipelines.  To build a moat, 
an AI startup needs intense request-flow and an AI pipeline that generates massive QBaWa from requests.  More and better
QBaWa would produce better responses to requests, which will produce more requests.  This is the QBaWa flywheel and 
Google has been using this model for decades.  

To spin up the QBaWa flywheel, a startup will:
Build an initial AI pipeline and use a combination of LLM and human judges to create and select best and worse answers 
for the requests made of the pipeline.

Use the QBaWa from 1 to drive automatic prompt optimization with a system like DSPy [link].  An automated approach to 
prompt optimization allows you to tune each prompt to a specific LLM and to try a wider range of prompts than a human 
can.  

Use the QBaWa to fine tune a small model, maybe even a sub 1B parameter model, to answer requests for a specific step 
in the AI pipeline.   Small models trained on high quality data -- like Phi-3 or TinyLlama -- are fast, resource 
efficient and get great results.  It is just a guess at this point, but I think good micro models fine-tuned to answer 
requests for a specific step in a pipeline will produce the highest quality and lowest cost results for pipelines with 
the volume to justify the investment.  

The framework I am working on, LLMonPY, creates huge amounts of QBaWa through the use of what I call tournaments.  The 
typical LLMonPy pipeline step is a contest between multiple prompts and LLMs that are judged by other LLMs (humans as 
used as judges early on to build a foundation).  Each round of a tournament produces a best answer and worse answer -- 
so, QBaWa.

The feature of LLMonPy that drives an explosion of QBaWa is automated replay of past requests.  For every request, the 
inputs and outputs of every step are recorded.  These recordings can then be used to replay a single step of a request 
using hundreds of different strategies simultaneously.  Of course, each strategy will be compared to past results and 
each other, generating an enormous amount of QBaWa.  You can replay sets of requests, say 100 broadly representative 
requests the pipeline can expect, so you can do a realistic evaluation of which strategies are the best -- and generate 
more QBaWa.    

LLMonPy makes a huge number of LLM requests that would have been impractical with the rate limits and token costs of 
even a few months ago.  However, $1000 of spend at OpenAI gets you 10k requests and 10,000,000 tokens per minute.  $250 
at Mistral gets you 1200 requests per 20,000,000 tokens per minute.  Google's newest models allow similar request and 
token rates.  Even with a hobbiest budget, I was able to process 6k LLM calls in a couple of minutes when I was working 
on Needle in a needlestack (link)

LLMonPy is just a bunch of pieces of software scattered about right now.  I will turn into something more coherent that 
you can use over the next few weeks.  Follow me on X/Twitter (link) to keep up with my progress.  

My guess is that there are thousands of people in silicon valley (and like minded places) thinking similar things.  If 
this is right, the implication is that there will be a furious land grab for the most valuable request-flows and that 
thousands of startups in the race will consume huge amounts of GPU time to get the best QBaWa.  NVIDIA may grow even 
faster!   

You might also want to check out the resources that have inspired much of my thinking -- LLM as judges, more agents 
is all you need, Lora models beat gpt4, DPO, ORPO, DSPy, link to course