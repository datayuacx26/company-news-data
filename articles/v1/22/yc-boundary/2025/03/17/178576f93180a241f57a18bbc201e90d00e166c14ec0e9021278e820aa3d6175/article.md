---
schema_version: "1.0.0"
document_id: "178576f93180a241f57a18bbc201e90d00e166c14ec0e9021278e820aa3d6175"
company_key: "yc-boundary"
company: "Boundary"
source_id: "yc-boundary-news-import-b6810cf62b42"
canonical_url: "https://boundaryml.com/blog/qwq-32b-function-calling"
published_at: "2025-03-06T00:00:00+00:00"
first_seen_at: "2026-07-24T21:11:53.929220+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:897ce192960f913d290fffbae4e7d09cdec04603dd6c9fa10b260ad891c1862a"
---

# Structured outputs with QwQ 32B

Qwen recently released[QwQ-32B](https://qwenlm.github.io/blog/qwq-32b/) , their latest reasoning-focused model that excels at structured outputs and function calling. QwQ-32B is built on top of Qwen2.5 and brings significant improvements in reasoning capabilities, making it particularly well-suited for complex tasks requiring structured outputs.


[BAML](https://boundaryml.com/) is our prompting framework that makes it easy to work with function calling across any LLM. Let's explore how to use QwQ-32B with BAML for some practical examples.


## How to use QwQ-32B with BAML


Let's look at an interactive example that showcases QwQ's strong reasoning capabilities!


### Complex Classification with QwQ


First, we'll write some BAML code to handle a more complex classification task that requires reasoning. QwQ is particularly good at understanding context and making nuanced decisions.


You can run this in Python like this:


```text
from baml_client import b
response = b.AnalyzeTicket(
ticket_description="Production database is down affecting all customer transactions."
)
print(f"Priority: {response.priority}")
print(f"Department: {response.department}")
print(f"Reasoning: {response.reasoning}")
print(f"Est. Time: {response.estimated_time} hours")


```


### Multi-Tool Reasoning with QwQ


One of QwQ's strengths is its ability to reason about which tool to use based on complex requirements. Here's an example with multiple possible actions:


You can use it in Python like this:


```text
from baml_client import b
from baml_client.types import CreateDocument, UpdatePermissions, ArchiveDocument


response = b.HandleDocumentRequest(
user_request="Please create a new internal documentation about our API rate limiting policy"
)


if isinstance(response, CreateDocument):
print("Creating new document:")
print(f"Title: {response.title}")
print(f"Access Level: {response.access_level}")
print(f"Tags: {', '.join(response.tags)}")
elif isinstance(response, UpdatePermissions):
print("Updating permissions:")
print(f"Document: {response.document_id}")
print(f"New Access Level: {response.new_access_level}")
print(f"Reason: {response.reason}")
elif isinstance(response, ArchiveDocument):
print("Archiving document:")
print(f"Document: {response.document_id}")
print(f"Retention Period: {response.retention_period} days")


```


QwQ-32B's advanced reasoning capabilities make it particularly good at:


1. Understanding complex context and requirements.
2. Making nuanced decisions between multiple options.
3. Providing detailed explanations for its choices.


That's it!


BAML works with many languages (Ruby, TS, Python, etc), so feel free to check those out!


Also, look at our[docs](https://docs.boundaryml.com/) for more examples and detailed usage instructions!
