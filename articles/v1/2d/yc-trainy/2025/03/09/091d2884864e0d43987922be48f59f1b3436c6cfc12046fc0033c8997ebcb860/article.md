---
schema_version: "1.0.0"
document_id: "091d2884864e0d43987922be48f59f1b3436c6cfc12046fc0033c8997ebcb860"
company_key: "yc-trainy"
company: "Trainy"
source_id: "yc-trainy-rss-dacd52d4547a"
canonical_url: "https://www.trainy.ai/blog/preflight-checks-gpu-fabric"
published_at: "2025-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.988023+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:44885c180183ff46bc9788e1a0a6601f059a95817d4c5ed6539378bc9ecb8224"
---

# Preflight Checks for GPU Fabric before Multi-node Training

Given the scale of modern LLMs, having a performant GPU network fabric is one of the most important factor for your training speed. The ability to efficiently communicate across hundreds of nodes allows you to maximize GPU utilization and explore more complex training parallelisms such as in[Deepspeed](https://www.deepspeed.ai/) or[Fully Sharded Data Parallel](https://pytorch.org/docs/stable/fsdp.html) . These critically rely on high performance networking solutions requiring specialized hardware and software like InfiniBand or RDMA over Converged Ethernet (RoCE) with current vendor options often going up to now 3200 Gbps.


## Estimating Step Time for GPT language models


To illustrate, how important GPU fabric is to training, let's consider training an 7B GPT-style language model trained at 2048 context length using a pair of H100x8 nodes and performing an all-reduce operation which is used to average gradients over GPUs. Typical model flops utilization (MFU) for falls[around ~40%](https://github.com/mosaicml/llm-foundry/tree/main/scripts/train/benchmarking) . For an H100, that roughly translates to` 396 TFLOP/second` . To perform, forward and backward pass of the model with reactivation using BF16, the[number of TFLOP can be estimated as](https://github.com/bigscience-workshop/bigscience/tree/master/math#calculate-tflops) :


```text
tflop = model_size_in_B * 4 * 2 * seqlen * global_batch_size / (total_gpus * 1e3)


```


We already have` seqlen = 2048` and` model_size_in_B = 7` . A[typical batch size](https://github.com/mosaicml/llm-foundry/tree/main/scripts/train/benchmarking) might be` global_batch_size = 16` . This gives us 114 TFLOP. So we expect, computing each step to take roughly` 114 TFLOP / 396 TFLOP/second = 0.29 seconds` .


Now we need to estimate the time it takes to average the gradients over all the GPUs. For a 7B model, we'll pass gradients in fp32 meaning we need to pass S := 7 Billion x 4 Bytes = 28 GB in gradients every iteration. With n GPUs, for all-reduce, each GPU communicates its gradients to all the others resulting in 2(n - 1) data transfer operations over n links of bandwidth B to perform them on. So the time to perform one round of all-reduce can be estimated as


```text
t = (S/B) * (2*(n-1)/n)


```


Consider networking with B = 1000Gbps = 125GB/second and n = 16, we have t = 0.42 seconds to perform one reduction. So we are network bound since the time to compute one step is longer than the time spent communicating between GPUs 0.29 seconds < 0.42 seconds. If we had chosen B = 3200Gbps = 400GB/second, t = 0.13 seconds and we enter the compute limited regime. Advanced algorithms will try to overlap the communication of gradients with the computation of later layers. Even if communication and computation perfectly overlapped every step, in the network bound case, there is at least 0.13 seconds of idling, which is almost 0.13 seconds / 0.42 seconds = 30% of the total step time! Very wasteful.


## NCCL Test


NCCL (NVIDIA Collective Communications Library) is a library developed by NVIDIA to provide in collective communication primitives for GPU-GPU communication, which is used while training large models. If you've profiled your model training before, you've probably noticed the` ncclKernel_AllReduce` call. It's common to run the all reduce benchmark which is included as part of NVIDIA's[nccl-test](https://github.com/NVIDIA/nccl-tests/) to QA a GPU fabric. We ran this on a pair of 1600 Gbps machines, which showed the following:


```text
out-of-place                       in-place
#       size         count      type   redop    root     time   algbw   busbw #wrong     time   algbw   busbw #wrong
#        (B)    (elements)                               (us)  (GB/s)  (GB/s)            (us)  (GB/s)  (GB/s)
8             2     float     sum      -1    29.73    0.00    0.00      0    29.65    0.00    0.00      0
16             4     float     sum      -1    28.43    0.00    0.00      0    28.02    0.00    0.00      0
32             8     float     sum      -1    28.60    0.00    0.00      0    28.48    0.00    0.00      0
64            16     float     sum      -1    29.02    0.00    0.00      0    29.01    0.00    0.00      0
128            32     float     sum      -1    29.14    0.00    0.01      0    29.13    0.00    0.01      0
256            64     float     sum      -1    29.47    0.01    0.02      0    29.56    0.01    0.02      0
512           128     float     sum      -1    31.29    0.02    0.03      0    30.83    0.02    0.03      0
1024           256     float     sum      -1    32.81    0.03    0.06      0    32.54    0.03    0.06      0
2048           512     float     sum      -1    35.57    0.06    0.11      0    35.71    0.06    0.11      0
4096          1024     float     sum      -1    37.27    0.11    0.21      0    36.39    0.11    0.21      0
8192          2048     float     sum      -1    38.52    0.21    0.40      0    38.15    0.21    0.40      0
16384          4096     float     sum      -1    42.18    0.39    0.73      0    41.70    0.39    0.74      0
32768          8192     float     sum      -1    43.50    0.75    1.41      0    42.53    0.77    1.44      0
65536         16384     float     sum      -1    48.09    1.36    2.56      0    45.70    1.43    2.69      0
131072         32768     float     sum      -1    56.49    2.32    4.35      0    55.72    2.35    4.41      0
262144         65536     float     sum      -1    103.7    2.53    4.74      0    93.15    2.81    5.28      0
524288        131072     float     sum      -1    97.47    5.38   10.09      0    97.48    5.38   10.08      0
1048576        262144     float     sum      -1    101.6   10.32   19.35      0    101.4   10.34   19.39      0
2097152        524288     float     sum      -1    115.6   18.15   34.03      0    110.6   18.97   35.57      0
4194304       1048576     float     sum      -1    148.7   28.21   52.88      0    149.5   28.06   52.62      0
8388608       2097152     float     sum      -1    223.3   37.57   70.44      0    221.7   37.84   70.96      0
16777216       4194304     float     sum      -1    303.1   55.35  103.77      0    303.8   55.23  103.55      0
33554432       8388608     float     sum      -1    511.7   65.58  122.96      0    510.9   65.68  123.15      0
67108864      16777216     float     sum      -1    953.1   70.41  132.03      0    945.6   70.97  133.07      0
134217728      33554432     float     sum      -1   1581.3   84.88  159.15      0   1578.5   85.03  159.43      0
268435456      67108864     float     sum      -1   2945.4   91.14  170.88      0   2950.6   90.98  170.58      0
536870912     134217728     float     sum      -1   5852.4   91.73  172.00      0   5821.6   92.22  172.91      0
1073741824     268435456     float     sum      -1    11567   92.83  174.05      0    11580   92.72  173.86      0
2147483648     536870912     float     sum      -1    22518   95.37  178.81      0    22437   95.71  179.46      0
# Out of bounds values : 0 OK
# Avg bus bandwidth    : 48.8808


```


For training large models, we really only care the value of the busbw in the last row (which in this case is 178.81GB/s) since during training, gradients are bucketed and sent in large payloads such as the 2147483648 = 2GB message size in the last row. It's possible to achieve better efficiency through further tuning, with 95% of the theoretical bandwidth commonly achieved. Usually network tuning will just be handled by your cloud provider, but due diligence and silent faults with GPU fabrics say that you should health check your fabric periodically.


## Torch DDP Throughput


Next, a quick DDP test helps ensure we can efficiently train models end-to-end. To iterate through datasets faster, Distributed Data Parallel (DDP) is commonly used to shard batches across data parallel workers by copying the model onto each GPU and communicating gradients between workers. To make this easier to test across clouds, we added this[torch DDP benchmark to Skypilot](https://github.com/skypilot-org/skypilot/tree/master/examples/torch_ddp_benchmark) . After running this, we can get the following summary.


```text
Benchmark: resnet101 with batch size 32


sec/iter    ex/sec      sec/iter    ex/sec      sec/iter    ex/sec      sec/iter    ex/sec
1 GPUs --   no ddp:  p50:  0.064s     501/s  p75:  0.064s     499/s  p90:  0.064s     497/s  p95:  0.065s     491/s
1 GPUs --    1M/1G:  p50:  0.064s     502/s  p75:  0.064s     502/s  p90:  0.064s     502/s  p95:  0.064s     501/s
2 GPUs --    1M/2G:  p50:  0.066s     486/s  p75:  0.066s     486/s  p90:  0.066s     484/s  p95:  0.066s     482/s
4 GPUs --    1M/4G:  p50:  0.068s     468/s  p75:  0.069s     464/s  p90:  0.070s     457/s  p95:  0.077s     417/s
8 GPUs --    1M/8G:  p50:  0.069s     465/s  p75:  0.069s     464/s  p90:  0.069s     463/s  p95:  0.069s     463/s
16 GPUs --    2M/8G:  p50:  0.089s     359/s  p75:  0.090s     356/s  p90:  0.091s     350/s  p95:  0.094s     340/s


```


Here we see that our throughput is not scaling ideally linearly, which is indicative of poor communications bandwidth.


## Conclusion


The dream of any ML engineer is linear scaling. 2x the GPUs should mean half the training time. However, the only way to practically achieve this is with networking capabilities that can communicate gradients as faster than your GPU can compute them.


Is your team or organization struggling with distributed training? I'd love to chat and see if Trainy can help! Reach out atroanak@trainy.ai .
