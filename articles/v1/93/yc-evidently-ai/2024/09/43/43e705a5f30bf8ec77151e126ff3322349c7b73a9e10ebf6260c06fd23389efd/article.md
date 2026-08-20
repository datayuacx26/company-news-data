---
schema_version: "1.0.0"
document_id: "43e705a5f30bf8ec77151e126ff3322349c7b73a9e10ebf6260c06fd23389efd"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-hallucination-examples"
published_at: "2024-09-25T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:be580cc50891e9a434faa4eccd3e766ea126079925b6f8a110c3b08adbb3f761"
---

# LLM hallucinations and failures: lessons from 5 examples

Large language models (LLMs) are powerful tools that enable AI applications like chatbots, content generation, and coding assistants. However, LLMs are not flawless. One common issue is LLM hallucination, where the model generates factually incorrect or fabricated information. Malicious users can also attempt to jailbreak LLMs and bypass their security measures, making the system perform unintended actions or produce restricted content. Some LLM products fail spectacularly, making the headlines, losing their company’s money, and leaving users frustrated.


This post will explore five real-world examples of LLM hallucinations and other failures that can occur in LLM-powered products in the wild, such as jailbreaks and out-of-scope usage scenarios. These LLM failures make good examples for developers, AI engineers, and product managers to learn from.


Each case highlights critical gaps that can occur when working with LLM-powered applications from development to production. We will explore what went wrong and suggest how to avoid these pitfalls.


## LLM hallucination examples


### Support chatbot cites nonexistent policy


Air Canada, the flag carrier and the largest airline in Canada, was[ordered](https://www.forbes.com/sites/marisagarcia/2024/02/19/what-air-canada-lost-in-remarkable-lying-ai-chatbot-case/) to compensate a passenger who received incorrect information concerning refund policies from the airline's chatbot.


When the customer sought a refund, the airline admitted the chatbot’s error but refused to honor the lower rate. However, a tribunal ruled that Air Canada is responsible for all information on its website, whether it comes from a static page or a chatbot. The tribunal also ruled that the airline “failed to take reasonable care in ensuring its chatbot's accuracy” and ordered payment of the fare difference.


> **Pro tip.** AI regulatory landscape evolves rapidly. Check out this[guide on AI regulations](https://www.evidentlyai.com/blog/ai-regulations) to learn what they mean for teams building AI-powered products.


Making up a nonexistent policy — as Air Canada’s chatbot did — is a typical example of LLM hallucination, where the AI-generated response isn’t grounded in factual information like a company policy document or a support knowledge base. If not monitored and tested properly, sometimes LLMs can make things up!


> **Takeaway:** use RAG to ground the responses and build an evaluation system to test it.
>
>
> To prevent LLM from hallucinating, it is common to implement RAG - retrieval-based generation, where you first look up documents that can support the answer and then generate the answer itself. Ideally, you can also instruct your LLM app to accompany its responses with references or links to verified sources to ensure accuracy.
>
>
> To make sure that this architecture works well and that LLM’s responses are grounded in the appropriate context, you also need evaluations and testing, both before you deploy the system and to continuously test the model’s responses in production. One way to evaluate the groundedness of responses is to use another LLM to evaluate the outputs of your AI system, a method known as " **LLM-as-a-judge** ". (To learn how to create and run[LLM evaluators](https://www.evidentlyai.com/llm-guide/llm-evaluation) , check out this[guide on LLM judges](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) and a hands-on[tutorial](https://www.evidentlyai.com/blog/llm-as-a-judge-tutorial) ).


### ChatGPT references fake legal cases


During the New York federal court filing, one of the lawyers was caught citing non-existent cases. It turned out he was using ChatGPT to conduct legal research — the bot referenced fake cases to the attorney.


Responding to the incident, a federal judge issued a standing[order](https://www.txnd.uscourts.gov/judge/judge-brantley-starr) that anyone appearing before the court must either attest that “no portion of any filing will be drafted by generative artificial intelligence” or flag any language drafted by AI to be checked for accuracy.


> **Takeaway:** clearly communicate to the users how your product works.
>
>
> This case is another LLM hallucination example, where the AI confidently generates sources that appear plausible but are entirely fabricated. Beyond rigorous testing, it’s crucial to communicate to users that AI can make mistakes: ChatGPT should not be treated as a knowledge database. In scenarios where freeform answers are generated without strict contextual grounding, users should be cautious and verify the LLM's outputs. Today, many LLM-powered features already include disclaimers, acknowledging that errors may occur: if you are creating a product that can be misused, it’s important to provide such clarity (like labeling specific responses as AI-generated) so that users must remain vigilant in double-checking critical information.


### Transcription tool creates fabricated text


OpenAI’s Whisper transcription tool, increasingly used by hospitals to turn doctor appointments into written records, has been[shown](https://www.wired.com/story/hospitals-ai-transcription-tools-hallucination/) to “invent” text not spoken by patients or doctors. According to one[study](https://arxiv.org/abs/2402.08021) , 1% of transcription samples included entirely hallucinated phrases or sentences that did not exist in any form in the underlying audio, and nearly 40% of the hallucinations were harmful or concerning because the speaker could be misinterpreted or misrepresented. Made-up fragments included racial commentary, violent rhetoric, and even imagined medical treatments.


Despite OpenAI warning that Whisper is unsuited for “high-risk domains” like medicine, over 30,000 medical professionals reportedly use Whisper-based systems to transcribe patient encounters.


> **Takeaway:** for high-risk domains, stress-test your system for potential hallucinations and use LLM-powered monitoring and human oversight to check for accuracy in critical cases.
>
>
> Language models like Whisper inherently predict what is “most likely” rather than what is strictly accurate, especially when audio is unclear or ambiguous. The original Whisper card[describes](https://github.com/openai/whisper/blob/main/model-card.md#performance-and-limitations) this phenomenon as follows: *“Because the models are trained in a weakly supervised manner using large-scale noisy data, the predictions may include texts that are not actually spoken in the audio input (i.e. hallucination). We hypothesize that this happens because, given their general knowledge of language, the models combine trying to predict the next word in audio with trying to transcribe the audio itself.”*
>
>
> This issue could be mitigated by deploying a second AI model designed to detect segments of unclear audio where Whisper is prone to fabricating content. The model could then flag those sections so a human reviewer can later verify their accuracy.


LLM hallucination is not the only–though common–LLM flaw. Here are more examples of LLM failures to learn from.


## Unintended behavior and LLM prompt injection


### Support chatbot generates code


A Swedish fintech company, Klarna, launched its AI chatbot that handles two-thirds of customer support chats. A month after the chatbot’s release, Klarna[announced](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/) that its AI assistant had 2.3 million conversations, equivalent to 700 full-time support agents. The company reported a 25% drop in repeat inquiries and a 5X decrease in errands resolving time. The chatbot is available in 23 markets, 24/7, and communicates in more than 35 languages.


Users who have deliberately tested the chatbot for common behavior failures[report](https://blog.pragmaticengineer.com/klarnas-ai-chatbot/) that it is well-built. However, one user got the Klarna chatbot to generate code by asking for some help with Python, a conversation scenario that should not be expected from a support chatbot.


*Source: Collin Fraser account on X*


Preparing the chatbot to detect and handle off-topic scenarios is an important consideration in the AI product design. Beyond benign occasions where customers treat your chatbot as a coding assistant for fun, you should also consider jailbreaking attacks – attempts to bypass built-in LLM safety measures or[prompt injections](https://www.evidentlyai.com/llm-guide/prompt-injection-llm) that instruct the LLM system say or do things it was not intended to do.


> **Takeaway:** consider implementing a classifier to detect types of queries your product should not handle.
>
>
> The key lesson from this example is that AI-powered systems should be thoroughly tested for how they respond to off-topic or inappropriate prompts. One effective solution is using filters to check whether the user's inquiry fits the expected scenarios and implement this validation directly inside your product. If it doesn’t, you can return a preset response like “I can’t answer this question” or redirect the query to a live agent to prevent misuse and ensure the chatbot remains focused on its core function.


### Chatbot sells a car for $1


Another example of unintended behavior is from a Chevrolet customer service chatbot. By instructing the LLM to agree with every demand, the user got the chatbot to sell him a late-model Chevrolet Tahoe for a dollar and[posted](https://twitter.com/ChrisJBakke/status/1736533308849443121) the screenshots on X.


*Source: Chris Bakke account on X*


The post went viral and sparked a series of other funny incidents: users managed to make the chatbot do a Python script,[offer](https://x.com/colin_fraser/status/1736851563300434162) a 2-for-1 deal for all new vehicles, and[recommend](https://x.com/colin_fraser/status/1736851563300434162) a Tesla.


*Source: Colin Fraser account on X*


*Source: Collin Muller account on X*


> **Takeaway:** implement output guardrails and perform adversarial testing during product development.
>
>
> The lack of guardrails in Chevy’s chatbot allowed skilled users to generate responses beyond the customer service chatbot scope and obtain almost any kind of response from the app. While some instances of off-topic use may seem harmless, intentional jailbreaking and prompt injection can potentially cause security breaches in AI-powered systems and undermine AI assistants’ reliability, as well as harm your brand perception.
>
>
> To address this risk, you can implement protections inside your product, both by adding specific instructions to your prompt and implementing real-time output guardrails. To see how well this works, you should test your LLM product for these behaviors during development using adversarial testing. You can try to purposefully steer your LLM product to misbehave, like asking it to talk about competition and see how it handles this. In production, you should continue monitoring: for example, you can use[custom LLM judges](https://www.evidentlyai.com/blog/llm-as-a-judge-tutorial) to detect unintended AI-system behavior.


## What can we learn from the examples?


LLM-powered systems are not the “deploy-and-forget” type of solutions. To mitigate the risk of LLM hallucinations and other failures like prompt injections we must thoroughly test AI products, evaluate their performance on specific tasks and monitor it over time. Here are some considerations to keep in mind when building an LLM applications:


**When to evaluate?** At every step of the process, actually! While developing **** your app and experimenting with prompts and models, you can create evaluation datasets to test specific behaviors. When making changes, you’ll want to perform regression testing to[ensure](https://www.evidentlyai.com/blog/llm-regression-testing-tutorial) that tweaking a prompt does not break anything and that the new version performs as expected. In production, you can run checks on live data to keep tabs on the outputs of your LLM-powered system and ensure they are safe, reliable, and accurate.


**Define what quality is to you.**[Evaluating LLM systems](https://www.evidentlyai.com/llm-guide/llm-evaluation) is task-specific: the perceived quality of an airline support chatbot will differ from that of a coding assistant. Specify the criteria that matter to you – whether it's relevance, correctness, or safety, and design evaluation datasets that are representative of your use case.


**Often, simple metrics are not enough.** When it comes to generative tasks, you often need to design custom[LLM evaluation metrics](https://www.evidentlyai.com/llm-guide/llm-evaluation-metrics) that match your use case. One of the ways to approach this problem is to use[LLM-as-a-judge](https://www.evidentlyai.com/blog/llm-as-a-judge-tutorial) . LLM judges assess outputs based on your rubric – you can create multiple judges and provide precise guidelines for each task.


**Monitor metrics in time.** By tracking the system’s inputs and outputs over time, you can monitor trends, recognize changes in user behavior, and spot recurring issues. Having a visual dashboard makes monitoring and debugging much easier.


## Get your AI risk assessment with Evidently


AI risks don't show up in demos. You need to test your LLM system to ensure it's ready for the real world. That’s why we built[Evidently](https://www.evidentlyai.com/) . Our open-source library, with over 25 million downloads, makes it easy to test and evaluate LLM-powered applications, from chatbots to RAG. It helps build and manage LLM evaluation workflows end-to-end. To run LLM evals, you can pick from a library of metrics or easily configure custom LLM judges that fit your quality criteria.


*Example Evidently monitoring dashboard with custom evaluations.*


To work as a team and get a live monitoring dashboard, try[Evidently Cloud](https://www.evidentlyai.com/register) , a collaborative AI observability platform for teams building LLM-powered products. The platform lets you run evaluations on your LLM outputs code-free. You can trace all interactions, create and manage test datasets, run evaluations, and create LLM judges from the user interface.


[Sign up for free](https://www.evidentlyai.com/register) , or schedule a[demo](https://www.evidentlyai.com/get-demo) to see Evidently Cloud in action.
