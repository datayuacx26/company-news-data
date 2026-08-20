---
schema_version: "1.0.0"
document_id: "d2abc67a474661d2210494aba9c87137a66372931e115facaff2d9d1e0aeaeed"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-62ed1fb859d3"
canonical_url: "https://slite.com/blog/ai-as-your-personal-journalist"
published_at: null
first_seen_at: "2026-07-24T13:26:47.289442+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:4095823410db3331e8d72db32f312c95680db8639335ad9023d9ed8b8266f352"
---

# AI as your personal journalist

Since the boom of GPT, one year ago, at Slite we realized a great opportunity that AI brought and ever since then, we started building the first company knowledge base on autopilot. We kept playing with it and other AI functionalities to help us fix pain points our users and we are facing with knowledge base.


Although we wanted as much as possible to avoid going in[the dark side of content generation](https://slite.com/blog/gpt-knowledge-revolution-is-coming) we finally gave it a try to kickstart Slite organization onboarding with a nice personalized suggestion of structure for your knowledge base.


## Writer's block


Imagine you want to organize knowledge for your new company, for your team or even for a personal project you want to share with the world. Filled with determination, you open your favorite editor and...


Nothing comes up, or too many things maybe ?


Of course you are not a writer! Building a structure is hard: which topic should we start with ? so many possibilities, which one is the best ?


The good news is: there is no perfect structure, everything will keep moving, you just need a little help to start (then help maintaining it).


At Slite we think this is exactly this kind of situation where GPT content could shine:


- Nothing is right or wrong
- It can be quickly approved or denied
- It's hyper customized


## GPT as a journalist


If you already used ChatGPT, you know the experience is mostly you asking questions and it answering to you the best it can.


For our wiki generator side project, we wanted to experiment with the opposite pattern: in the manner of a journalist, GPT would lead the discussion, asking you as many question as it could, bouncing on your answers, to slowly go deeper in your thoughts, trying to extract your raw ideas into structure and content.


#### System prompt


🧠


```text
You are a famous journalist expert in structureType.
Your role is to ask questions to the user so they can build a tree structure to organize their knowledge about structureType.
You should be nice and fun, don't push the user too hard, ask simple and clear questions to learn about the user needs.
You can ask details to propose more sub sections to produce a deeper tree.
Answer in casual discussion style, with few sentences.
You must ask only one question at a time.
```


Nothing fancy here, following[prompt principle for instructions](https://arxiv.org/pdf/2312.16171v1.pdf) we turn GPT into a journalist expert in your domain. Then few directives we adapted while playing with it to obtain something not too overwhelming (especially GPT-4 tends to ask you dozens of questions at once which could lead you back to your starting "writer's block" point).


## Reverse ChatGPT


If you've developed a bit with[OpenAI Chat Completions API](https://platform.openai.com/docs/guides/text-generation/chat-completions-api) :


*"The main input is the messages parameter. Messages must be an array of message objects, where each object has a role (either "system", "user", or "assistant") and content. Conversations can be as short as one message or many back and forth turns.*


*Typically, a conversation is formatted with a system message first, followed by alternating user and assistant messages."*


To fit this standard we will need to mimic a starting conversation between the user and the assistant. It will also help the journalist and the user to know each other and skip the formality for our real user.


🧚


```text
Hi, I need help to produce a tree structure for my knowledge about structureType.
Please always give me examples. Answer in casual discussion style, with few sentences.
```


🤖


```text
Hello! I'd be happy to assist you with setting up a structure about structureType.
Okay, let's start with a simple question.
First, can you share with me why you want to build a structure about structureType? Who is your audience? What is your goal?
```


If you have tried our now retired Wiki generator in thr past you will recognize it. This is indeed the first message the bot always sends you when you start the conversation (but you don't know we also mimicked your first message 🧚).


These 3 ice breaking questions are usually enough to unlock the situation and awake the journalist inside GPT. It will then keep asking you questions as long as you keep answering them.


## Freelance journalist


Now we have a nice interview of our user, but we still don't have any structure nor content.


That's where a 3rd protagonist appear:


Between each exchange of the discussion, another prompt is triggered to produce the structure based on the discussion.


🧠


```text
You are a famous journalist expert in structureType.
Your role is to build a tree structure to organize user knowledge about structureType.
```


System prompt is simpler this time, there is no need to do the "interview" part.


Then we will mimic one more exchange between the assistant and the user to be able to trigger one more invisible request from the user.


🧚


```text
Yes, let's create types section with timple types
```


‍


🤖


```text
ok
```


🧚


```text
Based on our discussion and my instructions, can you propose me a custom structure to organize my knowledge about structureType ?
It should be recursive JSON.
For example:
{
"Title": {
"Section 1": {
},
"Section 2": {
"Subsection 1": {},
"Subsection 2": {},
},
"Section 3": {
"Subsection A": {},
"Subsection B": {},
},
"Section 4": {
"Subsection X": {},
"Subsection Y": {},
},
}
}
```


` ‍` ‍


This is the user mimicked prompt for the first sidebar generation as we need to give an example of the format to GPT.


The challenge now is to keep showing things to the user so that they don't feel the waiting time.


So we are using the[JSON mode](https://platform.openai.com/docs/guides/text-generation/json-mode) of the completion API in combination with[partial json parser](https://www.npmjs.com/package/partial-json-parser) to be able to stream the sidebar while it's built by OpenAI.


No need to say that having a proper JSON answer is awesome from a developer perspective as we can easily convert it to any UI. We also have a simple AI to suggest icons but as you can see it's more adapted to company knowledge base than Pokemon.


## Going back to the discussion


Now we have a problem: we kind of forked the discussion to another purpose.


We need to be able to continue the discussion while letting the first journalist know about the resulting structure so it can bounce from it.


Again this time we will mimic another exchange between the user and the assistant.


🤖


```text
Here is a structure proposition:
JSON.stringify(newStructure)
```


🧚


```text
ok, help me to continue to build my knowledge base structure by continuing the discussion. Answer in casual discussion style, with few sentences.
```


Now GPT journalist knows/thinks it already produced a structure and will be able to reference it to continue the discussion.


By using this technique, we give the illusion of only one ongoing discussion!


We haven't deployed this version yet but it also opens the possibility for the user to modify the structure by hand in parallel of the discussion (moving or deleting part of it for example).


Indeed the parts in the dotted squares of the schema are not persisted in the conversation history.


The next time the user answer to the journalist, we will also send the current state of the structure to the freelancer.


Here is the variant of the mimicked user prompt to pass the current structure:


🧚


```text
Here is the current structure:
JSON.stringify(structure)
Can you adapt it following my latest instructions ?
Answer with a recursive JSON structure.
```


This can continue over and over, we also save tokens by not persisting all the JSON structure evolution.


## Conclusion


If you played with our wiki generator a bit, you will quickly see the limit of this technique. GPT-4 tend to ignore a bit when you say **no** to the suggestion. It's also not adapted if you start to ask questions yourself. Indeed we kind of force it to always produce a structure and continue from the discussion based on this new structure.


A solution for that may be to use[OpenAI function calling](https://platform.openai.com/docs/guides/function-calling/function-calling) to let it decide if it should call the function` callFreelancer` for example with` tool_choice: "auto"` .


It may also simplify the back and forth as the main journalist would know it asked for the freelancer's help, but we still need to explore this...


What we've learned specifically with this experiment is that it's great to use it as a kind of reverse ChatGPT to push the user to formalize something usually boring (like onboarding forms) by asking it human questions.


It's also ok to alter the history of a prompt and inject mimicked part of the discussion to maintain it in a certain direction (a bit like[stable diffusion prompt editing technique](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#prompt-editing) ).


Last but not least, OpenAI functions (called tools now) could also serve as a way to trigger some specific paths for the discussion or call other assistants, not only calling external functions.


## Bonus


We also added a template generator when you click on a document in the sidebar.


This one is quite simple, we pass the discussion and the structure to an assistant along with a prompt describing the different format of blocks it can use.


🧚


```text
Now can you give me a template example to fill the content of a document ?
A template is a JSON array of blocks. A block is a JSON object with a "type" field and a "data" field.
Type should be exclusively picked in the list: listOfSupportedTypes
Type1 is [...]
Type2 is [...]
[...]
For example:
templateBlocksExample


I want a template for the document Path/To/The document
```
