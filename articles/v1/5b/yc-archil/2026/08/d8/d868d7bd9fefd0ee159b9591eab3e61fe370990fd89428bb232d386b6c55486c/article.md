---
schema_version: "1.0.0"
document_id: "d868d7bd9fefd0ee159b9591eab3e61fe370990fd89428bb232d386b6c55486c"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/the-file-system-is-the-sandbox"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:bbb88314a10c3349b85c6ecc81ed68b0fd1e8ebb460b569e915e90dd6bf5ce14"
---

# The file system is the sandbox

Let's start by thinking about the Ship of Theseus, which asks the question "if you replace every plank in a ship, at what point does it become a different ship?". This is kind of hard to answer, but surprisingly, with computers it's simpler.


If you, over the course of a month, replaced every component in your personal computer with an identical component, at what point would it not be "your computer".


The answer here should be obvious, it stops being "your computer" when your data -- your keys, your files, your programs -- are the piece that gets wiped. In fact, if you manage to keep your data across this transition, then it's still your computer.


This simple thought exercise reveals a surprising truth about cloud computing and sandboxes. The compute itself was never the identify of the machine. And yet, every cloud (AWS --> EC2, Fly --> machines, Sandbox providers --> sandboxes) is focused around how to make you think about launching and managing the lifecycle of compute products.


This model meant that the users had to figure out how to use this compute in an ephemeral way, to treat their compute as "cattle not pets", and to figure out how to get their data (the thing that actually matters) to the box itself.


Over the past several months, my friends have struggled to watch me draw the same diagram over and over and over again.


The underlying reason why clouds work this way, forcing you to figure out how to get things to amnesic servers, is because they don't have an easy to use, infinite, multi-attach state storage system -- a file system.


Over the past few months, this problem has just been exacerbated with the need for AI agents to launch sandboxes to run arbitrary code, bash, and POSIX commands. These products work great, but they mean that you (the user) need to think through how to get your data into and out of them, how you manage the lifecycle of these machines, and making sure that you fully utilize them to avoid billing surprises.


The people who are building the next generation of agents don't have time to think about these issues. To solve this, we propose a new model, a model that honors the fact that the "identity" of a server is really it's data.


We see the storage -- the file system -- as the central "resource" in the next-generation cloud, not servers. Users can put their data into this file system, using a variety of tools -- including lazily synchronizing it from origin services like S3 -- and then they can run compute on the file system when they want to either extract information from it or edit it in some way.


Because the file system is multi-attach, users can run many of these "compute functions" in parallel, each function getting its own, dedicated CPU and RAM allotment. Because the function only runs to completion, users are only billed for the amount of time that they are actively using the compute.


We call this model Serverless Execution, and we're excited to roll it out to customers over the next few days. Let me give you a peak of how it works.


```text
import   { Archil }   from   '@archildata/client'


// Create a disk, backed by your s3 bucket
const   disk   =   await   Archil  .  disks  .create  ({
name  :   'agent-workspace'  ,
mounts  :   [{ type  :   's3'  ,   bucketName  :   'agent-bucket'  }]  ,
})


const   {   fileList   }   =   await   disk  .exec  (  "ls"  )


const   grepPromises   =   fileList  .map  (file   =>
disk  .exec  (  "grep ERROR "   +   file)
)


const   results   =   Promise  .all  (grepPromises)
```


We're launching a new ".exec" function in our Javascript client that allows users to run arbitrary code on the file system directly, in the Archil cloud.


This example, for instance, allows a user to do a full map-reduce full text search across an entire bucket. Each exec gets it's own container, allowing you to do a full scale-out search that's faster than grep and faster than ripgrep.


This power also allows you to have agents that work directly on the file system. For instance, you might want to use "exec" as a way to provide a bash tool to an agent running locally or in the cloud:


```text
import   { generateText  ,   tool }   from   'ai'
import   { z }   from   'zod'
import   { Archil }   from   '@archildata/client'


const   disk   =   await   Archil  .  disks  .create  ({
name  :   'agent-workspace'  ,
mounts  :   [{ type  :   's3'  ,   bucketName  :   'agent-bucket'   }]  ,
})


const   bash   =   tool  ({
description  :
'Run a shell command inside the Archil-backed workspace disk and return stdout/stderr.'  ,
inputSchema  :   z  .object  ({
command  :   z  .string  ()  .describe  (  'Shell command to execute, e.g. ls, cat file.txt, grep ERROR app.log'  )  ,
})  ,
execute  :   async   ({ command })   =>   {
const   result   =   await   disk  .exec  (command)


return   {
stdout  :   result  .stdout   ??   ''  ,
stderr  :   result  .stderr   ??   ''  ,
exitCode  :   result  .exitCode   ??   0  ,
fileList  :   result  .fileList   ??   undefined  ,
}
}  ,
})


const   result   =   await   generateText  ({
model  :   'openai/gpt-5.2'  ,
prompt  :   'List files, then look for ERROR lines.'  ,
tools  :   { bash }  ,
maxSteps  :   10  ,
})


console  .log  (  result  .text)
```


We think that the possibilities are endless here, and they will allow our users to accelerate how they build the next-generation of agents without needing to think through moving their data or managing sandbox lifecycles.


Serverless execution will be available to Archil users over the next few days, please DM if you want early access.
