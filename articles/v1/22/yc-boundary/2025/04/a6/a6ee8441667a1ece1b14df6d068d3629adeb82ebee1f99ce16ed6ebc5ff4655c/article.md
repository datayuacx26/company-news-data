---
schema_version: "1.0.0"
document_id: "a6ee8441667a1ece1b14df6d068d3629adeb82ebee1f99ce16ed6ebc5ff4655c"
company_key: "yc-boundary"
company: "Boundary"
source_id: "yc-boundary-news-import-b6810cf62b42"
canonical_url: "https://boundaryml.com/blog/llama-api-tool-calling"
published_at: "2025-04-29T00:00:00+00:00"
first_seen_at: "2026-07-24T21:11:53.929220+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:6d2d9ac5155622cea3c0e2c2bf2fa30a3e2d63faf415f9875b39a9b90f240f3f"
---

# Tool use with Llama API (and reasoning)

Meta's released a new[API](https://docs.llmapi.com/) for their[Llama models](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) .


We'll show you how to get improved tool-calling / function-calling with BAML (our prompting framework) ([github](https://github.com/boundaryml/baml) ). BAML lets you write prompts for structured extraction using a simple syntax.


We'll additionally show how to make the default Llama models do tool-calling / function-calling WITH reasoning.


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


### Tool calling with Llama 4 and BAML with reasoning


In BAML you can also use several tools. Here's another example


The calling code has no changes! BAML will automatically pull out only the tool call part of the API response. Try pressing play.


```text
from baml_client import b
from baml_client.types import ProductSearch, ScheduleAppointment
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
