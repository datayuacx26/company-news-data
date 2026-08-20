---
schema_version: "1.0.0"
document_id: "c2482526ff16fc79b1f5c5099db0e0e3f6a6715aebb1d033719d2645f96da961"
company_key: "yc-greywing"
company: "Greywing"
source_id: "yc-greywing-news-import-fac42ec76768"
canonical_url: "https://www.grey-wing.com/blog/how-we-had-to-build-ocean-oracle"
published_at: null
first_seen_at: "2026-07-21T22:08:18.820655+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:9b32cd215125c66c8c7588e365ddfe705f2c76ca47d1ab9175c9e52171f7fd71"
---

# How we *had* to build OceanOracle

# How we *had* to build OceanOracle


How we were forced to become the first company to build a visual AI for documents


Hrishi Olickel


Published on 1st Feb 2024


5 minutes read


*"We have some PDFs we'd like to ask questions to. Is that something you can add to your AI?"*


This was the question that started us down a path that involved a lot of toil and trouble, and how we ended up with a genuine improvement over state-of-the-art RAG solutions.


> RAG stands for Retrieval-Augmented-Generation, which is the current umbrella term that covers methods for giving AI access to information that they don't already know - like your pdf. Modern AI is a lot like a fresh college graduate - they know a little about everything, but not much about any specific thing - and definitely not your documents. Modern RAG solutions function the same way as equipping this new grad with a good librarian. When a question is asked, this librarian can retrieve the right pages from the right books, the new grad (the AI) can then read them and try to answer your question.


Here's the story.


## **Looking for a solution**


When we were asked to integrate pdfs and unstructured information into our AI, our immediate response was to think that someone would have already done it. Very large companies exist in the space of[chat with pdf](https://www.chatpdf.com/) , and the sector is[projected to grow](https://www.sequoiacap.com/article/generative-ai-act-two/) even further. It seemed reasonable to us that off-the-shelf solutions had to exist that could assimilate reasonably sized pdfs and answer questions based on them.


### Recommended Reading


[Generative AI: Act Two](https://www.sequoiacap.com/article/generative-ai-act-two/)


Unfortunately, none of the solutions we tried really worked. The biggest problem was question complexity. This is easy to illustrate. Let's take this article as an example.


At the simplest level, we can ask: *'What is question complexity?'*


Most RAG systems out there can answer this question with reasonable success. The question has words that are present in the text, and right after this article mentions 'question complexity', I go on to explain the answer.


Let's go one level higher: *'Did Greywing find an off-the-shelf solution to their problem?'*


This is harder because the answer requires the libraries to fetch sections of this article that talk about us trying to find off-the-shelf solutions, and nothing else. Other sections - where we talk about the solution we built - will be confusing, and likely lead to an answer of 'Yes'.


One more level:


None of the products we tried in the retrieval space were able to answer questions where a direct answer was not already in the document. This was already a problem - but then we got handed another one.


They sent over some test documents.


## **More troubles**


The documents they sent over were hundreds of pages that looked like this:


Complex schematics, pipe diagrams, UI frames, with high density of information per page. Knowing the visual relationship between elements is the only way to tell someone how to traverse one part of the ship and another, or how to operate a particular screen.


At this point we were entirely in no-man's-land: there really aren't any products that would work with this data. Even the ones that claim an ability to use visual data usually go through an[OCR](https://en.wikipedia.org/wiki/Optical_character_recognition) step where the images are converted to text, which is then operated on.


We needed to build our own.


## **VisionRAG takes form**


The key core component in our solution made use of a new development in the world of AI that was only a few weeks old at the time:[Multi-modal LLMs](https://encord.com/blog/vision-language-models-guide/) . Beginning to be called Vision Models today, these are Transformer models (exactly like ChatGPT), that have been given the gift of sight.


By encoding images in place of text (through systems far too complex to explain here), these models can really see information - they can understand spatial relationships, they can identify objects and items, and they can read text in relation to the information on a page.


Using these models however is far from easy. How they can be steered and made useful is still an open field of study, and it took us three weeks and all of our expertise to understand how they can be used to understand a document (or multiple), and how to use them.


Once we did, we had an initial prototype that could process large visual documents, and answer questions. However, we still hadn't solved the problem of question complexity. How do we answer questions that would actually be useful to humans?


## **VisionRAG becomes WalkingRAG**


The key to us seemed to be the relationships contained in the documents themselves - and in considering how we approach these problems as humans. Asking a retrieval system to find the right pages and give you a perfect answer in a single shot seems too much - even the best humans couldn't always flip to the right pages to answer complicated questions.


So what do we do? We read the pages we can find, and use them to understand which pages we should look at next. If you read closely, the information you find will tell you where to look next - or where you shouldn't.


In a nutshell, that's how WalkingRAG - which powers Ocean Oracle - works. When we import documents, we extract relationships - references, topics, section names, etc - from the pages in the pdf. These tell us where to look for more information when we look at a page. Our AI retrieves the right information using a completely new retrieval system developed in-house, then uses those pages to understand where to look next. Even better, it does those things in parallel - to save time and cost.


### Recommended Watch


[Greywing announces Ocean Oracle](https://www.linkedin.com/feed/update/urn:li:activity:7158612380275380225)


Far better than a librarian and a college grad, WalkingRAG functions more like a team of librarians at your command, working together and looking through multiple documents - taking notes and collecting parts of the answer until they've got all of it.


## **The future**


This is what we have today. WalkingRAG is turning out to be a completely novel system that promises AI that can truly understand and traverse the information we've created as humans - to serve as a copilot and a subject matter expert for our organizations and engineers. I use it every day to understand large documents, and it's something we're excited to be delivering to anyone that will find it helpful.


Want to stay updated on all things AI we're building for Maritime? Sign up to our monthly newsletter[here](https://www.grey-wing.com/contact-us)


## Read more from **Proteus**


[Back to blog](https://www.grey-wing.com/blog)[10 minute read Proteus Superficially adopting AI or genuinely leveraging it? How to think about your AI Strategy in 2024. Nick Clarke @NickGreywing 12th Apr 2024](https://www.grey-wing.com/blog/superficially-adopting-ai-or-genuinely-leveraging-it)


[3 minute read Proteus Proteus Vs Ocean Oracle… which is better? Can I pit our two newest products against each other? Nick Clarke @NickGreywing 19th Feb 2024](https://www.grey-wing.com/blog/proteus-vs-ocean-oracle-which-is-better)


[5 minute read Proteus Inception: The origin story of Ocean Oracle Unlike in ancient history, we're giving you real data. Nick Clarke @NickGreywing 30th Jan 2024](https://www.grey-wing.com/blog/the-origin-story-of-ocean-oracle)
