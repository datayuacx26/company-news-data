---
schema_version: "1.0.0"
document_id: "136a5ab32dd28010668ee4b90a75b6d915ae0b5c1f1b0c87a5681c1cafa64d86"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/tasks-in-v3/"
published_at: "2024-12-29T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:42a1d6c72ddeedb0e4edb456f74491ff8c69b9e026a432f61d0c7e91dae47c7f"
---

# Tasks in v3

We are adding[Task](https://sst.dev/docs/component/aws/cluster#tasks) , a new component, powered by AWS Fargate that allows you to run asynchronous tasks in your apps. Here’s a video where we talk about this and async jobs in general.


[Play](https://youtube.com/watch?v=3PJM7mtF-eo)


## Background


Most applications have a need to run some background tasks. Typically these take a long time to run so they are triggered asynchronously. Or they are invoked through a cron job. Unfortunately you can’t run them in a Lambda function because they might take longer than 15 minutes.


And since these are triggered asynchronously, they can be harder to test locally. You can mock their invocation but it’d be much better to test them through the usual flow of your application.


To fix this, we are adding the new[Task](https://sst.dev/docs/component/aws/cluster#tasks) component.


---


## Task


1. It uses AWS Fargate that can **run as long** as you need and is **cheaper than Lambda** .
2. Can be invoked directly from a **cron job** .
3. Comes with a **JS SDK** , but can also be invoked with the AWS SDK.
4. Has its **own dev mode** , so it can be invoked remotely but it’ll run locally.


You can[check out an example](https://sst.dev/docs/examples/#aws-task) if you want a quick start.


---


## Getting started


Tasks are built on AWS Fargate and are tied to an Amazon ECS cluster. And so` Task` is created as a part of the` Cluster` component.


#### Create a task


sst.config.ts


```text
const   cluster   =   new     sst  .  aws  .  Cluster  (  "  MyCluster  "  , {   vpc   }  );    const   task   =   new     sst  .  aws  .  Task  (  "  MyTask  "  , {   cluster   }  );
```


By default, this looks for a` Dockerfile` in the root directory. You can configure this.


sst.config.ts


```text
new   sst  .  aws  .  Task  (  "  MyTask  "  , {         cluster,         image: {           context:   "  ./app  "  ,           dockerfile:   "  Dockerfile  "  ,         },    });
```


---


#### Run the task


Once created, you can run the task through:


1.


**Task SDK**


With the[Task JS SDK](https://sst.dev/docs/component/aws/task#sdk) , you can run your tasks, stop your tasks, and get the status of your tasks.


You can call this from your functions, frontends, or container services. For example, you can link the task to a function.


sst.config.ts


```text
new   sst  .  aws  .  Function  (  "  MyFunction  "  , {         handler:   "  src/lambda.handler  "  ,         link: [task],    });
```


Then from your function start the task.


src/lambda.ts


```text
import   { Resource }   from     "  sst  "  ;    import   { task }   from     "  sst/aws/task  "  ;
const   runRet   = await   task  .  run  (Resource  .  MyTask  );    const   taskArn   =   runRet  .  tasks  [  0  ]  .  taskArn  ;
```


**Other languages**


The JS SDK is calling the AWS ECS SDK behind the scenes. So if you are using a different language, you can directly call the AWS SDK. Here’s[how to run a task](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) .


2.


**Cron jobs**


You can also connect your task to a[Cron](https://sst.dev/docs/component/aws/cron) job.


sst.config.ts


```text
new   sst  .  aws  .  Cron  (  "  MyCronJob  "  , {         task,         schedule:   "  rate(1 day)  "  ,    });
```


This works by connecting the task to the cron job through EventBridge.


---


## Dev mode


You can test your tasks locally in` sst dev` in a similar way to how you test your functions[Live](https://sst.dev/docs/live/) .


Any tasks that are invoked remotely are proxied to your local machine that runs the` dev.command` you have. These also show up under the **Tasks** tab in the multiplexer sidebar.


sst.config.ts


```text
new   sst  .  aws  .  Task  (  "  MyTask  "  , {         dev: {           command:   "  node src/tasks.js  "  ,         },    });
```


If your` Vpc` has` bastion` enabled, then your tasks have access to resources in your VPC as well.


---


## Console logs


The[Console](https://sst.dev/docs/console/) supports viewing logs for your tasks when they are in production.


---


## Cost


You are only charged for the time it takes to run the task. With the default memory and vCPU it costs roughly **$0.02 per hour** .


When running in` sst dev` , you are charged for the time it takes to run the task locally as well.


---


## Next steps


Learn more in our docs.


- [Adding a task](https://sst.dev/docs/component/aws/cluster/#tasks)
- [Dev mode](https://sst.dev/docs/component/aws/cluster/#dev-1)
- [JS SDK](https://sst.dev/docs/component/aws/task/#sdk)
- [Cost](https://sst.dev/docs/component/aws/cluster/#cost)


And check out these examples.


- [Invoking a task with a function](https://sst.dev/docs/examples/#aws-task)
- [Invoking a task with a cron job](https://sst.dev/docs/examples/#aws-task-cron)


---


#### Comparison to v2


If you are coming from SST v2, there are a couple of differences between` Task` and[Job](https://v2.sst.dev/constructs/Job) .


1. ` Task` is based on AWS Fargate.` Job` used a combination of AWS CodeBuild and Lambda.
2. Since` Task` is natively based on Fargate, you can use the AWS SDK to interact with it, even in runtimes the SST SDK doesn’t support.
3. In dev mode,` Task` uses Fargate only, whereas` Job` used Lambda.
4. While CodeBuild is billed per minute, Fargate is a lot cheaper than CodeBuild. Roughly **$0.02 per hour** vs **$0.3 per hour** on X86 machines.
