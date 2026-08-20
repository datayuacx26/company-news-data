---
schema_version: "1.0.0"
document_id: "1a549a15f1a2fa1f64531095c092037d5658d09eed522c9ea2ffb6fe026fb289"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/identify-unreliable-llm-app-inputs-of-production-traffic"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d0ca6d99f6caa3b9523ba8e7fa7973d518e68d7721c70a3d83c7d572aa4c511f"
---

# How to detect unreliable behavior of LLM apps

[Joschka Braun](https://joschkabraun.com/) on Aug 6, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


[Sixfold](https://www.sixfold.ai/) builds a risk assessment AI solution for insurance underwriters. Recently, we talked to their team who came up with a simple, yet powerful way to assess the reliability of LLM apps using Parea. This blogpost goes into more detail about that.


## ​


Idea: Identify High Entropy Outputs


The idea is to automatically flag any logs which have high entropy responses. To do that, you can simply rerun your LLM app on that particular sample and measure the difference between the two outputs. The larger their distance, the more likely it is an input for which your LLM app is unreliable. In the sections below we will discuss what distance metrics are useful to measure the difference between two responses.


The easiest way to build intuition on this is to think about prompt engineering. How often have you rerun your prompt and noticed that it only works in half of the cases? If that is the case, you are dealing with high entropy (uncertainty) responses as the prompt isn’t reliable for this input.


## ​


Implementation Using Parea


To see how easy this workflow can be implemented with Parea, let’s take a look at the following example. We have some function` llm_app` which we want to test for high entropy responses. In our evaluation function` is_unreliable_input` , we rerun our function again and compare the new output with the original one.


```text
from   parea   import   trace
from   parea.schemas   import   Log


# function which does the work
def   llm_app  (  **  kwargs  ) ->   str  :
pass


def   is_unreliable_input  (  log  : Log) ->   bool  :
# the output of the first execution
output1   =   log.output
# call the LLM app again
output2   =   llm_app(  **  log.inputs)
# return whether the two outputs are different
return   output1   !=   output2


# trace the function and apply the evaluation function in 10% of the cases
@trace  (  eval_funcs  =  [is_unreliable_input],   apply_eval_frac  =  0.1  )
def   tested_llm_app  (  **  kwargs  ) ->   str  :
return   llm_app(  **  kwargs)


```


The benefit of doing that via` trace` is that our eval is executed in the background (so no additional latency) and we can choose to only apply it in a fraction of the cases (here 10%).


## ​


Metric 1: Levenshtein Distance


A simple way to measure the difference between two strings is to calculate[the Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) . The Levenshtein distance is the minimum number of single-character edits (insertions, deletions, or substitutions) required to change one string into the other. We can use it in our eval function in the following way:


```text
from   parea.evals.general.levenshtein   import   levenshtein_distance


def   is_unreliable_input  (  log  : Log) ->   float  :
output1   =   log.output
output2   =   llm_app(  **  log.inputs)
return   1   -   levenshtein_distance(output1, output2)


```


## ​


Metric 2: Semantic Similarity


The drawback of traditional NLP metrics such as the Levenshtein distance is that they are not able to capture the semantic similarity between two strings. Instead, we can embed both outputs as vector, calculate their cosine similarity and return the inverse of that as our metric.


```text
from   parea.evals.general   import   semantic_similarity_factory


semantic_similarity   =   semantic_similarity_factory(  embd_model  =  "text-embedding-3-small"  )


def   is_unreliable_input  (  log  : Log) ->   float  :
output1   =   log.output
output2   =   llm_app(  **  log.inputs)
return   1   -   semantic_similarity(Log(  output  =  output1,   target  =  output2))


```


## ​


Metric 3: LLM Judge


Embedding models will give a general sense of how closely related the two outputs are. Yet, oftentimes you care about specific aspects of the outputs. For that scenario, you should use a LLM judge and instruct it to compare the two outputs along certain dimensions.


```text
from   parea.evals   import   call_openai


def   is_unreliable_input  (  log  : Log) ->   bool  :
output1   =   log.output
output2   =   llm_app(  **  log.inputs)


response   =   call_openai(
model  =  'gpt-4o'  ,
messages  =  [
{  "role"  :   "system"  ,
"content"  :   "You are CompareGPT, a machine to verify if two responses match. Answer with only yes/no."  },
{
"role"  :   "user"  ,
"content"  :   f  """You are given a two responses and you have to decide. Compare "Response A" and "Response B" to determine whether they both match. All information in "Response A" must be present in "Response B", including numbers and dates. You must answer "no" if there are any specific details in "Response A" that are not mentioned in "Response B".


Response A:
{  output1  }


Response B:
{  output2  }


CompareGPT response:"""  ,
},
],
temperature  =  0.0  ,
)
return   "yes"   in   response.lower()


```


Note, the[SelfCheckGPT](https://arxiv.org/abs/2303.08896) paper explores an adjacent approach to measure factuality of LLM responses.


## ​


Conclusion


A key aspect of improving your LLM app is ensuring it’s reliably handling the long tail of uses cases. One general way to identify unreliable samples is to rerun your LLM app for these inputs and see how similar the outputs are. You can assess how similar they are by using metrics such as the Levenshtein distance, semantic similarity, or a LLM judge.
