---
schema_version: "1.0.0"
document_id: "e5f633b5fc02796fc6625e3638a3e5245c3659f91222ec6c834c640478701132"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/who-will-build-vercel-for-agents"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:29ecdbde71bb6daf13bcc058f798775c5d0124605d4c2a3fc8f74ea030345bfb"
---

# Who will build Vercel for agents?

This isn't surprising to you, but I believe there's a reason why none of the "vercel for agents" companies have won yet: none of these technologies have innovated in how to manage stateful applications.


\[tweet:[https://x.com/utpalnadiger/status/2062702881019744474](https://x.com/utpalnadiger/status/2062702881019744474) \]


Fundamentally, AI agents are *a very different shape* than what we used to deploy in the cloud.


Previously, a "cloud application" would be an API wrapped around a database and S3. It would only need to access its own data, and the *server* didn't need to contain any state at all, so they were super simple to spin up/replace -- the "kubernetes" model of deployment.


If you look out at the "agent deployment" options, everyone is sort of competing on what the *thing* is that will allow them to satisfy this new shape of application.


The sandbox companies believe that the fundamental problem is compute. If you can spin up compute fast enough and flexibly enough, then you can serve agents. I don't find this very inspiring, but it also doesn't *help* . Spinning up a server quickly is helpful, but it's not enough for making agents simple -- you've just replicated what people were doing prior to Vercel for websites.


\[side note: it's funny to me that Vercel themselves haven't seemed to release anything yet to recognize this, but it's early days\]


I think the next most-inspired option is a company that believes that *networking* is the fundamental thing that needs to change in order to serve agents well. This is the category where I put[@ssh_exe_dev](https://x.com/ssh_exe_dev) . They're betting on the fact that having servers that are super easy to address (ssh exe.dev ) and spin up copies of themselves is the fundamental primitive required to serve agents well.


This seems to mostly be working, for select parts of the agent stack. We most commonly see people want to use exe.dev for the "agent loop" portion of the problem (where do you run the harness), but it doesn't necessarily solve the ability for that agent loop to call untrusted code or manipulate their context window.


Instead, I \[of course\] believe that the real DX advancement that needs to happen to solve for incredibly simple agent deployment and management is *storage* .


The reason for this is simple: agents are stateful in a way that no previous application was, so you need to figure out how to handle state differently than "configure a Kubernetes ProvisionedVolume if you want".


Agents are actually made up of two different parts: the agent loop (the "trusted" code which contains the pieces that connect the LLM to the tools) and the sandbox (the "untrusted" code that runs to manipulate context). The agent loop needs to be addressable via a URI (for example, in order to set up a webhook that triggers the agent) and the sandbox needs to be cheap to spin up in large quantities.


These two separate pieces need access to the same underlying data set -- simultaneously. (For example, the agent loop may call ReadFile, while the sandbox may call "sed -i <whatever>"). The agent loop may want to spin up more than one sandbox to manipulate the context.


This requires that the underlying storage service can be *shared* across multiple machines -- in other words, file storage.


Interestingly enough, the other reason why the old "ProvisionedVolume" model isn't the right one for agents comes to the second property that we spoke about. Not only are agents stateful, but they also (uniquely) need access to a great *breadth* of data instead of a singular data store that the application might own.


A simple agent might combine: an organization's skills (markdown files, potentially enabled on a per-user basis), access to reference data (for example, Notion or Salesforce), agent-owned memory (markdown files that the agent can manipulate itself), and shared-memory (usually versioned using something like git or stored in a database like Postgres or SQLite).


This means that the file-storage that the agent is using for its data needs to be *composable* across several different sources, with different access controls, permissions, and mutation strategies.


Ideally, some of these composable units are actually reading from the systems of record directly so that the organization building agents doesn't have to maintain a second source of truth.


This isn't a problem that you can solve with compute, no matter how fast it spins up or how many different operating systems you support.


This is a problem that you solve with a new storage primitive.


One of the reasons that we know this is the case is that infrastructure tools win when they have network effects that compound to push the industry farther.


React and the Javascript frameworks won the web because it was simple for developers to modularize (with components) front-end UX with other developers, creating a compounding effect that made the entire web better than what we had before.


There is no such effect today in agent-building. Everyone is ETL'ing their data into individual sandbox platforms and rebuilding how their agents communicate with sandboxes, share state, and interact with the LLM.


If we had a composable storage system which allow synchronization from original sources, the industry could share and iterate (together) on open-source recipes to represent the data from these systems of record in the most efficient way for different LLMs to access.


If these stateful applications are composable and open-sourced, it's simpler for agents to build *other agents* because the amount of pre-training on the patterns and code sharing increase as the industry adopts the standard. It seems obvious to me that agents building agents are the reason that "no-code" will not be the way to win this market.


I, of course, believe that the technology that[@archildata](https://x.com/archildata) has built will underpin this next phase of the agent revolution. We're working directly with several sandbox providers now to push the industry in this direction, but this actually isn't enough.


I expect that in the coming months we're going to start thinking very hard about how we start to represent Archil, as a framework, that can be used to programmatically define and interact with the context that agents need.


If you're interested in collaborating on what could be the biggest opportunity of this moment, we're hiring right now -- please reach out.


If you're building agents and you have opinions on what this looks like, please reach out -- we love to collaborate directly with developers and get more data on how the future should be shaped.


Until then, just know that a better world is possible, and it's coming.
