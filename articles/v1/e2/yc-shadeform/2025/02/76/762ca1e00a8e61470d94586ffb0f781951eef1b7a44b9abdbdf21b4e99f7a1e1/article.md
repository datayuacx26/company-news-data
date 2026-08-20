---
schema_version: "1.0.0"
document_id: "762ca1e00a8e61470d94586ffb0f781951eef1b7a44b9abdbdf21b4e99f7a1e1"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/tutorials/self-host-and-run-deepseek-r1"
published_at: "2025-02-19T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c82c39838c9b5d6602fc321dfcdb34428b92c569f6aaa221d7f979e831391e08"
---

# Self Host and Run DeepSeek R1

## DeepSeek R1 Has Changed the Game:


DeepSeek R1 has emerged as an incredibly important model as the first Open Source model to scale Inference Time Compute. R1 and its distilled siblings have introduced two new capabilities to the AI Community. They enable recording of Chain-of-Thought tokens which are otherwise hidden by foundational model APIs. And they’re the first Open Source model usable to fine-tune or distill for reasoning use cases.


The model's architectural improvements deliver better throughput, which translates to decreased costs per token, and prices for inference are lower than a lot of foundational model APIs.


This will kick off a large amount of improvement on difficult tasks by scaling inference time compute.


## Why Self-Hosting is More Important Than Ever


The demand for Self-Hosting is now higher than ever to use full inference.


Here’s why:


- There are concerns over privacy and data security for the popular overseas hosting platforms.
- R1 Models now generate reasoning traces and very long outputs, on top of being very large (671 Billion Parameters for the Full R1 Model), which stresses GPU RAM requirements on even the largest nodes.
- For Inference services, running multiple requests for different customers on the same node can be difficult and cause high variance in performance. Self hosting offers more inference stability for applications where performance and latency are important.


Shadeform provides instant access to affordable, secure, and enterprise ready compute by aggregating supply from 20+ clouds into a centralized marketplace, making it easier than ever for customers to self host at the best prices.


## Templates Make Deployment Even Easier


Setting up the correct configurations to deploy these models can be quite tricky, so we have created “templates”: a way to re-use deployment configs across the GPU clouds on our platform.


We have templates for the R1 models already made, so deploying DeepSeek R1 is just a click or two away.


## How to Run DeepSeek-R1:


We’ve gone ahead and created a[template](https://platform.shadeform.ai/templates/cf7899a5-e722-4e32-83a9-53c5a23d7005) that is ready for a 1-Click deployment on an 8xH200 node. With this template, we use vLLM to serve the model with the following configuration:


- We’re serving the full` deepseek-ai/DeepSeek-R1` model
- We’re deploying this on an 8xH200 Node for the highest memory capacity, and splitting our model across the 8 GPU’s with` --tensor-parallel-size 8`
- We’re enabling vLLM to` --trust-remote-code` to run the custom code the model needs for setting up the weights/architecture.


To deploy this[template](https://platform.shadeform.ai/templates/cf7899a5-e722-4e32-83a9-53c5a23d7005) , simply click “Deploy Template”, select the lowest priced 8xH200 node available, and click “Deploy”.


Once we’ve deployed, we’re ready to point our SDK’s at our inference endpoint!


## How to interact with R1 Models:


There are now two different types of tokens output for a single inference call: “thinking” tokens, and normal output tokens. For your use case, you might want to split them up.


Splitting these tokens up allows you to easily access and record the “thinking” tokens that, until now, have been hidden by foundational reasoning models. This is particularly useful for anyone looking to fine tune R1, while still preserving the reasoning capabilities of the model.


The below code snippets show how to do this with AI-sdk, OpenAI’s Javascript and python SDKs.


### AI-SDK:


` import { createOpenAI } from '@ai-sdk/openai';`
` import { generateText, wrapLanguageModel, extractReasoningMiddleware } from 'ai';`


` // Create OpenAI provider instance with custom settings`
` const openai = createOpenAI({`
` baseURL: "http://your-ip-address:8000/v1",`
` apiKey: "not-needed",`
` compatibility: 'compatible'`
` });`


` // Create base model`
` const baseModel = openai.chat('deepseek-ai/DeepSeek-R1');`


` // Wrap model with reasoning middleware`
` const model = wrapLanguageModel({`
` model: baseModel,`
` middleware: \[extractReasoningMiddleware({ tagName: 'think' })\]`
` });`


` async function main() {`
` try {`
` const { reasoning, text } = await generateText({`
` model,`
` prompt: "Explain quantum mechanics to a 7 year old"`
` });`


` console.log("\\n\\nTHINKING\\n\\n");`
` console.log(reasoning?.trim() || '');`
` console.log("\\n\\nRESPONSE\\n\\n");`
` console.log(text.trim());`
` } catch (error) {`
` console.error("Error:", error);`
` }`
` }`


` main();`


### OpenAI JS SDK:


` import OpenAI from 'openai';`
` import { fileURLToPath } from 'url';`


` function extractFinalResponse(text) {`
` // Extract the final response after the thinking section`
` if (text.includes("</think>")) {`
` const \[thinkingText, responseText\] = text.split("</think>");`
` return {`
` thinking: thinkingText.replace("<think>", ""),`
` response: responseText`
` };`
` }`
` return {`
` thinking: null,`
` response: text`
` };`
` }`


` async function callLocalModel(prompt) {`
` // Create client pointing to local vLLM server`
` const client = new OpenAI({`
` baseURL: "http://your-ip-address:8000/v1", // Local vLLM server`
` apiKey: "not-needed" // API key is not needed for local server`
` });`


` try {`
` // Call the model`
` const response = await client.chat.completions.create({`
` model: "deepseek-ai/DeepSeek-R1",`
` messages: \[`
` { role: "user", content: prompt }`
` \],`
` temperature: 0.7, // Optional: adjust temperature`
` max_tokens: 8000 // Optional: adjust response length`
` });`


` // Extract just the final response after thinking`
` const fullResponse = response.choices\[0\].message.content;`
` return extractFinalResponse(fullResponse);`
` } catch (error) {`
` console.error("Error calling local model:", error);`
` throw error;`
` }`
` }`


` // Example usage`
` async function main() {`
` try {`
` const { thinking, response } = await callLocalModel("how would you explain quantum computing to a six year old?");`
` console.log("\\n\\nTHINKING\\n\\n");`
` console.log(thinking);`
` console.log("\\n\\nRESPONSE\\n\\n");`
` console.log(response);`
` } catch (error) {`
` console.error("Error in main:", error);`
` }`
` }`


` // Replace the CommonJS module check with ES module version`
` const isMainModule = process.argv\[1\] === fileURLToPath(import.meta.url);`


` if (isMainModule) {`
` main();`
` }`


` export { callLocalModel, extractFinalResponse };`


### Langchain:


` from langchain_openai import ChatOpenAI`
` from langchain.prompts import ChatPromptTemplate`
` from langchain.schema.runnable import RunnablePassthrough`


` from typing import Optional, Tuple`
` from langchain.schema import BaseOutputParser`


` class R1OutputParser(BaseOutputParser\[Tuple\[Optional\[str\], str\]\]):`
` """Parser for DeepSeek R1 model output that includes thinking and response sections."""`
``
` def parse(self, text: str) -> Tuple\[Optional\[str\], str\]:`
` """Parse the model output into thinking and response sections.`
``
` Args:`
` text: Raw text output from the model`
``
` Returns:`
` Tuple containing (thinking_text, response_text)`
` - thinking_text will be None if no thinking section is found`
` """`
` if "</think>" in text:`
` # Split on </think> tag`
` parts = text.split("</think>")`
` # Extract thinking text (remove <think> tag)`
` thinking_text = parts\[0\].replace("<think>", "").strip()`
` # Get response text`
` response_text = parts\[1\].strip()`
` return thinking_text, response_text`
``
` # If no thinking tags found, return None for thinking and full text as response`
` return None, text.strip()`
``
` @property`
` def _type(self) -> str:`
` """Return type key for serialization."""`
` return "r1_output_parser"`


` def main(prompt_text):`
` # Initialize the model`
` model = ChatOpenAI(`
` base_url="http://your-ip-address:8000/v1",`
` api_key="not-needed",`
` model_name="deepseek-ai/DeepSeek-R1",`
` max_tokens=8000`
` )`


` # Create prompt template`
` prompt = ChatPromptTemplate.from_messages(\[`
` ("user", "{input}")`
` \])`


` # Create parser`
` parser = R1OutputParser()`


` # Create chain`
` chain = (`
` {"input": RunnablePassthrough()}`
` | prompt`
` | model`
` | parser`
` )`


` # Example usage`
` thinking, response = chain.invoke(prompt_text)`
` print("\\nTHINKING:\\n")`
` print(thinking)`
` print("\\nRESPONSE:\\n")`
` print(response)`


` if __name__ == "__main__":`
` main("How do you write a symphony?")`


### OpenAI Python SDK:


` from openai import OpenAI`


` def extract_final_response(text: str) -> str:`
` """Extract the final response after the thinking section"""`
` if "</think>" in text:`
` all_text = text.split("</think>")`
` thinking_text = all_text\[0\].replace("<think>","")`
` response_text = all_text\[1\]`
` return thinking_text, response_text`
` return None, text`


` def call_deepseek(prompt: str) -> str:`
` # Create client pointing to local vLLM server`
` client = OpenAI(`
` base_url="http://your-ip-:8000/v1", # Local vLLM server`
` api_key="not-needed" # API key is not needed for local server`
` )`
``
` # Call the model`
` response = client.chat.completions.create(`
` model="deepseek-ai/DeepSeek-R1",`
` messages=\[`
` {"role": "user", "content": prompt}`
` \],`
` temperature=0.7, # Optional: adjust temperature`
` max_tokens=8000 # Optional: adjust response length`
` )`
``
` # Extract just the final response after thinking`
` full_response = response.choices\[0\].message.content`
` return extract_final_response(full_response)`


` # Example usage`
` thinking, response = call_deepseek("what is the meaning of life?")`
` print("\\n\\nTHINKING\\n\\n")`
` print(thinking)`
` print("\\n\\nRESPONSE\\n\\n")`
` print(response)`


## Other DeepSeek Models:


With the R1 Release, DeepSeek released the Full R1 model, plus a series of Distilled models. These models are based off of a series of popular OpenSource models of varying sizes, and trained by distillation from the full R1 Model itself.


We have already created templates for the[Llama-3.1.8B](https://platform.shadeform.ai/templates/a71fd5c6-339d-4c5c-8279-671f97f3fdd4) and[Qwen-2.5-32B](https://platform.shadeform.ai/templates/b46b3338-bacd-49f2-a274-58d2c58a6bfc) models so you can deploy them on Shadeform with a few clicks. They offer great tradeoffs for accuracy, performance and memory requirements. Below we have the different R1 models, and the suggested GPU configurations that account for increased memory usage of thinking tokens.


Model


Recommended GPU Config


` —tensor-parallel-size`


Notes


DeepSeek-R1-Distill-Qwen-1.5B


1x L40S, A6000, or A4000


1


This model is very small, depending on your latency/throughput and output length needs, you should be able to get good performance on less powerful cards.


DeepSeek-R1-Distill-Qwen-7B


1x L40S


1


Similar in performance to the 8B version, with more memory saved for outputs.


DeepSeek-R1-Distill-Llama-8B


1x L40S


1


Great performance for this size of model.
[Deployable via this template](https://platform.shadeform.ai/templates/a71fd5c6-339d-4c5c-8279-671f97f3fdd4) .


DeepSeek-R1-Distill-Qwen-14


1xA100/H100 (80GB)


1


A great in-between for the 8B and the 32B models.


DeepSeek-R1-Distill-Qwen-32B


2x A100/H100 (80GB)


2


This is a great model to use if you don’t want to host the full R1 model.
[Deployable via this template](https://platform.shadeform.ai/templates/b46b3338-bacd-49f2-a274-58d2c58a6bfc) .


DeepSeek-R1-Distill-Llama-70


4x A100/H100


4


Based on the Llama-70B model and architecture.


deepseek-ai/DeepSeek-V3


8xA100/H100, or 8xH200


8


Base model for DeepSeek-R1, doesn’t utilize Chain of Thought, so memory requirements are lower.


DeepSeek-R1


8xH200


8


The Full R1 Model.


## Recap


DeepSeek-R1 represents a shift in how we deploy Language models:


- We now need more memory for our deployments for much longer outputs
- We can scale reasoning tokens and inference time to increase accuracy on the most complicated tasks
- We can keep our reasoning traces for analysis and future fine-tuning
- We can finetune these base models for our use cases
- We have easier Reinforcement Learning via GRPO to accelerate time to market


Shadeform’s enterprise compute platform make it dead simple to access and deploy these models across multiple clouds - ensuring that you get the best experience and price across all providers. We’re excited about what this all means for use cases throughout AI. Stay tuned as we dive deeper into DeepSeek and fine-tuning!
