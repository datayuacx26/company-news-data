---
schema_version: "1.0.0"
document_id: "bf4bdc3d57dc56ecd4568314b14a9bc320de768ba12aac19d84b2e6a8a5dae93"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-03-10-tinfoil-containers"
published_at: "2026-03-10T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:5b6f14a04d37acb8911b5a5cdfc9c4d9513bd0a48d0ecf84fdad7507134d8a4a"
---

# Introducing Tinfoil Containers

[← Back to Posts](https://tinfoil.sh/blog)


# Introducing Tinfoil Containers


Mar 10, 2026


•


9 min read


Tinfoil Team


Until a year ago, the only way to build a fully private AI application was to run everything locally or on-prem. Our[Private Inference API](https://tinfoil.sh/inference) allowed you to privately extend local applications with a model running on the cloud. But inference is just one part of the stack. Real applications have auth servers, business logic such as server-side prompt configuration, custom analytics, and agent loops that decide when to do inference and what to do with the response. Calling a private inference API from an unprotected server defeats the purpose.


After working in close collaboration over the past few months with our partner[Workshop Labs](https://workshoplabs.ai/) and researchers at Berkeley and Stanford, we're excited to launch[Tinfoil Containers](https://tinfoil.sh/containers) today. Tinfoil Containers lets you deploy your application backend, your training pipeline, or your proprietary model on Tinfoil and build end-to-end verifiably private AI services. You and your users get the same cryptographic security guarantees as our[Private Inference API](https://tinfoil.sh/inference) , extended to your entire stack. Even Tinfoil can't see what's happening inside.


Your container runs inside a confidential VM on Intel TDX or AMD SEV-SNP. For GPU workloads, Tinfoil Containers supports NVIDIA Confidential Computing with multi-GPU setups of up to eight H200 or B200 GPUs per container.


Setting up enclaves has always been an error-prone and tedious experience. Our goal with Private Inference was to make verifiably private AI a drop-in replacement for OpenAI. Our goal with Tinfoil Containers is the same idea taken further: your confidential workflow should feel no different from your normal one, and you should get all the security benefits of using an enclave out-of-the-box without having to twiddle with a multitude of configurations.


## From Docker image to enclave


Tinfoil Containers lets you take any Docker image and deploy it into a secure enclave. You start from a[template repo](https://github.com/tinfoilsh/tinfoil-containers-template) that gives you a` tinfoil-config.yml` :


Point it at your Docker image published in a container registry and pinned by SHA256 digest, then set your resources, environment variables, and secrets. Next, push a Git tag. Each tag creates an auditable record in the[Sigstore](https://sigstore.dev/) transparency log. Your container comes up at` https://<name>.<org>.containers.tinfoil.dev` .


On every connection, our SDKs verify the enclave's attestation report automatically:


This confirms the hardware is genuine, the code matches what's published in the transparency log, and nothing has been tampered with. Otherwise the connection fails. This is what makes privacy cryptographically verifiable.


While Tinfoil itself is open-source, your application doesn't need to be for attestation to work. Read more about[how verification works with private images](https://docs.tinfoil.sh/containers/private-images) .


## Why existing enclave options aren't enough


The best implementations of enclaves for AI are[Apple Private Cloud Compute](https://security.apple.com/blog/private-cloud-compute/) and[Google Private AI Compute](https://blog.google/technology/ai/google-private-ai-compute/) , but are built exclusively for their own products. If you're an AI startup trying to offer your users verifiable privacy, they don't help you.


What's actually available to everyone else from major cloud providers, is Azure Confidential VM, GCP Confidential VM, and AWS Nitro Enclaves. These are designed for enterprise compliance, and are more about defense-in-depth and achieving "checkbox security." They're very difficult to use and are built with confused and often contradictory security assumptions.


The whole point of an enclave is to remove the need to trust someone. Enclaves were originally designed to remove trust in the cloud provider: your data runs on their hardware, but they can't see it. Even for this stated goal, cloud providers do a poor job. Azure, for instance, uses a proprietary Microsoft-operated service for attestation verification. The technology exists to remove trust in the cloud provider, but the cloud provider has made itself a required part of the verification process.


But we think there's a more important use case: removing trust in the *service operator* . Consider a bank using a third-party voice AI provider. The bank may already trust the cloud, because all their data is there. What they don't trust is the AI vendor. If that vendor can prove cryptographically that it has no visibility into the bank's data, the bank doesn't need to take the vendor's word for it.


Current enclave implementations aren't designed for this. They don't attest the application code by default, so there's no way for a customer to verify what the operator is actually running. They give the operator SSH access into the enclave by default. They're built for enterprises adding a layer of security to their own infrastructure, not for application developers proving something to their customers.


AWS Nitro Enclaves have seen more adoption but come with their own maze of vsock networking, enclave image formats, closed-source components, and attestation that's difficult to integrate into a product.


For GPU workloads, things get worse. Azure and GCP are the only providers offering GPU confidential VMs, both limited to a single H100, and availability is minimal even for that. If you need to run a real open-source model that requires multiple GPUs, you're out of luck.


Tinfoil is designed differently. The default is that the operator is removed from the trust boundary. There is no SSH access into the enclave. Attestation covers the application code and the model weights. The client verifies directly against a public transparency log, not against a service we run. We were the first provider to offer multi-GPU secure enclaves, built on bare metal because we wanted to support the models people actually use. We now support both Hopper and Blackwell multi-GPU enclaves. And we've tried to make the development experience the opposite of miserable: debug mode for troubleshooting in an isolated enclave environment, zero-downtime blue-green deployments, easy to configure secrets, automated client side attestation and built-in metrics.


## Who this is for


**Companies building end-user privacy and data control into their core value proposition.**


We're working closely with[Workshop Labs](https://workshoplabs.ai/) . They are building a fully private post-training and inference system on Tinfoil Containers. Their customers send sensitive training data to finetune frontier open-source models, and normally whoever runs the infrastructure can see that data. Workshop Labs built their architecture so that even Workshop Labs cannot access it. Training data is encrypted into the enclave, finetuning happens inside it, and the resulting model weights never leave. They run their auth, their serving infrastructure, and their multi-GPU training jobs all as Tinfoil Containers. As Rudolf Laine, co-founder of Workshop Labs, put it:


> "We have fast deployment cycles for servers that we run on Tinfoil TEEs to guarantee customer privacy. Tinfoil Containers makes the TEE deployment friction almost nonexistent and lets us iterate quickly. It's an important step towards the future where most ML workloads are secured by running on verifiably-private TEEs."


This kind of product wouldn't have been possible before: a cloud finetuning service where the provider is cryptographically locked out of the data. Health data, legal documents, financial models, proprietary IP. There are entire categories of valuable information that people won't hand over to a cloud service on the strength of "we promise we won't look." The interesting question is what becomes possible when that barrier goes away.


**Teams that need enclaves that just work.**


Secure enclaves are a great idea that almost nobody uses, because the experience of actually setting one up is awful. Most teams that would benefit from enclaves have looked into them, winced, and gone back to writing privacy policies instead.


We built Tinfoil Containers to make this a solved problem. As researcher Darya Kaviani at UC Berkeley put it:


> "Running our own custom Docker container on Tinfoil Containers is a major unlock. It lets us run our full end-to-end system in trusted hardware using the same simple Python SDK we already use to call Tinfoil's embedding and LLM models. Serverless enclaves have finally arrived!"


And as Erik Chi, who's been building[The Open Anonymity Project](https://openanonymity.org/) at Stanford and UMich, put it:


> "We were using Azure's confidential containers (ACI) which is a nightmare to set up correctly, from TLS certificate binding, hardware measurements, reproducible image digests, etc. We can do the same thing on Tinfoil Containers in less than 20 minutes with the nice attestation SDK, clear docs, debug mode, almost zero update downtime, and transparent architecture that everyone can audit."


**Teams building for integrity, not just privacy.** There's a thing about attestation that's underappreciated. It doesn't just prove that data is kept confidential. It proves what code is running.


If you can prove what code is running, you can prove you're serving the actual model you claimed and not a cheaper substitute (we talk about this in our[previous post](https://tinfoil.sh/blog/2026-02-03-proving-model-identity) ). You can prove that safety guardrails are in place, not just promised in a policy document. You can run moderation logic where the rules are verifiably enforced without the moderator ever seeing the underlying content. "Can anyone see my data?" and "is the system doing what it claims?" are two different questions answered by the same attestation report. We'll have more to say about this soon.


## What's next


Tinfoil Containers is a new way to build AI products where privacy is enforced by design, not policy. When your users and customers can verify that their data is protected, they give the AI more access, more context, and more control. Products where privacy doesn't limit what's possible, but expands it.


Enclaves are a really useful tool to help build this sort of privacy infrastructure. But they're miserable to use and even worse to understand and explain to your users. Our goal with Tinfoil Containers is for the enclave to get out of the way. It should be invisible infrastructure that lets you focus on your application.


We intend to stay at the frontier of what's possible here. NVIDIA's latest chips support multi-GPU confidential computing, and with Vera Rubin, enclaves that span nodes. This will be useful for large-scale private training in the near future.


We believe this new way of deploying software will become the standard for building private applications.


Tinfoil Containers is available today.[Read the docs](https://docs.tinfoil.sh/containers/overview) or reach out atcontact@tinfoil.sh to get started!


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2026-02-03-proving-model-identity)[Next Post](https://tinfoil.sh/blog/2026-05-01-supply-chain-client)
