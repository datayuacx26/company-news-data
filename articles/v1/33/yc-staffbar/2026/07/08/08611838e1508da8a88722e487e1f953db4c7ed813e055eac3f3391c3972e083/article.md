---
schema_version: "1.0.0"
document_id: "08611838e1508da8a88722e487e1f953db4c7ed813e055eac3f3391c3972e083"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/an-introduction-to-apples-foundation-model-framework"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:ad6ca17c57a04b733035315adafe088607e13eab036f2afa0799fe00473e5ad8"
---

# An Introduction to Apple's Foundation Model Framework

With[W.W.D.C. 2025](https://developer.apple.com/wwdc25/) kicking off, Apple has officially taken the wraps off of several new APIs and frameworks that developers can use. One that caught my eye? Local, on-device LLM capabilities via the nascent[Foundation Models](https://developer.apple.com/documentation/foundationmodels) framework. Using it, developers can prompt Apple's large language model to generate text responses — which has all sorts of interesting use cases for apps.


In this post, I'll show you the basics of how to get started with the Foundation Models framework, and we'll even make a few calls to its API to generate some responses.


## Availability checks


Before you craft some prompts and invoke the API, you'll have to make sure a few conditions are met. For example, not *all* devices support the framework (I believe it falls under the Apple Intelligence-capable devices), it has to be enabled and there are battery considerations as well.


```text
let   systemModel   =   SystemLanguageModel.  default


guard   systemModel.isAvailable   else   {
return
}
```


The` isAvailable` property is an all-encompassing convenience check. If it returns` true` , then you're all set. If you need to know specifics, you can switch off of the` availability` property from a` SystemLanguageModel` instance:


```text
switch   systemModel.availability {
case   .  available  :
print  (  "  Ready to use.  "  )
case   .  unavailable  (  .  appleIntelligenceNotEnabled  )  :
print  (  "  Prompt user that Apple Intelligence is off.  "  )
case   .  unavailable  (  .  deviceNotEligible  )  :
print  (  "  Prompt user that device isn't capable of using Apple Intelligence.  "  )
case   .  unavailable  (  .  modelNotReady  )  :
print  (  "  Prompt user that the model is being downloaded.  "  )
case   .  unavailable  (  _  )  :
print  (  "  Unavailable.  "  )
}
```


## Model session and prompts


Once you've confirmed that the device can invoke the model, calling it can be as simple as three lines of code:


```text
let   session   =   LanguageModelSession  ()
let   prompt   =   Prompt  (  "  Tell me a knock knock joke.  "  )
let   response   =   try   await   session.  respond  (  to  : prompt  )
```


The response we get here is:


```text
Knock, knock.
Who's there?
Lettuce.
Lettuce who?
Lettuce in, it's freezing out here!
```


Using the new` #Playground` macro, you can even see the results right in Xcode without having to run your app:


```text
import   Playgrounds


#Playground   {
let   session   =   LanguageModelSession  ()
let   prompt   =   Prompt  (  "  Tell me a knock knock joke.  "  )
let   response   =   try   await   session.  respond  (  to  : prompt  )
}
```


For these "single shot" prompts, the flow consists of:


1. Creating an instance of a` LanguageModelSession` .
2. Crafting a` Prompt` instance.
3. Await a response.


## Multiturn interactions


In many cases, it would be useful for the session to keep its context of your prompts and its results. In our example, we've performed a "single-turn" interaction. In contrast, multiturn interactions are done by simply retaining an instance of the session, and using it again with follow up prompts:


```text
let   session   =   LanguageModelSession  ()
let   prompt   =   Prompt  (  "  Tell me a knock knock joke.  "  )
let   response   =   try   await   session.  respond  (  to  : prompt  )


let   followUpPrompt   =   Prompt  (  "  Tell me another knock knock joke, but make it similar to
this one:   \(  response.  content  )  "  )
let   followUpResponse   =   try   await   session.  respond  (  to  : followUpPrompt  )
```


Here, the followup response is:


```text
Of course! Here's a similar one:
Knock, knock.
Who's there?
Olive.
Olive who?
Olive you and I need to get out of this cold place!
```


As you can see, Apple's large language model took the lettuce knock knock joke and was able to come up with a new one — but still in the same vein as the original. While we're doing knock knock jokes here to show an easy demo, it's easy to envision how much opportunity this opens up to developers:


- A workout app could automatically come up with a name for a workout based on the exercises listed.
- A writing app could help an author brainstorm ideas, or overcome writer's block.
- Any app that has a rich corpus of domain-specific data could come up with insights about it on the fly.


## Crafting prompts


The term "prompt engineering" refers to the craft of composing effective prompts for a large language model, specifically suited to the task. There's a give and take at play here, too. When a prompt is too large it could result in the model taking longer than necessary to provide a result. Or, if it's too short, maybe there's not enough context to generate something useful.


Apple recommends creating prompts that are conversational. Specifically, they should aim to be a direct command, or clearly structured as a question. Further, you can provide specific preferences for the output.


In our knock knock joke scenario, we expect to receive tight responses. That's the nature of the request, but if it were more open-ended — then it would help to explicitly provide the length you're after. For example:


"Come up with a good joke about a baker whose latest attempt at a wedding cake went wrong" versus "Come up with a good joke about a baker whose latest attempt at a wedding cake went wrong. Keep it to one paragraph or less."


## Instructions


When you create a model session, you can also pass it instructions. When they are present, they take precedence over the prompt itself. Instructions can be useful to point the model towards a specific goal, persona or general task that it should perform.


To write effective instructions, you might provide answers to one, or all, of these questions:


- What should the model be doing?
- What role is it specifically responsible for?
- What kind of preferences should it consider in the response?


You can also provide example responses in instructions to help guide its output. For our scenario, we could try something like this:


```text
let   instructions   =   Instructions  (  """
You are a comedian coming up with knock knock jokes, and each
one works in some type of food. Your jokes aren't longer
than a few sentences, and always follow the classic knock
knock joke structure.


I'll tell you a food, or genre of food, and you'll create the
knock knock joke using it. For example, for lettuce — your
response could be:


Knock knock.
Who's there?
Lettuce.
Lettuce who?
Lettuce in, it's freezing out here!
"""  )
let   session   =   LanguageModelSession  (  instructions  : instructions  )
let   prompt   =   Prompt  (  "  Cheese.  "  )
let   response   =   try   await   session.  respond  (  to  : prompt  )
```


Here, the response I first received was:


```text
Knock knock.
Who's there?
Cheese.
Cheese who?
Cheese move, I'm coming through!
```


Even though we only provided one word in the` Prompt` instance, the model knew what to do because of the instructions we provided it.


## Just the start


The Foundation Models framework has a lot packed into it. While it's early days, there's even more it can do and we've only covered the basics here. Here are some highlights I've seen from reading over the docs:


- Use` GenerationOptions` to modify the temperature, which helps you steer the response in more predictable or creative directions.
- A streaming response type, so you can show the response as it generates in your interface. This is how most LLM apps work today, like chatGPT. You ask it something, and the text is gradually written as the response is formed.
- There's a flavor of structured outputs that developers can create using a Swift macro.
- Safety guardrails built into the API that you can provide.
- Tool calling API to let the model invoke app-specific logic that would help it construct responses.
- And, hydrating responses with a previous transcript.


## Wrapping up


W.W.D.C. is always one of our favorite times of the year over at Superwall. Like many of you, building with the latest and greatest APIs is an annual tradition for all of us. While Apple continues to deliver some of the best APIs around to build compelling apps, we'll keep working to make sure you can monetize anything you ship to the App Store and beyond.


Get started with a free[Superwall](https://superwall.com/sign-up) account to get started with testing paywalls, robust analytics and more. This W.W.D.C., be sure to keep it locked to our blog. As I discover more APIs new this year, I'll be sure to share about them here.
