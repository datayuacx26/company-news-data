---
schema_version: "1.0.0"
document_id: "99815ad56ea12b56b4352e2ac7bc69fcf99bada5b72eda9453b70a815800fdb7"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/agentcore-migration"
published_at: "2026-07-12T00:00:00+00:00"
first_seen_at: "2026-07-23T23:50:46.032613+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:2923ef26d19127723f0c6334aba0a986c161b9fd02b57bcb10e03c2f254fd146"
---

# How We Built Secure, Scalable Agent Infrastructure with AWS Bedrock AgentCore

> **TL;DR:** just ask your agent to summarize this post for you.
>
>
> At[Browser Use](https://cloud.browser-use.com/?utm_source=blog&utm_campaign=agentcore-infra-blogpost&utm_medium=post_cta_beginning&utm_content=result_direct) we run millions of untrusted AI agents in production, each in a private micro-VM with zero credentials. Every outbound call passes through a control plane, and the whole thing scales up and down with demand.


## The architecture


Six months ago I wrote up[how we sandbox untrusted agents](https://browser-use.com/posts/two-ways-to-sandbox-agents) : zero-secret micro-VMs, a control plane holding all credentials, one image running everywhere. The abstraction is still what protects the fleet. What changed underneath is the runtime: AWS Bedrock AgentCore now provisions and schedules the per-session micro-VMs, each with a durable` /mnt/workspace` mount scoped to the session.


## A sandbox that keeps your agent's credentials safe


The VM sits in a private VPC with an execution role that can only pull its image from ECR and write logs to CloudWatch. It cannot reach S3, the database, or any other AWS API on its own. Egress is allowlisted to the control plane and to public HTTPS.


There are no secrets baked into the VM either. The runtime holds only a control-plane URL, and everything else (session token, session id, per-run flags) arrives inside the` /invocations` payload for each request, so nothing sensitive persists on the VM between runs.


Each session also gets its own micro-VM with a` /mnt/workspace` mount scoped to the session id, so sessions cannot see each other.


## Native AWS across the whole stack


The rest of our stack is AWS-native (ECS, RDS, S3, IAM, etc). Running the sandbox on AgentCore means the runtime layer belongs to the same world.


- **IAM-scoped runtime execution roles.** The runtime pulls its image from ECR and writes logs to CloudWatch using an IAM role, the same way any ECS task does.
- **Standard VPC networking.** The runtime sits inside our VPC with a dedicated security group. No inbound. Egress allowlisted to the control plane's internal ALB (LLM proxy, callbacks), to VPC endpoints for ECR and CloudWatch, and to public HTTPS for browser CDP URLs and presigned URLs.
- **AgentCore-managed session storage.** Each session gets an isolated 1 GB workspace mounted at` /mnt/workspace` , durable across stop/resume.
- **Terraform-managed.** One` aws_bedrockagentcore_agent_runtime` resource per environment, plus a standard execution role.
- **Pay per use.** No idle bill.


## Your agent never sees a real credential


Think of the control plane as a proxy service. The sandbox has no direct access to the outside world. Every request has to hop through the control plane. Need to call an LLM? Goes through the control plane. Need to upload a file to S3? Goes through the control plane. It is the only way the agent can talk to anything outside its VM.


It is a stateless FastAPI service. Every request from the sandbox carries a header with a session token. The control plane looks up the session by token, validates that it is still active, and executes the operation with real credentials.


### LLM proxying


The agent's SDK calls its normal provider endpoint, except the base URL points at the control plane (` /api/v4/llm/anthropic/v1` ,` /api/v4/llm/openai/v1` , etc). The control plane validates the session token, swaps it for the real upstream API key, meters usage, and forwards to the provider. The response comes back the same way, minus the upstream key.


The sandbox never sees a real API key or talks to a provider directly, and cost caps and billing happen on the way through.


### Agent-safe file access


Every session has an isolated 1 GB workspace at` /mnt/workspace` , backed by AgentCore's managed session storage. It is per-session, durable across stop/resume, and no other session can see it.


For files the agent needs to move in or out (session files, workspace-shared files, browser downloads), the sandbox never holds AWS credentials. It asks the control plane for a presigned URL scoped to the session:


1. Sandbox calls the CP for a presigned URL (upload or download, session-scoped).
2. CP generates the URL against its own S3 credentials.
3. Sandbox uses the URL over plain HTTPS.


Files the agent generates during a run get promoted to S3 via the control plane at run completion, so a wiped VM never loses them.


### Session state survives stop and resume


The sandbox is designed to be disposable, but some per-session state is expensive to rebuild. Bcode's SQLite state (` bcode.db` ) lives on the durable` /mnt/workspace` mount, so it survives:


- Idle stops (the 15-minute idle timeout).
- Session recycles (the 8-hour max lifetime).


It's only wiped on a runtime version bump or when a session has been idle for 14 days. Below that, the control plane is the cold-recovery source of truth. The durable mount is a cache that makes warm resumes cheap.


## Scales from zero to thousands of concurrent agents


Our workload is spiky. Agent runs cluster hard, we can go from a quiet minute to thousands of concurrent sandboxes and back again in a short window.


Against a curve like that, a fixed fleet is the wrong shape. If you size it for peak, you pay for capacity that sits idle most of the time. If you size it for the average, you drop or queue traffic at the spikes.


AgentCore treats each agent session as its own primitive. Sessions boot on demand, stop when idle, and bill only for active session time. Capacity tracks the demand curve instead of sitting above it.


The control plane scales independently. It runs on ECS Fargate in private subnets behind an ALB and auto-scales on CPU. Sandboxes scale independently through AgentCore. Each session gets its own VM, and AgentCore handles the scheduling.


## Running it on AgentCore


For the curious, here is what the AgentCore integration actually looks like end-to-end.


Sessions are scoped to our own session id (` runtimeSessionId = session_id` ), so AgentCore's session concept and ours are the same thing. One session id gives us one warm VM, one workspace, and one lifecycle.


The runtime is driven by exactly two boto3 calls against the` bedrock-agentcore` client:


```text
# Dispatch a task into an agent session.
client.invoke_agent_runtime(
agentRuntimeArn  =  RUNTIME_ARN  ,
runtimeSessionId  =  session_id,
payload  =  json.dumps(invocation).encode(),
contentType  =  "application/json"  ,
accept  =  "application/json"  ,
)


# Kill an in-flight session.
client.stop_runtime_session(
agentRuntimeArn  =  RUNTIME_ARN  ,
runtimeSessionId  =  session_id,
)
```


Session context arrives inside the` /invocations` payload, not on the VM's environment. Two runs on the same VM can carry entirely different context, and neither persists after the request returns.


Two settings matter in prod:


- **Idle timeout** , 15 minutes of no activity.
- **Max lifetime** , 8 hours. Hard ceiling per session.


## The takeaway


The abstraction we bet on in the[sandbox post](https://browser-use.com/posts/two-ways-to-sandbox-agents) is holding up. Zero-secret micro-VMs and a control plane holding all credentials, that part has not moved. What changed is that the runtime layer is now native AWS too.


For us the case is:


- Elastic scale that tracks a spiky demand curve, without a fleet to size by hand.
- IAM, VPC, S3, CloudWatch stack all the way down. One less foreign concept in the critical path.
- A per-session isolated micro-VM with durable state, sitting in a private VPC with zero credentials of its own.


Same takeaway as the original: your agent should have nothing worth stealing and nothing worth preserving.


If you want to run Browser Use agents on this architecture without hosting it yourself, that is what we ship at[Browser Use Cloud](https://cloud.browser-use.com/?utm_source=blog&utm_campaign=agentcore-infra-blogpost&utm_medium=post_cta_ending&utm_content=result_direct) .
