---
schema_version: "1.0.0"
document_id: "303e869fe8a6687456754dd13936a9e40d00ed0e51e60d8a81acce61300ac94e"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/build-document-feature-with-reducto"
published_at: "2026-06-08T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:e5121ec03286ed6730381a349200e07304841d98accd41f4fcec206f8b7c4f26"
---

# Build your first document workflow with Reducto

It’s a typical flow across many AI products today: a user uploads a PDF, scan, form, spreadsheet, or image. And somewhere inside that file is the data your application needs. The hard part is turning that messy input into structured outputs your system can actually use.


That’s where Reducto comes in. You can integrate the entire suite of document tooling into your product in less than five minutes.


In this guide, we’ll build a simple first workflow:


```text
1. Upload document
2. Parse with Reducto
3. Get a structured output
4. Use it in your product
```


## 1. Create an API key


Sign up at[studio.reducto.ai](http://studio.reducto.ai/) .


Create an API key in the sidebar, then store it as an environment variable:


```text
bash  export REDUCTO_API_KEY="your_api_key_here"


```


Even better, you can give your agent[the Reducto MCP](https://docs.reducto.ai/mcp-server) or directly paste this blog post into Claude Code, Codex, Cursor, or your coding tool of choice. It’s that easy.


If you’d like to do it the analog way, keep reading.


## 2. Upload and parse a document


```text
bash  pip install reducto


```


Then initialize the client:


```text
python  from pathlib import Path
from reducto import Reducto


client = Reducto()
```


Pick a real file: a customer PDF, statement, form, scan, or spreadsheet-style report. The best test is something that looks like what your users actually upload:


```text
python  upload = client.upload(file=Path("sample-document.pdf"))
result = client.parse.run(input=upload)
print(result.job_id)
print(result.usage)
```


Reducto returns structured output, including logical chunks and layout-aware blocks. You can think of blocks as paragraphs, headers, tables, figures, list items. Chunking controls how these blocks are *grouped* together when returned in the API response.


## 3. Inspect the result in Studio


After your document is parsed, Reducto gives you a Studio link you can use to visually inspect the output. This is useful for debugging because you can see what Reducto parsed, where it came from (with accurate bounding boxes), and how the document was broken into structured pieces.


Instead of guessing whether the parse worked, you can inspect the actual document alongside the returned content.


```text
python  print(result.studio_link)
```


Open the link and look for a few things:


- Did Reducto capture the important sections?
- Are tables, headers, and paragraphs separated correctly?
- Do the bounding boxes line up with the source document?
- Is the output structured enough for your product to use?


For the first test, don’t use a perfect sample PDF. Use a real document that looks like what your users actually upload.


Not what you expected? If you’re having trouble with getting the right results, whether that’s content, format, chunking, or something else - check out our[Best Parse Practices](https://docs.reducto.ai/parse/best-practices) .


## 4. Access the parsed content


You can also inspect the response directly in code.


The response contains` chunks` , which are logical sections of the document. Each chunk has a` content` field with the full text for that section and a` blocks` field with the individual elements inside it.


Blocks can represent content like titles, section headers, text, tables, figures, key-value pairs, and more.


```text
python  # Loop through each chunk
for i, chunk in enumerate(result.result.chunks):
print(f"\\n=== Chunk {i + 1} ===")
print(chunk.content[:500])  # First 500 characters


# Look at individual blocks within this chunk
for block in chunk.blocks:
print(f"  [{block.type}] on page {block.bbox.page}")


# Tables are returned as HTML by default
if block.type == "Table":
print(f"  Table content: {block.content[:200]}...")
```


This gives you two ways to work with the document:


- Use` chunk.content` when you want the full text of a logical section
- Use` chunk.blocks` when you need more granular elements like tables, headers, paragraphs, or figures


Each block also includes a` bbox` field with bounding box coordinates, so you know exactly where that content came from on the page. That can be reflected in your product, for example showing your user what clause the returned text came from.


## 5. Plug it into your workflow


Once you have structured content, the next step is deciding what your product needs from the document.


If you’re building a RAG pipeline, you might send` chunk.content` into your retrieval system.


If you’re building a review UI, you might use` blocks` and` bbox` to show where each extracted field came from.


If you’re processing financial statements, invoices, or spreadsheets, you might start by pulling out tables:


```text
python  tables = []


for chunk in result.result.chunks:
for block in chunk.blocks:
if block.type == "Table":
tables.append({
"page": block.bbox.page,
"content": block.content
})


print(f"Found {len(tables)} tables")
```


From there, you can tune the workflow for your product: store the output, send it to an LLM, power a review interface, run validation checks, or trigger an automation.


## Final thoughts


Reducto is the fastest and easiest way to process your unstructured data at scale. Once you have one API call going, it’s easy to scale to your whole document corpus.


The best way to evaluate document AI is to test it on your own files. Take representative docs - whether they’re scans, dense spreadsheets, or a mix of everything. Send it through Reducto. Inspect the outputs, see the bounding boxes, and tweak your inputs to get better results.


If your current pipeline depends on brittle OCR, custom parsers, or prompt chains that break on new formats, you don’t need to spend weeks evaluating and integrating something better. Just feed this to your agent and get results right away.


Just sign up today at[studio.reducto.ai](http://studio.reducto.ai/) to get started!
