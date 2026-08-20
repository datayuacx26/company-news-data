---
schema_version: "1.0.0"
document_id: "183395182f015bcaca6c7ae9a99fd0d0e8e62d58e81a21f3e815491f43efac71"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/fine-tuning-a-multi-modal-model-for-video-intelligence"
published_at: "2026-05-28T16:22:16.297+00:00"
first_seen_at: "2026-07-20T23:20:16.234471+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:aa5dcfa9c96b5592923cd736deca82a0950f7b032f5c4bcd27eb811dc2f770bb"
---

# Fine-tuning a multimodal model for video intelligence

Published on


May 28, 2026 (2 months ago)


# Fine-tuning a multimodal model for video intelligence


By[Joshua Alphonse](https://www.mux.com/team/joshua-alphonse) • 12 min read •[Product](https://www.mux.com/blog/category/product) &[Engineering](https://www.mux.com/blog/category/engineering)


---


**TL;DR**


I fine-tuned a small multimodal model for Mux-specific video intelligence workflows, like transcript-based summaries and chapter generation. I then integrated that model into the open-source[@mux/ai SDK](https://github.com/muxinc/ai) . Along the way, I added[Baseten](https://www.baseten.co/) as a provider, generated 10,000 synthetic JSONL training examples, and used LoRA to fine-tune[Mistral Small 3.1.](https://huggingface.co/mistralai/Mistral-Small-3.1-24B-Instruct-2503) I found that the fine-tuned model produced more concise, opinionated, and workflow-specific outputs than the default Mux Robots experience. Mux Robots is still the fastest way to add video AI features, but fine-tuning a model for @mux/ai is a better fit when you want more control over how the model behaves.


We’ve been on quite the run for the first half of 2026.[VideoJS](https://videojs.org/) got an upgrade, and[Mux Robots,](https://www.mux.com/docs/guides/robots) the new video intelligence layer for videos on Mux, was just released. There is one more project that I think is special and should get more attention and that would be the[@mux/ai SDK](https://github.com/muxinc/ai) .


Mux Robots use[@mux/ai](https://github.com/muxinc/ai) under the hood. The difference is that[@mux/ai](https://github.com/muxinc/ai) is an open source version where you bring your own API keys to access AI workflows with the use of your own subscriptions like Open AI, Anthropic, Elevenlabs and more.


[@mux/ai](https://github.com/muxinc/ai) is really customizable and great for those projects that require a more nuanced developer experience and that's where fine-tuning comes in. There are some benefits to using a fine-tuned model like privacy, control and customization. You can’t get that experience from GPT or Claude but there are a lot of services out there that provide APIs to these open source models and let you fine-tune from that same API.


So it clicked for me.[@mux/ai](https://github.com/muxinc/ai) is the open source older sibling of Mux Robots. With[@mux/ai](https://github.com/muxinc/ai) you can bring your own API keys for the usual suspects (OpenAI, Anthropic, Elevenlabs) and use the models of your choice from the providers you choose. Now this also means that you have to do a lot of third party management that Mux Robots takes care of. As long as I have an OpenAI API compatible endpoint I could use any multimodal model I want right?


So at first, I forked @mux/ai and added Ollama as a provider to test models locally and not pay for tokens when I'm just experimenting.


[@mux/ai](https://github.com/muxinc/ai) has only shipped with OpenAI, Anthropic, Google and Elevenlabs as major providers out of the box. Luckily[@mux/ai](https://github.com/muxinc/ai) is open source so I decided to fork the repo and add Ollama and Baseten as providers. You can do the same and add any model provider you'd like.


I tested out open source multimodal models like Mistral 3.1 small and[Gemma 4](https://huggingface.co/google/gemma-4-31B) and I thought they were pretty good at producing acceptable summarizations of my videos but I was running the models locally on my Mac and it turns out that my computer’s GPU was the bottleneck. It would take an unpredictable amount of time to get a response from the model. But hey, free is free right?


I knew these models were capable but I also knew there could be a lot more squeezed out of them especially if I used a dedicated GPU on the cloud.


Since I wanted to scale this up and needed a dedicated GPU, I chose[Baseten](https://www.baseten.co/) as my inference provider. I met some cool folks from their team so Baseten became an even easier decision. @mux/ai uses the[AI SDK from Vercel](https://ai-sdk.dev/docs/introduction) . So I was already set up for a seamless integration. This was the start of the power of @mux/ai totally unlocking for me.


## How I’m using Baseten to fine-tune my own model for @mux/ai


Adding[Baseten](https://www.baseten.co/) as a model provider to work with[@mux/ai](https://github.com/muxinc/ai) was the easy part. I didn’t really do anything special. The Mux AI team already laid the foundation for me and all I had to do was follow the pattern of the other providers in[@mux/ai](https://github.com/muxinc/ai) and adding Baseten was a piece of cake.


typescript


```text
export    type    SupportedProvider    =    "openai"    |    "baseten"    |    "anthropic"    |    "google"  ;
```


The hard part was figuring out how I wanted to fine-tune this model, getting all the data and choosing the right model for the job. Having Baseten as a provider gives you access to a huge library of open source models. Baseten also has a new[training SDK](https://docs.baseten.co/reference/sdk/training) that allows you to fine-tune models and then make a dedicated deployment of your fine-tuned model that hooks it up to a GPU of your choice, paired with an endpoint that gives you access to the model from an HTTP request. So when the dedicated deployment is up and running, you can just use the model’s name when configuring @mux/ai in your code like this:


typescript


```text
import    {   getSummaryAndTags  }    from    "@mux/ai/workflows"  ;


const   result  =    await    getSummaryAndTags  (  "your-mux-asset-id"  ,    {
provider :    "baseten"  ,
model :    "your-fine-tuned-model-from-baseten"  ,
credentials :    {
basetenApiKey :   process .  env .  BASETEN_API_KEY  ,
basetenBaseUrl :   process .  env .  BASETEN_BASE_URL  ,
}  ,
}  )  ;


console  .  log  (  result .  title )  ;
console  .  log  (  result .  description )  ;
console  .  log  (  result .  tags )  ;
```


## LoRA- adapters for video intelligence


So let’s start with the first challenge, figuring out how I wanted to fine-tune this model. There are a ton of philosophies and techniques of how you can fine-tune or train a model. This post won’t cover all of them, but I’ll explain the technique I used called[LoRA](https://huggingface.co/docs/peft/main/en/conceptual_guides/lora) .


LoRA stands for **Low‑Rank Adaptation** . It’s a way to fine-tune big models (like LLMs) without updating all their weights, which saves huge amounts of compute and memory. A good analogy of a LoRA adapter is like having an expensive camera (the pre-trained model) and then adding a special macro lens to take pictures of insects up close (the LoRA adapter). That extra lens allows the camera to behave a bit differently but the main function of the camera still stands. LoRA is affordable, flexible and still allows the model to keep traits form the original pre-trained version in addition to some new behavioral adjustments.


## The Data


[@mux/ai](https://github.com/muxinc/ai) and Mux Robots both operate with multimodal models only. We need the models to utilize modalities like text, audio, video and images. @mux/ai’s AI workflows use your video’s asset tracks like the transcript , audio track and the frames of the video to complete an AI workflow.


Next I had to create a dataset in JSONL format because that's what LoRA likes. My dataset structured for training for only transcript-driven @mux/ai workflows:


- **summarize**


- **generate-chapters**


- **ask-questions**


- **translate-captions**


- **edit-captions**


LoRA adapters are great because I can leave the image modality used to analyze the frames alone and just modify the text modality that I’m using for the transcript analysis. In the future with enough data, I can update this model with changes to other modalities if needed. I heard someone say the wise words “Better data, better model.” once and it’s stuck with me since so I’m going to be careful here. At first I didn’t have enough organic data that I needed to make this a sufficient fine-tuned model. That would have required me to have thousands of videos uploaded and transcribed and that just didn’t seem doable with the time I had to prove my point that a smaller, fine-tuned model can do a great job when paired with @mux/ai. So I decided to use[codex](https://openai.com/codex/) to generate 10,000 synthetic datapoints that covered what a conversation in ai format would look like if there was to be a response from mux/ai.


json


```text
{
"id"  :    "example-id"  ,
"workflow"  :   "summarize |
generate-chapters | ask-questions |
translate-captions | edit-captions" ,
"messages"  :    [
{
"role"  :    "system"  ,
"content"  :   "Instructions for the
model..."
}  ,
{
"role"  :    "user"  ,
"content"  :   "Workflow input ,
transcript ,   captions ,   questions ,   etc..."
}  ,
{
"role"  :    "assistant"  ,
"content"  :   " {  \"structured\" :  \"JSON
response as a string\" }  "
}
]
}
```


The goal was to make the model more reliable at Mux-specific workflows: returning valid JSON, following the expected schemas, handling transcripts and captions, and producing task-specific outputs consistently.


Here's an example of what a few datapoints from my dataset look like:


json


```text
{  "id"  :    "summarize-0001"  ,    "workflow"  :    "summarize"  ,    "messages"  :    [  {  "role"  :    "system"  ,    "content"  :    "Generate structured summary metadata for media transcripts. Return valid JSON only with keys title, tags, and description."  }  ,    {  "role"  :    "user"  ,    "content"  :    "Workflow: summarize\nTone: neutral\nMaximum tag count: 6\n\nTranscript:\nThis internal workshop focuses on stream health alerts for application engineers as part of an internal migration. The synthetic project label is Harbor Beacon 0001 and the implementation centers on the translation staging queue 0001. The presenter explains how the team uses webhook signature verification to improve rollout safety. The speaker starts with the operational context and then walks through the full implementation. The recording also covers testing, operational tradeoffs, and production checks that keep the workflow dependable."  }  ,    {  "role"  :    "assistant"  ,    "content"  :    "{\"title\":\"Stream Health Alerts for Application Engineers\",\"tags\":[\"mux\",\"media infrastructure\",\"api integration\",\"stream health alerts\",\"internal workshop\",\"webhook signature verification\"],\"description\":\"A neutral summary of a internal workshop about stream health alerts. The speaker shows how webhook signature verification helps application engineers improve rollout safety, then closes with validation steps and rollout advice for Harbor Beacon 0001.\"}"  }  ]  }
{  "id"  :    "generate-chapters-0001"  ,    "workflow"  :    "generate-chapters"  ,    "messages"  :    [  {  "role"  :    "system"  ,    "content"  :    "You generate chapter markers from timestamped media transcripts. Return valid JSON only with the key chapters. Each chapter object must contain title and start_time."  }  ,    {  "role"  :    "user"  ,    "content"  :    "Workflow: generate-chapters\nSource language: en\nOutput language: en\n\nTranscript:\n[00:00] Welcome to this session on subtitle translation for QA engineers working on Juniper Studio 0001.\n[00:59] We explain why player embed controls matters and define the operating constraints for the ingest gate 0001.\n[02:25] Next we configure the workflow, review transcripts, and inspect edge cases.\n[04:01] Then we validate the output in the dashboard and compare rollout options.\n[05:58] Finally we cover production guidance, alerts, and follow-up actions."  }  ,    {  "role"  :    "assistant"  ,    "content"  :    "{\"chapters\":[{\"title\":\"Subtitle Translation Workflow Introduction Juniper Studio 0001\",\"start_time\":0},{\"title\":\"Initial Configuration\",\"start_time\":59},{\"title\":\"Transcript And Captions\",\"start_time\":145},{\"title\":\"Results Inspection\",\"start_time\":241},{\"title\":\"Next Steps\",\"start_time\":358}]}"  }  ]  }
{  "id"  :    "ask-questions-0001"  ,    "workflow"  :    "ask-questions"  ,    "messages"  :    [  {  "role"  :    "system"  ,    "content"  :    "Answer questions about media content. Return valid JSON only with the key answers. Each answer must include skipped, reasoning, question, confidence, and answer."  }  ,    {  "role"  :    "user"  ,    "content"  :    "Workflow: ask-questions\nAllowed answers: yes, no\n\nTranscript:\nIn this product walkthrough, the speaker covers webhook retries before a compliance review. They explain how dashboard inspection fits into the workflow for Lattice Launchpad 0001, point out validation checks in the playback policy layer 0001, and describe what happens before and after deployment.\n\nQuestions:\n1. Is there a presenter explaining the workflow directly?\n2. Does the content describe a production media workflow?\n3. Is this clip primarily about gardening tools?"  }  ,    {  "role"  :    "assistant"  ,    "content"  :    "{\"answers\":[{\"skipped\":false,\"reasoning\":\"The transcript directly discusses the workflow and provides enough evidence to answer confidently. This is consistent with the Lattice Launchpad 0001 walkthrough.\",\"question\":\"Is there a presenter explaining the workflow directly?\",\"confidence\":0.9,\"answer\":\"yes\"},{\"skipped\":false,\"reasoning\":\"The described walkthrough includes details that make the answer clear from the content alone. This is consistent with the Lattice Launchpad 0001 walkthrough.\",\"question\":\"Does the content describe a production media workflow?\",\"confidence\":0.91,\"answer\":\"yes\"},{\"skipped\":false,\"reasoning\":\"The speaker names the process explicitly, so the answer is supported by the transcript. This is consistent with the Lattice Launchpad 0001 walkthrough.\",\"question\":\"Is this clip primarily about gardening tools?\",\"confidence\":0.92,\"answer\":\"no\"}]}"  }  ]  }
{  "id"  :    "translate-captions-0001"  ,    "workflow"  :    "translate-captions"  ,    "messages"  :    [  {  "role"  :    "system"  ,    "content"  :    "Translate captions accurately while preserving cue order and timing. Return valid JSON only with the key translated_vtt."  }  ,    {  "role"  :    "user"  ,    "content"  :    "Workflow: translate-captions\nSource language: en\nTarget language: it\nTarget language name: Italian\n\nOriginal VTT:\nWEBVTT\n\n00:00.000 --> 00:02.500\nWelcome to this Mux walkthrough.\n\n00:02.800 --> 00:06.600\nToday we review stream health alerts and show how asset lifecycle hooks supports the workflow.\n\n00:07.000 --> 00:10.800\nThe examples are designed for application engineers preparing a production rollout on Nimbus Console 0001.\n"  }  ,    {  "role"  :    "assistant"  ,    "content"  :    "{\"translated_vtt\":\"WEBVTT\\n\\n00:00.000 --> 00:02.500\\nBenvenuti nel tutorial di Mux.\\n\\n00:02.800 --> 00:06.600\\nPer prima cosa carica il video e attendi l elaborazione.\\n\\n00:07.000 --> 00:10.800\\nPoi controlla i sottotitoli e conferma la trascrizione. Nimbus Console 0001.\\n\"}"  }  ]  }
{  "id"  :    "edit-captions-0001"  ,    "workflow"  :    "edit-captions"  ,    "messages"  :    [  {  "role"  :    "system"  ,    "content"  :    "Edit captions according to profanity-censorship rules and static replacements. Return valid JSON only."  }  ,    {  "role"  :    "user"  ,    "content"  :    "Workflow: edit-captions\nProfanity mode: blank\nAlways censor: crap\nStatic replacements: analtyics -> analytics\n\nOriginal VTT:\nWEBVTT\n\n00:00.000 --> 00:03.100\nWhat the crap is analtyics doing in this audio normalization walkthrough for Ember Cluster 0001?\n\n00:03.500 --> 00:06.800\nWe should keep the cue timing and formatting stable.\n"  }  ,    {  "role"  :    "assistant"  ,    "content"  :    "{\"edited_vtt\":\"WEBVTT\\n\\n00:00.000 --> 00:03.100\\nWhat the [____] is analytics doing in this audio normalization walkthrough for Ember Cluster 0001?\\n\\n00:03.500 --> 00:06.800\\nWe should keep the cue timing and formatting stable.\\n\",\"total_replacement_count\":2,\"auto_censor_profanity\":{\"replacements\":[{\"cue_start_time\":0,\"before\":\"crap\",\"after\":\"[____]\"}]},\"replacements\":{\"replacements\":[{\"cue_start_time\":0,\"before\":\"analtyics\",\"after\":\"analytics\"}]}}"  }  ]  }
```


## Baseten Truss for Mux AI


I’m sure in the future I’ll have plenty of real production data that I can use, but the synthetic generation made the first version so much easier. I just needed to share some example JSON responses from a @mux/ai or Mux Robots job and then use those as the shape for a larger training set.


For fine-tuning, I used[Baseten’s training SDK](https://docs.baseten.co/reference/sdk/training) to run a LoRA supervised fine-tuning job on the synthetically generated chat-style JSONL dataset like the example I shared above. For this first version I went with[Mistral 3.1 Small](https://huggingface.co/mistralai/Mistral-Small-3.1-24B-Instruct-2503) , which I used for during my testing with[Ollama](https://ollama.com/) . Mistral 3.1 Small is only 24b parameters. It is small enough to iterate on compared to larger frontier models, open source, and works well with LoRA.


Just like the example dataset above, each row contained a system prompt, user prompt, and expected assistant response for transcript-driven tasks like summaries, chapters, question answering, caption translation, and caption editing.


The actual Baseten training project had four main files:


- **config.py** for the Baseten training project, runtime, compute, and environment variables
- **run.sh** for installing dependencies and starting the training script
- **train.py** for loading the model, formatting the dataset, applying LoRA, and saving checkpoints
- **requirements.txt** for the Python dependencies like transformers, trl, peft, datasets, and accelerate


The dataset generation step produced two files:


bash


```text
synthetic-shared-train.jsonl
synthetic-shared-train.manifest.json
```


The manifest made it easy to sanity check the dataset before training. It confirmed that I had 10,000 total rows, split evenly across the five workflows:


json


```text
{
"total_datapoints"  :    10000  ,
"workflow_distribution"  :    {
"summarize"  :    2000  ,
"generate-chapters"  :    2000  ,
"ask-questions"  :    2000  ,
"translate-captions"  :    2000  ,
"edit-captions"  :    2000
}
}
```


In **config.py** , I defined the Baseten training job. The job used a PyTorch CUDA base image, enabled checkpointing, enabled caching, and requested one H100 GPU. I also configured the training hyper parameters as environment variables so I could tweak the run without rewriting the training script:


python


```text
"BASE_MODEL"  :    "mistralai/Mistral-Small-3.1-24B-Instruct-2503"  ,
"DATASET_PATH"  :    "./synthetic-shared-train.jsonl"  ,
"VALIDATION_SPLIT"  :    "0.05"  ,
"MAX_LENGTH"  :    "2048"  ,
"PER_DEVICE_BATCH_SIZE"  :    "1"  ,
"GRADIENT_ACCUMULATION_STEPS"  :    "16"  ,
"LEARNING_RATE"  :    "2e-4"  ,
"NUM_TRAIN_EPOCHS"  :    "1"  ,
"LORA_R"  :    "16"  ,
"LORA_ALPHA"  :    "32"  ,
"LORA_DROPOUT"  :    "0.05"
```


The runtime command was intentionally simple. Baseten starts the container, then Truss runs run.sh:


bash


```text
chmod   +x ./run.sh  &&   ./run.sh
```


Inside run.sh, I upgraded pip, installed the training dependencies, and started the training script:


bash


```text
python3  -m   pip  install    --upgrade   pip
python3  -m   pip  install    -r   requirements.txt
python3 train.py
```


The interesting work happened in train.py. I wrote this script to load the JSONL dataset, checked that every row had a messages array, and converted each chat-style example into one training string. If the tokenizer had a chat template, it used the tokenizer’s template. Otherwise, it fell back to a Mistral-style format with system, user, and assistant turns.


Then the script split off 5% of the dataset for validation and loaded the base model with bfloat16 when CUDA was available. For Mistral 3.1, I targeted the main attention and MLP projection layers with LoRA:


- **q_proj**
- **k_proj**
- **v_proj**
- **o_proj**
- **gate_proj**
- **up_proj**
- **down_proj**


These layers show the core benefit of LoRA. The MLP projection layers allow us to peel off parts of the model without having to re-train the full 24B parameters and modalities. It attached a small adapter (like the camera lens analogy) and trained only those adapter weights, which made the job much more practical.


Training was handled by TRL’s SFTTrainer, with PEFT providing the LoRA configuration. During the job, Baseten produced checkpoints of the training process for me and eventually written into Baseten’s checkpoint directory:


bash


```text
$BT_CHECKPOINT_DIR
```


At the end of training, the script saved the adapter, tokenizer, and a small training-metadata.json file with details like the base model, dataset path, number of train and eval examples, approximate steps, and final training loss.


To launch the training job, I pushed the Truss training config to Baseten via the CLI:


bash


```text
truss train push config.py
```


Truss packaged the training code, dataset, dependencies, and job config, then Baseten ran the training job on the H100 GPU. During the job, the script loaded the base instruct model, formatted the chat examples, attached the LoRA adapter, and trained only those adapter weights instead of retraining the full model.


After a while the training finished, and Baseten saved the LoRA checkpoints. The deploy step used deploy_config.py, which combined the original base model with the trained LoRA checkpoint and produced a dedicated hosted endpoint with the fine-tuned behavior applied at inference time.


The deployment config referenced the base model and the LoRA checkpoint:


bash


```text
base_model_id  =  "mistralai/Mistral-Small-3.1-24B-Instruct-2503"
```


Next, I deployed the checkpoint from the finished training job:


python


```text
export TRAINING_JOB_ID =  <  job_id >
truss train deploy_checkpoints  -  -  job -  id    <  job_id >
```


Once the model was deployed, I took the model’s base URL, the model name, and my Baseten API key and used them to call Mux AI jobs from the SDK.


bash


```text
BASETEN_API_KEY  =  your_baseten_api_key
BASETEN_BASE_URL  =  https://model-123.api.baseten.co/environments/production/sync/v1
BASETEN_MODEL  =  your-baseten-chat-model
```


I then took the model’s base URL, along with the name of the model and my Baseten api key to call Mux AI jobs from the SDK.


At that point, the model behaved like any other @mux/ai provider. The difference was that this endpoint was now carrying the fine-tuned model. In the example video above we are just doing a simple summarization AI workflow.


Summarizations worked pretty well and so did the generate chapters workflow. One H100 was more than enough and it gave me a fast response time. Obviously if you plan on scaling this up just throw more GPUs at it. that just seems like a popular choice to solve all our AI problems nowadays.


bash


```text
npm   run example:chapters -- PLA5HDxpnpz2wTvJxM5iocdwKiDF019mQI1NSpQKAq8g  --provider   baseten


>   @mux/ai@0.17.1 example:chapters
>   npx tsx examples/chapters/basic-example.ts PLA5HDxpnpz2wTvJxM5iocdwKiDF019mQI1NSpQKAq8g  --provider   baseten


🎯 Generating chapters  for   asset: PLA5HDxpnpz2wTvJxM5iocdwKiDF019mQI1NSpQKAq8g
📝 Language: en
🤖 Provider: baseten


✅ Success !
⏱️  Duration: 3884ms
📊 Generated  8   chapters


📋 Chapter List:
1  .  0  :00 - Introduction
2  .  0  :21 - Tailwind: Whats the hype?
3  .  1  :11 - Tailwind overview
4  .  2  :37 - Tailwind: The downsides
5  .  4  :21 - Tailwind: The upsides
6  .  7  :22 - Worth it
7  .  8  :07 - Conways Law
8  .  9  :32 - Conclusion


🎬 Mux Player Format:
[
{
"startTime"  :    0  ,
"title"  :    "Introduction"
}  ,
{
"startTime"  :    21  ,
"title"  :    "Tailwind: Whats the hype?"
}  ,
{
"startTime"  :    71  ,
"title"  :    "Tailwind overview"
}  ,
{
"startTime"  :    157  ,
"title"  :    "Tailwind: The downsides"
}  ,
{
"startTime"  :    261  ,
"title"  :    "Tailwind: The upsides"
}  ,
{
"startTime"  :    442  ,
"title"  :    "Worth it"
}  ,
{
"startTime"  :    487  ,
"title"  :    "Conways Law"
}  ,
{
"startTime"  :    572  ,
"title"  :    "Conclusion"
}
]
```


Overall I was happy with what 10,000 synthetically generated datapoints was able to produce. I even integrated my fine-tuned model into Robotube- a video streaming app for IOS and Android that uses @mux/ai and Mux Robots and to summarize, moderate, generate chapters, and caption translations.Here’s an example of the same video but, same summarization workflow but different models. One side uses Mux robots and the other uses my fine-tuned model.


Look, AI isn't perfect and I think both summaries need some improvements. Notice that the Mux Robots response is more verbose compared to how[@mux/ai](https://github.com/muxinc/ai) did with my fine-tuned model. Personally I think a more concise and straight to the point summary is best for the user experience and I was able to capture that type of summary with my own fine-tuned model paired with[@mux/ai](https://github.com/muxinc/ai) .


This doesn’t mean Mux Robots isn’t a good fit for your projects. In fact it can be the right fit for a project if you want to move fast and have all the model wiring done for you. So that there's no need to handle third-party API keys/access tokens. Robots can be called right from the Mux API or even through the Mux dashboard.


[@mux/ai](https://github.com/muxinc/ai) gave me the granular control I was looking for. If your team needs a more customized experience when it comes to video intelligence then[@mux/ai](https://github.com/muxinc/ai) is a tool worth trying.


## Written By


### [Joshua Alphonse – Community Engineer](https://www.mux.com/team/joshua-alphonse)


A Long Island native of Caribbean heritage, and a software engineer with a passion for record collecting and creative coding.


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
