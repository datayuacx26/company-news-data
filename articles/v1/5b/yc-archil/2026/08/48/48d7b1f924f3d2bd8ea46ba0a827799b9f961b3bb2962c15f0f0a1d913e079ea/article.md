---
schema_version: "1.0.0"
document_id: "48d7b1f924f3d2bd8ea46ba0a827799b9f961b3bb2962c15f0f0a1d913e079ea"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/just-bash-support"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:ffa85cd932b891f3ba8f9ff82355f39364fa68eee2edbf2c17ea20e34ce5b3c0"
---

# Archil now supports mounting from just-bash

TL;DR: You can launch an sandboxed instance of just-bash with an Archil disk by running:


```text
npx @archildata/just-bash <region> <disk>


```


---


Vercel's just-bash gives AI agents a sandboxed bash environment in TypeScript—shell access without the security risks of real bash. It's especially useful in environments like AWS Lambda or Fargate where you don't have root access to set up traditional sandboxing.


Today, we're launching` @archildata/just-bash` , an adapter that connects just-bash to Archil. Agents can now run bash commands against your cloud data, working with datasets far larger than what fits in local memory.


## What is just-bash?


[github.com vercel-labs/just-bash A sandboxed bash environment written in TypeScript, designed for AI agents requiring secure shell access.](https://github.com/vercel-labs/just-bash)


just-bash is a bash interpreter written in TypeScript by Malte Ubl at Vercel. It implements 60+ Unix commands—` ls` ,` cat` ,` grep` ,` find` ,` sed` ,` awk` , and more—in a sandboxed environment that can't escape to the host system.


This matters for AI agents. When you give an LLM access to a shell, you're handing it a sharp tool. just-bash provides the utility of bash commands without the security risk of actual shell access. The agent can manipulate files, run pipelines, and explore data—all within a controlled environment.


## How the integration works


Archil already turns S3 buckets into POSIX-compatible filesystems. Our caching layer provides sub-millisecond reads for hot data while maintaining full read-write access to the underlying bucket.


The just-bash integration brings this same capability to sandboxed agent environments:


```text
import   { ArchilClient }   from   '@archildata/client'  ;
import   { ArchilFs }   from   '@archildata/just-bash'  ;
import   { Bash }   from   'just-bash'  ;


// Connect to Archil
const   client   =   await   ArchilClient  .connect  ({
region  :   'aws-us-east-1'  ,
diskName  :   'myaccount/agent-workspace'  ,
});


// Create filesystem adapter and shell
const   fs   =   new   ArchilFs  (client);
const   bash   =   new   Bash  ({ fs });


// Agent now has full bash access to S3
await   bash  .exec  (  'ls -la /data'  );
await   bash  .exec  (  'cat /data/context.txt | grep "relevant"'  );
await   bash  .exec  (  'echo "agent output" >> /data/results.log'  );
```


Every file operation flows through Archil's caching layer to S3. The agent sees a normal filesystem. Under the hood, it's backed by infinite, durable cloud storage.


## Why this matters


**Agents can finally persist state.** An agent running on Lambda can write to` /data/memory.json` , terminate, and pick up exactly where it left off on the next invocation. No more losing context between runs.


**Agents can work with massive datasets.** We're working with partners building AI agents that need access to terabytes of data. Now agents can` grep` through gigabytes of logs or iterate through millions of files—all through familiar bash commands.


**Agents can share data.** Multiple agents can mount the same Archil filesystem. One agent writes a file; another reads it. No coordination layer needed. The filesystem handles consistency.


**It works everywhere TypeScript runs.** Mac development machines. Fargate tasks. Lambda functions. Edge workers. If you can import a package, you can give your agent S3-backed storage.


## Getting started


[npmjs.com @archildata/just-bash Connect Archil's S3-backed filesystem to just-bash for AI agents.](https://www.npmjs.com/package/@archildata/just-bash)


Install the packages:


```text
npm   install   @archildata/just-bash   @archildata/client   just-bash
```


Or use the CLI directly:


```text
npx   @archildata/just-bash   aws-us-east-1   myaccount/mydisk
```


This drops you into an interactive bash session backed by your Archil disk. Useful for debugging what your agents will see.
