---
schema_version: "1.0.0"
document_id: "45d022ee0dfffd7896933617a2087012f3d3a0759d8713262aeb4dd0d673cb69"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/zeroentropy-migration-guide/"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-26T06:27:17.647384+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:109fd67059badb1fa47b10af9de6e8dacceba3d1288ea2e61a6823391a2db25e"
---

# ZeroEntropy Migration Guide

TL;DR


- ZeroEntropy provides two things — **rerank/embed models** and **zSearch** (managed document retrieval). This guide covers a replacement path for each.
- Models: swap to a managed API (Cohere, Voyage, OpenAI), or self-host our now-Apache-2.0 open weights for identical scores.
- Search: reproduce zSearch on Elasticsearch (engine owns the models) or a vector database plus your own embed/rerank calls.
- Existing data can be exported through the ZeroEntropy API with no re-parsing or OCR.


This guide describes how to migrate an application off the ZeroEntropy API onto a self-managed stack. ZeroEntropy provides two things — **rerank/embed models** and **zSearch** (managed document retrieval) — and this guide covers a replacement path for each. Follow the section(s) for whichever your application uses.


If the app calls… Migrate to… Section


` /models/rerank` ,` /models/embed` a managed model API (or self-hosted ZeroEntropy weights) **Models**


zSearch —` /collections/*` ,` /documents/*` ,` /queries/top-*` a search engine you run (Elasticsearch, Turbopuffer, Milvus, …) **Search**


If the app uses both, follow both sections.


## Models (rerank / embed)


A direct API swap; nothing to host unless you want identical scores.


ZeroEntropy call Managed replacement Notes


` POST /models/rerank` Cohere` rerank-v3.5` (self-serve), or Voyage` rerank-2.5` on AWS (SageMaker Marketplace) different score scale — re-tune thresholds


` POST /models/embed` Voyage` voyage-3-large` / OpenAI` text-embedding-3-large` re-embed the corpus with the new model


```text
# rerank (Cohere)
import   cohere
r   =   cohere.ClientV2().rerank(  model  =  "rerank-v3.5"  ,   query  =  query,   documents  =  docs,   top_n  =  10  )


# embed (Voyage)
import   voyageai
vecs   =   voyageai.Client().embed(texts,   model  =  "voyage-3-large"  ,   input_type  =  "document"  ).embeddings
```


**Want identical scores?** ZeroEntropy’s models are open weights on HuggingFace under[zeroentropy](https://huggingface.co/zeroentropy) —` zerank-2-reranker` ,` zerank-1-reranker` ,` zerank-1-small-reranker` ,` zembed-1-embedding` . Serve them yourself and point the client at that endpoint. Three deploy recipes follow.


No model code — a declarative Truss config builds and serves the model on Baseten’s BEI / TensorRT-LLM stack. Source:` baseten/` (` zerank-2-bei/` ,` SETUP.md` ).


**Prereqs:** a Baseten API key;` pip install truss baseten-performance-client transformers` . No HF token needed — the packaged checkpoint (` baseten-admin/zerank-2-reranker-seq` ) is public.


Authenticate Truss (` ~/.trussrc` ):


```text
[baseten]
remote_provider = baseten
api_key = <BASETEN_API_KEY>
remote_url = https://app.baseten.co
```


```text
truss   init   zerank-2-bei   --non-interactive   --name   zerank-2-bei
rm   -rf   zerank-2-bei/model          # no model.py for a TRT-LLM build
```


The file —` zerank-2-bei/config.yaml` :


```text
model_metadata  :
example_model_input  :
inputs  :
- -   Baseten is a fast inference provider
- -   Classify this separately.
raw_scores  :   true
truncate  :   true
truncation_direction  :   Right
model_name  :   "Zerank 2 DISTRIBUTION model"
python_version  :   py39
resources  :
accelerator  :   H100
cpu  :   '1'
memory  :   10Gi
use_gpu  :   true
trt_llm  :
build  :
base_model  :   encoder
checkpoint_repository  :
repo  :   baseten-admin/zerank-2-reranker-seq
revision  :   main
source  :   HF
max_num_tokens  :   40960
num_builder_gpus  :   1
quantization_type  :   fp8
runtime  :
webserver_default_route  :   /predict
```


Deploy, then pin replicas (each replica = 1 H100;` min:0` scales to zero):


```text
cd   zerank-2-bei   &&   truss   push   --remote   baseten   --non-interactive     # ~10 min build
curl   -X   PATCH   \
https://api.baseten.co/v1/models/  <  MODEL_I  D  >  /deployments/  <  DEPLOY_I  D  >  /autoscaling_settings   \
-H   "Authorization: Api-Key <BASETEN_API_KEY>"   \
-d   '{"min_replica":8,"max_replica":8,"scale_down_delay":60}'
```


Call it with` baseten_performance_client` (Rust/HTTP-2 —` requests` /` httpx` cap throughput before the GPUs saturate), formatting each` (query, doc)` with the Qwen chat template. The endpoint returns a relevance score per document at` /predict` .


Builds the TRT engine from the fp8 checkpoint, snapshots weights for fast cold-start, and serves a proxy-authed FastAPI endpoint on H100. Deploy:


```text
RELEASE_NAME  =  zerank-2-prod   MODEL_ID  =  zeroentropy/zerank-2-fp8   MIN_CONTAINERS  =  1   \
modal   deploy   inference/inference/modal/deploy_rerank.py
```


The file —` inference/inference/modal/deploy_rerank.py` :


```text
import   asyncio
import   math
import   os
from   typing   import   Any, cast


import   modal
import   torch
from   dotenv   import   load_dotenv
from   huggingface_hub   import   HfApi
from   pydantic   import   BaseModel
from   transformers.models.auto.tokenization_auto   import   AutoTokenizer
from   transformers.tokenization_utils_fast   import   PreTrainedTokenizerFast


load_dotenv(  override  =  True  )


# E.g.,
# RELEASE_NAME="zerank-2-dev"
# MODEL_ID="zeroentropy/zerank-2-fp8"
# MIN_CONTAINERS=0


RELEASE_NAME   =   os.environ[  "RELEASE_NAME"  ]
MODEL_ID   =   os.environ[  "MODEL_ID"  ]
MIN_CONTAINERS   =   int  (os.environ[  "MIN_CONTAINERS"  ])


_hf_rev   =   os.environ.get(  "HF_REVISION"  )
if   _hf_rev   is   None  :
_hf_rev   =   HfApi().model_info(  MODEL_ID  ).sha
assert   isinstance  (_hf_rev,   str  )
HF_REVISION   =   _hf_rev


CHECKPOINT_PATH   =   "/root/models/checkpoint"
TOKENIZER_PATH   =   "/root/models/tokenizer"
ENGINE_PATH   =   "/root/models/engine"


MAX_TOKENS   =   16384
PER_DEVICE_BATCH_SIZE_TOKENS   =   30_000
MAX_BATCH_SIZE   =   64


def   sigmoid  (x:   float  ) ->   float  :
return   1   /   (  1   +   math.exp(  -  x))


def   inference_time_cost  (batch_size:   int  , max_tokens:   int  ) ->   float  :
"""Empirical cost model fitted from H100 measurements (zerank-2 fp8)."""
total_tokens   =   max_tokens   *   batch_size
if   batch_size   ==   1  :
a, b, c   =   4.012996e-07  ,   1.438241  ,   8.051325e-03
return   a   *   total_tokens  **  b   +   c
else  :
b   =   1.075192   +   0.216606   *   math.exp(  -  0.199718   *   batch_size)
c   =   0.001322   *   batch_size   +   0.006120
a   =   math.exp(  -  7.0161   *   b   +   -  4.4164  )
return   a   *   total_tokens  **  b   +   c


def   create_optimal_batches  (
token_lengths: list[  int  ],
max_batch_tokens:   int   =   PER_DEVICE_BATCH_SIZE_TOKENS  ,
max_batch_size:   int   =   MAX_BATCH_SIZE  ,
) -> list[list[  int  ]]:
"""Create optimized batches that minimize total inference time."""
if   len  (token_lengths)   ==   0  :
return   []
n   =   len  (token_lengths)
if   n   <=   max_batch_size:
max_tokens   =   max  (token_lengths)
if   max_tokens   *   n   <=   max_batch_tokens:
return   [  list  (  range  (n))]
batches: list[list[  int  ]]   =   []
current_batch: list[  int  ]   =   []
current_max_tokens   =   0
for   i   in   range  (n):
token_length   =   token_lengths[i]
new_max_tokens   =   max  (current_max_tokens, token_length)
new_batch_size   =   len  (current_batch)   +   1
new_total_tokens   =   new_max_tokens   *   new_batch_size
can_add   =   new_batch_size   <=   max_batch_size   and   new_total_tokens   <=   max_batch_tokens
if   can_add:
if   len  (current_batch)   >   0  :
current_cost   =   inference_time_cost(  len  (current_batch), current_max_tokens)
combined_cost   =   inference_time_cost(new_batch_size, new_max_tokens)
new_batch_cost   =   inference_time_cost(  1  , token_length)
if   (  len  (current_batch)   >=   max_batch_size   //   2
and   current_cost   +   new_batch_cost   <   combined_cost   *   1.1  ):
batches.append(current_batch)
current_batch   =   [i]
current_max_tokens   =   token_length
continue
current_batch.append(i)
current_max_tokens   =   new_max_tokens
else  :
if   len  (current_batch)   >   0  :
batches.append(current_batch)
current_batch   =   [i]
current_max_tokens   =   token_length
if   len  (current_batch)   >   0  :
batches.append(current_batch)
return   batches


def   test_model  () ->   None  :
from   tensorrt_llm.runtime   import   ModelRunnerCpp
runner   =   ModelRunnerCpp.from_dir(
engine_dir  =  ENGINE_PATH  ,   rank  =  0  ,   max_output_len  =  1  ,   gather_generation_logits  =  True  )
warmup_tokens   =   torch.tensor([[  100  ]   *   10  ],   dtype  =  torch.long)
runner.generate([warmup_tokens[  0  ]],   max_new_tokens  =  1  ,   end_id  =  0  ,
return_dict  =  True  ,   output_generation_logits  =  True  )


image   =   (
modal.Image.debian_slim(  python_version  =  "3.12"  )
.apt_install([  "git"  ,   "wget"  ,   "libopenmpi-dev"  ])
.run_commands(
"wget https://developer.download.nvidia.com/compute/cuda/repos/debian12/x86_64/cuda-keyring_1.1-1_all.deb"  ,
"dpkg -i cuda-keyring_1.1-1_all.deb"  ,
"apt-get update"  ,
"apt-get -y install libcudnn9-cuda-12 libcudnn9-dev-cuda-12"  ,
)
.pip_install([
"torch==2.7.1"  ,   "fastapi[standard]==0.115.4"  ,   "transformers==4.53.1"  ,
"pydantic==2.11.3"  ,   "huggingface_hub==0.36.0"  ,   "python-dotenv==1.2.1"  ,
])
.pip_install([  "tensorrt-llm==1.0.0"  ],   extra_index_url  =  "https://pypi.nvidia.com"  )
.pip_install([  "onnx==1.19.1"  ,   "onnx-graphsurgeon==0.5.8"  ])
.env({  "PYTORCH_CUDA_ALLOC_CONF"  :   "expandable_segments:True"  ,
"TOKENIZERS_PARALLELISM"  :   "false"  })
.run_commands(
f  'hf download Qwen/Qwen3-4B --include "tokenizer*" "vocab*" "merges*" "special_tokens_map.json" "tokenizer_config.json" --local-dir   {TOKENIZER_PATH}  '  ,
)
.run_commands(
f  "hf download   {MODEL_ID}   --revision   {HF_REVISION}   --local-dir   {CHECKPOINT_PATH}  "  ,
secrets  =  [modal.Secret.from_name(  "zeroentropy-huggingface"  )],
)
.run_commands(
f  "trtllm-build --checkpoint_dir   {CHECKPOINT_PATH}   --output_dir   {ENGINE_PATH}   "
f  "--gpt_attention_plugin bfloat16 --kv_cache_type disabled "
f  "--max_input_len 32768 --max_num_tokens 32768 --max_batch_size 256 "
f  "--norm_quant_fusion enable --gemm_plugin fp8 --gather_generation_logits"  ,
f  "rm -r   {CHECKPOINT_PATH}  "  ,
gpu  =  "H100"  ,
)
# Patch vocab_size for slim-head so gather_generation_logits reads the sliced lm_head.
.run_commands(
"python3 -c '"
"import json; "
'p="'   +   ENGINE_PATH   +   '/config.json"; '
"c=json.load(open(p)); "
'c["pretrained_config"]["vocab_size"]=c["pretrained_config"].get("num_labels",1); '
'json.dump(c,open(p,"w"),indent=4)'
"'"  ,
)
.run_commands(
f  'echo "RELEASE_NAME=  {RELEASE_NAME}\\  nMODEL_ID=  {MODEL_ID}\\  nMIN_CONTAINERS=  {MIN_CONTAINERS}\\  nHF_REVISION=  {HF_REVISION}  " > /root/.env'  ,
)
.run_function(test_model,   gpu  =  "H100"  )
)


app   =   modal.App(  name  =  RELEASE_NAME  ,   image  =  image)


class   Input  (  BaseModel  ):
query_documents: list[tuple[  str  ,   str  ]]


class   Output  (  BaseModel  ):
scores: list[  float  ]
token_lengths: list[  int  ]


@app.cls  (
gpu  =  "H100"  ,   cpu  =  4  ,   memory  =  65536  ,
min_containers  =  MIN_CONTAINERS  ,   max_containers  =  100  ,
scaledown_window  =  60  ,   region  =  [  "us-east"  ,   "us"  ,   "eu"  ],
enable_memory_snapshot  =  True  ,
)
@modal.concurrent  (  max_inputs  =  1  )
class   Model  :
tokenizer: PreTrainedTokenizerFast
runner: Any


@modal.enter  (  snap  =  True  )
def   snap  (self) ->   None  :
self  .tokenizer   =   cast(
PreTrainedTokenizerFast,
AutoTokenizer.from_pretrained(  TOKENIZER_PATH  ,   padding_side  =  "right"  ))
if   self  .tokenizer.pad_token   is   None  :
self  .tokenizer.pad_token   =   self  .tokenizer.eos_token


@modal.enter  ()
def   load  (self) ->   None  :
from   tensorrt_llm.runtime   import   ModelRunnerCpp
self  .runner   =   ModelRunnerCpp.from_dir(
engine_dir  =  ENGINE_PATH  ,   rank  =  0  ,   max_output_len  =  1  ,
gather_generation_logits  =  True  )
warmup_tokens   =   torch.tensor([[  100  ]   *   10  ],   dtype  =  torch.long)
self  .runner.generate([warmup_tokens[  0  ]],   max_new_tokens  =  1  ,   end_id  =  0  ,
return_dict  =  True  ,   output_generation_logits  =  True  )


@modal.fastapi_endpoint  (  method  =  "POST"  ,   requires_proxy_auth  =  True  )
async   def   endpoint  (self, input: Input) -> Output:
if   len  (  input  .query_documents)   ==   0  :
return   Output(  scores  =  [],   token_lengths  =  [])
all_input_texts: list[  str  ]   =   []
for   query, document   in   input  .query_documents:
messages   =   [
{  "role"  :   "system"  ,   "content"  : query.strip()},
{  "role"  :   "user"  ,   "content"  : document.strip()},
]
input_text   =   self  .tokenizer.apply_chat_template(
messages,   tokenize  =  False  ,   add_generation_prompt  =  True  )
assert   isinstance  (input_text,   str  )
all_input_texts.append(input_text)
batch_encoding   =   self  .tokenizer(
all_input_texts,   padding  =  False  ,   return_tensors  =  "np"  ,
truncation  =  True  ,   max_length  =  MAX_TOKENS  )
input_ids   =   cast(Any, batch_encoding[  "input_ids"  ])
token_tensors   =   [torch.tensor(seq,   dtype  =  torch.long)   for   seq   in   input_ids]
token_lengths   =   [t.shape[  0  ]   for   t   in   token_tensors]
permutation   =   sorted  (  range  (  len  (token_tensors)),   key  =lambda   i:   -  token_lengths[i])
token_tensors_permuted   =   [token_tensors[i]   for   i   in   permutation]
token_lengths_permuted   =   [token_lengths[i]   for   i   in   permutation]
batches_of_indices   =   create_optimal_batches(token_lengths_permuted)
logits_permuted: list[  float  ]   =   [  0.0  ]   *   len  (token_tensors_permuted)
for   batch_indices   in   batches_of_indices:
batch_tensors   =   [token_tensors_permuted[i]   for   i   in   batch_indices]
outputs   =   await   asyncio.to_thread(
self  .runner.generate, batch_tensors,   max_new_tokens  =  1  ,   end_id  =  0  ,
return_dict  =  True  ,   output_generation_logits  =  True  )
gen_logits   =   outputs[  "generation_logits"  ]
for   position, batch_idx   in   enumerate  (batch_indices):
logits_permuted[batch_idx]   =   float  (
gen_logits[position][  0  ,   0  ,   0  ].cpu().numpy())
all_logits: list[  float  ]   =   [  0.0  ]   *   len  (logits_permuted)
for   perm_idx, orig_idx   in   enumerate  (permutation):
all_logits[orig_idx]   =   logits_permuted[perm_idx]
scores   =   [sigmoid(logit   /   5.0  )   for   logit   in   all_logits]
return   Output(  scores  =  scores,   token_lengths  =  token_lengths)
```


Same structure as the rerank deploy, plus Matryoshka projection (truncatable` dimensions` ) and query/document prefixes. Deploy:


```text
RELEASE_NAME  =  zembed-1-prod   MODEL_ID  =  zeroentropy/zembed-1-fp8   MIN_CONTAINERS  =  1   \
modal   deploy   inference/inference/modal/deploy_embed.py
```


A simpler variant that runs the prebuilt TEI container instead of building an engine is at` inference/inference/modal/deploy_qwen_embed.py` .


The file —` inference/inference/modal/deploy_embed.py` :


```text
import   asyncio
import   base64
import   json
import   math
import   os
from   enum   import   Enum
from   typing   import   Any, cast


import   modal
import   numpy   as   np
import   torch
from   dotenv   import   load_dotenv
from   fastapi   import   HTTPException
from   pydantic   import   BaseModel
from   safetensors.torch   import   load_file
from   tensorrt_llm.runtime   import   ModelRunnerCpp
from   transformers   import   AutoTokenizer
from   transformers.tokenization_utils_fast   import   PreTrainedTokenizerFast


load_dotenv(  override  =  True  )


RELEASE_NAME   =   os.environ[  "RELEASE_NAME"  ]
MODEL_ID   =   os.environ[  "MODEL_ID"  ]
MIN_CONTAINERS   =   int  (os.environ[  "MIN_CONTAINERS"  ])


CHECKPOINT_PATH   =   "/root/models/checkpoint"
TOKENIZER_PATH   =   "/root/models/tokenizer"
ENGINE_PATH   =   "/root/models/engine"


MAX_TOKENS   =   16384
PER_DEVICE_BATCH_SIZE_TOKENS   =   30_000
MAX_BATCH_SIZE   =   64


NPTokens   =   np.ndarray[Any, np.dtype[np.int64]]


class   ProbeMetadata  (  BaseModel  ):
hidden_size:   int
probe_token_start:   int


total_tokens   =   max_tokens   *   batch_size
if   batch_size   ==   1  :
a, b, c   =   5.192278e-06  ,   1.125020  ,   7.528333e-03
return   a   *   total_tokens  **  b   +   c
else  :
b   =   1.041202   +   0.345358   *   math.exp(  -  0.172809   *   batch_size)
c   =   0.001274   *   batch_size   +   0.008115
a   =   math.exp(  -  8.3441   *   b   -   3.3713  )
return   a   *   total_tokens  **  b   +   c


def   create_optimal_batches  (
token_lengths: list[  int  ],
max_batch_tokens:   int   =   PER_DEVICE_BATCH_SIZE_TOKENS  ,
max_batch_size:   int   =   MAX_BATCH_SIZE  ,
) -> list[list[  int  ]]:
if   len  (token_lengths)   ==   0  :
return   []
n   =   len  (token_lengths)
if   n   <=   max_batch_size:
max_tokens   =   max  (token_lengths)
if   max_tokens   *   n   <=   max_batch_tokens:
return   [  list  (  range  (n))]
batches: list[list[  int  ]]   =   []
current_batch: list[  int  ]   =   []
current_max_tokens   =   0
for   i   in   range  (n):
token_length   =   token_lengths[i]
new_max_tokens   =   max  (current_max_tokens, token_length)
new_batch_size   =   len  (current_batch)   +   1
can_add   =   (new_batch_size   <=   max_batch_size
and   new_max_tokens   *   new_batch_size   <=   max_batch_tokens)
if   can_add:
current_batch.append(i)
current_max_tokens   =   new_max_tokens
else  :
if   len  (current_batch)   >   0  :
batches.append(current_batch)
current_batch   =   [i]
current_max_tokens   =   token_length
if   len  (current_batch)   >   0  :
batches.append(current_batch)
return   batches


def   test_engine  () ->   None  :
runner   =   ModelRunnerCpp.from_dir(
warmup_tokens   =   torch.tensor([[  100  ]   *   10  ],   dtype  =  torch.long)
return_dict  =  True  ,   output_generation_logits  =  True  )


image   =   (
modal.Image.debian_slim(  python_version  =  "3.12"  )
.apt_install([  "git"  ,   "wget"  ,   "libopenmpi-dev"  ])
.run_commands(
"dpkg -i cuda-keyring_1.1-1_all.deb"  ,
"apt-get update"  ,
"apt-get -y install libcudnn9-cuda-12 libcudnn9-dev-cuda-12"  ,
)
.pip_install([
"torch==2.7.1"  ,   "fastapi[standard]==0.115.4"  ,   "transformers==4.53.1"  ,
"pydantic==2.11.3"  ,   "huggingface_hub==0.36.0"  ,   "python-dotenv==1.2.1"  ,
])
.pip_install([  "onnx==1.19.1"  ,   "onnx-graphsurgeon==0.5.8"  ])
.env({  "PYTORCH_CUDA_ALLOC_CONF"  :   "expandable_segments:True"  ,
"TOKENIZERS_PARALLELISM"  :   "false"  })
.run_commands(
f  "hf download   {MODEL_ID}   --local-dir   {CHECKPOINT_PATH}  "  ,
f  "--gpt_attention_plugin bfloat16 --kv_cache_type disabled "
f  "--max_input_len 32768 --max_num_tokens 32768 --max_batch_size 256 "
f  "--norm_quant_fusion enable --gemm_plugin fp8 --gather_generation_logits "  ,
f  "cp   {CHECKPOINT_PATH}  /probe_metadata.json   {ENGINE_PATH}  /probe_metadata.json"  ,
f  "cp   {CHECKPOINT_PATH}  /projections.safetensors   {ENGINE_PATH}  /projections.safetensors"  ,
f  "rm -r   {CHECKPOINT_PATH}  "  ,
gpu  =  "H100"  ,
secrets  =  [modal.Secret.from_name(  "zeroentropy-huggingface"  )],
)
.run_commands(
f  'echo "RELEASE_NAME=  {RELEASE_NAME}\\  nMODEL_ID=  {MODEL_ID}\\  nMIN_CONTAINERS=  {MIN_CONTAINERS}  " > /root/.env'  ,
)
.run_function(test_engine,   gpu  =  "H100"  )
)


app   =   modal.App(  name  =  RELEASE_NAME  ,   image  =  image)


class   EmbeddingType  (  str  ,   Enum  ):
DOCUMENT   =   "document"
QUERY   =   "query"


class   Input  (  BaseModel  ):
embedding_type: EmbeddingType
input  : list[  str  ]
dimensions:   int   |   None   =   None


class   Output  (  BaseModel  ):
embeddings: list[  str  ]
token_lengths: list[  int  ]


@app.cls  (
gpu  =  "H100"  ,   cpu  =  4  ,   memory  =  32768  ,   scaledown_window  =  30  ,
min_containers  =  MIN_CONTAINERS  ,   max_containers  =  100  ,
)
@modal.concurrent  (  max_inputs  =  4  )
class   Model  :
tokenizer: PreTrainedTokenizerFast
runner: Any
probe_metadata: ProbeMetadata
query_prefix_tokens: NPTokens
document_prefix_tokens: NPTokens
suffix_tokens: NPTokens
inference_lock: asyncio.Lock
projections: dict[  int  , torch.Tensor]


@modal.enter  ()
def   setup  (self) ->   None  :
tokenizer   =   cast(Any, AutoTokenizer.from_pretrained(  TOKENIZER_PATH  ,   padding_side  =  "right"  ))
assert   isinstance  (tokenizer, PreTrainedTokenizerFast)
self  .tokenizer   =   tokenizer
with   open  (  f  "  {ENGINE_PATH}  /probe_metadata.json"  )   as   f:
self  .probe_metadata   =   ProbeMetadata.model_validate_json(f.read())
projections_tensors   =   load_file(  f  "  {ENGINE_PATH}  /projections.safetensors"  )
device   =   torch.device(  "cuda"   if   torch.cuda.is_available()   else   "cpu"  )
projections_tensors   =   {k: v.to(device)   for   k, v   in   projections_tensors.items()}
dims   =   sorted  (  int  (k)   for   k   in   projections_tensors.keys())
projections: dict[  int  , torch.Tensor]   =   {}
for   i, d   in   enumerate  (dims):
mat   =   projections_tensors[  str  (d)]
for   upper   in   dims[i   +   1  :]:
mat   =   projections_tensors[  str  (upper)]   @   mat
projections[d]   =   mat
self  .projections   =   projections
self  .runner   =   ModelRunnerCpp.from_dir(
self  .inference_lock   =   asyncio.Lock()
QUERY_PREFIX_TEXT   =   "<|im_start|>system  \n  query<|im_end|>  \n  <|im_start|>user  \n  "
DOCUMENT_PREFIX_TEXT   =   "<|im_start|>system  \n  document<|im_end|>  \n  <|im_start|>user  \n  "
SUFFIX_TEXT   =   "<|im_end|>  \n  "
self  .query_prefix_tokens   =   np.array(
tokenizer.encode(  QUERY_PREFIX_TEXT  ,   add_special_tokens  =  False  ),   dtype  =  np.int64)
self  .document_prefix_tokens   =   np.array(
tokenizer.encode(  DOCUMENT_PREFIX_TEXT  ,   add_special_tokens  =  False  ),   dtype  =  np.int64)
self  .suffix_tokens   =   np.array(
tokenizer.encode(  SUFFIX_TEXT  ,   add_special_tokens  =  False  ),   dtype  =  np.int64)
warmup_tokens   =   torch.tensor([[  100  ]   *   10  ],   dtype  =  torch.long)
return_dict  =  True  ,   output_generation_logits  =  True  )


async   def   endpoint  (self, request: Input) -> Output:
if   len  (request.input)   ==   0  :
return   Output(  embeddings  =  [],   token_lengths  =  [])
if   len  (request.input)   >   2048  :
raise   HTTPException(  status_code  =  400  ,   detail  =  "Input is too large, max 2048."  )
match   request.embedding_type:
case   EmbeddingType.  QUERY  :
prefix_tensor   =   self  .query_prefix_tokens
case   EmbeddingType.  DOCUMENT  :
prefix_tensor   =   self  .document_prefix_tokens
max_content_token_len   =   MAX_TOKENS   -   (  len  (  self  .query_prefix_tokens)   +   len  (  self  .suffix_tokens))
batch_encoding   =   await   asyncio.to_thread(
self  .tokenizer, request.input,   add_special_tokens  =  False  ,   padding  =  False  ,
return_tensors  =  "np"  ,   truncation  =  True  ,   max_length  =  max_content_token_len)
token_sequences   =   cast(list[NPTokens], batch_encoding[  "input_ids"  ])
token_sequences   =   [np.concatenate([prefix_tensor, seq,   self  .suffix_tokens])
for   seq   in   token_sequences]
token_tensors   =   [torch.tensor(seq,   dtype  =  torch.long)   for   seq   in   token_sequences]
token_lengths   =   [  len  (tokens)   for   tokens   in   token_tensors]
token_tensors_permuted   =   [token_tensors[i]   for   i   in   permutation]
token_lengths_permuted   =   [token_lengths[i]   for   i   in   permutation]
batch_indices   =   create_optimal_batches(token_lengths_permuted)
batches   =   [[token_tensors_permuted[i]   for   i   in   batch]   for   batch   in   batch_indices]
embeddings_permuted: list[np.ndarray[Any, np.dtype[np.float32]]]   =   []
async   with   self  .inference_lock:
for   batch_tensors   in   batches:
outputs   =   await   asyncio.to_thread(
return_dict  =  True  ,   output_generation_logits  =  True  )
for   logits   in   outputs[  "generation_logits"  ]:
embedding_logits   =   logits[  0  ,   0  ,
self  .probe_metadata.probe_token_start:
self  .probe_metadata.probe_token_start   +   self  .probe_metadata.hidden_size].float()
if   (request.dimensions   is   not   None
and   request.dimensions   !=   self  .probe_metadata.hidden_size):
if   request.dimensions   not   in   self  .projections:
raise   HTTPException(  status_code  =  400  ,
detail  =  f  "Invalid truncation dimension   {  request.dimensions  }  ."  )
embedding_logits   =   embedding_logits   @   self  .projections[request.dimensions]
embedding   =   embedding_logits.cpu().numpy()
norm   =   np.linalg.norm(embedding)
if   norm   >   0  :
embedding   =   embedding   /   norm
embeddings_permuted.append(embedding)
embeddings   =   [e   for   _, e   in   sorted  (  zip  (permutation, embeddings_permuted,   strict  =  True  ))]
embeddings_encoded   =   [
base64.b64encode(e.astype(  "<f2"  ).tobytes()).decode(  "utf-8"  )   for   e   in   embeddings]
return   Output(  embeddings  =  embeddings_encoded,   token_lengths  =  token_lengths)
```


## Search (zSearch)


` top-documents` is: embed the query → hybrid retrieval (keyword + semantic) → rerank → ranked results. Reproduce it on a search engine you run.


**These engines all do the same core job** — hybrid vector + keyword retrieval with filtering — so they’re interchangeable for retrieval. The practical difference is whether the engine also runs the models:


Engine Embeddings + rerank Best when


**Elasticsearch** / OpenSearch **Built in** (inference endpoints) — app sends text Fewest code changes; often already in the stack


**Turbopuffer** You call embed + rerank APIs Serverless, cheap, object-storage backed


**Milvus** / Zilliz Cloud You call embed + rerank APIs Open-source, vector-first; self-host or managed


Pinecone / Qdrant You call embed + rerank APIs Already in the stack


Postgres + pgvector You call embed + rerank APIs No new service at modest scale


That split gives two integration shapes — pick one:


- **Option A — engine owns the models** (Elasticsearch / OpenSearch): the app sends text; the engine embeds and reranks.
- **Option B — vector database + your own models** (Turbopuffer / Milvus / Pinecone / Qdrant): the app calls an embedding API and a reranker around the store.


**Text extraction** and the **data backfill** are the same for both.


### Option A — Elasticsearch (engine owns embeddings + rerank)


#### Create inference endpoints


Create inference endpoints for embeddings and reranking. Named endpoints keep the IDs stable:


```text
// Embeddings — ELSER (sparse) is a strong zero-tuning default
PUT _inference/sparse_embedding/ze-embed
{   "service"  :   "elser"  ,   "service_settings"  : {   "num_allocations"  :   1  ,   "num_threads"  :   1   } }


// Reranker — Elastic Rerank
PUT _inference/rerank/ze-rerank
{   "service"  :   "elasticsearch"  ,
"service_settings"  : {   "model_id"  :   ".rerank-v1"  ,   "num_allocations"  :   1  ,   "num_threads"  :   1   } }
```


(Elasticsearch also ships preconfigured endpoints whose IDs vary by version —` GET _inference` to list them. To rerank with Cohere, create` ze-rerank` with` "service": "cohere"` .)


#### Create the index


Create the index with a` semantic_text` field plus filter metadata.` semantic_text` auto-chunks and auto-embeds:


```text
PUT ze-search
{   "mappings"  : {   "properties"  : {
"content"  :    {   "type"  :   "semantic_text"  ,   "inference_id"  :   "ze-embed"   },
"collection"  : {   "type"  :   "keyword"   },
"path"  :       {   "type"  :   "keyword"   },
"source_url"  : {   "type"  :   "keyword"  ,   "index"  :   false   },
"metadata"  :   {   "type"  :   "object"  ,   "enabled"  :   true   }
}}}
```


#### Ingest


Index extracted text ( **OCR first** for file uploads):


```text
from   elasticsearch   import   Elasticsearch
es   =   Elasticsearch(  ELASTIC_URL  ,   api_key  =  ELASTIC_API_KEY  )


def   index_document  (collection, path, text, source_url  =  None  , metadata  =  None  ):
es.index(  index  =  "ze-search"  ,   id  =  f  "  {  collection  }  :  {  path  }  "  ,   document  =  {
"content"  : text,   "collection"  : collection,   "path"  : path,
"source_url"  : source_url,   "metadata"  : metadata   or   {}})
```


#### Search


Replace` top-documents` with one` _search` ; the retriever does hybrid + rerank:


```text
resp   =   es.search(  index  =  "ze-search"  ,   size  =  10  ,   retriever  =  {
"text_similarity_reranker"  : {
"retriever"  : {   "rrf"  : {   "retrievers"  : [
{   "standard"  : {   "query"  : {   "bool"  : {
"must"  : {   "match"  : {   "content"  : query } },
"filter"  : {   "term"  : {   "collection"  : collection } } } } } },
{   "standard"  : {   "query"  : {   "bool"  : {
"must"  : {   "semantic"  : {   "field"  :   "content"  ,   "query"  : query } },
"filter"  : {   "term"  : {   "collection"  : collection } } } } } },
],   "rank_window_size"  :   100   } },
"field"  :   "content"  ,   "inference_id"  :   "ze-rerank"  ,
"inference_text"  : query,   "rank_window_size"  :   100   } })
hits   =   [(h[  "_source"  ][  "path"  ], h[  "_score"  ])   for   h   in   resp[  "hits"  ][  "hits"  ]]
```


The inner` rrf` needs its own` rank_window_size` ≥ the reranker’s, or the search is rejected (` rrf` defaults its window to` size` ). One quirk to handle: when a query matches **zero** candidates,` text_similarity_reranker` returns HTTP 400 (` \[rank\] requires \[size\] greater than \[0\]` ) instead of an empty hit list — catch it and treat as no results.


### Option B — Vector database (Turbopuffer, Milvus, …)


The app owns three calls: **embed + upsert** at ingest, and **embed + search + rerank** at query. It’s a thin layer — **` migrate.py`** implements it for Turbopuffer, Pinecone, and Qdrant. Turbopuffer shown here:


**Ingest** ( **OCR first** for file uploads):


```text
import   turbopuffer
from   openai   import   OpenAI
tpuf   =   turbopuffer.Turbopuffer(  api_key  =  TPUF_KEY  ,   region  =  "gcp-us-central1"  )
ns   =   tpuf.namespace(  "ze-search"  )
openai   =   OpenAI()


def   embed  (text):
return   openai.embeddings.create(  model  =  "text-embedding-3-large"  ,   input  =  text).data[  0  ].embedding


def   index_document  (collection, path, text, source_url  =  None  ):
ns.write(  distance_metric  =  "cosine_distance"  ,   upsert_rows  =  [{
"id"  :   f  "  {  collection  }  :  {  path  }  "  ,   "vector"  : embed(text),
"text"  : text,   "collection"  : collection,   "path"  : path,   "source_url"  : source_url}])
```


**Search** — retrieve from the store, then rerank with Cohere:


```text
import   cohere
co   =   cohere.ClientV2()


def   search  (collection, query, k  =  10  ):
rows   =   ns.query(  rank_by  =  (  "vector"  ,   "ANN"  , embed(query)),   top_k  =  100  ,
filters  =  (  "collection"  ,   "Eq"  , collection),   include_attributes  =  True  ).rows
ranked   =   co.rerank(  model  =  "rerank-v3.5"  ,   query  =  query,
documents  =  [r[  "text"  ]   for   r   in   rows],   top_n  =  k)
return   [(rows[x.index][  "path"  ], x.relevance_score)   for   x   in   ranked.results]
```


Swap OpenAI→Voyage or Cohere→Voyage as desired. Milvus / Pinecone / Qdrant follow the same three-call shape (see` migrate.py` for adapters). Store original files in object storage and keep only` source_url` in the index.


### Text extraction (OCR) — new uploads


ZeroEntropy OCR’d and parsed uploaded PDFs, images, and Office files. This is the one capability taken fully in-house, and it’s the same for either option above: extract text, then index it. Use a managed OCR service — the mainstream choices are **AWS Textract** and **Google Document AI / Cloud Vision** , matched to your cloud.


OCR option Use when


**AWS Textract** On AWS — the common default; forms, tables, scans


**Google Document AI / Cloud Vision** On GCP — high-accuracy general OCR


**Azure AI Document Intelligence** On Azure


Mistral OCR / LlamaParse / Reducto Want Markdown-structured output tuned for RAG (newer, LLM-based)


Tesseract Open-source, self-hosted; simple docs, lower accuracy


```text
import   boto3
textract   =   boto3.client(  "textract"  )


def   extract_text  (file_bytes):                      # single image / small PDF page
resp   =   textract.detect_document_text(  Document  =  {  "Bytes"  : file_bytes})
return   "  \n  "  .join(b[  "Text"  ]   for   b   in   resp[  "Blocks"  ]   if   b[  "BlockType"  ]   ==   "LINE"  )
# Multipage PDFs: use start_document_text_detection (async, reads the file from S3).
```


For **digitally-generated** PDFs/Office files (embedded text, no scans), Elasticsearch’s` attachment` processor (Apache Tika) can extract text with no external service — but **scans/images need real OCR** . If the pipeline already sends plain text, skip this step.


## Migrate your existing data


Existing documents live in ZeroEntropy. Pull their already-extracted text through the API and load it into the target — **no re-parsing or OCR** :


1. ` POST /collections/get-collection-list` → collections
2. ` POST /documents/get-document-info-list` (paged) → documents
3. ` POST /documents/get-page-info` with` include_content: true` → extracted page text
4. Index into the target


- **Elasticsearch:**` backfill_to_es.py`
- **Vector database:**` migrate.py migrate` (Turbopuffer / Pinecone / Qdrant)


Both scripts reuse the same ZeroEntropy export.


## Test your search


- Run real queries against **both** ZeroEntropy and the new engine; compare top-k overlap (or NDCG).
- Tune until quality matches: for Elasticsearch adjust` rank_window_size` or the BM25/semantic mix; for a vector DB adjust the candidate count before rerank. Different models produce different score scales — compare **ordering** , not absolute scores.


## Checklist


- **Models:** rerank/embed callers repointed to the new API (or self-hosted ZeroEntropy weights).
- **Search:** engine provisioned; embeddings + rerank wired (inference endpoints for Elasticsearch, or embed/rerank API calls for a vector DB).
- Ingestion updated: OCR step added for file uploads; documents indexed into the engine.
- Search updated:` top-documents` replaced; collection/metadata filters preserved.
- Existing data backfilled from ZeroEntropy; per-collection counts match.
- Search tested against the ZeroEntropy baseline; retrieval + rerank tuned.
- Cut over behind a feature flag, then remove ZeroEntropy calls and keys.
