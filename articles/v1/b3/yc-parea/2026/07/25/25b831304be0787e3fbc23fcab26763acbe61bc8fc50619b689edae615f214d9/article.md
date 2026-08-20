---
schema_version: "1.0.0"
document_id: "25b831304be0787e3fbc23fcab26763acbe61bc8fc50619b689edae615f214d9"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/workflow-for-production-llm-apps"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:e234544f8362fefcaf8ad4881b2520fb3aaf597efb44aa650854d775db74ba9d"
---

# A Systematic Workflow to build Production-Ready LLM Applications

[Joel Alexander](https://www.linkedin.com/in/ajoeljr/recent-activity/all/) on Jul 17, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


This post outlines a practical workflow to help teams move from prototypes to production, focusing on continuous improvement and data-driven decision-making.


## ​


1. Hypothesis Testing with Prompt Engineering


Start simple. Begin with one of the leading foundation models and test your hypothesis using basic prompt engineering. The goal here isn’t perfection but establishing a baseline and confirming that the LLM can produce reasonable responses for your use case. If you already have a prompt/chain in place, skip to the next step.


Don’t spend too much time engineering your prompt at this stage. After you collect more data, you can make hypothesis-driven tweaks.


## ​


2. Dataset Creation from Users


Use an observability tool to collect real user questions and responses. It’s okay if you aren’t collecting feedback yet; first, you want a diverse dataset to iterate on.


Tools like Parea can simplify this process. Use our py/ts SDK to quickly instrument your code, and then use your logs to[create a dataset](https://docs.parea.ai/observability/dataset_from_trace) . The next step is to establish an evaluation metric.


Developing effective evaluation metrics is crucial but challenging. Start with something directional to help narrow your focus. For RAG (Retrieval-Augmented Generation) applications, consider these common evaluation techniques:


- [Relevance](https://docs.parea.ai/evaluation/overview#using-pre-built-sota-evaluations) : How well does the retrieved information match the query?
- [Faithfulness](https://docs.parea.ai/evaluation/overview#using-pre-built-sota-evaluations) : Does the generated response accurately reflect the retrieved information?
- [Coherence](https://docs.parea.ai/evaluation/overview#using-pre-built-sota-evaluations) : Is the response well-structured and logically consistent?


Adding a[custom](https://docs.parea.ai/evaluation/overview#custom-evaluations) LLM-as-judge evaluation can also provide quick insights.


python


```text
from   parea.schemas   import   EvaluationResult,   Log


def   helpfulness  (  log:   Log  ) -  >   EvaluationResult:
try:
response   =   client.chat.completions.create  (
model  =  "gpt-4o",
messages  =  [  {
"role"  :   "user",
"content"  :   f"""[Instruction]\nPlease act as an impartial judge and evaluate the quality of the response
provided. Your evaluation should consider factors such as the helpfulness, relevance, accuracy,
depth, creativity, and level of detail of the response. Be as objective as possible.
Respond in JSON with two fields: \n
\t 1. score: int = a number from a scale of 1 to 5 with 5 being great and 1 being bad.\n
\t 2. reason: str =  explain your reasoning for the selected score.\n\n
This is this question asked: QUESTION:\n{log.inputs["question"]}\n
This is the context provided: CONTEXT:\n{log.inputs["context"]}\n
This is the response you are grading, RESPONSE:\n{log.output}\n\n""",
}],
response_format  =  {  "type"  :   "json_object"},
)
r   =   json.loads  (  response.choices[0].message.content  )
return   EvaluationResult  (  name  =  "helpfulness",   score  =  int  (  r[  "score"  ]  )   /   5,   reason=r["reason"]  )
except   Exception   as   e:
return   EvaluationResult  (  name  =  "error-helpfulness",   score  =  0,   reason  =  f"Error in grading: {e}"  )


```


## ​


3. Experimentation and Iteration


With your dataset and evaluation metrics in place, it’s time to experiment. Identify the variables you can adjust, for example:


- Prompt variations
- Chunking strategies for RAG
- Embedding models
- Re-ranking techniques
- Foundation model selection


Run controlled[experiments](https://docs.parea.ai/evaluation/overview#running-and-naming-of-experiments) , changing one variable at a time. Use a tool like Parea to manage your experiments and analyze results.


Easily reference your datasets, add metadata, and run experiments via the SDK.


```text
def   model_call_factory  (  model  :   str  ):
@trace  (  eval_funcs  =  [eval_func])
def   func  (  topic  :   str  ) ->   str  :
return   llm_call(model, topic)
return   func


def   main  ():
metadata   =   dict  (
topk  =  str  (  TOPK  ),   num_sections  =  str  (  NUM_SECTIONS  ),
chunk_size  =  str  (  CHUNK_SIZE  ),   chunk_overlap  =  str  (  CHUNK_OVERLAP  )
)
for   model   in   [  "gpt-4o"  ,   "claude-3-haiku-20240307"  ]:
p.experiment(
name  =  "Coda-RAG"  ,
data  =  "CodaDataset"  ,
func  =  model_call_factory(model),
metadata  =  {  "model"  : model,   **  metadata},
).run(  run_name  =  f  "  {  model  }  -  {  str  (uuid.uuid4())[:  4  ]  }  "  )


```


In the code snippet above, we are testing different foundation models for the generation portion of our RAG pipeline.


Analyze your results to identify patterns and areas for improvement. This iterative process forms the core of your development loop.


## ​


Advanced Techniques


As you become more familiar with the initial workflow, consider these advanced techniques to refine your application further. I won’t go into too much detail here, as each option could be its own post!


### ​


Human Annotation


When automated evaluations fall short, incorporate human annotation. Focus on specific aspects of performance and try to codify your decision-making process. With Parea, you can take as little as 20-30 manually annotated samples and use our eval bootstrapping feature to create a custom evaluation metric that is aligned with your annotations.[Learn more](https://docs.parea.ai/manual-review/bootstrapped-eval)


### ​


Dynamic Few-shot Examples


Leverage your growing dataset to inject relevant examples into your prompts dynamically. Select high-performing examples based on user[feedback](https://joelparea.substack.com/p/on-feedback?r=2oybav) or evaluation metrics to guide the model toward better responses. Cookbook[py](https://github.com/parea-ai/parea-sdk-py/blob/04cde9f15e0d592628f9946593bda5c38647481c/cookbook/parea_llm_proxy/dynamic_few_shot_injection.py) /[ts](https://github.com/parea-ai/parea-sdk-ts/blob/31189224327aaf98e6c29ae30201719510c9ceda/cookbook/dynamic_few_shot_injection.ts)


## ​


Conclusion: Embracing Continuous Improvement


Building production-ready LLM applications is an ongoing process, not a one-time effort. You can rapidly iterate and improve your LLM-powered products by adopting a systematic workflow emphasizing real-world data, robust evaluation, and continuous experimentation.


What strategies have you found effective in getting (and keeping) your LLM applications production-ready?
