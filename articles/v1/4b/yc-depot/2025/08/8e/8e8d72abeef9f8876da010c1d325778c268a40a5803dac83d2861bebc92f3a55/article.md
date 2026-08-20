---
schema_version: "1.0.0"
document_id: "8e8d72abeef9f8876da010c1d325778c268a40a5803dac83d2861bebc92f3a55"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-runner-architecture-part-1-the-listener"
published_at: "2025-08-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:df7eae6eab6268dba4cb204528c7a8f6c3467c55fb57a7715a134eeada0a993e"
---

# GitHub Actions Runner architecture: The listener

Now that GitHub is[no longer taking contributions](https://github.com/actions/runner?tab=readme-ov-file#note) to the GitHub Actions runner codebase, I wonder if it is nearing end of life. It feels like a good time to take stock and write about how the runner itself works, including its most recent architectural changes.


The runner binary is responsible for running jobs defined in your GitHub Actions workflow. It is used by GitHub Actions in the hosted virtual environments, or you can self-host the runner on your own machine. At Depot we host the runner in highly tuned, extremely fast, ephemeral virtual machines.


The runner itself is an older C# codebase. The majority of the runner code comes from the initial commit of a fork of the[Azure Pipelines Agent](https://github.com/microsoft/azure-pipelines-agent) . The agent interacts with an Azure DevOps instance. The history of the Azure Pipelines Agent and Azure DevOps is pretty interesting, as it has gone through quite a few rebrandings and relaunches. That history is throughout the runner code with different names referring to similar concepts.


Over the last two years the runner has been slowly migrating from Azure to a different, although largely compatible, backend.


The runner has two executables, the listener and the worker; the listener gets commands from the Azure API and the worker executes job commands. We'll focus on how the listener works and its latest migrations.


```text
┌────────────────┐       ┌────────────────┐
│                  │       │                  │
│      Listener      │────▶│       Azure        │
│                  │       │                  │
└────────────────┘       └────────────────┘
│                        ▲
│                        │
▼                        │
┌────────────────┐                │
│                  │                │
│       Worker       │──────────────┘
│                  │
└────────────────┘
│
│
▼
┌────────────────┐
│                  │
│        Job         │
│                  │
└────────────────┘
```


Let's talk about how the data flows in the system first before getting into specifics.


## Registration and session setup


Before starting a runner, you first create a new "just in time" runner configuration request to the[GitHub API](https://docs.github.com/en/rest/actions/self-hosted-runners?apiVersion=2022-11-28#create-configuration-for-a-just-in-time-runner-for-an-organization) . Behind the scenes, GitHub API backend calls the Azure DevOps agent registration (See figure 1). Whereas GitHub uses the term "runner," Azure uses "agent."


Figure 1 - Agent configuration threat modeling diagram courtesy of Microsoft


GitHub returns a configuration with an Azure Pipeline API URL, an authorization URL, and a private RSA key. The RSA key signs the JWT request to the authorization URL whose response is an OAuth access token. The access token is bearer auth for requests to the Azure Pipeline API URL. This` ServerUrlV2` and the` UseV2Flow` boolean switch the runner listener away from the old Azure DevOps API to the new GitHub Actions Broker API.


```text
{
"AgentId"  :   "10450"  ,
"AgentName"  :   "my-self-hosted-runner"  ,
"DisableUpdate"  :   "True"  ,
"Ephemeral"  :   "True"  ,
"PoolId"  :   "1"  ,
"PoolName"  :   null  ,
"ServerUrl"  :   "https://pipelinesghubeus10.actions.githubusercontent.com/xxx/"  ,
"WorkFolder"  :   "/home/runner/work"  ,
"GitHubUrl"  :   "https://github.com/a-chocolate-bar"  ,
"ServerUrlV2"  :   "https://broker.actions.githubusercontent.com/"  ,
"UseV2Flow"  :   "true"
}
```


When the self-hosted runner listener process is started, it uses the runner configuration as a CLI argument. During process initialization, the runner begins by downloading Azure's ConnectionData. The ConnectionData is a REST schema. With the schema, the runner dynamically generates REST resource URL paths. You can[read](https://depot.dev/blog/reducing-queue-time-with-cached-schemas) about how we made that much faster and how it works.


Now, the runner listener sends a series of API calls to start a session and get a job to run.


Previously, the runner listener would call Azure DevOps's` /sessions` endpoint. Now, it calls the new GitHub Actions Broker API to create a session.


This API acts broadly the same as before, but the response content is slightly different.


The only fields that seem to matter during session registration are the` id` ,` name` and` version` fields. The` id` comes from the configuration returned by GitHub; the` name` comes from our runner registration; and the` version` is the version of the GitHub Actions Runner software.


If the version is too old, the Azure API will reject the request with a` 400 Bad Request` and message indicating the minimum required version. The version is inserted into the runner listener binary at build time.


```text
POST   ${ServerUrlV2}/sessions   HTTP  /  1.1
Authorization  :   Bearer {OAUTH_TOKEN}
Content-Type  :   application/json


{
"sessionId"  :   "00000000-0000-0000-0000-000000000000"  ,
"ownerName"  :   "{HOSTNAME} (PID: {PID})"  ,
"agent"  : {
"id"  :   $  {  AgentId  },
"name"  :   $  "{AgentName}"  ,
"version"  :   "2.327.1"  ,
"osDescription"  :   "Ubuntu 24.04.1 LTS"  ,
"ephemeral"  :   null  ,
"status"  :   0  ,
"provisioningState"  :   null
},
"useFipsEncryption"  :   false
}
```


In the response the only data really used is the` sessionId` . The older Azure DevOps API would echo back most of the request data, but the new GitHub Actions Broker API does not.


The` sessionId` is used for subsequent API calls to poll for job messages and to clean up the session when the runner stops.


```text
{
"sessionId"  :   "11112222-3333-4444-5555-666677778888"  ,
"ownerName"  :   "{HOSTNAME} (PID: {PID})"  ,
"assignmentQueued"  :   false  ,
"orchestrationId"  :   ""
}
```


After session creation the runner begins polling for command messages (see figure 2).


```text
GET   ${ServerUrlV2}/message?sessionId=${sessionId}&status=Online&runnerVersion=2.327.1&os=Linux&architecture=X64&disableUpdate=true   HTTP  /  1.1
Accept  :   application/json
```


The listener makes a long-polling request to the broker service. The server will hold the request open for up to 50 seconds waiting for a new job to run.


If there are no jobs, the server returns an empty body and interestingly a` 202 Accepted` . The listener will loop until there is a job to run.


When there is a job to run, the session returns a message with a` messageType` of` RunnerJobRequest` . This message has the same format as the old Azure DevOps API, but it is now sent through the broker service.


The` body` field contains an escaped JSON string that has the info we need to get the job's details. The` run_service_url` is the URL we will use to get the job and the` runner_request_id` is the unique identifier for this job request.


```text
{
"messageId"  :   1112223334445556667  ,
"messageType"  :   "RunnerJobRequest"  ,
"body"  :   "{\"runner_request_id\":\"aaaabbbb-cccc-dddd-eeee-ffff00001111\",\"run_service_url\":\"https://run-actions-1-azure-eastus.actions.githubusercontent.com/555/\",\"billing_owner_id\":\"A_bcDEFGHI_j\"}"
}
```


Now, the runner has about two minutes to acquire the job and start working on it.


Figure 2 - Command message polling diagram


### Job acquisition (recent migration)


The runner uses the` runner_request_id` to get the job details from the run service URL from the message body. The runner service is, again, a replacement for the old Azure DevOps API, and again, the response format is similar.


```text
POST   ${run_service_url}/acquirejob   HTTP  /  1.1


{
"jobMessageId"  :   "${runner_request_id}"  ,
"runnerOS"  :   "Linux"  ,
"billingOwnerId"  :   "${billing_owner_id}"
}
```


Note: the` acquirejob` API request's` jobMessageId` is the` runner_request_id` from the job message body. I'll try to keep pointing out when the names change.


The` acquirejob` response itself is very large because it is all the job instructions to execute. The instructions are sent to the` Runner.Worker` to run. We'll cover the Worker process in Part 2.


Of immediate interest in the response is the` planId` ; the` planId` is used to renew the job lock and to complete the job.


The new API's response returns the` planId` in two places; first, the` x-plan-id` header and second in the` .plan.planId` field of the response body.


## Job lock/renewal


Starting immediately after acquiring the job, the listener must start a background task to renew the job lock. This runs every minute until the job is completed or cancelled. Acting much like a heartbeat.


Like other APIs, the previous Azure DevOps API has recently been replaced by a functionally similar GitHub API.


The` renewal` endpoint requires the` planId` and` jobId` . Be aware of the name change again: the` jobId` is the` jobMessageId` from the` acquirejob` request and` runner_request_id` from the message.


```text
POST   ${run_service_url}/renewjob   HTTP  /  1.1


{
"planId"  :   "${x-plan-id}"  ,
"jobId"  :   "${runner_request_id}"
}
```


The response format has the date when the lock expires. From what I've seen, it is always ten minutes into the future. If a runner does not start the job within that time, the job will be cancelled by the server.


```text
{  "lockedUntil"  :   "2006-01-02T15:04:05.000000000Z"  }
```


## Working on the job


Once the job is acquired, the listener starts the Worker process. The Worker is responsible for executing the job commands. The Listener sends the job response to the Worker and the worker starts executing the job commands.


We'll cover the Worker process in Part 2.


## Conclusion


With the migration to the new GitHub Actions Broker API, the listener’s job hasn’t changed much in principle but the plumbing underneath it is entirely different. For anyone self-hosting runners or trying to squeeze more performance out of CI, understanding this architecture is key.


At Depot, we’ve built our own highly tuned, ephemeral GitHub Actions runners on top of this architecture to make GitHub Actions runs exponentially faster. If you’re looking to accelerate your GitHub Actions jobs, and save costs, you can check out more on how we accelerate GitHub Actions on our[product page](https://depot.dev/products/github-actions) .


In Part 2, we'll dig into the Worker process, how it executes job commands, what's changed there, and where the new architecture might be headed next.


## FAQ


Why focus on the Listener?


The Listener is the entry point for every GitHub Actions job. It registers the runner, polls for work, and hands jobs off to the Worker. Without it, no job ever starts.


Has the migration away from Azure DevOps changed how the Listener works?


Yes. The core responsibilities are the same, but it now talks to GitHub's Broker API instead of Azure DevOps endpoints. The request and response formats have shifted, and some fields are no longer returned.


Does this affect self-hosted runners?


Yes. These API changes are part of every job's lifecycle. Understanding them can help troubleshoot failures, speed up job starts, and adapt to future changes.


How does Depot fit into this?


After working with GitHub Actions infrastructure, we've developed deep expertise in making these systems performant. Understanding how GitHub Actions orchestrates jobs is helping us to build faster and more reliable CI/CD pipelines.


## Related posts


- [How we automated GitHub Actions runner updates with Claude](https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude)
- [Accelerating builds: Improve EC2 boot time](https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time)
- [GitHub Actions breaking the five second barrier](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [Self-hosted GitHub Actions runners aren't free](https://depot.dev/blog/self-hosting-github-actions)
- [Introducing Depot managed GitHub Actions Runners](https://depot.dev/blog/depot-github-actions-runners)


Chris Goller


Principal Software Engineer at Depot
