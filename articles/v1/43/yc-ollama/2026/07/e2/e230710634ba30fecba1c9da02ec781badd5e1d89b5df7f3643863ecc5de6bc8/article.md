---
schema_version: "1.0.0"
document_id: "e230710634ba30fecba1c9da02ec781badd5e1d89b5df7f3643863ecc5de6bc8"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/qwen3-vl"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:3720488b5eba4b7e2273fd5d81c566687dae83dd90f334f8baa0103fa7245aee"
---

# Qwen3-VL

**Qwen3-VL** , the most powerful vision language model in the Qwen series is now available on Ollama’s cloud. The models will be made available locally soon.


## Model capabilities


- **Visual Agent** : Operates PC/mobile GUIs—recognizes elements, understands functions, invokes tools, completes tasks
- **Visual Coding Boost** : Generates Draw.io/HTML/CSS/JS from images/videos
- **Advanced Spatial Perception** : Judges object positions, viewpoints, and occlusions; provides stronger 2D grounding and enables 3D grounding for spatial reasoning and embodied AI
- **Long Context & Video Understanding** : Native 256K context, expandable to 1M; handles books and hours-long video with full recall and second-level indexing
- **Enhanced Multimodal Reasoning** : Excels in STEM/Math—causal analysis and logical, evidence-based answers
- **Upgraded Visual Recognition** : Broader, higher-quality pre-training is able to recognize everything more types of objects—celebrities, anime, products, landmarks, flora/fauna, etc
- **Expanded OCR** : Supports 32 languages (up from 19); robust in low light, blur, and tilt; better with rare/ancient characters and jargon; improved long-document structure parsing
- **Text Understanding on par with pure LLMs** : Seamless text–vision fusion for lossless, unified comprehension


## Get started


1.


Download Ollama


2.


Run the model


```text
ollama run qwen3-vl:235b-cloud


```


Prompt the model with a message and image path(s). It is possible to use multiple images and drag and drop in images to make it easier to automatically type the file path.


## Examples


### Flower identification


**Prompt: What is this flower? Is it poisonous to cats?**


### Menu understanding and translation


**Prompt: Show me the menu in English!**


### Basic linear algebra


**Prompt: what’s the answer?**


## Using Qwen3-VL 235B


You can use[Ollama’s cloud](https://ollama.com/signup) for free to get started with the full model using Ollama’s CLI, API, and JavaScript / Python libraries.


### JavaScript Library


Install Ollama’s JavaScript library


```text
npm i ollama


```


Pull the model


```text
ollama pull qwen3-vl:235b-cloud


```


Example non-streaming output with image


```text
import ollama from 'ollama'


const response = await ollama.chat({
model: 'qwen3-vl:235b-cloud',
messages: [{
role: 'user',
content: 'What is this?',
images: ['./image.jpg']
}],
})
console.log(response.message.content)


```


Example streaming the output with image


```text
import ollama from 'ollama'


const message = {
role: 'user',
content: 'What is this?',
images: ['./image.jpg']
}
const response = await ollama.chat({
model: 'qwen3-vl:235b-cloud',
messages: [message],
stream: true,
})
for await (const part of response) {
process.stdout.write(part.message.content)
}


```


Ollama’s[JavaScript library](https://github.com/ollama/ollama-js) page on GitHub has more examples and API documentation.


### Python Library


Install Ollama’s Python library


```text
pip install ollama


```


Pull the model


```text
ollama pull qwen3-vl:235b-cloud


```


Example non-streaming output with image


```text
from ollama import chat
from ollama import ChatResponse


response: ChatResponse = chat(
model='qwen3-vl:235b-cloud',
messages=[
{
'role': 'user',
'content': 'What is this?',
'images': ['./image.jpg']
},
])
print(response['message']['content'])
# or access fields directly from the response object
print(response.message.content)


```


Example streaming the output with image


```text
from ollama import chat


stream = chat(
model='qwen3-vl:235b-cloud',
messages=[{
'role': 'user',
'content': 'What is this?',
'images': ['./image.jpg']
}],
stream=True,
)


for chunk in stream:
print(chunk['message']['content'], end='', flush=True)


```


Ollama’s[Python library](https://github.com/ollama/ollama-python) page on GitHub has more examples and API documentation.


## API


The model can also be accessed directly on ollama.com’s API.


1.


Generate an[API key](https://ollama.com/settings/keys) from Ollama.


2.


Set` OLLAMA_API_KEY` environment variable using your API key.


```text
export OLLAMA_API_KEY=your_api_key


```


1. Generate a response using[API examples](https://docs.ollama.com/cloud#generating-a-response) .


### OpenAI Compatible API


Ollama has OpenAI compatible API endpoints that support the[chat completions](https://docs.ollama.com/openai#%2Fv1%2Fchat%2Fcompletions) endpoint,[completions](https://docs.ollama.com/openai#%2Fv1%2Fcompletions) endpoint, and the[embeddings](https://docs.ollama.com/openai#%2Fv1%2Fembeddings) endpoint.


1.


Generate an[API key](https://ollama.com/settings/keys) from Ollama.


2.


The` base_url` should be set to` https://ollama.com/v1` and` api_key` set to the one generated from above.
