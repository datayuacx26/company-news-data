---
schema_version: "1.0.0"
document_id: "5800f94ac78b5410b26b5a5c6bac6747869797be676fcf573fa847a6f377e092"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/docker-model-runner/"
published_at: "2025-04-09T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:0165600f74b086b3a370a3b82af9caaa1c3c29f39e0709e5324631ad3bdd81e7"
---

# Create Local AI Agents with Dagger and Docker Model Runner

Dagger[natively integrates with LLM](https://docs.dagger.io/features/llm) s, making it easy to write AI Agents in just a few lines of code. Though it’s easy to run Dagger locally and connect to a remote LLM, sometimes you want to run your models locally. This may be required to work in air-gapped environments, for privacy concerns, or local development.


But when it’s time to run models locally, developers have to deal with local setup and management using unfamiliar tools. Enter[Docker Model Runner](https://docs.docker.com/desktop/features/model-runner/) , a new solution for easily setting up and running popular models in your local development environment that was[announced today](https://www.docker.com/blog/introducing-docker-model-runner/) .


#### Why Docker Model Runner?


Familiar to Docker users:


- Just like you download your container images with` docker pull` , you can now download models with` docker model pull`
- You run containers with` docker run` , now run your models with` docker model run`


**Based on common standards**


- Models are using the[GGUF](https://github.com/ggml-org/ggml/blob/master/docs/gguf.md) format
- They are packaged as[OCI](https://github.com/opencontainers/image-spec) artifactsIt’s easy to use, and many tools already handle them
- Any OCI compatible registry can host them
- It makes it (relatively) easy to push your own models


**Curated list of models**


- Think Docker Official Images but applied to AI models
- The models are hosted on the[Hub AI namespace](https://hub.docker.com/u/ai)


**Run natively**


- As you might have guessed, Docker Model Runner is not actually running models in containers. They are running natively on your host machine, using the machine’s GPU, since performance is key when you are working locally with AI models.


⚠️ Today, Docker Model Runner only runs models on Apple’s Metal APIs, meaning you need an Apple Silicon machine. The support might be extended in the future to other hardware.


💡 You need Docker Desktop version 4.40 or later to use Docker Model Runner. Enable the feature in the “Features in development” section of the Docker Desktop settings.


#### Now we can run models locally, how do we use them with Dagger?


Since running locally is a Dagger superpower, you just connect the Dagger LLM integration to the local models using a few environment variables. Then you’ll be set up to take advantage of Dagger’s automatic LLM tool calling (MCP) and prompting.


First, define the base URL to the OpenAI compatible engine:


` export OPENAI_BASE_URL=http://model-runner.docker.internal/engines/llama.cpp/v1/`


Then disable streaming of responses from the model since Docker Model Runner uses` llama.cpp` which currently has some limitations regarding streaming while using tools.


` export OPENAI_DISABLE_STREAMING=true`


Optionally, you can define the default model to use:


` export OPENAI_MODEL=index.docker.io/ai/qwen2.5:7B-F16`


⚠️These models can be quite large (the model above is about 14 GB), so you may want to` docker model pull` the model before using it with` dagger` .


Even if you define a default model, you can override it when accessing the` LLM` object in Dagger.


Now you are all set up, it’s time to run dagger with a local model using Docker Model Runner.


For this example, we’ll use Dagger Shell and ask the` LLM` to create some super simple ascii art, some smileys. We prepare an` env` for the` LLM` with an` "empty"` Dagger` Directory` object as input that returns a` "full"`` Directory` containing the requested ascii art` File` objects as output. We then pass the` env` to the` LLM` object, provide a prompt, and collect the results.


```text
$   dagger


⋈   llm   |   model
index.docker.io/ai/qwen2.5:7B-F16


⋈   myenv=  $(  env   |
with-directory-input   "empty"   $(  directory  )   "empty directory to add new files to"   |
with-directory-output   "full"   "a directory containing the ascii art files"  )


llm   |
with-env   $myenv   |
with-prompt   "start with empty, add 2 new smiley-themed pieces of ascii art, return as full"   |
env   |
output   "full"   |
as-directory   |
terminal
```


As the last step of the chain above, we attach a terminal to the returned` "full"` directory to inspect the files. You might see something like this:


```text
dagger   /src   $   ls
smiley1.txt    smiley2.txt


dagger   /src   $   cat   *
:  );)
```


Because everything is typed and cached, we can run something like this to get the contents of one of the` File` objects right from the cache:


```text
⋈ llm |
with-env $myenv |
with-prompt "start with empty, add 2 new smiley-themed pieces of ascii art, return as full" |
env | output "full" | as-directory | file smiley1.txt | contents


:)
```


Now, you can use Dagger to create your own local AI agents, and use Docker Model Runner to run your models locally.


To go deeper, check out the[Dagger docs](https://docs.dagger.io/quickstart/agent) and join us all on[Discord](https://discord.gg/dagger-io) !
