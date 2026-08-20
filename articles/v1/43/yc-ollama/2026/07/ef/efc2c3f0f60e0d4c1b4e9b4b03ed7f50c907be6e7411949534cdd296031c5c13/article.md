---
schema_version: "1.0.0"
document_id: "efc2c3f0f60e0d4c1b4e9b4b03ed7f50c907be6e7411949534cdd296031c5c13"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/secureminions"
published_at: null
first_seen_at: "2026-07-22T07:08:30.130278+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:33963ed9de731bfd566aaf2162e8e6ea15ce633ef662be6582b53de92c851aec"
---

# Secure Minions: private collaboration between Ollama and frontier models

Three months ago, Stanford’s Hazy Research lab introduced Minions ([ICML 2025](https://arxiv.org/abs/2502.15964) ), an[open-source](https://github.com/hazyResearch/minions/) research project that connects local Ollama models (such as Google’s[gemma3:4b](https://ollama.com/library/gemma3) ) to frontier models in the cloud (such as GPT-4o). In Minions, the raw context stays local and can only be accessed by the local LLM. The frontier model orchestrates local LLMs and aggregates their outputs. By sending fewer tokens to the cloud, the protocol reduces cloud costs by 5x-30x while achieving 98% of frontier model accuracy.


Cost savings aside, **local-first setups (like Ollama) have a big privacy upside** : sensitive context never leaves the device. But in the original Minions protocol, **some** information still goes to the cloud, and that information can be sensitive.


[Avanika Narayan](https://x.com/Avanika15) and[Dan Biderman](https://x.com/dan_biderman) from Stanford’s[Hazy Research lab](https://x.com/hazyresearch) then asked: can we encrypt an entire local-remote communication protocol end-to-end? Even from the cloud provider itself?


Their team built a comprehensive security protocol that builds around the new “confidential computing mode” introduced with NVIDIA’s Hopper H100 GPUs.


#### Here’s how it works:


- The local device and the H100 GPU exchange keys.
- The GPU proves it’s genuine and running in secure mode via **remote attestation** .
- Once verified, the H100 becomes a **secure enclave** : all memory and computation are encrypted, and even root users can’t access plaintext.
- The local LLM messages are encrypted before being sent to the GPU enclave, where they’re safely decrypted and processed by the cloud LLM. Its outputs are again encrypted before being sent back to the local client.


No plaintext is exposed - during transmission or remote LLM inference.


Even with long prompts (~8k tokens) and large models like Qwen-32B, the overhead is minimal: **less than 1% added latency** .


Confidential LLM collaboration is no longer theoretical — it’s here!


For full technical details,[see the HazyResearch blog post](https://hazyresearch.stanford.edu/blog/2025-05-12-security) .


### Get started


Clone the repository:


```text
git clone https://github.com/HazyResearch/minions.git


cd minions


```


**Optionally** , create a virtual environment with your favorite package manager (e.g. conda, venv, uv, etc.):


```text
python3 -m venv .venv


source .venv/bin/activate


```


Next, install the Python package and dependencies:


```text
pip install -e .


```


If you don’t have it yet, install[Ollama](https://ollama.com/download) and Google’s Gemma 3 model:


```text
ollama pull gemma3:4b


```


### Running the secure protocol in app


The provided streamlit app runs an interactive demo of both the Minion and MinionS protocols. To start it, run:


```text
streamlit run app.py


```


A browser window will open. Select your Remote Provider to be “Secure”. Set the Secure Endpoint URL to[http://20.57.33.122:5056](http://20.57.33.122:5056/) . Set your local client to Ollama and select the model you would like to use.


### Example code


To run the secure protocol programmatically in Python, the` minions` package can be used.


First create a file named` example.py` and add the following contents:


```text
from minions.clients.secure import SecureClient
from minions.clients.ollama import OllamaClient
from minions.minion import Minion


remote_client = SecureClient(
endpoint_url="http://20.57.33.122:5056",
verify_attestation=True,
)


local_client = OllamaClient(model_name="gemma3:4b")


protocol = Minion(local_client=local_client, remote_client=remote_client)


task = "How many grand slams did he win"
context = """John Doe, a legendary tennis player, known for his powerful serve and agile footwork, won a total of 20 grand slam titles during his illustrious career. He started playing tennis at the age of 5, inspired by his father who was a local tennis coach. Throughout his career, he faced numerous challenges, including a severe knee injury that almost ended his career prematurely. Despite these setbacks, John managed to come back stronger, winning his first grand slam at the age of 22. His rivalry with another top player, Jane Smith, was legendary, often drawing huge crowds and media attention. Off the court, John was known for his philanthropic efforts, particularly in supporting underprivileged children to access sports facilities. His favorite tournament was Wimbledon, where he won 7 of his 20 grand slam titles, often citing the grass courts as his preferred playing surface."""
output = protocol(
task=task,
doc_metadata="file",
context=[context],
max_rounds=5,  # you can adjust rounds as needed for testing
)


```


Run the example:


```text
python example.py


```


### Read more


- [Minions](http://github.com/HazyResearch/minions) GitHub repository
- [Hazy Research blog post](https://hazyresearch.stanford.edu/blog/2025-05-12-security)
