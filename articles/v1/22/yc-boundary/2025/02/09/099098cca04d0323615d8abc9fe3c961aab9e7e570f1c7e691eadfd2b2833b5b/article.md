---
schema_version: "1.0.0"
document_id: "099098cca04d0323615d8abc9fe3c961aab9e7e570f1c7e691eadfd2b2833b5b"
company_key: "yc-boundary"
company: "Boundary"
source_id: "yc-boundary-news-import-b6810cf62b42"
canonical_url: "https://boundaryml.com/blog/gemini-2-function-calling"
published_at: "2025-02-05T00:00:00+00:00"
first_seen_at: "2026-07-24T21:11:53.929220+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:df30b00234dfd0c4c3a9684c646c7f2b7a273566c9947e1c5b7c46ed2c7ef723"
---

# Structured outputs with Gemini 2.0

Here is how we can get structured output from Gemini 2.0 reliably without having to write tedious JSON schemas.


For this, we are going to use[BAML](https://boundaryml.com/) , which is a simple prompting configuration file to write prompts with types, that you can then import in your code. It helps keep your code clean, organized, and best of all -- comes with a[VSCode playground](https://marketplace.visualstudio.com/items?itemName=Boundary.baml-extension) to run your prompts immediately.


Then when you want to run it in python you can save the BAML file, and the VSCode extension will generate a` baml_client` with your types and your function:


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


### Tool calling with Gemini 2.0 and BAML


Here is another example of using function calling with Gemini 2.0 -- where Gemini can decide to book an appointment or search for an item.


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


That's it!


BAML works with many languages (Ruby, TS, Python, etc), so feel free to check those out!


Check out the[docs](https://docs.boundaryml.com/) for more!
