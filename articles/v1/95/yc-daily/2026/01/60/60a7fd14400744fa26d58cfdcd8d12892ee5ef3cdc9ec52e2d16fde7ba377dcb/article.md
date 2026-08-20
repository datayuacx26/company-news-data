---
schema_version: "1.0.0"
document_id: "60a7fd14400744fa26d58cfdcd8d12892ee5ef3cdc9ec52e2d16fde7ba377dcb"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/smart-turn-v3-2-handling-noisy-environments-and-short-responses/"
published_at: "2026-01-07T18:05:44+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T22:24:01.443100+00:00"
content_hash: "sha256:d990ffc066306e5d071f2cf8c7dcdd8020c79196d6d12e54b3bff53102329fa2"
---

# Smart Turn v3.2: Handling noisy environments and short responses

We’re happy to kick off the New Year with a new[Smart Turn](https://github.com/pipecat-ai/smart-turn) release, with two key improvements to the responsiveness of your AI voice agents.


Smart Turn is an open-source turn detection model, which listens to raw audio data and determines when a user has finished speaking. Using Smart Turn, an AI voice agent can tell precisely when to respond to the user, without interrupting them, or waiting unnecessarily.


As usual, all parts of the model are open: the[weights](https://huggingface.co/pipecat-ai/smart-turn-v3/tree/main) , the[datasets](https://huggingface.co/pipecat-ai/datasets) , and the[training code](https://github.com/pipecat-ai/smart-turn) .


## What's new in v3.2


### Short utterances


We’ve significantly improved the model’s handling of short utterances, for example single words like “yes” or “okay”. These samples are now **miscategorized 40% less often** according to our public benchmarks.


We’ve made two changes which make this possible: firstly, a new dataset of short utterances which we plan to expand over time, and secondly, a fix for a[padding issue](https://github.com/pipecat-ai/smart-turn/issues/35) during training reported by the community, which was reducing accuracy.


### Background noise


Smart Turn v3.2 is more robust to background ambience, thanks to the addition of realistic cafe/office noise to our training and testing datasets. The result is that the model will perform better in real-world scenarios where the user’s audio isn’t studio-quality.


## Usage


The new version is a drop-in replacement for v3.1, and as before, we’re shipping the model in 8MB (CPU) and 32MB (GPU) variants. The weights are available now on HuggingFace:


[https://huggingface.co/pipecat-ai/smart-turn-v3/tree/main](https://huggingface.co/pipecat-ai/smart-turn-v3/tree/main)


As with v3.1, we’ll bundle the weights with the next Pipecat release for use with` LocalSmartTurnAnalyzerV3` . You can also use v3.2 with Pipecat right now by setting the` smart_turn_model_path` parameter in the` LocalSmartTurnAnalyzerV3` constructor.


## More information and benchmarks


For more details on how the model was trained, including our full training code, please see our GitHub repo:


[https://github.com/pipecat-ai/smart-turn](https://github.com/pipecat-ai/smart-turn)


We’ve released two new datasets, which were used to train and test this release respectively:


- [smart-turn-data-v3.2-train](https://huggingface.co/datasets/pipecat-ai/smart-turn-data-v3.2-train)
- [smart-turn-data-v3.2-test](https://huggingface.co/datasets/pipecat-ai/smart-turn-data-v3.2-test)


For accuracy benchmarks with the new test dataset, please see the following link:


[https://huggingface.co/pipecat-ai/smart-turn-v3/tree/main/benchmarks](https://huggingface.co/pipecat-ai/smart-turn-v3/tree/main/benchmarks)


## Stay in touch


We hope you enjoy the new model! If you have questions about Smart Turn or run into any issues, feel free to[join our Discord server](https://discord.com/invite/pipecat) , or open a ticket on[GitHub](https://github.com/pipecat-ai/smart-turn/issues) .


#### Never miss a story


Get the latest direct to your inbox.
