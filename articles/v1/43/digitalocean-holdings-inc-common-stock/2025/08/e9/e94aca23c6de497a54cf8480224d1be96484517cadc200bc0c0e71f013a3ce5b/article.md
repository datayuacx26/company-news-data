---
schema_version: "1.0.0"
document_id: "e94aca23c6de497a54cf8480224d1be96484517cadc200bc0c0e71f013a3ce5b"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/gpt-oss-models-on-gradient-ai-platform"
published_at: "2025-08-20T16:21:33.671+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:24323322e0eca235561e28dd5f7aacc9f9c4880bf0948470c4421ce06ce64605"
---

# Announcing OpenAI gpt-oss Models on the DigitalOcean Gradient™ AI Platform

OpenAI’s first open-source GPT models (20b and 120b) are now available on the[Gradient AI Platform](https://www.digitalocean.com/products/gradient/platform) . This launch brings even more flexibility and choice to developers building AI-powered applications, whether you’re starting with a quick prototype or scaling a production agent.


## What’s new


- **Open-source GPT models:** Access gpt-oss 20b and 120b directly on the Gradient AI Platform.
- **Code + UI support:** Call the models through our Serverless Inference API or select them in the Gradient dashboard when creating agents, or try them out in the model playground.
- **Integrated experience:** Unified billing, observability, and traceability built into the platform—no need to stitch together multiple vendors, billing, or monitoring tools.


## How to get started


**[With code:](https://gradientai-sdk.digitalocean.com/)** Call the models directly through our Serverless Inference API.


python


```text
import   os
import sys
from gradient import Gradient


model_access_key = os.environ.get( "GRADIENT_MODEL_ACCESS_KEY"  )


if not model_access_key:
sys.stderr.write( "Error: GRADIENT_MODEL_ACCESS_KEY environment variable is not set. \n  "  )
sys.exit(1)


inference_client = Gradient(model_access_key=model_access_key)


inference_response = inference_client.chat.completions.create(
messages=[
{
"role": "user",
"content": "Write a product description for an eco-friendly water bottle.",
}
],
model="openai-gpt-oss-20b",
)


print(inference_response.choices[0].message.content)


```


**[In the UI:](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai)** Head to Agent Creation and select gpt-oss 20b or 120b from the model dropdown.


## What you can build


- Customer support agents trained with your knowledge base
- Content generation tools with cost-optimized model choices
- AI-powered apps that balance performance with affordability


This launch marks another step toward making Gradient AI Platform the simplest, most flexible way to build real, production-ready AI applications.


## Deploy now


**For code-first developers:**


👉[Deploy gpt-oss via API:](https://gradientai-sdk.digitalocean.com/) Spin up the 20b or 120b models directly through the Gradient Serverless Inference API and start building in just a few lines of code.


**For UI users:**


👉[Build with gpt-oss in console:](https://cloud.digitalocean.com/registrations/new?activation_redirect=%2Fgen-ai&redirect_url=%2Fgen-ai) Create an agent in the Gradient AI Platform, select gpt-oss from the model dropdown, and deploy instantly—no code required.
