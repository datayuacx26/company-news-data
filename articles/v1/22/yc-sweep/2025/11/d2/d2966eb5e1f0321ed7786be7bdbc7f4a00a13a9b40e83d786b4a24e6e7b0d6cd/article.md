---
schema_version: "1.0.0"
document_id: "d2966eb5e1f0321ed7786be7bdbc7f4a00a13a9b40e83d786b4a24e6e7b0d6cd"
company_key: "yc-sweep"
company: "Sweep"
source_id: "yc-sweep-rss-0fc78e2214cd"
canonical_url: "https://blog.sweep.dev/posts/autocomplete-context"
published_at: "2025-11-16T00:00:00+00:00"
first_seen_at: "2026-07-24T02:57:34.319784+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:273e4bbe181a841978374a6f40afbf1b609332f28ee8a600f2836a0712444808"
---

# Using the JetBrains program structure interface for codebase context

posts


Using the JetBrains program structure interface for codebase context


# Using the JetBrains program structure interface for codebase context


William Zeng


•


November 16, 2025


*Sweep autocomplete can take on huge codebases using the Program Structure Interface*


---


At Sweep, we want to build the best autocomplete for JetBrains IDEs.


When we first launched AI autocomplete, the main feedback we heard from developers was that the autocomplete was fast, but it didn’t understand the codebase that well. For example, imagine the developer just started typed` client.` :


```text
// app.ts
import   { DatabaseClient }   from   './database'  ;


const   client   =   new   DatabaseClient  ();
client.  █
```


Without codebase context the autocomplete doesn’t know what the` DatabaseClient` class actually does and is going to hallucinate a method that doesn’t exist. However, if we can provide the correct context to the autocomplete we can suggest the correct completion.


Looking at` database.ts` , we can see that it has a method like` query` , and the developer likely wants a suggestion using that method.


```text
// database.ts
export   class   DatabaseClient   {
async   connect  (  connectionString  :   string  )  :   Promise  <  void  > {   ...   }
async   query  <  T  >(  sql  :   string  ,   params  ?:   any  [])  :   Promise  <  T  []> {   ...   }
...
}


// app.ts
import   { DatabaseClient }   from   './database'  ;


const   client   =   new   DatabaseClient  ();
client.  query  (  'SELECT * FROM users WHERE id = $1'  , [  1  ])  ;   // <- what the developer wants
```


Before we describe how we solved the codebase context problem, it’s helpful to understand how our autocomplete works.


## How autocomplete works


Instead of using a model API endpoint like gpt-4o-mini (what GitHub Copilot uses), our autocomplete uses a specialized LLM that runs on our own inference engine. This has a couple of advantages, primarily control over inference and network latency.


We wrote about how we used techniques like speculative decoding to speed up inference[in a previous post](https://blog.sweep.dev/posts/next-edit-jetbrains#speculative-decoding) , and after implementing those optimizations, we found that network latency made up the bulk of the remaining latency. If a developer is in San Francisco, and the API they are using is located in Virginia (AWS’s us-east-1), they’ll experience an extra 100ms latency per request simply due to round-trip time.


By hosting our model on our own datacenters, we can provision GPUs closer to the developer. For example, we host our west coast datacenter in Oregon which decreases network latency for west coast developers from 143ms to 32ms. To provide the best autocomplete experience, we want to stay within our latency budget of 100ms.


## Why this is hard


We can’t simply send the entire codebase to the model every time because this would increase latency and decrease quality. Even frontier LLMs struggle with long contexts. In Chroma’s repeated words benchmark, models were asked to replicate a sequence of repeated words with one unique word inserted.


*Source:[Chroma Research on Context Rot](https://research.trychroma.com/context-rot#results)*


Despite being a straightforward copy task, models showed significant performance degradation as input length increased. For example, Claude Sonnet 4’s accuracy dropped from 95% at 1000 tokens to 50% at 10,000 tokens.


### Latency vs. Context Length


This would also increase latency because for every 10k tokens of context (~500 lines of code), we add 100ms of latency.


#### Context Size vs Latency


To stay under 100ms we need to stay below 10k tokens of context.


We tried a few options to improve codebase context.


## Using the other open files in the IDE


This comes from the following assumption: the developer is likely to be working on files that they have open. Because they are reading these files, typically they want the autocomplete to be aware of them. These make a great source of context as this assumption is usually true.


The primary limitation with this is that it’s dependent on the developer actually having the file open. Take the case where a central abstraction is used across the codebase such as a base API client class:


lib/api/BaseApiClient.ts (not open)


```text
export   abstract   class   BaseApiClient   {
protected   async   request  ()


protected   async   get  ()


protected   async   post  ()
}
```


The autocomplete may need to know the definition of` BaseApiClient` when implementing a subclass of` BaseApiClient` :


services/UserService.ts (currently open)


```text
import   { BaseApiClient }   from   '../lib/api/BaseApiClient'  ;


export   class   UserService   extends   BaseApiClient   {
async   getUser  (  id  :   string  ) {
...
}
}
```


Unless the developer is changing the core logic of` BaseApiClient` (unlikely) they usually won’t have it open, leading to a context gap. This makes using the developer’s open files a good start, but not enough on its own.


## Keyword Search using TF-IDF


TF-IDF (Term Frequency-Inverse Document Frequency) is a classic information retrieval algorithm. It’s excellent at taking a query string and finding the most relevant results from a larger collection. TF-IDF scores documents based on how often query terms appear in them, weighted by two main factors:


1. **Term Frequency (TF)** : How often the term appears in the document, where more occurrences increase the score.
2. **Inverse Document Frequency (IDF)** : How rare the term is across all documents, where rarer terms increase the score.


For example, if you search for` DatabaseClient` , a file with 10 occurrences of` DatabaseClient` would rank higher than one with just 1 occurrence. Similarly, searching for a rare class name like` SpecializedAuthenticator` would rank higher than common terms like` function` or` class` . This works great for traditional search, but autocomplete presents unique challenges.


### The query construction problem


The fundamental problem with TF-IDF for autocomplete is that we don’t know what to search for. Let’s revisit our earlier example. The developer is typing` client.query(` .


app.ts


```text
import   { DatabaseClient }   from   './database'  ;
const   client   =   new   DatabaseClient  ();
client.  query  (  █);
```


What should we search for? The naive approach would be to extract terms from the visible code context:` client` ,` query` ,` DatabaseClient` . But here’s what happens:


- **` client`** might appear hundreds or thousands of times across the codebase (every API client, database client, HTTP client, etc.)
- **` query`** is also extremely common (query builders, query parsers, SQL queries, etc.)
- **` DatabaseClient`** might be more specific, but TF-IDF doesn’t know this is the important term


TF-IDF will return dozens of files, most of which are irrelevant. Without semantic understanding, we can’t distinguish between a variable named` client` (not useful) and the class` DatabaseClient` that defines the methods we need (very useful). We need to somehow know to search for` DatabaseClient` specifically, not just` client` .


Why not use a semantic search model (like vector search)?


## Vector Search doesn’t work either


Vector databases have become popular for code search, using embedding models to find semantically similar code. In theory, this could solve the query construction problem—embeddings understand that` client` is an instance of` DatabaseClient` and could retrieve the right definition.


However, vector search introduces three critical problems for real-time autocomplete: **latency** , **privacy** , and **correctness** .


### Latency and privacy


Vector search requires building and maintaining an index of embeddings for your entire codebase. This index must live somewhere:


**Option 1: Server-side index**


This approach requires uploading your entire codebase to a remote server where it’s permanently stored as embeddings. This is a deal-breaker for privacy-conscious developers as well as companies who don’t want their proprietary code leaving their machines.


**Option 2: Client-side index**


Building the index locally consumes significant memory on the developer’s laptop, as embeddings are much larger than TF-IDF indices. The index also needs to be rebuilt whenever files change, adding CPU and memory overhead while the developer is coding. Here’s an example of using an 22M parameter embedding model like[sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) . When testing on an M4 MacBook Pro, we observed:


Metric Value


Mean Latency 11.13 ms


Median Latency 6.33 ms


Minimum Latency 4.97 ms


Maximum Latency 46.43 ms


Standard Deviation 12.82 ms


This is too slow for real-time autocomplete, and for lower-end devices the latency can spike to over 100ms (our entire latency budget).


### Usages vs. Definitions


Even if we solved latency and privacy, both vector search and TF-IDF share one final critical flaw: **they can’t distinguish between where code is used versus where it’s defined** . When you type` client.query(` , you need the **definition** of the` DatabaseClient` class to see what methods are available. But search-based approaches will return:


- files that import and use` DatabaseClient` .
- test files that mock` DatabaseClient` .


These two types of occurrences are not helpful. The actual` DatabaseClient` class definition will get buried in the search results because the search algorithm treats all occurrences equally, making it hard to find the definition you actually need.


We needed a completely different approach.


## Our solution: Program Structure Interface


Because we exclusively serve JetBrains IDEs, we can leverage a powerful primitive that solves the above problems: the Program Structure Interface (PSI). PSI is how JetBrains IDEs provide features like syntax highlighting, error analysis, and refactoring.


Here’s what that looks like in action:


#### PSI Cross-File Lookup


PSI instantly resolves the type and fetches the exact definition.


UserService.ts


```text
export class UserService
extends  BaseApiClient   {
async getUser(id: string) {
return   get(id)
}
}
```


→


BaseApiClient.ts


```text
class  BaseApiClient   {
async  get  <T>(...)
...
}
```


This is similar to the Language Server Protocol (LSP) used by VSCode, but with one critical difference being that the **PSI runs in the same process as the IDE** . This in-process architecture means the IDE maintains a nearly perfect representation of your codebase in memory, updated incrementally as you type. Our autocomplete can “look up” any definition instantly without indexing, embeddings, or search queries.


Sweep’s autocomplete uses the PSI to fetch all definitions around your cursor. This works on any codebase regardless of size or language as long as the IDE has indexed it.


This lookup takes 30ms before the cache is populated, and drops to <1ms after cache hydration. This means that Sweep is able to make a suggestion using the exact definitions that the developer sees in their IDE, while taking up almost none of our 100 ms latency budget.


## What’s next


After launching this, we noticed that our autocomplete acceptance rate improved by 3% without any additional latency! If you’d like to try it out, Sweep is available in the JetBrains plugin marketplace[here](https://plugins.jetbrains.com/plugin/26860-sweep-ai) . You can also join our[Discord server](https://discord.gg/invite/sweep) to get early access to our blogs and other updates!
