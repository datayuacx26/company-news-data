---
schema_version: "1.0.0"
document_id: "b15fc16458eff21ebf8768ea70ef56bfafe38604898b9f73141abb333f6c3dc3"
company_key: "yc-boundary"
company: "Boundary"
source_id: "yc-boundary-news-import-b6810cf62b42"
canonical_url: "https://boundaryml.com/blog/llama-4-function-calling"
published_at: "2025-04-06T00:00:00+00:00"
first_seen_at: "2026-07-24T21:11:53.929220+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:7629e7e653c764cca7f3fafb6681dfd2858115cc4894300e5dcb06cf0cf3b243"
---

# Structured outputs with Llama 4

Meta's[Llama 4](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) is the newest release of the Llama series. It touts native multi-modality and a Mixture of Experts (MoE) architecture and can be used for tool-calling / function-calling.


Let's look at an example of how to use it with our prompting framework,[BAML](https://boundaryml.com/) , which lets you write prompts for structured extraction using a simple syntax.


### Solving a reasoning problem with a schema


Imagine we are trying to figure out the employee hierarchy chart for a company with this:


```text
George is the CEO of the company. Kelly is the VP of Sales. Asif is the global head of product development. Mohammed manages the shopping cart experience. Tim manages sales in South. Stefan is responsible for sales in the f100 company. Carol is in charge of user experience"


```


Here is an interactive example that you can run to see how Llama 4 can solve this reasoning problem!


You can run this in python like this:


```text
from baml_client import b
response = b.ExtractHierarchy(message="""
George is the CEO of the company.
Kelly is the VP of Sales. Asif is the global head of product development.
Mohammed manages the shopping cart experience.
Tim manages sales in South.
Stefan is responsible for sales in the f100 company.
Carol is in charge of user experience""")


print(response) # fully type-safe and validated!


```


### Tool calling with Llama 4 and BAML


In BAML you can also use several tools. Here's another example


You can call this in python like this (we also support other languages!):


```text
from baml_client import b
from baml_client.types import ProductSearch, ScheduleAppointment
response = b.ChooseOneTool(user_message="Find me running shoes under $100 in the sports category")
print(response)


if isinstance(response, ItemSearch):
print(f"Item Search called:")
print(f"Query: {response.query}")
print(f"Max Price: ${response.maxPrice}")
print(f"Category: {response.category}")
elif isinstance(response, BookAppointment):
print(f"Book Appointment called:")
print(f"Customer: {response.clientName}")
print(f"Service: {response.serviceRequested}")
print(f"Date: {response.datePreferred}")
print(f"Duration: {response.timeDuration} minutes")


```


BAML is fully open-source and free to use, and it works with many other languages (Ruby, TS, Python, etc).


Check out the[docs](https://docs.boundaryml.com/) for more!
