---
schema_version: "1.0.0"
document_id: "74b7395b00d71b76034d67322d1c0f6473d20af1d1f38020949a3f8675111902"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/capsule-job-department-mapping/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:de031002ce454fdf4e726da51914536624155f1853b8c53a06d298446955eae2"
---

# How to Map Job Titles to Departments with Capsule

Understanding a lead’s department (Engineering, Product, etc.) is crucial for analysis, routing, and scoring. Getting this data, however, can be difficult. Often all you have is the person’s title, and extracting the department from this can be quite manual.


Fortunately, within[Capsule](https://capsulecrm.com/) there’s an easy way to map a title to a job department - all within seconds and fully automated. This method uses natural language processing (NLP), the same machine learning tool that powers automated translation,[text sentiment analysis](https://www.nyckel.com/blog/text-classification/) , and more. The model ingests any business title and outputs the predicted department.


Enabling this within Capsule is easy. This guide will walk you through it. At a high level, it involves signing up for (1)[Zapier](https://www.zapier.com/) , a marketing automation tool, and (2)[Nyckel](https://www.nyckel.com/) , a machine learning tool with a premade Job Department model.


## How to Use Capsule to Map Titles to Job Departments


You’ll set up automation using Zapier that will send a contact’s work title from Capsule to Nyckel for department classification. When that’s done, Zapier will then automatically update the contact’s Capsule record with the right department.


Once that department field is populated, you can now use it for routing, sourcing, and/or analysis.


## First, set up Nyckel


Unless you have more than 1,000 leads a month, Nyckel is free to set-up and use.


1. Create a free account on[Nyckel](https://www.nyckel.com/) .
2. Email sales@nyckel.com and request access to the “Job Department Mapping” tool. It’s free, but they have to enable it.


## Next, prep the Capsule record


You’ll want to make sure your contact record has the below fields.


- Title *(input)*
- Department *(output)*


Both of these should be open text fields, and it doesn’t matter what they are exactly named.


## Finally, integrate via Zapier


This requires the[Zapier](https://www.zapier.com/) Starter plan (the basic paid plan), as you need multi-step Zaps.


(1) Use Zapier’s AI prompt and use the text below.


*I want to link Capsule to Nyckel and back. When a contact’s “title” field is updated, I want to send that title to Nyckel for classification. The result from Nyckel should then update the “department” field from same record.*


This will create the Zap’s framework.


(2) After that’s created, go through each step to authenticate your credentials.


(3) For the Nyckel step, make sure that:


- Action Event = Classify
- Input Data = The input “Title” field from Step 1
- Function = Choose the “Job Department Mapping” function


(4) For the final step that updates Capsule:


- Update the “Department” field with “Label Name” from Step 2 (Nyckel’s response).


(5) Save


Now, any time a new contact is created in Capsule, Zapier will send their title to Nyckel to be classified. The predicted job department will then be sent back to Capsule automatically.


You can then use this data for routing, sourcing, analysis, and more.
