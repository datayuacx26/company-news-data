---
schema_version: "1.0.0"
document_id: "f7a09dfdcc146f3c0d8833b9c8a8e19b3cf21256b8b15d9e8a6df29cf92b7b37"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/introducing-depot-registry"
published_at: "2025-03-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:5f820b6e3c57aac1317e365bd97de05f59c9933a3a78d5dee6029fc63e0ed2cb"
---

# Now available: Depot Registry

We're closing the month with our latest product launch, Depot Registry!


Depot Registry is a new, faster, and more powerful registry that is now available to everyone. We've taken what we learned from building our ephemeral registry for our[accelerated Docker build service](https://depot.dev/products/container-builds) to create a high-performance registry that is globally distributed and easy to integrate into your Depot builds.


## Evolution of the Depot Registry


At the end of 2023, we[released our first iteration on a registry product](https://depot.dev/blog/depot-ephemeral-registry) , the ephemeral registry. The goal at that time was to provide a way to store and share Docker images between builds. Folks wanted a way to build a Docker image once and then be able to reuse the built image across builds without having to push it to their final registry or start another build inside of Depot.


So, we built the ephemeral registry to be one that you could quickly save your images to and then retrieve in a subsequent CI job or locally. Once all your tests and checks passed, you could then transfer the built image to your final registry.


I gave a talk at re:Invent last year that offered a peek into how we built the ephemeral registry and how it works. If you're curious, you can check it out on[YouTube](https://www.youtube.com/watch?v=mPbI3Qxdocc) .


The ephemeral registry was a great first step, but it was limited in a few ways. While backed by R2 and Cloudflare Workers, there were limitations and workarounds we had to implement to ship large-layer blobs to R2 and get them back out quickly globally. We also never built out any visualization of what was in your registry; images were automatically deleted after 7 days, and the performance had limitations because of the workarounds mentioned earlier.


## Depot Registry today


We've revamped the ephemeral registry and promoted it to a full product. Depot Registry is a new, faster, and more feature-rich registry that we're excited to keep iterating on. It's available to all users starting today.


Here are a few of the key features of Depot Registry.


### Enhanced performance


The Depot registry is engineered to serve images from the most optimal location, no matter where in the world the client is located.


When images are built, they are first pushed to S3 in the same AWS region as the build server and then are replicated to[Tigris](https://www.tigrisdata.com/) . If that image is pulled from inside the same AWS region, the registry serves the layer content directly from S3.


However if pulling from outside AWS, the registry will serve that that content from Tigris, who automatically replicate that layer content to the closest of 13 global storage regions, effectively acting as both CDN and object storage for the registry.


Tigris supports this intelligent content routing and replication automatically, unlike our previous storage system that was limited to a single R2 region. Tigris offers an S3-compatible API, so migrating to it was a drop-in replacement for our existing S3 clients.


### Registry dashboard


We've added a new registry dashboard that allows you to see all of the images in your registry, including their size, push date, and which build produced the image. You can also easily select and delete images you no longer need to keep.


### Image retention


We've added a new image retention policy that allows you to specify how long you want to keep your images in the registry. By default, images are kept for **7 days** , but you can now select a custom retention policy for each Depot project. Possible values are 7, 14, and 30 days, or you can specify "Unlimited", and images won't be deleted from your registry.


This means that you can keep your images in the registry for as long as you need them for each project, and they will automatically be deleted when they are no longer needed.


### Automatic integration with Depot GitHub Actions runners


You no longer need to do anything to authenticate to your Depot Registry when you are using our[GitHub Actions runners](https://depot.dev/products/github-actions) . When you use` depot pull` from within our runner, we automatically pull the image from the closest cache edge to your runner. This means that your images are always close to you, and your builds are fast locally and in CI.


## How to use Depot Registry


To get started with Depot Registry, you will need a Depot account and organization. You can[sign up for a free trial](https://depot.dev/start) if you don't already have an account and then create your organization.


Once you have an account and organization, you can start using Depot Registry by specifying the` --save` flag as part of your build command. This will save your image to the registry.


Alternatively, if you're using actions like[depot/build-push-action](https://github.com/depot/build-push-action) or[depot/bake-action](https://github.com/depot/bake-action) inside of GitHub Actions, you can specify` save: true` to save your images to the registry.


### Using an image from the registry


Once an image is stored in the registry, you pull it using either` depot pull` or` docker pull` commands and even use the images in your registry inside of your Kubernetes environment.


#### Using` depot pull`


You can use the[depot pull](https://depot.dev/docs/cli/reference/container-builds#depot-pull) command to pull an image from the registry.


```text
depot   pull   --project   <  your-depot-project-i  d  >   <  depot-build-i  d  >
```


#### Using` docker pull`


You can also use the` docker pull` command to pull an image from the registry. Before you can use` docker pull` , you will need to authenticate to the registry. You can do this by running the following command:


```text
docker   login   registry.depot.dev   -u   x-token   -p   <  depot-access-toke  n  >
```


Where the Depot access token can be a project, organization, or user token. You can learn more about different token types in our[authentication guide.](https://depot.dev/docs/cli/authentication)


#### Using with Kubernetes


You can use the images in your registry inside of your Kubernetes environment by adding a secret to your Kubernetes cluster via` kubectl` . You can do this by running the following command:


```text
kubectl   create   secret   docker-registry   regcred   \
--docker-server=registry.depot.dev   \
--docker-username=x-token   \
--docker-password=  <  depot-access-token  >
```


## Pricing details


The Depot Registry is included in all plans. We don't charge for the registry itself or data transfer, but we do charge for the storage your images use. Image storage is charged at **$0.20/GB/month** .


By default, image retention is **7 days** . We will automatically delete any images older than that default retention policy. However, you can customize your retention policy on a project-by-project basis.


## What's next?


We're excited to bring Depot Registry to everyone. We've been working hard on this release and are excited to see what you build with it. We have some big things planned for the next iteration, so you can stay tuned for upcoming developments by joining our[Discord Community](https://discord.gg/MMPqYSgDCg) .


If you're new to Depot, thanks for checking us out! You can get started with Depot Registry by[signing up for a free trial](https://depot.dev/start) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
