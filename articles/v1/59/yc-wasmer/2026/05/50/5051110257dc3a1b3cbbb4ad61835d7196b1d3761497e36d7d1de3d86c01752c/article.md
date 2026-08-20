---
schema_version: "1.0.0"
document_id: "5051110257dc3a1b3cbbb4ad61835d7196b1d3761497e36d7d1de3d86c01752c"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/python-on-the-edge-powered-by-webassembly"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:b798f32a1f813a19a17bf1c5e97d598b153e5a45cedd38f897ae6720fdb33385"
---

# Python on the Edge: Fast, sandboxed, and powered by WebAssembly

With AI workloads on the rise, the demand for Python support on WebAssembly on the Edge has grown rapidly.


However, bringing Python to WebAssembly isn't trivial as it means supporting native modules like` numpy` ,` pandas` , and` pydantic` . While projects like[pyodide](https://pyodide.org/en/stable/) made strides in running Python in the browser via WebAssembly, their trade-offs don't fully fit server-side needs.


After months of hard work, today we're thrilled to announce **full Python support in Wasmer Edge (Beta)** powered by WebAssembly and[WASIX](https://wasix.org/) .


Now you can run **FastAPI, Streamlit, Django, LangChain, MCP servers and more** directly on Wasmer and Wasmer Edge! To accomplish it we had to:


- Add support for **dynamic linking** (` dlopen` /` dlsym` ) into WASIX
- Add` libffi` support (so Python libraries using` ctypes` could be supported)
- Polish **Sockets and threading** support in WASIX
- Release our own[Python Package Index](https://pythonindex.wasix.org/) with many of the **most popular Python Native libraries** compiled to WASIX
- Create our own alternative to[Heroku Buildpacks](https://devcenter.heroku.com/articles/buildpacks) /[Nixpacks](https://nixpacks.com/docs/getting-started) /[Railpack](https://railpack.com/) /[Devbox](https://www.jetify.com/devbox) to automatically detect a project type from its source code and deploy it (including running with Wasmer or deploying to Wasmer Edge!). Updates will be shared soon!


## How fast is it?


This Python release is much faster than any of the other Python releases we did in the past.


It is fast. Insa…natively fast (it's even faster than our[py2wasm project](https://wasmer.io/posts/py2wasm-a-python-to-wasm-compiler) !)


```text
$ wasmer run python/python@=0.2.0 --dir=. -- pystone.py
Pystone(1.1) time for 50000 passes = 0.562538
This machine benchmarks at 88882.9 pystones/second
$ wasmer run python/python --dir=. -- pystone.py # Note: first run may take time
Pystone(1.1) time for 50000 passes = 0.093556
This machine benchmarks at 534439 pystones/second
$ python3 pystone.py
Pystone(1.1) time for 50000 passes = 0.0827736
This machine benchmarks at 604057 pystones/second


```


*timings from running in a M3 max laptop, mileage may vary


That's 6x faster, and **nearly indistinguishable from native Python performance** … quite good, considering that your Python apps can now run fully sandboxed anywhere!


*Note: the first time you run Python, it will take a few minutes to compile. We are working to improve this so no time will be spent on compilation locally.*


> 🚀 Even faster performance coming soon: we are trialing an optimization technique that will boost Python performance in Wasm to[95% of native Python speed](https://x.com/wasmerio/status/1934923052896096639) . This is already powering our[PHP server](https://speakerdeck.com/syrusakbary/making-php-extremely-fast-in-webassembly) in production. Result: Near-native Python performance, fully sandboxed. Stay tuned!


## What it can run


Now, you can run any kind of Python API server, powered by` fastapi` ,` django` ,` flask` , or` starlette` , connected to a MySQL database automatically when needed ([FastAPI template](https://wasmer.io/templates/fastapi-starter?intent=at_VnP7IptLCpKm) ,[Django template](https://wasmer.io/templates/django-starter?intent=at_WK0DIkt3CeKX) ).


You can run` fastapi` with **websockets** ([example repo](https://github.com/wasmer-examples/python-fastapi-websockets) ,[demo](https://python-fastapi-websockets.wasmer.app/) ).


You can run` mcp` servers ([deploy using our MCP template](https://wasmer.io/templates/mcp-chatgpt-starter?intent=at_vRxJIdtPCbKe) ,[demo](https://www.loom.com/share/9c003076d8884ed994cbd04141d5fdee) ).


You can run image processors like` pillow` ([example repo](https://github.com/wasmer-examples/python-pillow) ,[demo](https://python-pillow-example.wasmer.app/) ).


You can run` ffmpeg` inside Python ([example repo](https://github.com/wasmer-examples/python-ffmpeg) ,[demo](https://python-ffmpeg-example.wasmer.app/) ).


You can run` streamlit` and` langchain` ([deploy using our LangChain template](https://wasmer.io/templates/langchain-streamlit-starter?intent=at_oR36ILtOC6nZ) ,[demo](https://langchain-starter-template.wasmer.app/) ).


You can run` sqlalchemy` with FastAPI ([deploy using our FastAPI + SQLAlchemy template](https://wasmer.io/templates/fastapi-sqlalchemy?intent=at_ynYYILtNCZnX) ,[demo](https://fastapi-sqlqlchemy-example.wasmer.app/) ).


You can even run` pypandoc` ! ([example repo](https://github.com/wasmer-examples/python-pandoc-converter) ,[demo](https://pandoc-converter-example.wasmer.app/) ).


Soon, we'll have full support for:


- ` curl_cffi`
- ` polars`
- ` Pytorch`
- ` greenlet`


> Update! Greenlets are now fully supported in Wasmer, read more about it here:[https://wasmer.io/posts/greenlet-support-python-wasm](https://wasmer.io/posts/greenlet-support-python-wasm)


# Wasmer VS alternatives


Python on Wasmer Edge is just launching, but it's already worth asking: how does it stack up existing solutions?


### Quick Comparison


Feature / Platform **Wasmer Edge** **Cloudflare** **AWS Lambda**


**Versioned Native modules** (` numpy` ,` pandas` , etc.) ✅ Supported[*](https://pythonindex.wasix.org/) ❌ Limited[*](https://pyodide.org/en/stable/usage/packages-in-pyodide.html) ✅ Full support


**Concurrency Model** ✅ Fluid (multi threaded) ✅ Fluid (single threaded[*](https://developers.cloudflare.com/workers/reference/how-workers-works/#distributed-execution) ) ❌ One request per instance


**Multithreading & multiprocessing** (` ffmpeg` ,` pandoc` ) ✅ Yes ❌ No ✅ Yes


**ASGI / WSGI** frameworks (` uvicorn` ,` daphne` ) ✅ Supported ❌ Patched / limited ⚠️ Needs wrappers


**WebSockets** (` streamlit` ) ✅ Yes ❌ No¹ ❌ No


**Raw sockets** (` libcurl` ) ✅ Supported ❌ JS` fetch` only² ✅ Supported


**Greenlets** (needed for` sqlalchemy` ) ✅ Yes ❌ No ✅ Yes


**Multiple Python versions** ✅ 3.12, 3.13 (3.14 in roadmap) ❌ Tied to bundled runtime ✅ Supported


**Cold starts** ⚡ Extremely fast ⏳ Medium (V8 isolates) ⏳ Slow


**Code changes** required ✅ None ⚠️ Some ⚠️ Wrappers


**Pricing** 💰 Affordable 💰 Affordable 💰 Higher


### Cloudflare Workers (Python) / Pyodide


> ℹ️ Most of the demos that we showcased on this article, are not runnable inside of Cloudflare:` ffmpeg` ,` streamlit` ,` pypandoc` .


Cloudflare launched Python support ~18 months ago, by using **Pyodide** inside[workerd](https://github.com/cloudflare/workerd) , their JavaScript-based Workers runtime.


While great for browser-like environments, Pyodide has trade-offs that make it less suitable server-side. Here are the limitations when running Python in Cloudflare:


- ❌ No support for` uvloop` ,` uvicorn` , or similar event-native frameworks (JS event loop patches break compatibility with native).
- ❌ No **[pthreads](https://github.com/pyodide/pyodide/issues/237)** or multiprocessing support, you can't call subprocesses like` ffmpeg` or` pypandoc`
- ❌ No raw HTTP client sockets ([HTTP clients are patched](https://developers.cloudflare.com/workers/languages/python/packages/#http-client-libraries) to use JS` fetch` , no` libcurl` available).
- ❌ Limited to a bundled Python version and package set.
- ❌ No support for` greenlet` package.
- ⏳ Cold starts slower due to V8 isolate warmup.


**Why the limitations?** Cloudflare relies on Pyodide: great in-browser execution, but server-side it implies no sockets, threads, or multiprocessing. The result: convenient for lightweight browser use, but might not be the best fit for real Python workloads on the server.


> In contrast, Wasmer Edge runs **real Python on WASIX** unmodified, so everything "just works", with near-native speed and fast cold starts.


### Amazon Lambda


AWS Lambda doesn't natively run unmodified Python apps:


- ❌ You need adapters (such as[https://github.com/slank/awsgi](https://github.com/slank/awsgi) or[https://github.com/Kludex/mangum](https://github.com/Kludex/mangum) ) for running your WSGI sites. *Update: on the Hacker News announcement, one user commented that there's an official repo for the adapter:[https://github.com/awslabs/aws-lambda-web-adapter](https://github.com/awslabs/aws-lambda-web-adapter) ).
- ❌ Requires creating a new instance per request, not allowing concurrency on warm instances.
- ❌ Billed by CPU Wall Time.
- ❌ WebSockets are unsupported.
- ⚠️ Setup is complex, adapters are often unmaintained.


**Why the limits?** AWS Lambda requires you to use their HTTP lambda handler, which can cause incompatibility into your own HTTP servers. Also, because their lambda handlers are HTTP-based, there's no easy support for WebSockets or even reusing the same instance for different requests.


> In contrast, Wasmer Edge supports any Python HTTP servers without requiring any code adaptation from your side.


### Why Wasmer Edge Stands Out


- **Closer to native Python** than Pyodide (no JS involvement at all).
- **Faster cold starts and more compatibility** than Cloudflare's Workers.
- **More compatible** than AWS Lambda (no wrappers/adapters).
- **More affordable** across the board.


---


# 🐍 It's Showtime!


Python support in Wasmer and Wasmer Edge is already available and ready to use. We have set up many Python templates to help you get started in no time.


[https://wasmer.io/templates?language=python](https://wasmer.io/templates?language=python)


To make things even better, we are working on a **MCP server for Wasmer** , so you will be able to plug Wasmer into ChatGPT or Anthropic and have your websites deploying from your vibe-coded projects. Stay tuned!


> ⚠️ Python in Wasmer Edge is **still in Beta** , so expect some rough edges if your project doesn't work out of the box… *if you encounter any issues, please **[report them](https://github.com/wasmerio/wasmer/discussions/5758)** so we can work on enabling your workloads on Wasmer Edge.*


## Create your first MCP Server in Wasmer


1. Go to[https://wasmer.io/templates/mcp-chatgpt-starter?intent=at_vRxJIdtPCbKe](https://wasmer.io/templates/mcp-chatgpt-starter?intent=at_vRxJIdtPCbKe)
2. Connect your Github account
3. Create a git repo from the template
4. Deploy and enjoy!


*Note: source code available here:*[https://github.com/wasmer-examples/python-mcp-chatgpt-starter](https://github.com/wasmer-examples/python-mcp-chatgpt-starter)


## Create your first Django app


We have set up a template for using Django + Uvicorn in Wasmer Edge.


You can start using it very easily, just click Deploy:[https://wasmer.io/templates/django-starter?intent=at_WK0DIkt3CeKX](https://wasmer.io/templates/django-starter?intent=at_WK0DIkt3CeKX)


Deploying a Django app will create a MySQL DB for you in Wasmer Edge (Postgres support is coming soon), run migrations and prepare everything to run your website seamlessly.


*Note: source code available here:[https://github.com/wasmer-examples/django-wasmer-starter](https://github.com/wasmer-examples/django-wasmer-starter)*


---


Ready to deploy your first Python app on Wasmer Edge?


Here are the best places to begin:


- 🚀 **Starter Templates** →[Browse Python templates](https://wasmer.io/templates?language=python)
- 📖 **Docs & Examples** →[Wasmer GitHub](https://github.com/wasmerio/wasmer)
- 💬 **Community Support** →[Join our Discord](https://discord.gg/rWkMNStrEW)


**👉** **[Deploy your first Python app now](https://wasmer.io/templates?language=python)**


With WebAssembly and Wasmer, Python is now portable, sandboxed, and running at near-native speeds. Ready for AI workloads, APIs, and anything you can imagine at the edge.


The sky is the limit ❤️.


---


### Cloudflare notes


The Cloudflare Workers team reached us and did some clarifications on their current support


1. Cloudflare workers currently supports WebSockets in their Javascript offering counterpart
2. Support for raw sockets for Python is in their roadmap
