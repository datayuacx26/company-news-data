---
schema_version: "1.0.0"
document_id: "5cfee64b0d6ac2651df4d19728f41b16a47bf296b30343fd1e05966a14c391e9"
company_key: "yc-bolna-ai"
company: "Bolna AI"
source_id: "yc-bolna-ai-rss-f48b573fe12c"
canonical_url: "https://blog.bolna.ai/setting-up-pre-call-journey/"
published_at: "2025-11-19T02:39:34+00:00"
first_seen_at: "2026-07-26T23:55:31.875103+00:00"
fetched_at: "2026-07-28T20:55:16.082008+00:00"
content_hash: "sha256:e1a04f32be858c9833884b265b5e73a883f80057d93cd06f903c8d952a42d38f"
---

# Setting Up Pre-Call journey in Voice AI: Personalize Every Call Before It Starts

Before a call even begins, there is a small window where the experience can be shaped. This moment decides whether your Voice AI sounds like a generic bot or a thoughtful assistant that already understands who it is speaking to. This is where the idea of a Pre Call Journey comes in. It prepares your agent with the context it needs so conversations feel smoother, more personal and far more human. Instead of waiting for the caller to explain everything, your AI starts with the right information at the right time.


On[Bolna](https://www.bolna.ai/) , this preparation can come from many places such as your CRM, a simple CSV, a Google Sheet or an internal API. The goal is the same. Give your agent the context it needs before the first hello.


In this article, let us explore how this preparation comes together and how you can use it to build more intuitive and efficient experiences powered by voice AI and clean crm sync before call workflows.


## Table of Contents


## **What is Pre-Call Personalization?**


Pre-call personalization is the step where your voice AI prepares for a conversation before the call even connects. Instead of waiting to “learn” who the caller is during the call, the system quietly pulls in the right data ahead of time and keeps it ready as variables. Platforms like[Bolna](https://www.bolna.ai/) use this to make every interaction feel more intentional and human.


Bolna Pre-Call Personalization Flow


This preparation can include fetching a customer’s name, plan details, last transaction, pending dues, or any context your workflow needs. All of this information becomes part of the AI’s internal state, so the agent can speak naturally and respond with confidence. These variables are then woven into your agent’s script or prompt, which is what makes pre-call journey setups so effective for ivr automation setup and other voice workflows that rely on accuracy from the first word.


For example, if your CRM indicates the customer is *Ravi Kumar* , and his March payment is pending, your AI might begin with:


> “Hi Ravi, I noticed your loan payment for March is still pending. Would you like to clear it now?”


This happens without manual input because` {name}` and` {due_month}` were automatically collected before the call began. The result is smoother, more personal conversations that set the tone for better engagement.


## **Dynamic Script Generation**


Dynamic script generation is what makes your[Voice AI](https://blog.bolna.ai/recruiting-tools-in-2025/) sound natural instead of robotic. Once the pre-call journey loads the variables, Bolna uses them to shape the exact lines your agent will speak. You write your script with simple placeholders such as` {name}` ,` {plan}` ,` {due_amount}` , and the system fills them in automatically before the conversation begins.\\


This flexibility is at the heart of modern pre-call journey setups and keeps your voice experience fluid even when the data varies from person to person. It also helps your ivr automation setup feel far more intelligent because the script adjusts itself rather than forcing the same line for every caller.


Here is a quick example of how a prompt might look once variables are injected:


```text
"Hello {name}, your subscription for {plan} is expiring on {expiry_date}.
Would you like me to renew it for you?"
```


If a value like` {expiry_date}` or` {plan}` is missing, Bolna naturally rephrases or removes that part so the agent never says anything incomplete or awkward. The result is a smooth and personalized response for every customer without the need to hardcode dozens of script variations.


This approach also sets the foundation for deeper customer data prefill ai calls, letting your application adapt to whatever information your CRM provides at the moment the call begins.


## **Outbound Call Personalization**


Outbound journeys let you prepare every call with the right context before the AI even speaks. Bolna supports multiple ways to feed customer data into your workflow, so your voice AI can open the conversation with accurate details instead of generic scripts.


This preparation helps with everything from pre-call journey ivr automation setup to crm sync before call, making it easy to run highly personalized outbound campaigns.


### **a) Individual Calling (Manual Input)**


Individual mode is ideal when you are testing your voice AI or running very small batches of calls. You enter the variables manually inside the Bolna dashboard and the system uses them instantly during the call. It is a straightforward way to check whether your customer data prefill ai calls logic is working as expected without dealing with files or integrations.


Example setup on the dashboard:


```text
{name} = Ravi
{plan} = Gold
{expiry_date} = 10th March
```


When the call begins,[Bolna](https://platform.bolna.ai/) replaces those placeholders inside your prompt, so your agent speaks naturally with the right information. It is also a useful mode when you want to troubleshoot how the AI handles missing or partially filled values.


### **b) Batch Calling (CSV Upload)**


Batch calling is designed for campaigns where you might have hundreds or thousands of customers to contact. You upload a CSV that contains every variable you want to use in your script. Bolna reads each row and prepares a personalized call for that customer.


Column names must match your variable names exactly. This ensures clean mapping and avoids confusion during large-scale automation. If a field is empty, your voice AI adjusts its output so it does not attempt to read blank values. This makes it safe to run campaigns even when your data is imperfect, which is common in pre-call journey ivr automation setup flows.


Example CSV:


phone_number name plan expiry_date


9876543210 Ravi Gold 10 March


8765432109 Priya Platinum 18 March


Bolna automatically turns each row into its own personalized call environment. You get consistency at scale without writing any custom logic.


### **c) API Calling (CRM or Database Integration)**


API-based calling is the most automated and flexible option. This is where your crm sync before call workflow truly shines. Instead of uploading files or entering data manually, your backend passes the customer’s context directly into Bolna using the[Make Call API](https://www.bolna.ai/docs/api-reference/calls/make?playground=open) . You can trigger calls instantly when an event occurs in your system such as a new lead, a payment reminder, a missed delivery, or a support follow-up.


Example request:


```text
{
"to": "+919876543210",
"agent_id": "123e4567-e89b-12d3-a456-426614174000",
"user_data": {
"name": "Ravi Kumar",
"plan": "Gold",
"expiry_date": "10 March",
"due_amount": "₹1,299"
}
}
```


Bolna receives the data in real time and injects it into the script before the call begins. This method is ideal when you want complete automation, especially for time-sensitive workflows where customer context can change quickly.


## **Inbound Call Personalization**


Inbound calls get much more powerful when your system can understand who is calling and what context they carry before the first word is spoken. This is where pre-call journey setups help your voice AI greet callers naturally and resolve queries faster. Bolna supports multiple ways to preload details so your IVR automation setup feels smart instead of repetitive, and every conversation starts with the right context.


### **a) CSV Upload**


A simple way to personalize inbound experiences is by uploading a CSV that maps phone numbers to all the variables you want your agent to reference. This works well for businesses that maintain internal lists or run campaigns where customer data does not change frequently.


Example table:


phone_number name plan due_amount


9876543210 Ravi Gold ₹1,299


8765432109 Priya Platinum ₹2,499


When a call arrives at your number, Bolna checks if that phone number exists in your sheet and immediately loads values into the call session. This gives your agent prefilled context, allowing it to speak more personally without needing to ask unnecessary questions.


If someone calls from` 9876543210` , your voice AI automatically receives:


```text
{name} = Ravi
{plan} = Gold
{due_amount} = ₹1,299


```


This keeps your customer data prefill AI calls smooth and responsive.


### **b) API Connection**


For businesses with live CRMs or constantly changing records, syncing data through API is the most reliable approach. Each time someone calls, Bolna triggers your CRM or internal service to fetch up-to-date information. This ensures the agent always works with the latest status, tickets, balances, or preferences.


Example API response:


```text
{
"name": "Priya Sharma",
"account_status": "Active",
"last_ticket": "Resolved"
}


```


With that context loaded before the call begins, your AI can respond with clarity and relevance:


> “Hi Priya, I can see your previous support request was resolved. Are you facing something similar again?”


This keeps your CRM sync before call workflows efficient and prevents your callers from repeating information they have already provided elsewhere.


### **c) Public Google Sheet**


For lighter or no-code setups, a public Google Sheet works surprisingly well. You structure it just like a CSV, publish it, and paste the link in your inbound configuration. Whenever a call comes in, Bolna fetches the matching row in real time and fills those variables into the conversation.


This option is perfect for teams that update records frequently but don’t want to manage APIs or backend services. It keeps your voice AI agile while still maintaining a personalized, prefilled experience for every caller.


## **Additional Pre-Call Controls**


Before a call begins, it’s not just about loading variables. You also need the right guardrails so your voice AI workflows stay efficient, secure, and predictable. Bolna includes a few helpful controls that sit in the pre-call layer and give you more oversight on how your ivr automation setup behaves across inbound and outbound traffic.


**Limit inbound access**
You can choose to accept calls only from numbers that exist in your uploaded CSV, Google Sheet, or synced CRM. This reduces unwanted traffic, blocks spam, and ensures your customer data prefill ai calls run on verified information rather than random inputs.


**Max calls per number**
Set daily or hourly limits on how many times a single number can reach your system. This helps you avoid accidental loops, misuse, or repeated retries that burn through call credits.


**Bypass list**
If you have internal testers, QA teams, or sandbox numbers, you can allow them to skip limits entirely. This makes it easier to test new workflows without affecting real customers or triggering caps meant for production callers.


These controls give you a clean pre-call layer where your voice AI can operate smoothly, with accurate data coming in and unnecessary noise filtered out.


## **JSON Examples**


**Example 1: Outbound Batch Call Setup (CSV → JSON Conversion)**


When you upload a CSV for large outbound campaigns, Bolna converts each row into a structured JSON payload behind the scenes. This is how variables are passed into the Voice AI so it can use them during dynamic script generation and pre-call workflow processing.


```text
[
{
"phone_number": "+919876543210",
"name": "Ravi",
"plan": "Gold",
"expiry_date": "10 March"
},
{
"phone_number": "+918765432109",
"name": "Priya",
"plan": "Platinum",
"expiry_date": "18 March"
}
]
```


Each object represents an individual call. Bolna reads the keys as variable names and injects them directly into your script, ensuring the *ivr automation setup* stays consistent across large volumes of outbound calls.


---


**Example 2: Inbound Data Mapping Response**


For inbound flows, Bolna identifies the caller and fetches their details from your chosen source — CSV, Google Sheet, or CRM API. Once the data is mapped, the platform formats it internally like this:


```text
{
"caller_number": "+919876543210",
"context": {
"name": "Ravi Kumar",
"plan": "Gold",
"due_amount": "₹1,299",
"account_status": "Active"
}
}
```


This` context` object is what powers customer data prefill ai calls on inbound journeys. It ensures your Voice AI knows who is calling and can greet them naturally without repeating verification steps or generic questions.


## **Real Use Case Example**


A common scenario where pre-call journey setups shine is subscription renewals. Teams often need their Voice AI to remind customers about upcoming expiries and handle the entire interaction end-to-end without manual effort. With Bolna, this becomes a smooth mix of IVR automation setup and CRM sync before call execution.


Imagine you have a list of users whose plans expire within the next week. The moment a call is triggered, Bolna pulls in details like name, plan type, expiry date, and any pending amount. These values are prefilled as variables so the AI can speak in a way that feels personal instead of generic.


Your script might open with something like:


> “Hi {name}, I noticed your {plan} plan is coming up for renewal on {expiry_date}. Would you like help renewing it right now?”


If the CRM doesn’t return a due amount or the customer doesn’t have one, the AI simply avoids that part of the sentence. This makes the experience feel intentional and natural rather than mechanical. And because everything is handled through pre-call data loading, your team doesn’t have to manage context manually for each customer.


This same pattern applies to other situations as well, such as loan reminders, delivery confirmations, appointment follow-ups, and support callbacks. Any workflow where customer data prefill ai calls improves speed or clarity becomes much easier to automate when Bolna handles the preparation before the call begins.


## **Benefits of Pre-Call Journeys**


Pre-call journeys make your voice experiences sharper and more reliable. By preparing the right information before a call begins, your workflows get smarter and your agents sound far more intentional. Here are five benefits that standout when you use Bolna’s system.


- Your voice AI starts each conversation with the right context, which makes the call feel more natural and reduces confusion for the caller.
- Customer data is already in place before the first word is spoken, so your IVR automation setup becomes faster and more reliable.
- Teams save time because variables from your CRM sync before each call, removing the need for manual lookups or repeated customer inputs.
- Campaigns become far more efficient since customer data prefill AI calls can scale across outbound and inbound flows without extra effort.
- Personalization improves conversion and satisfaction because callers feel understood from the very beginning of the interaction.


## Conclusion


Pre-call journeys turn a basic Voice AI into something far more empathetic and capable. When your system already knows who is calling, why they are calling, and what context matters, the conversation immediately feels more human. With the right setup, your IVR automation becomes faster, your customer wait times shrink, and every interaction starts with clarity instead of confusion.


Bolna makes this preparation feel natural by connecting your CRM sync before call, pulling the right context, and letting your agent speak with confidence from the very first second. Whether the data comes from a CSV, an API, or a live sheet, your voice workflows stay consistent and predictable across both outbound and inbound experiences.


Give your customers the experience of being understood in their own language at[platform.bolna.ai .](https://platform.bolna.ai/)


## Frequently Asked Questions


### What happens if a variable is blank?


Bolna quietly skips it so your voice AI never reads out empty values. The line is naturally rephrased to keep the conversation smooth.


### Can I use both API and CSV together for pre-call workflow AI?


Yes. CSV can serve as a reliable fallback when your CRM sync before call is slow, unavailable, or missing data.


### **How do I test whether my variables are mapped correctly?**


You can run quick checks through the[Bolna platform](https://platform.bolna.ai/) under Outbound or Inbound Testing. The logs show exactly which fields were prefilled before the call.


### Can variables change during the conversation?


They can. Bolna supports real-time updates through function calling and API hooks, which allows the agent to adapt mid-call when new information appears.


### Does pre-call personalization work for multilingual callers?


Yes. Bolna can personalize and deliver responses across languages as long as the data is provided in your incoming source.
