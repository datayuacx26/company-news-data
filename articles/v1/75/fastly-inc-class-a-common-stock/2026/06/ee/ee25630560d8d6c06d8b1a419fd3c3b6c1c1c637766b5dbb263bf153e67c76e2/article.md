---
schema_version: "1.0.0"
document_id: "ee25630560d8d6c06d8b1a419fd3c3b6c1c1c637766b5dbb263bf153e67c76e2"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/python-sdk-beta-how-language-ai-runs-faster-safer-with-fastly/"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:7b7b5e0c4f256b89a2f513393070e782fdc1ff4e3bf6b49450921fdc5e8e7974"
---

# Python SDK Beta: How the Language of AI Runs Faster and Safer with Fastly

Your backends are written in Python. You’re building AI agents in Python. Wouldn’t it make sense to write your edge code in Python too? Now you can, with the beta release of the[Fastly Compute Python SDK](https://github.com/fastly/compute-sdk-python#fastly-compute-python-sdk) !


Because Fastly runs one of the largest, fastest, most secure networks in the world, we can give your[growing body of Python code](https://survey.stackoverflow.co/2025/technology#most-popular-technologies-language-language-prof-ai) every possible advantage. Whether you are vibe-coding your first AI project, validating access control tokens, or doing something we haven’t yet imagined, this new SDK moves your Python to the network’s edge, close to your users. And it does so with industry-leading speed and security.


**Standard CPython, Standard Frameworks**


For maximum compatibility, we cross-compile standard CPython so it runs inside our isolated WebAssembly sandboxes. Then, we provide an adapter to run standard[WSGI](https://wsgi.readthedocs.io/en/latest/what.html) apps atop our high-performance HTTP implementation. If you use popular Python web frameworks like Flask — or even an unusual one, as long as it conforms to WSGI — your code can run on our edge nearly unmodified and feel fully idiomatic. Pure Python modules also work great right out of the box.


A simple hello-world Flask application running on Fastly Compute looks like this:


Copied!


```text
from fastly_compute.wsgi import WsgiHttpIncoming
from flask import Flask


app = Flask(__name__)


@app.route("/")
def hello_world():
return "Hello from Python, living on the Fastly edge!"


# The only special line: register the Flask app as the recipient
# of incoming requests, using our WSGI adapter.
HttpIncoming = WsgiHttpIncoming(app)
```


## How We Make Python Fast


If you caught our recent talk at Wasm I/O on[Componentizing Fastly Compute ,](https://www.youtube.com/watch?v=s2XLrjl3BO4) you know that getting runtime-heavy languages to perform well at the edge requires serious engineering work. We shook out a diverse bag of tricks to eliminate CPython’s typically slow startup and present the quickest possible performance to the end user.


-


**Memory Snapshotting:** In the common case, a great deal of Python code, like module top levels, executes at startup, introducing massive cold-start delays. Fastly completely bypasses this. We let Python do that execution *at build time* , then create a pre-initialized snapshot of memory. In production, we instantiate from this snapshot within microseconds, allowing it to start up even faster than ordinary Python on a local machine.


-


**Data Loops at the Edge vs. The Cloud:** For real-time applications and AI agents, proximity matters. If the use of your application involves a series of network round trips to the user — for instance, checking a context, calling a tool, then grabbing a secret — shuttling back and forth to a centralized cloud server kills performance. Keeping that data-orchestration loop localized near the user, care of Fastly’s edge network, keeps the experience snappy and the customer happy.


## How We Make Python Secure


The conversation around AI has evolved significantly in a very short time. As we saw across the dedicated AI and Security tracks at PyCon 2026 last month, the focus has shifted from mere prompt engineering to moving autonomous, agentic workflows into production.


This introduces enormous engineering challenges because, as a very dynamic language, Python has historically been hard to sandbox. If you are running LLM-generated code that isn’t fully trustworthy, doing so inside your core infrastructure poses major security risks. Conversely, Fastly’s isolation model offers a bulletproof place to host that code. We isolate every single request execution into its own ephemeral WebAssembly sandbox.


If code does something unintended, whether a mistake or malice, the blast radius is limited to our network edge, completely out of reach from your critical backend infrastructure. In addition, many exploits, like the recent[Copy Fail](https://www.fastly.com/blog/why-your-code-is-safe-from-copy-fail-on-fastly-compute) and its brethren, are impossible on our platform because the kernel API surface area is protected — not the case for typical container-based isolation.


## How We Keep Python Beautiful


On Fastly, you don't have to write **clunky cross-language wrapper code** . By leveraging WebAssembly Components and the[WIT](https://component-model.bytecodealliance.org/design/wit.html) IDL, we provide type-safe APIs that look, feel, and act like pure, modern Python, even though they mate up to high-performance native code on the backend.


Beyond the WSGI support mentioned earlier, we make additional HTTP requests familiar and quick by giving you a **compatible implementation of the de-facto-standard**` **requests**` **library** that delegates to our optimized network stack. Just import it like so…


Copied!


```text
from fastly_compute import requests
```


... and then use it to make HTTP calls as you normally would.


### Current Caveats


Because this is a beta and because of our focus on security and speed, please keep these caveats in mind:


-


**Native ML computation frameworks aren’t here yet** . NumPy, SciPy, and Pandas need some additional work before they’ll run in our environment. Pure Python logic and any native extensions that support[WASI](https://wasi.dev/) are fair game today.


-


**Top-level imports needed** . So our memory-snapshotting build process can snag them, packages must be imported at the top level of some module. (If you have third-party code that uses inner imports, you can work around this by importing them at the top level elsewhere.)


-


**No filesystem or threads** . If you are loading templates off disk or using multiple threads within a web request, your code will need adjustment. There isn’t yet a virtual filesystem or that sort of parallelism within our sandboxes.


## Get started today


Ready to bring your Python code to the edge? Here’s how to get started:


-


Create a[Fastly account](https://www.fastly.com/signup) (if you do not already have one), and start using the free tiers.


-


[Follow the instructions here](https://github.com/fastly/compute-sdk-python/tree/main#quick-start) to write some Python code and run it on Fastly’s network.


-


[Shape what’s coming next](https://community.fastly.com/t/python-sdk-beta-now-available/4424) : We want your unfiltered feedback.[Open Source](https://www.fastly.com/fast-forward) is critical to the work we do at Fastly, and this SDK is an open collaboration for folks using Python.


We encourage submitting[pull requests or open issues on the GitHub repo](https://github.com/fastly/compute-sdk-python/blob/main/CONTRIBUTING.md) . Let us know any Python packages that are giving you trouble, what you are building, and how we can refine the developer experience on our way to General Availability!
