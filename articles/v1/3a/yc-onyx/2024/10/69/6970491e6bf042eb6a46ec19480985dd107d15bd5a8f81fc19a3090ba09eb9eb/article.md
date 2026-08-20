---
schema_version: "1.0.0"
document_id: "6970491e6bf042eb6a46ec19480985dd107d15bd5a8f81fc19a3090ba09eb9eb"
company_key: "yc-onyx"
company: "Onyx"
source_id: "yc-onyx-news-import-2a3a6e22cda1"
canonical_url: "https://onyx.app/blog/onyx-and-unstructured-elevating-enterprise-search-capabilities"
published_at: "2024-10-21T00:00:00+00:00"
first_seen_at: "2026-07-25T17:58:53.878122+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:5cdbdd3de9befe04f1dbec77262e849fd560310e5d7e8ab678d57fb494de71fa"
---

# Onyx and Unstructured: Elevating Enterprise Search Capabilities

## Why they\`re better together


Generative AI is rapidly transforming the way that people access knowledge, making it both faster to access and easier to digest. Companies like OpenAI and Perplexity are reimagining the search experience and the tech-forward crowd who have adopted this technology are seeing incredible benefits. But the one aspect of life, the one where people use search the most, is surprisingly unaddressed-work.


For those unfamiliar,[Danswer](https://github.com/danswer-ai/danswer) provides a single place to find all work related knowledge. By connecting to all of the org's internal knowledge (like Slack, Google Drive, Sharepoint, GitHub, Confluence, etc.), Danswer makes GenAI much more useful in the professional setting.


As a user, you unlock the ability to ask questions like “What are the top bugs reported by the community” or “When is the SSO feature supposed to be released for general availability?” And underlying the capability to chat with the docs, of course, is the ability to accurately parse document types.


This is why we've partnered with[Unstructured](http://unstructured.io/) to enable parsing and chunking of the[26 file types](https://docs.unstructured.io/api-reference/api-services/supported-file-types) they support.


Unstructured Parsing Code


But as they have said in the AI space for many decades: “garbage in, garbage out”. A core requirement to be able to provide the most relevant knowledge is the ability to cleanly parse the incoming documents. That's where the magic of[Unstructured](http://unstructured.io/) comes into play.


Unstructured takes raw files of[almost any format](https://docs.unstructured.io/api-reference/api-services/supported-file-types) and transforms them into plain text with high to perfect information fidelity. Additionally, Unstructured organizes the final plaintext in such a way that is perfect for LLMs and other deep learning models to understand-exactly what we need for Danswer.


Prior to partnering with Unstructured, we used our own document parsing directly within the Danswer project. As a dev team, it was constant overhead to extend it for our users and to add support for new file formats and ensure all edge-cases were well handled.


When we integrated with Unstructured, we immediately unlocked an additional 13 types of files that were previously unhandled. Unstructured can also handle visual elements that we previously just skipped. PDFs that are just image scans are now cleanly processed and even images embedded in other document types can now be described into natural language and indexed into Danswer so that we can surface them. Previously, we needed to rely on the text context around the image. With Unstructured providing this pristine input data for Danswer, our context retrieval and RAG quality has seen a sizable improvement with very little effort on our part.


## Why we chose Unstructured


Security, flexibility, community. These are some of the biggest benefits of working with an open source project. Since Danswer is open source, we wanted to work with an open source partner to continue providing the benefits of OSS to our users (check out the integration[here](https://github.com/danswer-ai/danswer/blob/main/backend/danswer/file_processing/unstructured.py) ).


Most Danswer users are ingesting sensitive company knowledge like customer call transcripts, internal discussions, design docs, and even the core IP like the code repository. The knowledge of how all of this knowledge is processed and stored is critical for many security conscious teams.


Even if your team doesn't have the time to audit the code directly, you know it's been reviewed by the other large open source users. Rather than trusting a single team, you know the code has been pored over by engineering at top companies like Netflix, Uber, Zendesk, and many others.


The same is true for[Unstructured](https://github.com/unstructured-io) . Since they\`re in the chain of document ingestion, we want our users to have complete peace of mind and the ability to know EXACTLY how their documents are being handled.


Flexibility is another big advantage. Unstructured offers both a[hosted Serverless API](https://docs.unstructured.io/api-reference/api-services/saas-api-development-guide) and on-prem deployment options on[AWS](https://docs.unstructured.io/api-reference/api-services/aws) and[Azure](https://docs.unstructured.io/api-reference/api-services/azure) . Since the library is open source, this means that users can even customize the software or the deployment. Finally, having a community in this space means that all the different tools and file formats that people care about are all going to receive attention and support.


Unstructured Repo


## New capabilities via Danswer + Unstructured


With better document processing, a lot of previously desired but impossible use cases are now possible.


For example, a sales team we recently connected with wanted to ask questions against a large corpus of invoices. They were previously physical copies that were then scanned into PDFs and uploaded to google drive. Prior to the Unstructured integration, Danswer did not support multimodal RAG, as the PDFs documents ingested would only contain the PDF name and metadata. Since the actual contents were images of the scans, the system did not ingest the actual details of the invoices.


Now with the new Unstructured integration, the PDFs are being processed in full, including tables, the headers, and the actual images.


Unstructured Setup In Danswer


## Conclusion


We're super excited about this partnership! From our point of view, it's an absolute slam dunk with easy wins for our users and community. The integration between Danswer and Unstructured is ready to go and it's extremely easy to use. Just grab an API key from[https://app.unstructured.io/keys](https://app.unstructured.io/keys) and set it in the Danswer admin panel (see screenshot). And there's even more to come! We'll be working with the Unstructured team to push out options where you can run Unstructured locally within the Danswer deployment.


If you have any feedback or questions please join the[Danswer](https://join.slack.com/t/danswer/shared_invite/zt-2sslpdbyq-iIbTaNIVPBw_i_4vrujLYQ) or[Unstructured](https://unstructuredw-kbe4326.slack.com/signup#/domain-signup) public Slack channels, excited to see you there!
