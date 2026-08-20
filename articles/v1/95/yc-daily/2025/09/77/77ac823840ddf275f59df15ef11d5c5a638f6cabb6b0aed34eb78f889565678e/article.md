---
schema_version: "1.0.0"
document_id: "77ac823840ddf275f59df15ef11d5c5a638f6cabb6b0aed34eb78f889565678e"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/announcing-smart-turn-v3-with-cpu-inference-in-just-12ms/"
published_at: "2025-09-11T16:15:32+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:791672fbc8686aef00ea413532a20eb9f06fcca0bc0219340441c5b4144d8b26"
---

# Announcing Smart Turn v3, with CPU inference in just 12ms

Today we’re excited to share our updated Smart Turn v3 model, which leapfrogs both the previous version and competing models in size and performance. For the first time, Smart Turn is small and fast enough to run on CPU.


This new version is still fully open source — this includes the weights, training data, and the training script.


- [Model weights](https://huggingface.co/pipecat-ai/smart-turn-v3)
- [GitHub repo with code for training and inference](https://github.com/pipecat-ai/smart-turn)
- [Previous v2 announcement](https://www.daily.co/blog/smart-turn-v2-faster-inference-and-13-new-languages-for-voice-ai/) with additional background info about the model
- [Datasets](https://huggingface.co/pipecat-ai/datasets) used for training and testing


- [pipecat-ai/smart-turn-data-v3-train](https://huggingface.co/datasets/pipecat-ai/smart-turn-data-v3-train)
- [pipecat-ai/smart-turn-data-v3-test](https://huggingface.co/datasets/pipecat-ai/smart-turn-data-v3-test)


## Changes


- **Nearly 50x smaller** than v2, at only 8 MB 🤯
- **Lightning-fast CPU inference:** 12ms on modern CPUs, 60ms on a low cost AWS instance. No GPU required — run directly inside your Pipecat Cloud instance!
- **Expanded language support:** Now covers 23 languages:


- 🇸🇦 Arabic, 🇧🇩 Bengali, 🇨🇳 Chinese, 🇩🇰 Danish, 🇳🇱 Dutch, 🇩🇪 German, 🇬🇧🇺🇸 English, 🇫🇮 Finnish, 🇫🇷 French, 🇮🇳 Hindi, 🇮🇩 Indonesian, 🇮🇹 Italian, 🇯🇵 Japanese, 🇰🇷 Korean, 🇮🇳 Marathi, 🇳🇴 Norwegian, 🇵🇱 Polish, 🇵🇹 Portuguese, 🇷🇺 Russian, 🇪🇸 Spanish, 🇹🇷 Turkish, 🇺🇦 Ukrainian, and 🇻🇳 Vietnamese.


- **Better accuracy** compared to v2, despite the size reduction


## How Smart Turn v3 compares


We’re always pleased to see innovation from other developers in this space, and since we released Smart Turn v2, two other promising native audio turn detection models have been announced. Here is a high-level comparison:


**Smart Turn v3** **Krisp** **Ultravox**


**Size** 8 MB 65 MB 1.37 GB


**Language support** 23 languages Trained/tested on English only 26 languages


**Availability** Open weights, data, and training script Proprietary Open weights


**Architecture Focus** Single-inference decision latency Multiple inferences to maximize decision confidence Using conversation context alongside audio from current turn


We’re currently working on an open and transparent benchmark to compare the accuracy of models, and are working together with both the Krisp and Ultravox teams on this project. We’ve included our own accuracy benchmarks below, and you can reproduce these using` benchmark.py` and our open test dataset.


## Performance


Smart Turn v3 has dramatically improved performance, with a 100x speedup on a` c8g.medium` AWS instance compared to v2, and a 20-60x improvement on other CPU types.


The figures below include both audio preprocessing and inference. We found that CPU preprocessing contributes approximately 3ms to the execution time, and this starts to outweigh the actual inference time on fast GPUs.


**Smart Turn v2** **Smart Turn v3**


**NVIDIA L40S (Modal)** 12.5 ms 3.3 ms


**NVIDIA L4 (Modal)** 30.8 ms 3.6 ms


**NVIDIA A100 (Modal)** 19.1 ms 4.3 ms


**NVIDIA T4** 74.5 ms 6.6 ms


**CPU (AWS c7a.2xlarge)** 450.6 ms 12.6 ms


**CPU (AWS c8g.2xlarge)** 903.1 ms 15.2 ms


**CPU (Modal, 6 cores)** 410.1 ms 17.7 ms


**CPU (AWS t3.2xlarge)** 900.4 ms 33.8 ms


**CPU (AWS c8g.medium)** 6272.4 ms 59.8 ms


**CPU (AWS t3.medium)** - 94.8 ms


For CPU inference, we got the best results with the following session options, and it may be possible to increase performance further with additional tuning.


```text
def build_cpu_session(onnx_path):
so = ort.SessionOptions()
so.execution_mode = ort.ExecutionMode.ORT_SEQUENTIAL
so.inter_op_num_threads = 1
so.graph_optimization_level = ort.GraphOptimizationLevel.ORT_ENABLE_ALL
return ort.InferenceSession(onnx_path, sess_options=so, providers=["CPUExecutionProvider"])


```


## Architecture


Smart Turn v2 was based on the` wav2vec2` speech model, which is around 400MB in size.


For v3, we experimented with several architectures before settling on **Whisper Tiny** , which has only 39M parameters. In our testing, despite the small size of the model, it was able to achieve better accuracy than v2 on our testing set.


Only the encoder layers of Whisper were required, onto which we added the existing linear classification layers from Smart Turn v2, resulting in a model with **8M parameters** in total.


We have also applied` int8` quantization to the model, in the form of static QAT (quantization aware training). We found that this preserves the accuracy of v2, while leading to significantly increased performance, and a 4x smaller filesize of **8 MB** .


Currently we’re exporting the model in ONNX format. Since we’re focusing on quantization and optimized CPU inference in this release, ONNX seemed like a great fit.


## Accuracy results


Smart Turn v3 maintains or improves accuracy across all supported languages compared to v2. Please see the table below for the results from our test dataset.


You can reproduce these results yourself using our open testing dataset, and` benchmark.py` from the Smart Turn GitHub repo.


If you’d like to help clean up the dataset to improve accuracy further by listening to audio samples, please visit the following link:[https://smart-turn-dataset.pipecat.ai/](https://smart-turn-dataset.pipecat.ai/)


Language Test samples Accuracy (%) False Positives (%) False Negatives (%)


🇹🇷 Turkish 966 97.10 1.66 1.24


🇰🇷 Korean 890 96.85 1.12 2.02


🇯🇵 Japanese 834 96.76 2.04 1.20


🇳🇱 Dutch 1,401 96.29 1.86 1.86


🇩🇪 German 1,322 96.14 2.50 1.36


🇫🇷 French 1,253 96.01 1.60 2.39


🇵🇹 Portuguese 1,398 95.42 2.79 1.79


🇮🇹 Italian 782 95.01 3.07 1.92


🇫🇮 Finnish 1,010 94.65 3.27 2.08


🇵🇱 Polish 976 94.47 2.87 2.66


🇮🇩 Indonesian 971 94.44 4.22 1.34


🇬🇧 🇺🇸 English 2,846 94.31 2.64 3.06


🇺🇦 Ukrainian 929 94.29 2.80 2.91


🇳🇴 Norwegian 1,014 93.69 3.65 2.66


🇷🇺 Russian 1,470 93.67 3.33 2.99


🇮🇳 Hindi 1,295 93.44 4.40 2.16


🇩🇰 Danish 779 93.07 4.88 2.05


🇪🇸 Spanish 1,295 91.97 4.48 3.55


🇸🇦 Arabic 947 88.60 6.97 4.44


🇨🇳 Chinese 945 88.57 4.76 6.67


🇮🇳 Marathi 774 87.60 8.27 4.13


🇧🇩 Bengali 1,000 84.10 10.80 5.10


🇻🇳 Vietnamese 1,004 81.27 14.84 3.88


## How to use the model


As with v2, there are several ways to use the model.


### With Pipecat


Support for Smart Turn v3 is already integrated into Pipecat using` LocalSmartTurnAnalyzerV3` . You’ll need to download the ONNX model file from our HuggingFace repo.


To see this in action in an application, please see our[local-smart-turn](https://github.com/pipecat-ai/pipecat-examples/tree/main/local-smart-turn) sample code.


⚠️


Note: the` LocalSmartTurnAnalyzerV3` class will be added in Pipecat v0.0.85 (out soon). You can use it right away by using the` main` branch of Pipecat.


### Standalone


You can run the model directly using the ONNX runtime. We’ve included some sample code for this in` inference.py` in the GitHub repo, and this is used in` predict.py` and` record_and_predict.py` .


Note that a VAD model like Silero should be used in conjunction with Smart Turn. The model works with audio chunks up to 8 seconds, and you should include as much context from the current turn as possible. For more details, see the README.


## Conclusion


Support for CPU inference is a huge step for Smart Turn, and we encourage you to use this new release directly in your Pipecat Cloud bot instances.


If you speak any of the languages in the list above (particularly those with lower accuracy), we’d appreciate your help listening to some data samples to improve the quality:[https://smart-turn-dataset.pipecat.ai/](https://smart-turn-dataset.pipecat.ai/)


And if you have any thoughts or questions about the new release, you can get in touch with us at the[Pipecat Discord server](https://discord.gg/pipecat) or on our[GitHub repo](https://github.com/pipecat-ai/smart-turn) .


#### Never miss a story


Get the latest direct to your inbox.
