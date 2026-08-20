---
schema_version: "1.0.0"
document_id: "07c62e062776d7f11c43c0abbdaf22e6e76fe21e8b28facd90ae899da587e577"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/stable-diffusion-25-percent-faster-and-save-seconds"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:0c6a2eb9cc835351ae0d6d70e8aa1d749b1d79ba79629ac30851dc0ac6a8173b"
---

# Making stable diffusion 25% faster using TensorRT

At Photoroom we build photo editing apps, and being able to generate what you have in mind is a superpower. Diffusion models are a recent take on this, based on iterative steps: a pipeline runs recursive operations starting from a noisy image until it generates the final high-quality image. Their quality and expressiveness, starting from a user prompt, were an opportunity to improve the PhotoRoomer experience.


This GIF shows all the intermediates frames generated during the iterative diffusion process:


As you can see, the iterative process produces photo-realistic images with amazing quality! Unfortunately, it is also very slow and not convenient for deployment across a huge number of users. The main disadvantage is that we need to run models that required intensive computation at each step. In fact, there are two main axes to reduce the execution time of the diffusion process: reduce the number of iterative processes or reduce the number of intensive operations per step. The problem with the first option is that it can directly impact the quality of the rendering. As a photo editing apps company, we don’t want to degrade the image quality: this is why we have chosen the second way.


In this post, we want to show how Photoroom saves several seconds per request by optimizing diffusion models with[Nvidia’s TensorRT](https://github.com/NVIDIA/TensorRT) library. This library generally helps us reach great inference performance while reducing the memory consumption on Nvidia GPUs. If you are interested you can find more details in this[previous post](https://www.photoroom.com/inside-photoroom/faster-image-segmentation-trtorch-tensorrt) .


The blog post details the steps to achieve the diffusion inference and optimization:


1.


Run inference of a diffusion model


2.


Save seconds with Nvidia’s TensorRT


## Part I: Run inference of a diffusion model


Several AI blocks are used during the diffusion process to iteratively generate an image. Reproducing from scratch the full pipeline of a diffusion model can be time-consuming and challenging. Luckily, there are open-source repositories that allow us to run it in a few lines of code and with pre-trained weights.


## Hugging Face stable diffusion models


This is the case of Hugging Face who recently shared the Stable Diffusion code and weights.


[Stable Diffusion](https://github.com/CompVis/stable-diffusion/) is the state-of-the-art text-to-image model, which allows photo-realistic image generation from a text prompt. At Photoroom we are very excited about the quality of this algorithm, this is why we put it on top of our AI list! If you want to know more about it we strongly recommend you to read this[seminal paper](https://arxiv.org/abs/2112.10752) .


Hugging Face made its[diffusers library](https://github.com/huggingface/diffusers) fully compatible with Stable Diffusion, which allows us to easily perform inference with this model. From that, you can easily generate images with this technology. This[great blog](https://huggingface.co/blog/stable_diffusion) post explains how to run set-by-step a diffusion model.


## Stable diffusion inference script


Now you can write a script inspired by the Hugging Face Blog that would allow you to run the inference phase of the stable-diffusion model:


```text
import torch
from torch import autocast
from transformers import CLIPTextModel, CLIPTokenizer
from diffusers import AutoencoderKL, UNet2DConditionModel, PNDMScheduler
from diffusers import LMSDiscreteScheduler
from tqdm.auto import tqdm
from PIL import Image


torch_device = "cuda"


YOUR_TOKEN='Add your Hugging Face Token Here'


prompt = ["A big banana leaf with lemons in front of it"]
height = 512                        # default height of Stable Diffusion
width = 512                         # default width of Stable Diffusion
num_inference_steps = 100           # Number of denoising steps
guidance_scale = 7.5                # Scale for classifier-free guidance
batch_size = len(prompt)
UNET_INPUTS_CHANNEL=4
# 1. Load the autoencoder model which will be used to decode the latents into image space.
vae = AutoencoderKL.from_pretrained("CompVis/stable-diffusion-v1-4",
subfolder="vae",
use_auth_token=YOUR_TOKEN)


# 2. Load the tokenizer and text encoder to tokenize and encode the text.
tokenizer = CLIPTokenizer.from_pretrained("openai/clip-vit-large-patch14")
text_encoder = CLIPTextModel.from_pretrained("openai/clip-vit-large-patch14")


# 3. The UNet model for generating the latents in Float16 datatype.
unet = UNet2DConditionModel.from_pretrained("CompVis/stable-diffusion-v1-4",
subfolder="unet",
torch_dtype=torch.float16,
revision="fp16",
use_auth_token=YOUR_TOKEN)
scheduler = LMSDiscreteScheduler(beta_start=0.00085,
beta_end=0.012, beta_schedule="scaled_linear",
num_train_timesteps=1000)
#Set the models to your inference device
vae.to(torch_device)
text_encoder.to(torch_device)
unet.to(torch_device)


text_input = tokenizer(prompt, padding="max_length", max_length=tokenizer.model_max_length, truncation=True, return_tensors="pt")
text_embeddings = text_encoder(text_input.input_ids.to(torch_device))[0]


max_length = text_input.input_ids.shape[-1]
uncond_input = tokenizer(
[""] * batch_size, padding="max_length", max_length=max_length, return_tensors="pt"
)
uncond_embeddings = text_encoder(uncond_input.input_ids.to(torch_device))[0]


text_embeddings = torch.cat([uncond_embeddings, text_embeddings]).half().cuda()


latents = torch.randn(
(batch_size, UNET_INPUTS_CHANNEL, height // 8, width // 8))
latents = latents.to(torch_device)


scheduler.set_timesteps(num_inference_steps)


latents = latents * scheduler.sigmas[0]


scheduler.set_timesteps(num_inference_steps)
# Denoising Loop
with torch.inference_mode(), autocast("cuda"):
for i, t in tqdm(enumerate(scheduler.timesteps)):
# expand the latents if we are doing classifier-free guidance to avoid doing two forward passes.
latent_model_input = torch.cat([latents] * 2)
sigma = scheduler.sigmas[i]
latent_model_input = latent_model_input / ((sigma**2 + 1) ** 0.5)


# predict the noise residual
noise_pred = unet(latent_model_input, t, encoder_hidden_states=text_embeddings)["sample"]


# perform guidance
noise_pred_uncond, noise_pred_text = noise_pred.chunk(2)
noise_pred = noise_pred_uncond + guidance_scale * (noise_pred_text - noise_pred_uncond)


# compute the previous noisy sample x_t -> x_t-1
latents = scheduler.step(noise_pred, i, latents)["prev_sample"]


# scale and decode the image latents with vae
latents = 1 / 0.18215 * latents
image = vae.decode(latents)


#Convert the image with PIL and save it
image = (image / 2 + 0.5).clamp(0, 1)
image = image.detach().cpu().permute(0, 2, 3, 1).numpy()
images = (image * 255).round().astype("uint8")
pil_images = [Image.fromarray(image) for image in images]
pil_images[0].save('image_generated.png')
```


Note that we add small modifications compared to the blog post from Hugging Face. One of the main differences is that we use the Float16 UNet model version instead of the Float32.


We recommend you save your script as a python file named` step_by_step.py`


You can now run the script in order to generate an image corresponding to your prompt.


## Processing time of the diffusion model


The diffusion pipeline requires several iterations to produce an image. Each iteration requires a non-negligible amount of time, depending on your inference device. We’ve benchmarked the stable diffusion pipeline following the[Photoroom blog post](https://www.photoroom.com/inside-photoroom/faster-image-segmentation-trtorch-tensorrt) recommendations. The benchmark was performed on an Nvidia A10 GPU as this is a standard GPU used in production. The number of iterations is set at 100.


Benchmarking results:


```text
Average Latency : 12.43 seconds


Denoising Loop : 11.4 seconds
UNet part: 11.1 seconds
Scheduler + Guidance : 0.003 seconds


Decoding Phase:
VAE decoding: 1.03 seconds
```


The UNet part of the denoising loop takes more than 90% of the full pipeline execution time! Now we will show you how to optimize the UNet part with TensorRT to save seconds in the full pipeline.


## Part II: Save seconds with Nvidia’s TensorRT


In this section, we will show you how to produce the optimized TensorRT model of the UNet part and how to integrate it into your PyTorch code.


## Prerequisite


First of all, we recommend you run your code using the latest Nvidia PyTorch docker container.


Use this command to pull the latest version:


```text
sudo docker pull nvcr.io/nvidia/pytorch:22.08-py3
```


Now, you can run your code from this container:


```text
sudo docker run -it --ipc host -v /home:/home \
--name stable_diffusion_tensorrt --gpus=all \
http://nvcr.io/nvidia/pytorch:22.08-py3 bash
```


In this example, we need to modify some operators inside the stable-diffusion model graph to make it compatible with TensorRT. For that, you need to install the https://github.com/huggingface/diffusers library from the source code. Please, follow the installation instructions from the Hugging Face GitHub.


You also need to install` onnx` and` onnx-simplifer` . These libraries are used to generate the input model for TensorRT.


## Convert the UNet part to ONNX


ONNX is an open format used to represent and exchange machine learning models. In this example, we use ONNX to create a digest UNet for TensorRT.


A simple UNet ONNX converter function would look like this:


```text
import onnx
import torch
from diffusers import UNet2DConditionModel


unet = UNet2DConditionModel.from_pretrained("CompVis/stable-diffusion-v1-4",
torch_dtype=torch.float16,
revision="fp16",
subfolder="unet",
use_auth_token=YOUR_TOKEN)
unet.cuda()


with torch.inference_mode(), torch.autocast("cuda"):
inputs = torch.randn(2,4,64,64, dtype=torch.half, device='cuda'), torch.randn(1, dtype=torch.half, device='cuda'), torch.randn(2, 77, 768, dtype=torch.half, device='cuda')


# Export the model
torch.onnx.export(unet,               # model being run
inputs,                         # model input (or a tuple for multiple inputs)
"unet_v1_4_fp16_pytorch.onnx",   # where to save the model (can be a file or file-like object)
export_params=True,        # store the trained parameter weights inside the model file
opset_version=12,          # the ONNX version to export the model to
do_constant_folding=True,  # whether to execute constant folding for optimization
input_names = ['input_0', 'input_1', 'input_2'],
output_names = ['output_0'])
```


This method will help you generate the ONNX graph of the UNet part of the stable diffusion model.


We specify the` revision` and the` torch_dtype` in half-precision to avoid bad cast conversion from` Float32` to` Float16` data format with TensorRT. The input data is also specified to be in the` Float16` format with the` .half()` PyTorch call.


If all goes well, you should encounter this error:


```text
torch.onnx.errors.UnsupportedOperatorError: Exporting the operator '::broadcast_to' to ONNX opset version 12 is not supported.
Please feel free to request support or submit a pull request on PyTorch GitHub: https://github.com/pytorch/pytorch/issues
```


Don’t worry, this is normal 🙂


*Note that since the release[v0.3.0](https://github.com/huggingface/diffusers/releases/tag/v0.3.0) the model is natively compatible with ONNX and the next step is no longer mandatory.*


The broadcast operator is not supported by either ONNX or TensorRT. A simple solution to bypass this issue is to use the` expand` operator instead of the` broadcast_to` operator. The` expand` operator is fully compatible with ONNX and TensorRT and will produce the same result as before.


The UNet model is defined in this file:` diffusers/models/unet_2d_condition.py` . From this file you can find the line that calls the` broadcast_to` operator:


```text
timesteps = timesteps.broadcast_to(sample.shape[0])
```


Simply replace the` broadcast_to` operator with the` expand` operator:


```text
timesteps = timesteps.expand(sample.shape[0])
```


Now all should go well with the ONNX conversion. The ONNX convert method should now create the ONNX file` unet_v1_4_fp16_pytorch.onnx` . This file is the ONNX representation of the UNet model from the diffusion model. The ONNX format is very useful but can add a lot of extras operators that are not mandatory at inference time. These redundant operators are most of the time replaceable by their constant folding.


The onnx-simplifier can do that in a glance from your shell:


```text
python -m onnxsim unet_v1_4_fp16_pytorch.onnx unet_v1_4_fp16_pytorch_sim.onnx
```


Once the simplification is done, the tool returns an overall comparison between the initial ONNX model and its simplified version:


```text
┏━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━┓
┃                       ┃ Original Model ┃ Simplified Model ┃
┡━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━┩
│ Add                   │ 361            │ 361              │
│ Cast                  │ 508            │ 316              │
│ Concat                │ 110            │ 110              │
│ Constant              │ 778            │ 0                │
│ ConstantOfShape       │ 1              │ 0                │
│ Conv                  │ 98             │ 98               │
│ Cos                   │ 1              │ 1                │
│ Div                   │ 198            │ 198              │
│ Einsum                │ 64             │ 64               │
│ Equal                 │ 1              │ 0                │
│ Erf                   │ 16             │ 16               │
│ Expand                │ 1              │ 1                │
│ Gather                │ 160            │ 160              │
│ Gemm                  │ 24             │ 24               │
│ Identity              │ 219            │ 0                │
│ InstanceNormalization │ 61             │ 61               │
│ MatMul                │ 160            │ 160              │
│ Mul                   │ 339            │ 338              │
│ Pow                   │ 48             │ 48               │
│ ReduceMean            │ 96             │ 96               │
│ Reshape               │ 410            │ 410              │
│ Resize                │ 3              │ 3                │
│ Shape                 │ 221            │ 200              │
│ Sigmoid               │ 68             │ 68               │
│ Sin                   │ 1              │ 1                │
│ Slice                 │ 34             │ 34               │
│ Softmax               │ 32             │ 32               │
│ Sqrt                  │ 48             │ 48               │
│ Sub                   │ 48             │ 48               │
│ Transpose             │ 160            │ 160              │
│ Unsqueeze             │ 333            │ 333              │
│ Where                 │ 1              │ 0                │
│ Model Size            │ 1.6GiB         │ 1.6GiB           │
└───────────────────────┴────────────────┴──────────────────┘
```


The onnx-simplifier tool creates the ONNX file` unet_v1_4_fp16_pytorch_sim.onnx` corresponding to the simplified version of the UNet model.


You are now ready to generate the TensorRT UNet model.


## Boost the UNet inference time with TensorRT


Nvidia’s TensorRT library provides an easy way to optimize an ONNX model for your Nvidia GPU.


The easiest way to use it is through the` trtexec` bash command:


```text
trtexec --onnx=unet_v1_4_fp16_pytorch_sim.onnx\
--saveEngine=unet_v1_4_fp16_pytorch_sim.trt
--fp16
```


*Note that we need to specify the*` —fp16` *flag as the precision set at ONNX generation time was Float16 precision. Since Nvidia’s Volta GPU architecture, TensorCores can provide better inference time with Float16 precision. If you want to know more about TensorCores,[here](https://blog.paperspace.com/understanding-tensor-cores/) is a good blog article that describes the technology.*


Once the process is finished, an Nvidia TensorRT model is saved under the file` unet_v1_4_fp16_pytorch_sim.trt` .


This model is not yet directly compatible with PyTorch, which means that if you want to integrate it inside the` step_by_step.py` script you need to manually handle the GPU memory transfers.


## Make a TensorRT model compatible with PyTorch


Manually handling the GPU memory transfer can lead to an additional amount of work. To avoid that we transform the TensorRT model to a TorchScript compatible model using the` torchtrtc` program:


```text
torchtrtc unet_v1_4_fp16_pytorch_sim.trt \
unet_v1_4_fp16_pytorch_sim.ts \
--embed-engine --device-type=gpu
```


*Note that your TensorRT inputs and outputs model names need to respect the Torch-TensorRT’s standard. In order to have a compatible model with*` torchtrtc` *, be sure that the inputs/outputs names fit with this layout: {input_0,…., input_n; output_0,…..,output_m}.*


Now the file` unet_v1_4_fp16_pytorch_sim.ts` embeds the optimized TensorRT model directly compatible with PyTorch.


## Run the optimized pipeline


Replace the previous UNet model in the` step_by_step.py` script with the optimized one :


```text
# 3. The UNet model for generating the latents.
#unet = UNet2DConditionModel.from_pretrained("CompVis/stable-diffusion-v1-4",
#																							subfolder="unet",
#																							use_auth_token=YOUR_TOKEN)
unet = torch.jit.load('unet_v1_4_fp16_pytorch_sim.ts')
unet.eval()
```


The IN/OUT layout is not exactly the same now as the model cannot manage by himself its inputs datatype. We need to explicitly specify that the inputs of the model are in the GPU in Float16 precision.


Modify the UNet call in the denoising loop as described here:


```text
# predict the noise residual
noise_pred = unet(latent_model_input.half().cuda(),
torch.Tensor(t).half().cuda(),
text_embeddings)
```


Now the optimized UNet with Nvidia’s TensorRT is integrated into the full pipeline!


Finally, we run the benchmarking on the optimized diffusion pipeline, here is the comparison with the initial stable diffusion pipeline:


```text
Average Latency Initial : 12.43 seconds
Average Latency with Nvidia TensorRT : 9.46 seconds


Denoising Loop Initial : 11.4 seconds
Denoising Loop with Nvidia TensorRT : 8.64 seconds
```


Amazing! We just need around 9 seconds now to run the stable diffusion pipeline instead of more than 12 seconds with the initial version!


Thanks to TensorRT we have reduced the UNet execution time from 111 ms to 81 ms. In the end, this acceleration saves us 3 seconds on the full diffusion pipeline.


The graph below shows you the difference between the initial stable diffusion and the same pipeline boosted with TensorRT:


## Conclusion


Diffusion models are very promising for photo-realistic image generation. However, the pipeline is iterative and needs to perform intensive computation at each step. This behavior can lead to long waiting periods for Photoroom users. In this blog post, we showed you how we use Nvidia’s TensorRT library to optimize the stable diffusion pipeline released by Hugging Face. We also showed how to make the model compatible with TensorRT and PyTorch. These modifications allowed us to shave 3 seconds from the pipeline, which can really improve the Photoroomer experience. The decoder can also be strongly optimized with TensorRT (~0.9 seconds saved), however, the amount of work exceeds a simple blog post. In an upcoming article, we will explore diffusion model compression with INT8 quantization or pruning methods.


### This could be also interesting for you:


Read all about the **[Remove background API](https://www.photoroom.com/api/remove-background)** which is used by Meta, Apple and Huggingface.
