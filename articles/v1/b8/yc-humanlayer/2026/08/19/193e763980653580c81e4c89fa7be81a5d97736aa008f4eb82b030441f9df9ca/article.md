---
schema_version: "1.0.0"
document_id: "193e763980653580c81e4c89fa7be81a5d97736aa008f4eb82b030441f9df9ca"
company_key: "yc-humanlayer"
company: "HumanLayer"
source_id: "yc-humanlayer-news-import-a7d3053d1b06"
canonical_url: "https://www.humanlayer.dev/blog/show-me-skill"
published_at: null
first_seen_at: "2026-08-13T04:00:05.256176+00:00"
fetched_at: "2026-08-13T04:00:06.423776+00:00"
content_hash: "sha256:ab5791b31cc66c08bae9d9f726c6b61dafa66d3906fd9ac5ebd093444ddef6bc"
---

# show-me: a coding agent skill for compact visual representations

tl;dr make your agent converse visually instead of in walls of prose.


```text
npx skills   add   humanlayer/skills --skill show-me
```


Lighter and faster than HTML, good enough for most dev-work shaped problems.


### ### Coding agents are pretty much unreadable


The former CEO of reddit:


[https://x.com/yishan/status/2086534431075098628](https://x.com/yishan/status/2086534431075098628)


[Mario Zechner](https://x.com/badlogicgames) , creator of pi:


[https://x.com/badlogicgames/status/2087068077309542889](https://x.com/badlogicgames/status/2087068077309542889)


Connor from Replicas:


[https://x.com/connortbot/status/2081881377147109413](https://x.com/connortbot/status/2081881377147109413)


[Dillon Mulroy](https://x.com/dillon_mulroy) even made a skill,` /bro` to ask the model to simplify language.


[https://x.com/dillon_mulroy/status/2079238358358778142](https://x.com/dillon_mulroy/status/2079238358358778142)


The contents are:


```text
Restate your last message. Stop using jargon and speak coherently.
State it more simply and concisely, like one human talking to another.
```


### ### i am so sick of this


agents got more intelligent on paper, but the experience of using them got noticeably worse along this dimension.


the thing people used to love about claude - its voice, it's personality, its "soul" has been flushed out in the RL dungeon


sol is somewhat less cringe but still regularly hits us with walls of jargon that make eyes glaze over


here's a response i got recently. this happens multiple times a day


### ### my proposal: show me


We've been playing with internal tools to make this better, specifically for coding, and publishing them in a skill we call` show-me` .


It's live in humanlayer today, and if you want it in any other coding agent - you can get it here:


```text
npx skills   add   humanlayer/skills --skill show-me
```


If you ever watched[coda hale's talk on intuition vs. attention in infrastructure systems](https://www.youtube.com/watch?v=e_6gkfTomUQ) , it's somewhat inspired:


- analyzing information is hard and exhausting
- your visual cortex was trained over millions of years to process rich visual information effortlessly
- optimize tools accordingly


> Just as an axe must fit the human hand to be useful, software must fit the human mind to be useful


` /show-me` prompts the agent to use concise visuals to explain what's happening, instead of walls of prose.


This is really good for[program design](https://hlyr.dev/wsff-gh#program-design) - the phase many folks skip these days, but that I think is essential. You should be discussing the shape of the code (the types, the signatures, the call stacks) before agents get to work on writing it.


The same techniques can also be used to explore large diffs post-hoc to understand what to dig into during review.


### ### What's inside


#### #### component trees


Same idea on the frontend, with the state hooks and module boundaries that matter kept in and everything else left out.


```text
<  SessionPage  >   (apps/example/src/routes/session.tsx)
useSessionEvents()
<  SessionToolbar  >
<  RunSkillButton  >   (packages/ui)
```


I shared this one on twitter back in december:


[https://x.com/dexhorthy/status/1998968236617199803](https://x.com/dexhorthy/status/1998968236617199803)


#### #### call stacks


For orchestration or control-flow work, or just any backend-shaped problem, dillon gave us this "call stack" shape.


```text
handleCreateSession
validateRequest
SessionStore.insert
publish(session.created)
AgentWorker.run
loadContext
callModel
persistResult
```


[https://x.com/dillon_mulroy/status/2059985696148849025](https://x.com/dillon_mulroy/status/2059985696148849025)


[tanish](https://x.com/tanishqk) even wrote a tool to compute them straight from the AST


[https://x.com/tanishqk/status/2085800689129935342](https://x.com/tanishqk/status/2085800689129935342)


#### #### diagrams


A classic. If your chat interface supports inline mermaid, these can help a lot. (Sometimes they're still slop, but it's usually better than reading words)


```text


```


Lots of options here. we like state diagrams and sequence diagrams the most.


```text


```


#### #### file layouts


A shallow file tree, one line of responsibility per entry. Good for "where does this live" and for scoping a refactor.


```text
src/
├── commands/           # parses user actions into intents
│   ├── registry.ts     # name -> handler, one place to add a command
│   └── show-me.ts      # expands the slash command
├── sessions/           # owns session state and lifecycle
│   ├── store.ts        # SessionStore - insert / list / open
│   ├── worker.ts       # AgentWorker.run - load context, call model
│   └── events.ts       # publishes session.created and friends
├── transport/          # talks to the API, knows nothing about sessions
│   ├── client.ts       # request / response mapping
│   └── stream.ts       # SSE decoding, reconnect, backoff
└── config.ts           # env + feature flags
```


#### #### pseudocode


Especially for algorithmic stuff, pseudocode can be more concise.


```text
capture  (  )                                       // only if this surface owns focus
target   =   keyboard focus
?   focusedBlock
:   firstBlockIntersectingViewportTop
anchor   =     wholeBlockAnchor  (  blocks  ,   target  )      // type + text + neighbor text
offset   =   targetRect  .  top   -   scrollRect  .  top     // signed: place within tall block
publish  (  {   anchor  ,   offset  ,   scrollTop  ,   revision   +     1     }  )


restore  (  snapshot  )                               // after editor + blocks exist
placement   =     resolveAnchor  (  blocks  ,   snapshot  .  anchor  )
exact text match
-  >   normalized  -  whitespace match  ,   scored by neighbor context
-  >     'outdated'
if   resolved
scrollTop   +=   targetRect  .  top   -   scrollRect  .  top   -   snapshot  .  offset
else
scrollTop   =   snapshot  .  scrollTop             // failure path
```


#### #### types and signatures


The shape of the code before any of it exists - the stuff that's too internal for an architecture doc but that an agent can still get wrong.


```text
interface     Item     {
id  :   ItemId
parentId  :   ItemId   |     null
// ...
}


interface     Cursor     {
position  :   ItemId
direction  :     'up'     |     'down'
// ...
}


resolveTarget  (  items  :   Item  [  ]  ,   cursor  :   Cursor  )     -  >   ItemId   |     null
```


#### #### diff syntax


You can also use diff syntax for this, if most of the content is unchanged:


For a component change:


```text
<SessionPage>
useSessionEvents()
<SessionToolbar>
+      <RunSkillButton />
<SessionTimeline>
+      <SkillResultCard />
```


For a call-tree change:


```text
handleCreateSession
validateRequest
+    enforceQuota
SessionStore.insert
publish(session.created)
AgentWorker.run
loadContext
+          fetchPriorTurns
callModel
persistResult
+          emitUsageEvent
```


For a file-layout change:


```text
src/
├── commands/
+  │   └── show-me.ts       # expands the slash command
├── sessions/
-  └── transport.ts
+  └── transport/
+      ├── client.ts
+      └── stream.ts
```


And for a state or control-flow change, where the shape is pseudocode rather than real code:


```text
on(save)
-    write content
+    if content is unchanged
+      return cached result
+    write new content
+    invalidate cache
```


#### #### html mockups


HTML has replaced figma for a lot of our prototyping work. (tbh i never really was super handy with figma anyway)


#### #### html diagrams


Sometimes a diagram or explainer is what you need.


In[humanlayer](https://humanlayer.com/) we let the agent include HTML directly in assistant responses.


But you can also just open it in your browser.


### ### other inspiration


I also wanna hat tip[Matt Pocock](https://x.com/mattpocockuk) for the html explainers that are generated by his` /teach` skill - very good.


## ## go try it


```text
npx skills   add   humanlayer/skills --skill show-me
```


After installing the skill, invoke` /show-me` or ask the agent to use the` show-me` skill. Point it at a route, service, feature, pull request, or current topic, or just use it to ask the model to restate a question or statement.


```text
this is too much content. show me.
```


or


```text
/show-me as an html explainer
```


Let us know what you think! Tag @humanlayer_dev or @dexhorthy on twitter with your results or what you customized/added and let's riff!
