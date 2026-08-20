---
schema_version: "1.0.0"
document_id: "bdb5d322a591554e95a7f066160d485dcba5401da566cd7f05cc2dccd52b4c3b"
company_key: "yc-fondo"
company: "Fondo"
source_id: "yc-fondo-news-import-3f319512b005"
canonical_url: "https://fondo.com/blog/zep-launches-5f3bf"
published_at: "2026-02-11T00:00:00+00:00"
first_seen_at: "2026-08-07T04:46:37.958513+00:00"
fetched_at: "2026-08-07T04:46:39.914894+00:00"
content_hash: "sha256:6296c996424787ea43dd5b8e3b5bd6dc4adeed3835e18d350f7e5de35515ff2e"
---

# Zep Launches: Fast, Accurate Structured Data Extraction for AI Assistant Apps

[Zep](https://www.getzep.com/) recently[launched](https://www.ycombinator.com/launches/LGH-zep-fast-accurate-structured-data-extraction-for-ai-assistant-apps) !


##### ‍
***"Memory for AI Apps"***


‍


###### ***10x faster than GPT-4o, with field format and validity guarantees.***


‍ ***‍*** ‍ **Founded by**[Daniel Chalef](https://www.linkedin.com/in/danielchalef/)


Many business and consumer apps must extract structured data from conversations between an LLM-powered Assistant and a human user. Often, the extracted data is the objective of the conversation. Consider completing a sales order, making a reservation, or requesting leave. All of these tasks require progressively collecting data from the conversation.


Latency and correctness are important. You will often want to identify the correct data values you have collected and those you still need. You’ll then prompt the LLM to request the latter.


[https://youtu.be/k8e8NsoVzFo](https://youtu.be/k8e8NsoVzFo)


If you’re making multiple calls to an LLM to extract and validate data on every chat turn, you’re likely adding significant latency to your response. This can be a slow and inaccurate exercise, frustrating your users.


#### **The Solution**


[Zep’s](https://www.getzep.com/) new Structured Data Extraction feature is a low-latency, high-accuracy tool for extracting the data you need from Chat History stored in Zep's Long-term Memory service.


> **Up to 10x faster than gpt-4o.** For many multi-field extraction tasks, you can **expect latency of under 400ms,** with the addition of fields increasing latency sub-linearly.


‍


#### **Comparing Zep with LLM JSON Mode**


Many model providers offer a JSON inference mode that guarantees the output will be well-formed JSON.


However:


- There are no guarantees that the field values will conform to the JSON Schema you define or that they are correct (vs. being hallucinated).
- All fields are extracted in a single inference call, with additional fields adding linearly or greater to extraction latency.


‍


#### **Preprocessing, Guided LLM Output, and Validation**


To ensure that the extracted data is in the format you expect and is valid given the current dialog,[Zep](https://www.getzep.com/) uses a combination of:


- dialog preprocessing, which, amongst other things, improves accuracy for machine-transcribed dialogs;
- using guided output inference techniques on LLMs running on Zep's own infrastructure;
- and post-inference validation.


You will not receive back data in an incorrect format when using a Zep field type such as email, zip code, or date time.


While there are limits to the extraction accuracy when the conversation is very nuanced or ambiguous, carefully crafting field descriptions can achieve high accuracy in most cases.


‍


#### **Up to 10x Faster than OpenAI gpt-4o**


When comparing like-to-like JSON Schema model extraction against gpt-4o,[Zep](https://www.getzep.com/) is up to 10x faster.
‍


***Image Credits:***[Zep](https://www.getzep.com/)


Zep's extraction latency scales sub-linearly with the number of fields in your model. That is, you may add additional fields with a low marginal increase in latency.


‍


#### **Support for Partial and Relative Dates**


[Zep](https://www.getzep.com/) understands various date and time formats, including relative times such as “yesterday” or “last week.” It can also parse partial dates and times, such as “at 3pm” or “on the 15th.”


‍


#### **Extracting from Speech Transcripts**


Zep can understand and extract data from machine-transcribed transcripts. Spelled-out numbers and dates will be parsed as if they were written language. Utterances such as “uh” or “um” are ignored.


‍


***Image Credits:***[Zep](https://www.getzep.com/)


‍


#### **Using Progressive Data Extraction To Guide LLMs**


Your application may need to collect several fields to accomplish a task. Since Zep's SDE is so fast, you can guide the LLM through this process by calling the extractor on every chat turn. This enables you to identify which fields are still needed and then direct the LLM to collect the remaining data.


***Image Credits:***[Zep](https://www.getzep.com/)


‍


‍


#### **Learn More**


###### **🌐 Visit**www.getzep.com **to learn more**


###### **‍ *‍***


###### ***🧠 Learn more in the***[Zep Structured Data Guide](https://help.getzep.com/structured-data-extraction)


**‍**


###### ***📝 Sign up for Zep's***[Long-term Memory Service for AI Assistants](https://www.getzep.com/)


**‍**


###### ***👣 Follow Zep on***[LinkedIn](https://www.linkedin.com/company/zep-ai/)


###### *‍*


*Image Credits:*[Zep](https://www.getzep.com/)


###### *‍*


‍
