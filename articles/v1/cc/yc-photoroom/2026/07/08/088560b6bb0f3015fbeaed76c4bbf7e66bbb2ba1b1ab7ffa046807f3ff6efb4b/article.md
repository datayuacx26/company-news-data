---
schema_version: "1.0.0"
document_id: "088560b6bb0f3015fbeaed76c4bbf7e66bbb2ba1b1ab7ffa046807f3ff6efb4b"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/faster-image-segmentation-trtorch-tensorrt"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:49f5628f34dcc12fdb76daaed50e9aa6dd7c08233707d0f4b7b5c8ee1b6d382a"
---

# 4 times faster image segmentation with TRTorch

At Photoroom, we build photo editing apps. One of our core features is to remove the background of any image. We do so by using deep learning models that will segment an image into different layers.


In our quest to improve the speed and the accuracy of our models, we quickly took the habit of compiling them using[Nvidia's TensorRT](https://github.com/NVIDIA/TensorRT) . As detailed in the chart below, leveraging TensorRT's optimized GPU kernels through[TRTorch](https://github.com/NVIDIA/TRTorch) can yield a performance boost of up to 10x:


*For very low resolution (160px), the speedup can be up to 10x and up to 1.6x faster for larger resolutions*


This blog post details the necessary steps to optimize your PyTorch model for the fastest inference speed:


1.


Part I: Benchmarking your original model's speed


2.


Part II: Boosting inference speed with TRTorch


For a more complete code example check the following[notebook](https://github.com/PhotoRoom/blog-resources/blob/main/FasterInferenceWithTRTorch/BlogTRTorch.ipynb) .


## Part I: Benchmarking your model's speed


To evaluate your original model's speed, we will need to ensure we're using the right settings and environment.


### 1- **CuDNN benchmark mode**


If your model uses a fixed input size, you can speed up your inference by enabling the cuDNN benchmark mode as follows:


```text
import torch
torch.backends.cudnn.benchmark = True
```


### 2- **Beware of asynchronous executions**


By default, the GPU tasks run asynchronously, which means that measuring the inference as done below will not work.


```text
start = time.time()
prediction = my_model(dummy_input)
end = time.time()
```


By the time we arrive at the third line, we have no idea whether the model computations are over or not.[More details here](https://pytorch.org/docs/stable/notes/cuda.html) . The simplest way to fix this is to force the synchronisation:


```text
start = time.time()
prediction = my_model(dummy_input)
torch.cuda.synchronize()
end = time.time()
```


### 3- **Use NVIDIA NGC deep learning framework containers.**


NVIDIA provides out-of-the-box[Docker containers](https://ngc.nvidia.com/catalog/containers/nvidia:pytorch) equipped with PyTorch and TensorRT that yield better performance, especially in FP16 mode. Our experience shows up to 1.5x gains when compared to running outside of the Docker image.


### 4- **Aggregating results over multiple runs**


As detailed in this[StackOverflow answer](https://stackoverflow.com/questions/24980500/which-metric-should-be-used-for-benchmarks/65430875#65430875) , to get the best estimate of your model's runtime you should use the minimum time over several runs instead of the average. Some runs may be slowed down by factors unrelated to the model (e.g. noise).


### 5- **Other tricks**


-


Do not forget to put your model in eval mode


-


PyTorch recently introduced an[inference_mode](https://pytorch.org/docs/stable/generated/torch.inference_mode.html) . Like[no_grad](https://pytorch.org/docs/stable/generated/torch.no_grad.html) , it may also be useful to optimize for speed and memory.


-


If your model has loads of` Conv` followed directly by` BatchNorm` layers, you can fuse them beforehand. Depending on the model, this will result in a 10-20% speedup. TRTorch does it automatically.


A simple benchmarking function would look like:


```text
def benchmark(model, resolution, dtype, device):
dummy_input = torch.ones(
(1,3,resolution, resolution),dtype=dtype,device=device
)


# Warm up runs to prepare Cudnn Benchmark
for warm_up_iter in range(10):
prediction = model(dummy_input)


# Benchmark
with torch.no_grad():
durations = list()
for i in range(100):
start = time.time()
prediction = model(dummy_input)
torch.cuda.synchronize()
end = time.time()
durations.append(end-start)
return min(durations)
```


## Part II: Boosting inference speed with TRTorch


[TRTorch](https://github.com/NVIDIA/TRTorch) is a library built on top of TensorRT, which provides an easy way to compile your PyTorch model into a TensorRT graph. The compilation is ahead of time, meaning that the optimisations happen before the first inference run.


The compiled model can then be used through` torch.jit` as you would for any scripted/traced model in PyTorch. Also, your compiled model can be used directly from C++ as it is independent of Python.


To compile your PyTorch model, you first need to script/trace it. The corresponding TorchScript module is then fed into TRTorch's compiler along with compilation settings such as:


-


Input shapes for each input


-


Operation precision (FP32, FP16, INT8)


### 1- Setting up TRTorch locally


The easiest way to set up TRTorch locally is to use one of the docker images provided. To do so you can execute the following commands:


```text
git clone https://github.com/NVIDIA/TRTorch.git


cd TRTorch


sudo docker build -f docker/Dockerfile.21.03 -t trtorch:dev


sudo docker run \
--gpus all \
-it \
--shm-size=40gb \
--env="DISPLAY" \
-v /home:/home \
--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
--name=trtorch \
--ipc=host \
--net=host trtorch:dev
```


The image comes with all the necessary packages installed (torch, torchvision, trtorch, jupyter, etc...)


### 2- Scripting with TorchScript


TorchScript saves your model's[operation graph](https://pytorch.org/docs/stable/jit.html) in a format that can be executed outside of Python.


Note that if your forward loop uses if/else statements, tracing won't work as it only records the particular flow of operations for the input you provide. For tracing to work, **branching in your code must not depend on the input.**


```text
net = torchvision.models.resnet101()
net.eval()
dummy_input = torch.randn((1,3,320,320)))
traced_model = torch.jit.trace(net, dummy_input)
```


### 3- Compiling a TorchScript module with TRTorch


```text
trtorch_settings = {
"inputs":[
trtorch.Input(
min_shape=[1,3,320,320],
opt_shape=[1,3,320,320],
max_shape=[1,3,320,320],
dtype=torch.float,
)
],
"enabled_precisions": {torch.float},
"debug":True,
}


trtorch_model = trtorch.compile(traced_model, settings)
torch.jit.save(trtorch_model, "./my_trtorch_model.ts")


```


Once your model is compiled, you can use it through` torch.jit.load` :


```text
my_compiled_model = torch.jit.load("./my_trtorch_model.ts")
dummy_input = torch.randn((1,3,320,320)))
dummy_prediction = my_compiled_model(dummy_model)
```


TRTorch also supports FP16, for which you only need to specify:


```text
trtorch_settings = {
"inputs":[
trtorch.Input(
min_shape=[1,3,320,320],
opt_shape=[1,3,320,320],
max_shape=[1,3,320,320],
dtype=torch.half,
)
],
"enabled_precisions": {torch.half},
"debug":True,
}
```


Using FP16 allows for **lower inference time** and a **lower memory footprint.** For some models, this can enable a higher input resolution with the same memory impact.


We ran the benchmarking code used for the PyTorch models defined in the first part for both FP32 and FP16 TRTorch models. As a result, we obtain the following results:


In FP16, TRTorch provides a significant speedup over PyTorch. However, surprisingly, we find that using **TRTorch in FP32 is slower** than PyTorch. We strongly recommend FP16 over FP32.


## Conclusion


TRTorch is a very efficient way to reduce the inference time of a PyTorch model. Because of the exceptional performance it provides, it is used extensively at Photoroom and by all the industry. In upcoming articles, we will cover how to convert custom layers and explore other quantization options such as INT8.
