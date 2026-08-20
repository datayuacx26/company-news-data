---
schema_version: "1.0.0"
document_id: "787c38d97839f96701adafd80df7c9a8a6b99cbb9a4eef460b09c7115051d8b9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/zeroprompt-streaming-acoustic-encoders-are-zero-shot-masked-lms"
published_at: "2023-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:668a30b83b125804494cea4fe3c183b27f83868513c48dff35d5dcb0d3d7a10a"
---

# ZeroPrompt: Streaming Acoustic Encoders are Zero-Shot Masked LMs

Do not index


Original Paper


[https://arxiv.org/abs/2305.10649](https://arxiv.org/abs/2305.10649)


Blog URL


[blog.athina.ai /zeropro...sked-lms](https://blog.athina.ai/zeroprompt-streaming-acoustic-encoders-are-zero-shot-masked-lms)


**Original Paper:**[https://arxiv.org/abs/2305.10649](https://arxiv.org/abs/2305.10649)


**By:**[Xingchen Song](https://arxiv.org/search/cs?searchtype=author&query=Song%2C%20X) ,[Di Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20D) ,[Binbin Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20B) ,[Zhendong Peng](https://arxiv.org/search/cs?searchtype=author&query=Peng%2C%20Z) ,[Bo Dang](https://arxiv.org/search/cs?searchtype=author&query=Dang%2C%20B) ,[Fuping Pan](https://arxiv.org/search/cs?searchtype=author&query=Pan%2C%20F) ,[Zhiyong Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Z)


**Abstract:**


> In this paper, we present ZeroPrompt (Figure 1-(a)) and the corresponding Prompt-and-Refine strategy (Figure 3), two simple but effective \\textbf{training-free} methods to decrease the Token Display Time (TDT) of streaming ASR models \\textbf{without any accuracy loss}. The core idea of ZeroPrompt is to append zeroed content to each chunk during inference, which acts like a prompt to encourage the model to predict future tokens even before they were spoken. We argue that streaming acoustic encoders naturally have the modeling ability of Masked Language Models and our experiments demonstrate that ZeroPrompt is engineering cheap and can be applied to streaming acoustic encoders on any dataset without any accuracy loss. Specifically, compared with our baseline models, we achieve 350 - 700ms reduction on First Token Display Time (TDT-F) and 100 - 400ms reduction on Last Token Display Time (TDT-L), with theoretically and experimentally equal WER on both Aishell-1 and Librispeech datasets.


---


###


Summary Notes


####


Streamlining Real-Time ASR: Exploring ZeroPrompt's Innovations


In the world of Artificial Intelligence (AI) and Machine Learning (ML), professionals are always on the lookout for ways to improve Automatic Speech Recognition (ASR) systems, especially for real-time use.


A new development, ZeroPrompt, promises to address a common issue in streaming ASR models: how to reduce latency without losing accuracy.


This post explores what ZeroPrompt is, how it works, and its benefits for streaming ASR applications.


####


Understanding the Need for Faster ASR


For end-to-end ASR models that operate in real-time, minimizing delay is crucial for a good user experience.


Traditional methods, while effective, often struggle to reduce the delay in converting speech to text, leading to noticeable lags.


ZeroPrompt offers an innovative solution to this problem, aiming to cut down on Token Display Time (TDT) while maintaining accurate transcriptions.


####


How ZeroPrompt Works


ZeroPrompt enhances ASR systems by using a few key techniques:


- **Appending Zeroed Frames:** It adds empty content to each data chunk, encouraging the ASR system to predict upcoming tokens sooner, which reduces delay.


- **Chunk-Level Autoregressive Attention Mask:** This ensures that predictions for the current chunk are accurate and not affected by the added empty frames.


- **Prompt-and-Refine Strategy:** As new actual data chunks arrive, the system refines its predictions to improve transcription accuracy over time.


Unlike other methods, ZeroPrompt does not need future frames to start working, making it a practical approach to lowering latency in real-time applications.


####


Testing ZeroPrompt: Results


ZeroPrompt's effectiveness was tested using two well-known datasets: Aishell-1 (Chinese Mandarin) and LibriSpeech (English).


It was applied to pre-trained chunk-based ASR models, and performance was measured using new metrics: First Token Display Time (TDT-F), Last Token Display Time (TDT-L), and Prompts Error Rate (PER).


Findings showed:


- **Faster TDT-F and TDT-L:** ZeroPrompt managed to reduce TDT-F by 350-700ms and TDT-L by 100-400ms, varying by chunk size and dataset.


- **Stable WER:** The Word Error Rate (WER) stayed the same as baseline models, meaning accuracy wasn't sacrificed.


- **Minor RTF Increase:** Processing the added content slightly raised the Real Time Factor (RTF), but the trade-off was worthwhile due to the notable latency improvements.


####


The Impact and Future of ZeroPrompt


ZeroPrompt offers an innovative and low-cost way to enhance streaming ASR systems by significantly reducing latency without compromising the quality of transcriptions.


It represents a crucial step forward in achieving the ideal balance between speed and accuracy in real-time speech recognition.


Future enhancements could lead to even better performance, with potential improvements in the autoregressive mask and refining strategy offering exciting opportunities for further research and development.


####


Wrapping Up: ZeroPrompt's Role in Advancing ASR


ZeroPrompt stands out as a significant breakthrough in streaming ASR technology, providing a viable solution to the challenge of balancing latency with accuracy.


Its implications for real-time applications like live captioning are significant, setting the stage for more responsive and precise speech recognition technologies.


As the field continues to evolve, approaches like ZeroPrompt will be key in shaping the future of ASR systems.


ZeroPrompt not only showcases the integration of masked language model (MLM) capabilities into ASR models but also establishes a new benchmark for reducing latency, heralding a new era of instant and accurate speech-to-text conversion.


####


Further Reading


For those interested in the technical details and results of ZeroPrompt, references include studies on connectionist temporal classification (CTC), attention-based encoder-decoder models, LookAhead methods, and recent progress in streaming ASR technology.


These resources provide a thorough understanding of the context and significance of ZeroPrompt's contributions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
