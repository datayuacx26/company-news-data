---
schema_version: "1.0.0"
document_id: "4796a931e385a900c91fae2cabd68ecd742547cab8292f71b1e0b5cceebe3309"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/cloud-models"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:e31a7836ba3ab5fcab05ea34e1e13cc931cd4818cbb14a75d99ee0ca8db5cd8c"
---

# Cloud models

[Cloud models](https://ollama.com/cloud) are now in preview, letting you run larger models with fast, datacenter-grade hardware. You can keep using your local tools while running larger models that wouldn’t fit on a personal computer. Ollama’s cloud does not retain your data to ensure privacy and security.


The same Ollama experience is now seamless across both local and in the cloud, integrating with the existing tools you already use. Ollama’s cloud models also work via Ollama’s[OpenAI-compatible API](https://docs.ollama.com/openai) .


### Get started


Download[Ollama v0.12](https://ollama.com/download) , then open a terminal and run a cloud model:


```text
ollama run qwen3-coder:480b-cloud


```


### Available models


- ` qwen3-coder:480b-cloud`
- ` gpt-oss:120b-cloud`
- ` gpt-oss:20b-cloud`
- ` deepseek-v3.1:671b-cloud`


### Usage


Cloud models behave like regular models. For example, you can` ls` ,` run` ,` pull` , and` cp` them as needed:


```text
% ollama ls
NAME                      ID            SIZE        MODIFIED
gpt-oss:120b-cloud        569662207105  -           5 seconds ago
gpt-oss:20b-cloud         875e8e3a629a  -           1 day ago
deepseek-v3.1:671-cloud   d3749919e45f  -           2 days ago
qwen3-coder:480b-cloud    11483b8f8765  -           2 days ago


```


### Usage in Ollama’s API


#### JavaScript


First, install Ollama’s JavaScript library:


```text
npm install ollama


```


Next, pull a cloud model:


```text
ollama pull gpt-oss:120b-cloud


```


Once the model is available locally, run it using JavaScript:


```text
import ollama from "ollama";


const response = await ollama.chat({
model: "gpt-oss:120b-cloud",
messages: [{ role: "user", content: "Why is the sky blue?" }],
});
console.log(response.message.content);


```


#### Python


Download Ollama’s Python library


```text
pip install ollama


```


Next, pull a cloud model:


```text
ollama pull gpt-oss:120b-cloud


```


Once the model is available locally, run it using Python:


```text
import ollama
response = ollama.chat(model='gpt-oss:120b-cloud', messages=[
{
'role': 'user',
'content': 'Why is the sky blue?',
},
])
print(response['message']['content'])


```


#### cURL


Pull a cloud model:


```text
ollama pull gpt-oss:120b-cloud


```


Next, call Ollama’s API to run the model


```text
curl http://localhost:11434/api/chat -d '{
"model": "gpt-oss:120b-cloud",
"messages": [{
"role": "user",
"content": "Why is the sky blue?"
}],
"stream": false
}'


```


### Signing in and out


Cloud models use inference compute on ollama.com and require being signed in to ollama.com:


```text
ollama signin


```


To stay signed out, run:


```text
ollama signout


```


### Cloud API access


Cloud models can also be accessed directly via[ollama.com’s API](https://docs.ollama.com/cloud#cloud-api-access) .
