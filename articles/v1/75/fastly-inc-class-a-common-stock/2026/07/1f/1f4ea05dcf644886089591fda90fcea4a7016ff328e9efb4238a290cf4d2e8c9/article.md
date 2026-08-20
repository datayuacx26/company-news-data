---
schema_version: "1.0.0"
document_id: "1f4ea05dcf644886089591fda90fcea4a7016ff328e9efb4238a290cf4d2e8c9"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/optimizing-compute-agentic-systems-new-request-handoff-functionality/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:d94da03de94380e231bdf9f3f93e8bf763287d220eb9bbe85ee2659178fec5b5"
---

# Optimizing Compute for Agentic Systems With New Request Handoff Functionality

A lot of the engineering work we have done over the past few years has centered around helping customers reduce CPU usage and wall clock time. A few months ago we[updated our pricing model](https://www.fastly.com/blog/shrink-your-bill-with-efficient-software) to reflect this, removing the duration meter to focus strictly on requests and vCPU consumption. This new model is giving customers more control so that when they write more efficient code, they directly reduce their spend.


This drive for efficiency is also reflected in some of our recent customer-driven engineering work where we were presented with the need to support longer-running backend requests, particularly for computationally intensive agentic workflows, which cause services to hit their wall clock limit.


Because proxying requests to Large Language Models (LLMs) takes time, we wanted to give developers a way to wait for these responses without tying up their available edge resources. Addressing this need has evolved into a broader platform improvement that we will continue to expand on. The improvement is a[new request handoff functionality](https://docs.rs/fastly/0.13.0/fastly/http/request/struct.PendingRequest.html#method.send_to_client) , and it is available now in version 0.13.0 of the Fastly Rust SDK.


## Managing Wasm guest limits and long-running requests


Compute typically enforces a 2-minute wall clock timeout for Wasm guests. This limit is intentional to protect resources and ensure a healthy turnover of requests. The design also works perfectly for the vast majority of microservices and edge logic where we maintain a 1:1 mapping of running Wasm guests to incoming requests.


However, this architecture can sometimes become problematic when waiting on slow or non-responsive backends. This may be due to high load at the origin slowing down all in-flight requests, or it can be due needing to finish intensive computation to generate a response. Sometimes, it might not even be something within the origin’s control, such as a long-running POST request that takes a while to write the body bytes to the origin due to a large upload or a slow trickle of bytes from a mobile client.


(Note: Wasm guests are compiled Wasm modules that run inside a Wasm host. Wasm guests are executed in a strict sandbox so the host can safely execute untrusted code without allowing it to compromise the underlying system.)


**The solution:**` PendingRequest::send_to_client`


To better support these long-running backend requests and increase RPS throughput, we’ve introduced a new` PendingRequest::send_to_client` capability. With this new functionality, we can send an asynchronous request to an origin and if it is a direct pass or[hit-for-pass](https://www.fastly.com/documentation/guides/concepts/cache/request-collapsing/#hit-for-pass) request we can hand it off from the guest to the host while it’s still in flight—even before we know the final response. We then instruct the platform to pass the response back to the downstream client whenever it eventually resolves. Because the request continues running in the background, the Wasm guest can terminate early and free up its resources for future requests. This approach drastically increases RPS throughput and protects Compute services from exhausting their available Compute instances by waiting on long-running requests to complete.


Hello World example:


Copied!


```text
use fastly::{Error, Request};


fn main() -> Result<(), Error> {
// 1. Receive the incoming downstream request from the client:
let req = Request::from_client();


// 2. Dispatch the direct pass request asynchronously to your slow backend:
let pending_req = req.with_pass(true).send_async("my_slow_origin_backend")?;


// 3. Hand off the unresolved, in-flight request directly to the host daemon:
pending_req.send_to_client()?;


// 4. Terminate the guest cleanly.
// The host daemon takes over, monitoring the request and piping the eventual
// response back downstream completely independently.
Ok(())
}
```


**Key use cases**


-


**Heavy computational AI gateways and long-running LLM loops:** Earlier this year, we explored how developers are building secure,[low-latency AI Agent loops](https://www.fastly.com/blog/ai-agents-fastly-compute-how-it-works-what-makes-it-secure) directly on Fastly Compute. In an agentic workflow, an edge service often coordinates multiple iterations of reasoning, tool calls, and external model prompts. While our runtime provides an exceptionally safe environment to run these autonomous agents, heavy text-generation tasks or complex processing from upstream LLM providers can scale processing times dramatically before headers are generated. By repurposing those connection patterns with the new API in the Rust SDK, you can use Fastly Compute as an intelligent, high-throughput AI gateway. Your edge application can authorize the request via dynamic backends, evaluate client permissions, or inject semantic caching layers, and then fire the heavy LLM call asynchronously.


-


**Shielding optimization:** Most architectures route initial requests to an edge POP, which then proxies to a shield POP where heavy manipulation occurs. The edge POP frequently does not modify the final response, but instead simply passes the shield POP’s response straight through. With pending request handoff, the edge POP can fire the shield request asynchronously, hand it off, and instantly terminate its local guest footprint—drastically minimizing total wall clock time at your closest edge POPs.


**Figure 1:** The graph shows a Compute service that shields to NYC and receives steady traffic in Brisbane (BNE) handing off its shield requests. The wall clock time in BNE almost disappears after the handoff functionality is used.


-


**Long-Running Uploads:** If your service receives incoming content at the edge from low-bandwidth clients, then using this feature allows your service to avoid hitting guest wall clock timeouts by allowing the platform to continue the upload independently of the Wasm guest’s execution.


## Header modification and error handling


This new functionality also includes the ability to modify headers on the eventual response. You can queue up changes to remove or insert specific headers before doing the actual handoff. This allows you to specify changes like “remove this header name,” or “insert this header with *X* value”, which will be done once the response arrives:


Copied!


```text
use fastly::{Error, Request};
use fastly::http::request::PendingResponseKind;


fn main() -> Result<(), Error> {
// 1. Receive the incoming downstream request from the client:
let req = Request::from_client();


// 2. Dispatch the direct pass request asynchronously to your slow backend:
let mut pending = req.with_pass(true).send_async("my_slow_origin_backend")?;


// 3.1 Remove the `Server` header added by the origin's application server:
pending.remove_response_header("Server", PendingResponseKind::Response);


// 3.2 Insert the current service version for debugging:
pending.set_response_header(
"X-Service-Version",
fastly::compute_runtime::service_version().to_string(),
PendingResponseKind::Any
);


// 4. Hand off the unresolved, in-flight request directly to the host daemon:
pending.send_to_client()?;


// 5. Terminate the guest cleanly.
// The host daemon takes over, monitoring the request and piping the eventual
// response back downstream completely independently.
Ok(())
}
```


Note the use of the[PendingResponseKind](https://docs.rs/fastly/0.13.0/fastly/http/request/enum.PendingResponseKind.html) type in the above example. This allows the service to control whether the header changes are applied to origin responses` PendingResponseKind::Response` , synthetic error responses generated when the in-flight request fails` PendingResponseKind::Error` , or both` PendingResponseKind::Any` . In the cases where the in-flight request failed, then an appropriate 5XX response will be generated and sent back to the client. Typically this will be one of:


-


504 Gateway Timeout: Returned automatically if the connection to your origin times out or the First Byte Timeout for the backend is reached.


-


502 Bad Gateway: Returned if the origin returns an incomplete or malformed response body, the backend has an invalid TLS certificate, or is otherwise not functioning and responding properly.


You can use the “Backend Requests & Errors” graph in the Observability dashboard of the Fastly Control Panel to see what kinds of backend errors are responsible for 502 and 504 responses. See[Backend Request Errors](https://www.fastly.com/documentation/reference/compute/errors/#backend-request-errors) for more information.


## Get started today


You don’t need to wait until you’ve deployed your service to production to see how this new functionality can benefit you! It is fully supported in recent releases of[Viceroy](https://github.com/fastly/viceroy) , our local testing environment. You can emulate long-running backend timeouts, refactor your code to use` Request::send_async/PendingRequest::send_to_client` , and debug your queued header operations right from your local development terminal. While the handoff functionality is currently exclusive to our Rust SDK (v0.13.0) we will be expanding it to the rest of our[official SDKs](https://www.fastly.com/documentation/reference/compute/sdks/) . Also, be sure to check out our[documentation](https://docs.rs/fastly/0.13.0/fastly/http/request/struct.PendingRequest.html#method.send_to_client) for more information and let us know what you are building in our[forum](http://community.fastly.com/) .
