---
schema_version: "1.0.0"
document_id: "bd37fcda8c1d23e058513148d53310fa8261c84ee22b64e81988732173495fc0"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2024/01/11/bypassing-aws-complexity"
published_at: "2024-01-11T14:30:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:fd3830597c7974efb7160cb6608121f0aa0869f7834caa25a72188d2decf5aa4"
---

# How We're Bypassing AWS Complexity

## Our mission.. #


..is to empower Rust developers by providing tooling that bypasses the need to interact with complicated AWS infrastructure or infrastructure files when the requirement is to move fast.


When you're self-hosting, using a VPS, or on the cloud, you have control over everything. It can also be very complex: anything related to AWS, Terraform files, Dockerfiles... the list goes on. If you're also playing the part as a Software Engineer, this can use up quite a lot of time, especially in cases where moving fast is a higher priority than having control over everything. What you end up with is:


- Spending time context-switching which interrupts your flow.
- Ending up with responsibilities that you may not specialize in, potentially spending more time debugging your work.
- Needing to figure out Cloud consoles; they solve a lot of problems, but are also very complex to use.


Other solutions aim to solve this problem by dockerizing your applications for you. However, on those platforms Rust is typically treated as a second-class citizen, requiring you to write your own Dockerfile to deploy on the platform. You may also hit errors that you normally wouldn't with other languages.


Our aim is to enhance the Rust development experience. We've built our open-source platform with Rust, and we strive to use Rust for our internal tools wherever feasible. Our goal is to provide Rust developers with a smooth and efficient deployment process comparable to what JavaScript developers enjoy with platforms like[Vercel](https://vercel.com/) , emphasizing ease-of-use with minimal setup.


## What are we doing currently? #


### 1. Providing tooling that simplifies deployments #


We provide tooling that simplifies deployment by allowing users to add our Shuttle runtime macro to their applications. This means all you need to do is run` shuttle deploy` and your web service gets sent to the Shuttle servers, containerized, and started. No further steps are required!


Check the video below for a short example:


Your browser does not support the video tag.


### 2. Making resource provisioning as easy as possible #


You shouldn't need to set up YAML files just to get your application going. When using our runtime, you can simply pass in annotation macros that[provision resources](https://docs.shuttle.dev/resources/overview) for you. Whether you need a database, key-value store, or some metadata about your project, we can provide that! By doing this, it allows a seamless experience between getting the infra stuff that you want and then going straight back to writing good code.


Your browser does not support the video tag.


### What we're doing in 2024 #


There are several updates we're looking forward to announcing this year. Most notably, our upcoming builder service which will allow many more use cases and bring additional platform stability. We are also planning on adding more integrations like Redis, S3 object storage, and more so keep an eye out for our 2024 product roadmap which will have more information!


Feedback is always welcome athello@shuttle.rs if there's anything you'd like us to know!
