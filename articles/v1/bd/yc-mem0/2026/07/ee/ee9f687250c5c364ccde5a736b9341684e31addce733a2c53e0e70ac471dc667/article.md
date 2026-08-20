---
schema_version: "1.0.0"
document_id: "ee9f687250c5c364ccde5a736b9341684e31addce733a2c53e0e70ac471dc667"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/mem0-s-infer-true-vs-infer-false-the-fact-your-ai-support-bot-forgot"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:60a44bd2942c470d3092472a0ca0f82b6dcc6b6b2c5a955281d0303daa62ae52"
---

# Mem0's infer=True vs infer=False: The Fact Your AI Support Bot Forgot

You've called` mem0.add()` before. You've probably never touched the` infer` parameter. You've never had to. It defaults to` True` . You just call` add()` , something gets saved, and you move on.


> **Here's the thing:** Two completely different things can happen inside that call, and you've probably only ever seen one of them run.


A "memory" in Mem0 doesn't mean RAM. It doesn't mean the thing your laptop runs out of when you have forty tabs open. In[Mem0](https://mem0.ai/) , a memory is one saved fact, sitting in a database that also happens to be a vector index. The word gets reused a lot, and that's not your fault.


` infer` is the switch that decides how that fact gets written.` infer=True` is a smart assistant, listening to what was said and writing down a tidy note in its own words.` infer=False` is a tape recorder, saving exactly what it heard, word for word. Same function call. Completely different output.


Please enter a valid YouTube, Vimeo, or direct video URL


By the end of this, you'll be able to look at an` add()` call and predict, before running it, roughly what comes out the other side, and whether you can trust it to have kept the one fact you actually needed. That sounds like a small trick. It is. It's also the first step to shaking the feeling that Mem0 either remembers things or it doesn't, no in-between, because there very much is an in-between, and it's this parameter.


> [👉Get a free API key at app.mem0.ai](https://app.mem0.ai/) **to follow along (free tier, no credit card, includes all the**` **add()**` **and**` **search()**` **calls shown below).**


## **Your first add()**


Start small. One message, one customer,` infer=False` .


```text
mem0.add(
messages= [{"role": "user", "content": "The $40 annual fee is more than I expected, can it be waived?"}]


```


```text
mem0.add(


```


```text
mem0.add(


```


What happens is almost nothing. The message goes straight to the embedding model and gets stored as its own record. No LLM reads it. Nothing gets compared against what's already saved. Mem0's own docs describe the payload as stored exactly as provided, and that's the whole story: what you typed is what you get back later.


Now the same message, same customer,` infer=True` instead.


```text
mem0.add(


```


```text
mem0.add(


```


```text
mem0.add(


```


This time something actually runs. An LLM reads the message and pulls out whatever facts it thinks are worth remembering. Mem0 checks its vector store for anything already saved that's related. Then the LLM decides, fact by fact, whether to add something new, update something that's now out of date, delete something that's been contradicted, or do nothing at all.


That last option, do nothing, is easy to skip past reading the docs. Hold onto it. It's going to matter in about two paragraphs.


## **What happened when we actually ran this**


Here's a real conversation:


Sent with` infer=False` , one of the seven saved records is this, verbatim. Sent with` infer=True` , from the exact same seven lines, here is the complete list of what got saved:


***Fig:***` ***infer= False***` ***vs***` ***infer = True***`


Read that list again. Somewhere between "I noticed I'm being charged a fee" and "I appreciate it," a fee got waived. That's the one fact a support agent would actually need three weeks later if Marcus called back disputing the charge.


## **Counting what got stored**


*This is the habit worth building:* before you trust` infer=True` with something, count what you'd actually get back.


` infer=False` never merges anything. Every message becomes its own record. Send the same fact three different ways across three tickets and you get three records, not one cleaned-up fact. Over months, that's not a memory, it's a growing, unsorted pile of everything anyone ever said. Mem0's own docs are direct about the consequence: switch between modes for the same underlying fact and you can save it twice, once as a raw record and once as an extracted one.


` infer=True` does the opposite. It's compressing on purpose. Seven lines of conversation went in. Two facts came out. That's not a bug, that's the feature working exactly as designed, minus the one fact you actually wanted. The count is the whole point of the mode, and also the whole risk of it.


***You might be thinking: fine, I'll just run both.***


That's the right instinct. Here's the part that'll bite you if you don't know about it first. The obvious way to tag them is` agent_id` , since that's exactly what it's documented for, a scoping identifier independent of` user_id` . In practice, testing this directly turned up a real gap: a memory created through` infer=False` reliably shows` "agent_id": "verbatim_log"` in the raw response. A memory created through` infer=True` 's extraction pipeline can come back with no` agent_id` field at all, even though you passed the exact same value to the exact same` add()` call.


` user_id` doesn't have that problem, because every memory has to belong to somebody, no exceptions. So the pattern that actually works is folding the policy into` user_id` itself:` customer_marcus::smart_summary` versus` customer_marcus::verbatim_log` . Keep passing` agent_id` too, it doesn't hurt, just don't rely on it alone to separate` infer=True` content later.


## **Try it yourself**


Everything above is us telling you what we saw. Here's how to check it yourself in a few minutes.


> **👉Wanna give it a try? Get a**[Mem0 API Key](https://app.mem0.ai/) **and try it yourself.**


### **Setup**


```text


```


```text


```


```text


```


Now, we have the setup done, lets import the installed libraries:


```text
import os
from dotenv import load_dotenv
from mem0 import MemoryClient


load_dotenv()
mem0 = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


```text
import os
from dotenv import load_dotenv
from mem0 import MemoryClient


load_dotenv()
mem0 = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


```text
import os
from dotenv import load_dotenv
from mem0 import MemoryClient


load_dotenv()
mem0 = MemoryClient(api_key=os.environ ["MEM0_API_KEY"]


```


You'll need a[Mem0 API key](https://app.mem0.ai/) . The free tier is enough for everything here. Nothing else, since Mem0 runs its own LLM call internally for` infer=True` ; this doesn't need a separate chat model of your own.


**Write the same conversation two ways:**


```text
VERBATIM_AGENT = "verbatim_log"
SUMMARY_AGENT = "smart_summary"


def scoped_user_id(customer_id: str, agent_id: str) -> str:
return f"{customer_id}::{agent_id}"


def write_verbatim(customer_id: str, messages: list [dict]  ) -> dict:
return mem0.add(
messages=messages,
user_id=scoped_user_id(customer_id, VERBATIM_AGENT),
agent_id=VERBATIM_AGENT,
infer=False,
)


def write_summary(customer_id: str, messages: list [dict]


```


```text
VERBATIM_AGENT = "verbatim_log"
SUMMARY_AGENT = "smart_summary"


def scoped_user_id(customer_id: str, agent_id: str) -> str:
return f"{customer_id}::{agent_id}"


def write_verbatim(customer_id: str, messages: list [dict]  ) -> dict:
return mem0.add(
messages=messages,
user_id=scoped_user_id(customer_id, VERBATIM_AGENT),
agent_id=VERBATIM_AGENT,
infer=False,
)


def write_summary(customer_id: str, messages: list [dict]


```


```text
VERBATIM_AGENT = "verbatim_log"
SUMMARY_AGENT = "smart_summary"


def scoped_user_id(customer_id: str, agent_id: str) -> str:
return f"{customer_id}::{agent_id}"


def write_verbatim(customer_id: str, messages: list [dict]  ) -> dict:
return mem0.add(
messages=messages,
user_id=scoped_user_id(customer_id, VERBATIM_AGENT),
agent_id=VERBATIM_AGENT,
infer=False,
)


def write_summary(customer_id: str, messages: list [dict]


```


Pass the whole seven-line conversation as` messages` , role-tagged, not just one line. That matters more than it sounds. A single transactional message like "I've applied a one-time waiver on this fee for you" gave` infer=True` nothing to extract at all in testing, probably because it reads as an action the assistant took, not a fact about the customer. A fuller exchange with the customer's own words in it is what actually gives the extraction step something to chew on.


**Read both stores back:**


```text
def read_store(customer_id: str, agent_id: str) -> list [dict]


```


```text
def read_store(customer_id: str, agent_id: str) -> list [dict]


```


```text
def read_store(customer_id: str, agent_id: str) -> list [dict]


```


Run the Marcus conversation through both functions, then call` read_store()` for each.` verbatim_log` comes back with 7 entries, one per message, no exceptions.` smart_summary` comes back with 2 entries, and if you go looking, you will not find the waiver in them. Same input, same customer, two completely different records of what happened. Count them yourself. That's the whole exercise.


**The gotcha, before you run this for real.**


Here's the part that'll trip you up if nobody warns you first. Because` infer=True` writes are asynchronous, reading a store back immediately after writing to it can catch the job mid-flight, still marked` PENDING` , before the extraction has actually finished. If you read at that exact moment, you'll see fewer memories than you're eventually going to get, and it'll look exactly like "nothing got saved" when really it's "not done yet." The fix is to check the write's own status before deciding whether to trust an empty-looking read:


```text
import time


def wait_for_settle(write_result: dict, customer_id: str, agent_id: str, before_ids: set, max_wait: float = 20.0) -> list [dict]


```


```text
import time


```


```text
import time


```


Capture the ID set before you write, not just whether the store looked empty. A store that already has memories from an earlier write will look "settled" the instant you check, even if the newest write is still queued behind it, and that's a real way to end up reporting stale results as if they confirmed the latest request.


One honest gap, while we're here: if` max_wait` runs out and the ID set still hasn't changed, that's genuinely ambiguous between "extraction found nothing" and "it just needed more time," not proof of either one. This snippet also skips retry and error handling around the network calls, which a real version would need and this one doesn't have.


## **Going further**


Three things come up fast once this runs against real customer data instead of a demo conversation.


-


You don't have to pick one. The two functions above write to two independent` user_id` scopes precisely so they can both run on every message, permanently. A regulated support desk can keep` verbatim_log` as the record of truth for disputes and audits, while` smart_summary` is what the agent actually searches against day to day for a fast read on a customer.


-


Search the summary store, not the verbatim one, for anything conversational.` search()` on a verbatim store gets noisier every month, since nothing there was ever merged or deduplicated. Point day-to-day` search()` calls at the scoped` user_id` running` infer=True` , and keep the verbatim store for the one moment someone needs the exact original wording, or needs to check whether a fact like a waiver actually made it into the summary at all.


-


Deletion isn't symmetric either.` delete_all(user_id=...)` works the same way on both scoped stores mechanically, but the right answer to "can we delete this" is not. A verbatim log kept for compliance reasons may be under a legal retention hold that a summary profile isn't, so "the right to be forgotten" can apply to one store on a customer and not the other, on the same account, at the same time.


Here's the whole thing in one shot.` infer` decides whether Mem0 writes down what was said or writes down what it decided mattered.` infer=False` never forgets a word and never merges anything, so it grows forever and duplicates on purpose.` infer=True` compresses on every write, which is usually what you want, except when the fact you needed most is exactly the one it judged not worth keeping. Neither mode is broken. The mistake is not knowing which one you're holding, and not checking, before it matters, what your smart note-taker quietly decided to leave out.


## **Try it!**


Everything above runs on Mem0's free tier. Read the[add() and infer docs](https://docs.mem0.ai/core-concepts/memory-operations/add) to see the extraction pipeline described here, then[grab a free API key](https://app.mem0.ai/) when you're ready to run a real conversation through both write paths yourself.
