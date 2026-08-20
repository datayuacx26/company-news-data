---
schema_version: "1.0.0"
document_id: "bb92d5b2c6cbc6f4e82cdd580ec563f27e9dde5b909e0496d83fcea4260339ba"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/build-a-personalized-ai-tutor-with-persistent-memory"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:6e5ee512d3ae60522ba4da94b92c048f3708f1e4f9cd3dcaf98a7e7ef5719782"
---

# Build a Personalized AI Tutor with Persistent Memory

A student mastered recursion three sessions ago. Your AI just re-taught it. No memory means no learner model, just a loop.


AI tutors that reset every session, the student re-explains their learning style every single session. So, you have an expensive chatbot, not a personalized AI tutor.


## **Quick Takeaways**


-


A stateless AI tutor resets every session, forcing students to re-establish context that a human tutor would never forget.


-


Dumping the full chat history into the context window incurs tokens linearly and breaks across devices or app restarts.


-


[Mem0](https://mem0.ai/) extracts semantic facts rather than storing raw logs, so retrieval is precise rather than verbose.


-


One Mem0 instance handles unlimited students via` user_id` scoping, with no per-student backend infrastructure required.


-


Adding` agent_id` separates memory by subject within the same student profile, eliminating cross-contamination between, say, math and CS tutoring.


Here is what that looks like in practice:


```text


```


```text


```


```text


```


This article walks you through one Streamlit app, roughly 100 lines of core Python logic, that wires Mem0 to any OpenAI-compatible LLM and produces the second response above.


Here is a glimpse of what you'll build👇


Please enter a valid YouTube, Vimeo, or direct video URL


The EdTech industry has been shipping personalization for years: a vanilla LLM with no memory layer, marketed as adaptive learning. The fix is not a better model. It is a missing storage layer.


A human tutor's memory is what you are actually paying for when you hire one. Not their knowledge of recursion -- you can get that from a textbook. What you are paying for is their mental model of *this student* , built session by session. Your AI tutor has the same raw knowledge as the model. That is not an LLM limitation. That is a missing storage layer.


## **Why context-stuffing is not the answer?**


The obvious workaround is to dump previous session transcripts into the context window at the start of each new session. Developers reach for this because it requires no new infrastructure -- just pass more history. But context-stuffing does not produce personalization. It produces recall. Dumping 40,000 tokens of chat history does not mean the model understands the learner; it means it can reference what was said. A system prompt with 5 extracted facts outperforms 40,000 tokens of raw history on adaptation quality, not just cost.


More practically, the approach breaks entirely when a student switches devices, clears their browser, or logs in from a new environment. Token cost scales linearly with sessions, and there is no persistent store backing the context. You have moved the problem around, not solved it.


## **How does Mem0's persistent memory work?**


Raw logs are a retrieval problem disguised as a storage solution. Storing "User said: I don't get base cases" gives you a timestamp and a quote. It tells you nothing about what the student currently knows, how that knowledge has changed, or what to do next session. Mem0 stores the extracted fact -- "student struggles with recursion base cases" -- as a discrete, queryable object. When the student later says they have finally got it, Mem0 does not append a contradiction. It updates the fact.


The extraction happens automatically. You call` mem0_client.add()` with a conversation, and Mem0 runs its own pipeline to pull semantic facts from the exchange. No prompt engineering required on your end.


### ` **user_id**` **scoping: one instance, many students**


Every memory write and retrieval is namespaced to a` user_id` . Your single Mem0 client instance handles every student in your app. No per-student backend or custom auth layer separating their data.


```text
# Writes facts extracted from this conversation to student_123's namespace
mem0_client  . add  (  messages  ,    user_id  = "student_123"  )


# Retrieves only facts stored under student_123 -- no other student's data bleeds in
mem0_client  . search  (  query  ,    filters  = {  "user_id"  :  "student_123"  }  )
```


```text
# Writes facts extracted from this conversation to student_123's namespace
mem0_client  . add  (  messages  ,    user_id  = "student_123"  )


```


```text
# Writes facts extracted from this conversation to student_123's namespace
mem0_client  . add  (  messages  ,    user_id  = "student_123"  )


```


This means you can onboard your 1,000th student without changing a single line of infrastructure code. The scoping is handled entirely at the Mem0 layer.


### **How search() shapes the next response**


Before every LLM call, your app runs a` search()` against Mem0 using the student's latest message as the query. Mem0 returns the top 5 most semantically relevant facts about that student. Those facts are injected directly into the system prompt. This is how Session 2 produces "I know base cases tripped you up last time" instead of a textbook definition -- Mem0 stored that fact at the end of Session 1.


The LLM then generates a response that is already calibrated to the learner. It knows their style and their gaps. It also knows what to skip.


## **Demo: Building the personalized AI tutor**


The demo runs across three sessions. Each session clears the chat history to prove that adaptation is not coming from the context window.


> [👉Get a free API key at app.mem0.ai](https://app.mem0.ai/) **to follow along.**


Session 1 is a trap -- and that is intentional. Both columns look identical. The student gets the same generic recursion explanation from both sides. If you showed this to a skeptic, they would close the tab. But Mem0 runs its extraction pipeline in the background. So, the divergence does not happen in Session 1. It happens *because* of Session 1.


### **What changes between sessions: the core argument**


Before seeing the full code, it is worth understanding exactly what Mem0 changes. The only thing that changes between a generic response and an adapted one is the system prompt. Here is what both sides look like:


```text
# WITHOUT MEM0 -- every session, every student, no variation
system   =  "You are a generic AI programming tutor. Give a standard textbook explanation."


# WITH MEM0 -- Session 2 onwards, the full MEM0_SYSTEM preamble plus injected facts:
# "You are a patient AI programming tutor. You MUST visibly adapt your response based
#  on the memory context provided..."
#
# What you already know about this student from past sessions:
# - Student struggles with recursion base cases
# - Student is a visual learner, hates walls of text
# - Student prefers step-by-step code traces over theory
```


```text
# WITHOUT MEM0 -- every session, every student, no variation


#  on the memory context provided..."
#
# What you already know about this student from past sessions:
# - Student struggles with recursion base cases
# - Student is a visual learner, hates walls of text
# - Student prefers step-by-step code traces over theory
```


```text
# WITHOUT MEM0 -- every session, every student, no variation


#  on the memory context provided..."
#
# What you already know about this student from past sessions:
# - Student struggles with recursion base cases
# - Student is a visual learner, hates walls of text
# - Student prefers step-by-step code traces over theory
```


This is what changes the response. The model on both sides is identical. The deployment is identical, but the only variable is whether the system prompt contains facts about the learner.


The intelligence is not in the LLM. It is in what you tell the LLM about the person it is talking to.


> This before/after is the entire argument for persistent memory in tutoring. Want to see the same pattern applied to a customer support agent?


### **The core logic**


The` respond_with_mem0` Function is where the architecture lives. Everything else is Streamlit scaffolding.


```text
def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
# Session 1: no memories exist yet -- store the exchange but respond generically
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
# Only send the latest turn to avoid re-processing prior messages
latest_turn   =  [  messages  [  - 1  ]  ,    {  "role"  :  "assistant"  ,    "content"  :  reply  }  ]
mem0_client  . add  (  latest_turn  ,    user_id  = st  . session_state  . user_id  )
return    reply  ,    [  ]  ,    BASE_SYSTEM


# Session 2+: retrieve relevant facts before the LLM call
# search() returns a list directly
memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)
memory_lines   =  [  m  [  "memory"  ]    for    m    in    memories    if    isinstance  (  m  ,    dict  )  ]    if    memories    else    [  ]


system   =  MEM0_SYSTEM
if    memory_lines  :
# Inject retrieved facts directly into the system prompt
system   +=  "\n\nWhat you already know about this student from past sessions:\n"
system   +=  "\n"  . join  (  f"-   {  line  }  "    for    line    in    memory_lines  )
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
# Only send the latest turn -- not the full accumulated history
return    reply  ,    memories  ,    system
```


```text
def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
# Session 1: no memories exist yet -- store the exchange but respond generically
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
# Only send the latest turn to avoid re-processing prior messages
return    reply  ,    [  ]  ,    BASE_SYSTEM


# Session 2+: retrieve relevant facts before the LLM call
# search() returns a list directly
memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)


system   =  MEM0_SYSTEM
if    memory_lines  :
# Inject retrieved facts directly into the system prompt
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
# Only send the latest turn -- not the full accumulated history
return    reply  ,    memories  ,    system
```


```text
def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
# Session 1: no memories exist yet -- store the exchange but respond generically
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
# Only send the latest turn to avoid re-processing prior messages
return    reply  ,    [  ]  ,    BASE_SYSTEM


# Session 2+: retrieve relevant facts before the LLM call
# search() returns a list directly
memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)


system   =  MEM0_SYSTEM
if    memory_lines  :
# Inject retrieved facts directly into the system prompt
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
# Only send the latest turn -- not the full accumulated history
return    reply  ,    memories  ,    system
```


Session 2 is the contrast session. Same questions, no chat history passed to either side. The plain column reverts to a textbook explanation. The Mem0 column retrieves the stored facts and opens with "I know base cases tripped you up last time." The divergence is immediate and visible.


### **The full app code**


**👉Wanna give it a try? Get a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and try it yourself.**


```text
import    streamlit    as    st
from    mem0    import    MemoryClient
from    openai    import    OpenAI
from    dotenv    import    load_dotenv
import    os
import    uuid


load_dotenv  (  )


@ st  . cache_resource
def    get_clients  (  )  :
mem0   =  MemoryClient  (  api_key  = os  . environ  [  "MEM0_API_KEY"  ]  )
openai   =  OpenAI  (  api_key  = os  . environ  [  "OPENAI_API_KEY"  ]  )
return    mem0  ,    openai


mem0_client  ,    openai_client   =  get_clients  (  )
MODEL   =  os  . environ  . get  (  "OPENAI_MODEL"  ,    "gpt-4o-mini"  )


BASE_SYSTEM   =  """You are a generic AI programming tutor. You have no prior knowledge of this student.
Give a standard textbook explanation. Keep responses to 3-4 sentences. Do not assume anything about the student."""


MEM0_SYSTEM   =  """You are a patient AI programming tutor. You MUST visibly adapt your response based on the memory context provided:
- If the student is a visual learner: use ASCII diagrams, step-by-step code traces -- not prose.
- If the student has struggled with something before: open with "I know X tripped you up before..." and address it directly.
- If the student has mastered something: skip re-explaining it and build forward.
Without the memory context below, you would give a generic answer -- the memory is what makes you different."""


SESSION_HINTS   =  {
1  :  [
"Hi! I really struggle with walls of text -- I learn best from diagrams and visual examples."  ,
"I'm trying to understand recursion. The base case totally confuses me, I never know when to stop."  ,
"Also I hate when tutors just give me theory. I need to see code traced step by step."  ,
]  ,
2  :  [
"Hey, can you explain recursion to me?"  ,
"Why does the base case matter? I keep forgetting."  ,
"Can you show me a factorial example?"  ,
]  ,
3  :  [
"Okay I think I finally get recursion. Binary trees are my new enemy though."  ,
"How does in-order traversal work?"  ,
"Show me an example -- the simpler the better."  ,
]  ,
}


for    key  ,    default    in    {
"session_num"  :  1  ,
"messages_plain"  :  [  ]  ,
"messages_mem0"  :  [  ]  ,
"last_memories"  :  [  ]  ,
"last_system_prompt"  :  MEM0_SYSTEM  ,
"user_id"  :  f"edtech_demo_  {  uuid  . uuid4  (  )  . hex  [  : 8  ]  }  "  ,
}  . items  (  )  :
if    key    not    in    st  . session_state  :
st  . session_state  [  key  ]   =  default


def    chat  (  system  ,    messages  )  :
resp   =  openai_client  . chat  . completions  . create  (
model  = MODEL  ,
max_tokens  = 350  ,
messages  = [  {  "role"  :  "system"  ,    "content"  :  system  }  ]   +  messages  ,
)
return    resp  . choices  [  0  ]  . message  . content


def    get_memory_count  (  )  :
result   =  mem0_client  . get_all  (  user_id  = st  . session_state  . user_id  )
return    len  (  result  )    if    result    else    0


def    respond_plain  (  messages  )  :
return    chat  (  BASE_SYSTEM  ,    messages  )


def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
return    reply  ,    [  ]  ,    BASE_SYSTEM


memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)


system   =  MEM0_SYSTEM
if    memory_lines  :
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
return    reply  ,    memories  ,    system


# --- Streamlit UI ---
st  . title  (  "AI Tutor: Stateless vs. Mem0-Powered"  )
st  . caption  (  f"Student ID:   {  st  . session_state  . user_id  }   | Session:   {  st  . session_state  . session_num  }  "  )


hints   =  SESSION_HINTS  [  st  . session_state  . session_num  ]
st  . info  (  f"Session   {  st  . session_state  . session_num  }   suggested inputs: "   +  " / "  . join  (  hints  )  )


col_plain  ,    col_mem0   =  st  . columns  (  2  )


with    col_plain  :
st  . subheader  (  "Plain AI (Stateless)"  )
for    msg    in    st  . session_state  . messages_plain  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )


with    col_mem0  :
st  . subheader  (  "Mem0-Powered AI"  )
for    msg    in    st  . session_state  . messages_mem0  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )
if    st  . session_state  . last_memories  :
with    st  . expander  (  "Memories retrieved this turn"  )  :
for    m    in    st  . session_state  . last_memories  :
st  . write  (  f"-   {  m  [  'memory'  ]  }  "  )


user_input   =  st  . chat_input  (  "Ask your tutor something..."  )
if    user_input  :
st  . session_state  . messages_plain  . append  (  {  "role"  :  "user"  ,    "content"  :  user_input  }  )
st  . session_state  . messages_mem0  . append  (  {  "role"  :  "user"  ,    "content"  :  user_input  }  )


plain_reply   =  respond_plain  (  st  . session_state  . messages_plain  )
mem0_reply  ,    memories  ,    system   =  respond_with_mem0  (
st  . session_state  . messages_mem0  ,    user_input  ,    st  . session_state  . session_num
)


st  . session_state  . messages_plain  . append  (  {  "role"  :  "assistant"  ,    "content"  :  plain_reply  }  )
st  . session_state  . messages_mem0  . append  (  {  "role"  :  "assistant"  ,    "content"  :  mem0_reply  }  )
st  . session_state  . last_memories   =  memories
st  . session_state  . last_system_prompt   =  system
st  . rerun  (  )


if    st  . button  (  f"Advance to Session   {  st  . session_state  . session_num   +  1  }  "  )    and    st  . session_state  . session_num   <  3  :
st  . session_state  . session_num   +=  1
st  . session_state  . messages_plain   =  [  ]
st  . session_state  . messages_mem0   =  [  ]
st  . session_state  . last_memories   =  [  ]
st  . rerun  (  )
```


```text
import    streamlit    as    st
from    mem0    import    MemoryClient
from    openai    import    OpenAI
from    dotenv    import    load_dotenv
import    os
import    uuid


load_dotenv  (  )


@ st  . cache_resource
def    get_clients  (  )  :
mem0   =  MemoryClient  (  api_key  = os  . environ  [  "MEM0_API_KEY"  ]  )
openai   =  OpenAI  (  api_key  = os  . environ  [  "OPENAI_API_KEY"  ]  )
return    mem0  ,    openai


mem0_client  ,    openai_client   =  get_clients  (  )
MODEL   =  os  . environ  . get  (  "OPENAI_MODEL"  ,    "gpt-4o-mini"  )


SESSION_HINTS   =  {
1  :  [
]  ,
2  :  [
"Hey, can you explain recursion to me?"  ,
"Why does the base case matter? I keep forgetting."  ,
"Can you show me a factorial example?"  ,
]  ,
3  :  [
"Okay I think I finally get recursion. Binary trees are my new enemy though."  ,
"How does in-order traversal work?"  ,
"Show me an example -- the simpler the better."  ,
]  ,
}


for    key  ,    default    in    {
"session_num"  :  1  ,
"messages_plain"  :  [  ]  ,
"messages_mem0"  :  [  ]  ,
"last_memories"  :  [  ]  ,
"last_system_prompt"  :  MEM0_SYSTEM  ,
"user_id"  :  f"edtech_demo_  {  uuid  . uuid4  (  )  . hex  [  : 8  ]  }  "  ,
}  . items  (  )  :
if    key    not    in    st  . session_state  :
st  . session_state  [  key  ]   =  default


def    chat  (  system  ,    messages  )  :
resp   =  openai_client  . chat  . completions  . create  (
model  = MODEL  ,
max_tokens  = 350  ,
)
return    resp  . choices  [  0  ]  . message  . content


def    get_memory_count  (  )  :
return    len  (  result  )    if    result    else    0


def    respond_plain  (  messages  )  :
return    chat  (  BASE_SYSTEM  ,    messages  )


def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
return    reply  ,    [  ]  ,    BASE_SYSTEM


memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)


system   =  MEM0_SYSTEM
if    memory_lines  :
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
return    reply  ,    memories  ,    system


# --- Streamlit UI ---
st  . title  (  "AI Tutor: Stateless vs. Mem0-Powered"  )


hints   =  SESSION_HINTS  [  st  . session_state  . session_num  ]


col_plain  ,    col_mem0   =  st  . columns  (  2  )


with    col_plain  :
st  . subheader  (  "Plain AI (Stateless)"  )
for    msg    in    st  . session_state  . messages_plain  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )


with    col_mem0  :
st  . subheader  (  "Mem0-Powered AI"  )
for    msg    in    st  . session_state  . messages_mem0  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )
if    st  . session_state  . last_memories  :
with    st  . expander  (  "Memories retrieved this turn"  )  :
for    m    in    st  . session_state  . last_memories  :
st  . write  (  f"-   {  m  [  'memory'  ]  }  "  )


user_input   =  st  . chat_input  (  "Ask your tutor something..."  )
if    user_input  :


plain_reply   =  respond_plain  (  st  . session_state  . messages_plain  )
mem0_reply  ,    memories  ,    system   =  respond_with_mem0  (
)


st  . session_state  . last_memories   =  memories
st  . session_state  . last_system_prompt   =  system
st  . rerun  (  )


st  . session_state  . session_num   +=  1
st  . session_state  . messages_plain   =  [  ]
st  . session_state  . messages_mem0   =  [  ]
st  . session_state  . last_memories   =  [  ]
st  . rerun  (  )
```


```text
import    streamlit    as    st
from    mem0    import    MemoryClient
from    openai    import    OpenAI
from    dotenv    import    load_dotenv
import    os
import    uuid


load_dotenv  (  )


@ st  . cache_resource
def    get_clients  (  )  :
mem0   =  MemoryClient  (  api_key  = os  . environ  [  "MEM0_API_KEY"  ]  )
openai   =  OpenAI  (  api_key  = os  . environ  [  "OPENAI_API_KEY"  ]  )
return    mem0  ,    openai


mem0_client  ,    openai_client   =  get_clients  (  )
MODEL   =  os  . environ  . get  (  "OPENAI_MODEL"  ,    "gpt-4o-mini"  )


SESSION_HINTS   =  {
1  :  [
]  ,
2  :  [
"Hey, can you explain recursion to me?"  ,
"Why does the base case matter? I keep forgetting."  ,
"Can you show me a factorial example?"  ,
]  ,
3  :  [
"Okay I think I finally get recursion. Binary trees are my new enemy though."  ,
"How does in-order traversal work?"  ,
"Show me an example -- the simpler the better."  ,
]  ,
}


for    key  ,    default    in    {
"session_num"  :  1  ,
"messages_plain"  :  [  ]  ,
"messages_mem0"  :  [  ]  ,
"last_memories"  :  [  ]  ,
"last_system_prompt"  :  MEM0_SYSTEM  ,
"user_id"  :  f"edtech_demo_  {  uuid  . uuid4  (  )  . hex  [  : 8  ]  }  "  ,
}  . items  (  )  :
if    key    not    in    st  . session_state  :
st  . session_state  [  key  ]   =  default


def    chat  (  system  ,    messages  )  :
resp   =  openai_client  . chat  . completions  . create  (
model  = MODEL  ,
max_tokens  = 350  ,
)
return    resp  . choices  [  0  ]  . message  . content


def    get_memory_count  (  )  :
return    len  (  result  )    if    result    else    0


def    respond_plain  (  messages  )  :
return    chat  (  BASE_SYSTEM  ,    messages  )


def    respond_with_mem0  (  messages  ,    user_message  ,    session_num  )  :
if    session_num   ==  1  :
reply   =  chat  (  BASE_SYSTEM  ,    messages  )
return    reply  ,    [  ]  ,    BASE_SYSTEM


memories   =  mem0_client  . search  (
user_message  ,
filters  = {  "user_id"  :  st  . session_state  . user_id  }  ,
limit  = 5
)


system   =  MEM0_SYSTEM
if    memory_lines  :
else  :
system   =  BASE_SYSTEM


reply   =  chat  (  system  ,    messages  )
return    reply  ,    memories  ,    system


# --- Streamlit UI ---
st  . title  (  "AI Tutor: Stateless vs. Mem0-Powered"  )


hints   =  SESSION_HINTS  [  st  . session_state  . session_num  ]


col_plain  ,    col_mem0   =  st  . columns  (  2  )


with    col_plain  :
st  . subheader  (  "Plain AI (Stateless)"  )
for    msg    in    st  . session_state  . messages_plain  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )


with    col_mem0  :
st  . subheader  (  "Mem0-Powered AI"  )
for    msg    in    st  . session_state  . messages_mem0  :
with    st  . chat_message  (  msg  [  "role"  ]  )  :
st  . write  (  msg  [  "content"  ]  )
if    st  . session_state  . last_memories  :
with    st  . expander  (  "Memories retrieved this turn"  )  :
for    m    in    st  . session_state  . last_memories  :
st  . write  (  f"-   {  m  [  'memory'  ]  }  "  )


user_input   =  st  . chat_input  (  "Ask your tutor something..."  )
if    user_input  :


plain_reply   =  respond_plain  (  st  . session_state  . messages_plain  )
mem0_reply  ,    memories  ,    system   =  respond_with_mem0  (
)


st  . session_state  . last_memories   =  memories
st  . session_state  . last_system_prompt   =  system
st  . rerun  (  )


st  . session_state  . session_num   +=  1
st  . session_state  . messages_plain   =  [  ]
st  . session_state  . messages_mem0   =  [  ]
st  . session_state  . last_memories   =  [  ]
st  . rerun  (  )
```


> Before you run the demo: grab a free Mem0 API key at[app.mem0.ai](https://app.mem0.ai/) . No credit card, takes 30 seconds. You will need it for the` .env` file below.


Create a` .env` file in your project directory:


```text


```


```text


```


```text


```


Then install dependencies and run:


```text


```


```text


```


```text


```


The app renders two side-by-side columns: plain AI on the left, Mem0-backed AI on the right. Use the Session 1 hints to profile the student, then advance to Session 2 and ask the same questions. The divergence is the demo.


## **Session 3: Memory updates**


In Session 3, the student says they finally understand recursion and that binary trees are now their problem. A naive logging approach would append that as a fourth entry alongside "struggles with recursion base cases," creating a contradiction. The system would then produce confused responses that simultaneously treat recursion as a known struggle and a solved one.


Mem0 resolves the conflict. It updates the stored fact, replacing "struggles with recursion base cases" with something closer to "mastered recursion, currently working on binary tree traversal." The next session starts from the right place: recursion is off the table, trees are the focus. This is the difference between a system that accumulates data and one that maintains a coherent model of the learner.


## **Scaling to multiple students**


If you want to scale this solution to a large number of students, then you might want to make some scaling changes :


### **One backend, N students**


Every student in your app maps to a unique` user_id` . Your single Mem0 client handles all of them. Memory is scoped by namespace, not by instance.


```text
# Student A's session -- stored under Alice's namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_alice"  )


# Student B's session -- completely isolated namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_bob"  )


# Retrieval is strictly scoped -- Alice's memories never appear for Bob
mem0_client  . search  (  query  ,    filters  = {  "user_id"  :  "student_alice"  }  )
```


```text
# Student A's session -- stored under Alice's namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_alice"  )


# Student B's session -- completely isolated namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_bob"  )


# Retrieval is strictly scoped -- Alice's memories never appear for Bob
```


```text
# Student A's session -- stored under Alice's namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_alice"  )


# Student B's session -- completely isolated namespace
mem0_client  . add  (  latest_turn  ,    user_id  = "student_bob"  )


# Retrieval is strictly scoped -- Alice's memories never appear for Bob
```


You do not spin up a separate database per student. You do not maintain separate auth contexts for memory reads. The` user_id` filter handles isolation at the Mem0 layer. At 10,000 students, the infrastructure cost is the same as at 10.


### **Subject separation with**` **agent_id**`


One student often needs tutoring across multiple subjects. A student working on both calculus and Python should not have their CS learning gaps polluting their math tutor's context. You solve this with` agent_id` .


```text
# Same student, different subjects -- completely separate memory namespaces
mem0_client  . add  (  latest_turn  ,    user_id  = "student_alice"  ,    agent_id  = "cs-tutor"  )
mem0_client  . add  (  latest_turn  ,    user_id  = "student_alice"  ,    agent_id  = "math-tutor"  )


# Each tutor retrieves only its own subject's memories
mem0_client  . search  (  query  ,    filters  = {  "user_id"  :  "student_alice"  ,    "agent_id"  :  "cs-tutor"  }  )
```


```text
# Same student, different subjects -- completely separate memory namespaces


# Each tutor retrieves only its own subject's memories
```


```text
# Same student, different subjects -- completely separate memory namespaces


# Each tutor retrieves only its own subject's memories
```


The` user_id` tells Mem0 which memories to access. The` agent_id` tells it which subject context to scope to. One student, multiple subjects, and zero cross-contamination. You add this parameter at write time and search time. Nothing else changes.


> If you want managed memory with zero infrastructure, create a free account at[app.mem0.ai](https://app.mem0.ai/) -- your first 1,000 memory operations are free.


## **Frequently Asked Questions**


### Q. Does Mem0 store raw chat history or extracted facts?


Mem0 extracts semantic facts from conversations, not raw transcripts. When you call` mem0_client.add()` with a conversation turn, Mem0's pipeline extracts statements like "student struggles with binary trees" and stores those as discrete, queryable memories. You get precision at retrieval time instead of having to re-parse a full chat log.


### Q. Can Mem0 be used to build an AI study assistant?


Yes. The same` user_id` scoping and` search()` pattern used in this tutoring demo applies directly to study assistants who track topic mastery, preferred study formats, and past quiz performance. Mem0 works with any conversational LLM app where you need facts to persist across sessions.


### Q. How does a personalized AI tutor differ from a standard AI chatbot?


A standard chatbot knows only the current conversation. A personalized AI tutor knows the learner: their style, their gaps, their progress. That difference requires a persistent memory layer outside the context window. Without it, the "personalization" resets every session, and the tutor is functionally a search engine.


### Q. How do I separate memory by subject (math vs. history)?


Add` agent_id` to your` add()` and` search()` calls. Use` agent_id="math-tutor"` for one subject and` agent_id="history-tutor"` for another. Mem0 scopes retrieval to the intersection of` user_id` and` agent_id` , so a student's CS struggles never appear in their math tutor's context.


### Q. What is the latency of a Mem0 search call?


A typical` search()` call over a student's memory profile completes in under 200ms for profiles with fewer than 100 stored facts. For most tutoring apps, this adds negligible overhead before the LLM call, which dominates total latency by 5 to 10x.


## **Conclusion**


The gap between a chatbot and a tutor is not the model. It is whether the system remembers who it is talking to. You have built the part that remembers. Three function calls, including` add()` ,` search()` ,` get_all()` and the LLM you already had starts behaving like something that knows your students. The` user_id` pattern handles scale. The` agent_id` pattern handles subjects. The only thing left is shipping it.


—


[Mem0](https://mem0.ai/) *is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.*


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=customer_service_chatbots_with_persistent_memory&utm_content=customer_service_chatbots_with_persistent_memory) or


Self-host mem0 from our[open source GitHub repository](https://github.com/mem0ai/mem0) .


—
