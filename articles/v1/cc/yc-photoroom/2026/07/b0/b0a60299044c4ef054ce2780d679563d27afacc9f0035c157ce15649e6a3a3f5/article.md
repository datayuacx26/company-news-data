---
schema_version: "1.0.0"
document_id: "b0a60299044c4ef054ce2780d679563d27afacc9f0035c157ce15649e6a3a3f5"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/stable-diffusion-100-percent-faster-with-memory-efficient-attention"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:faeb3bdb8c373419b53414afdbaf2fc36e24991cbabfb4ec0255dfa7f53e5fb5"
---

# Make stable diffusion up to 100% faster with Memory Efficient Attention

At Photoroom we build photo editing apps, and being able to generate what you have in mind is a superpower. Diffusion models are a recent take on this, based on iterative steps: a pipeline runs recursive operations starting from a noisy image until it generates the final high-quality image. Their quality and expressivity, starting from a user prompt, were an opportunity to improve the Photoroomer experience.


In a previous[blog post](https://www.photoroom.com/inside-photoroom/stable-diffusion-25-percent-faster-and-save-seconds) , we investigated how to make stable diffusion faster using TensorRT at inference time, here we will investigate how to make it even faster, using Memory Efficient Attention from the[xformers](https://github.com/facebookresearch/xformers) library.


## A few words about memory efficient attention


The attention operation is at the heart of the[Transformer](https://arxiv.org/abs/1706.03762) model architecture, which got popular in the last couple of years in the AI space. It’s very useful for a model to make sense of the connections which can happen between elements of a sequence, which can be sound bites, pixels or words for instance. This operation typically takes three inputs: the Query, the Key and the Value. If all three refer to the same tensor, it becomes known as self-attention.


This operation is not restricted to Transformers though, and the[latent diffusion](https://arxiv.org/abs/2112.10752) model on which is based Stable Diffusion uses it inside the core denoising steps, notably to take various forms of guidance into account. Its formulation is as follows, and looks fairly innocuous:


```text
attention = softmax(QKˆT).V;
```


From a complexity standpoint, three things can be considered here: the compute cost of this operation, its memory footprint, and the I/O (input/output, ie: memory operations) that it entails.


If we put aside the batch dimension (global multiplier), and use N for the context length and H for the head size (let’s suppose Q, K and V have the same dimensions for the sake of clarity), a breakdown of this operation as executed by PyTorch is as follows:


-


compute QKˆT


-


matrix multiplication (O(Nˆ2H))


-


NxN result stored in main memory, NxH reads


-


compute the softmax normalization


-


line per line


-


reads and writes are O(Nˆ2), compute is also O(Nˆ2)


-


NxN matrix stored in main GPU memory, often referred to as the “attention matrix”


-


compute the .V matrix product


-


matrix multiplication NN*NH (O(Nˆ2H))


-


final NxH result stored in main memory, O(Nˆ2) reads and O(NH) writes


Even in this simplified form, which does not account for training, and saving activation for instance, there are multiple takeaways. Note that N >> H, typically.


-


the max memory use scales with Nˆ2


-


the compute scales roughly with Nˆ2


-


there are multiple trips to the main memory attached to significant data sizes


**The attention operation is thus a lot more complicated and demanding than it looks. Both I/O and compute costs scale around O(Nˆ2)** , N is related to the size of the latent space in Stable Diffusion (which itself relates to the output resolution). Apples to oranges, but one can also remark that the IO needs are relatively comparable (in terms of the number of elements involved) to the compute. Now consider for instance[this nice blog post](https://horace.io/brrr_intro.html) from Horace He, and it becomes apparent that a significant amount of time will be spent on the I/O, which will be a bottleneck for the GPU compute units.


Luckily, there exist work tackling this issue, starting for instance with[Rabe](https://arxiv.org/abs/2112.05682v2) et al., and more recently with[Tri Dao](https://arxiv.org/abs/2205.14135) et al, under the name of Flash attention (‣). How this works is that **the above three steps can be fused into one computation, given the insights that there is no dependency across the lines of the attention matrix, and that the softmax computation can be done without materializing the full line** . K, Q are read over tiles, and a running softmax formulation is used. The resulting per-tile computation is immediately used against a tile of V, only the end result being written to the main GPU memory. This formulation removes all intermediate reads and writes, which increases speed by removing an I/O bottleneck. Coincidentally, it also relieves a lot of the memory pressure, since the full attention matrix is never materialized.


*”FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness”,* Tri Dao et al.


Implementing this efficiently on GPU is difficult, notably due to these chips requiring a high level of parallelism to be efficient. We used the kernels developed by the[xformers](https://github.com/facebookresearch/xformers) team, which refer to the original FlashAttention kernels in some cases but also use more optimized kernels for some configurations. OpenAI’s[Triton language](https://github.com/openai/triton) also proposes an implementation of this method. Note that the above is a very simplified description and that getting this to work for training is no small feat.


## Speeding up Stable diffusion


### Code updates


In order to leverage the memory efficient attention to speed up the unet we only need to update the file in` diffusers/src/diffusers/models/attention.py` and add the following two blocks


```text
import xformers
import xformers.ops
from typing import Any, Optional
```


and


```text
class MemoryEfficientCrossAttention(nn.Module):
def __init__(self, query_dim, context_dim=None, heads=8, dim_head=64, dropout=0.0):
super().__init__()
inner_dim = dim_head * heads
context_dim = default(context_dim, query_dim)


self.heads = heads
self.dim_head = dim_head


self.to_q = nn.Linear(query_dim, inner_dim, bias=False)
self.to_k = nn.Linear(context_dim, inner_dim, bias=False)
self.to_v = nn.Linear(context_dim, inner_dim, bias=False)


self.to_out = nn.Sequential(nn.Linear(inner_dim, query_dim), nn.Dropout(dropout))
self.attention_op: Optional[Any] = None


def forward(self, x, context=None, mask=None):
q = self.to_q(x)
context = default(context, x)
k = self.to_k(context)
v = self.to_v(context)


b, _, _ = q.shape
q, k, v = map(
lambda t: t.unsqueeze(3)
.reshape(b, t.shape[1], self.heads, self.dim_head)
.permute(0, 2, 1, 3)
.reshape(b * self.heads, t.shape[1], self.dim_head)
.contiguous(),
(q, k, v),
)


# actually compute the attention, what we cannot get enough of
out = xformers.ops.memory_efficient_attention(q, k, v, attn_bias=None, op=self.attention_op)


# TODO: Use this directly in the attention operation, as a bias
if exists(mask):
raise NotImplementedError
out = (
out.unsqueeze(0)
.reshape(b, self.heads, out.shape[1], self.dim_head)
.permute(0, 2, 1, 3)
.reshape(b, out.shape[1], self.heads * self.dim_head)
)
return self.to_out(out)
```


You will then need to update the` BasicTransformerBlock` as follows:


```text
_USE_MEMORY_EFFICIENT_ATTENTION = int(os.environ.get("USE_MEMORY_EFFICIENT_ATTENTION", 0)) == 1
class BasicTransformerBlock(nn.Module):
r"""
A basic Transformer block.
Parameters:
dim (:obj:`int`): The number of channels in the input and output.
n_heads (:obj:`int`): The number of heads to use for multi-head attention.
d_head (:obj:`int`): The number of channels in each head.
dropout (:obj:`float`, *optional*, defaults to 0.0): The dropout probability to use.
context_dim (:obj:`int`, *optional*): The size of the context vector for cross attention.
gated_ff (:obj:`bool`, *optional*, defaults to :obj:`False`): Whether to use a gated feed-forward network.
checkpoint (:obj:`bool`, *optional*, defaults to :obj:`False`): Whether to use checkpointing.
"""


def __init__(
self,
dim: int,
n_heads: int,
d_head: int,
dropout=0.0,
context_dim: Optional[int] = None,
gated_ff: bool = True,
checkpoint: bool = True,
):
super().__init__()
AttentionBuilder = MemoryEfficientCrossAttention if _USE_MEMORY_EFFICIENT_ATTENTION else CrossAttention
self.attn1 = AttentionBuilder(
query_dim=dim, heads=n_heads, dim_head=d_head, dropout=dropout
)  # is a self-attention
self.ff = FeedForward(dim, dropout=dropout, glu=gated_ff)
self.attn2 = AttentionBuilder(
query_dim=dim, context_dim=context_dim, heads=n_heads, dim_head=d_head, dropout=dropout
)  # is self-attn if context is none
self.norm1 = nn.LayerNorm(dim)
self.norm2 = nn.LayerNorm(dim)
self.norm3 = nn.LayerNorm(dim)
self.checkpoint = checkpoint


def _set_attention_slice(self, slice_size):
self.attn1._slice_size = slice_size
self.attn2._slice_size = slice_size


def forward(self, hidden_states, context=None):
hidden_states = hidden_states.contiguous() if hidden_states.device.type == "mps" else hidden_states
hidden_states = self.attn1(self.norm1(hidden_states)) + hidden_states
hidden_states = self.attn2(self.norm2(hidden_states), context=context) + hidden_states
hidden_states = self.ff(self.norm3(hidden_states)) + hidden_states
return hidden_states
```


And that should be enough to leverage` xformers` ’s memory efficient attention.


### Benchmarking both implementations


At Photoroom we rely heavily on the[NGC containers provided by NVidia](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) as they come with:


-


PyTorch


-


TensorRT


-


Torch-TensorRT


pre-installed. This is our work environment. Here we use the August Nvidia NGC container for our setup:


```text
sudo docker pull nvcr.io/nvidia/pytorch:22.08-py3


sudo docker run -it --gpus=all --ipc=host -v /home:/home nvcr.io/nvidia/pytorch:22.08-py3 bash


# Install xformers
pip install git+https://github.com/facebookresearch/xformers@51dd119#egg=xformers


# Install diffusers' dependencies
pip install transformers ftfy scipy


# Clone my branch if this not merged yet
git clone  [email protected]  :MatthieuTPHR/diffusers.git
cd diffusers && git checkout memory_efficient_attention
pip install -e .
```


To run a very quick benchmark, save the following snippet of code into a` test.py` file:


```text
import torch
from diffusers import StableDiffusionPipeline


pipe = StableDiffusionPipeline.from_pretrained(
"CompVis/stable-diffusion-v1-4", revision="fp16", torch_dtype=torch.float16, use_auth_token=True
).to("cuda")


with torch.inference_mode(), torch.autocast("cuda"):
image = pipe("a small cat")
```


We would normally advise to` torch.cuda.synchronize()` calls after each UNet passes with a better time profiler, but in this case, the speed up is so significant that it is not absolutely necessary.


You can finally run:


```text
# Run without the Memory Efficient Attention
python test.py


# Run with the Memory Efficient Attention
USE_MEMORY_EFFICIENT_ATTENTION=1 python test.py
```


And voilà!


Here are the speedups we obtain for various GPUs:


As we can see the memory-efficient attention kernels from the xformers library yield significant boosts in speed on various Nvidia GPUs with up to 100% improvement on the latest generation of inference GPUs, the Nvidia A10G.


## Conclusion


Diffusion model families are very promising for photo-realistic image generation from text prompts. However, the pipeline is iterative and needs to perform intensive computation at each step. This behavior can lead to long waiting periods for Photoroom users. In this blog post, we showed you how we exploit xformers’ memory efficient attentions to optimize the stable diffusion pipeline released by Hugging Face. This snippet of code is not yet compatible with TensorRT, but we are currently working on making this possible. These modifications allowed us to double the speed on the Nvidia A10G inference GPUs, which will really improve the Photoroomer experience.


## Acknowledgments


This work would not have been possible without the fantastic work of:


-


[Tri Dao](https://github.com/tridao) and his fellow authors of the[Flash Attention paper](https://arxiv.org/pdf/2205.14135.pdf)


-


The[xformers](https://github.com/facebookresearch/xformers/tree/main/xformers) team,[Franciso Massa](https://github.com/fmassa) ,[dan_the_3rd](https://github.com/danthe3rd) , and their fellow contributors


### This could also be interesting for you:


-


Read all about the[Remove background API](https://www.photoroom.com/api/remove-background) which is used by Meta, Apple and Huggingface.
