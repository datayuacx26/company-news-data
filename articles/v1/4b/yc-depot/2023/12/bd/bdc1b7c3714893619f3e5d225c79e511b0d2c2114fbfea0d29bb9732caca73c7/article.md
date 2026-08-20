---
schema_version: "1.0.0"
document_id: "bdc1b7c3714893619f3e5d225c79e511b0d2c2114fbfea0d29bb9732caca73c7"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-ephemeral-registry"
published_at: "2023-12-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:62ef7cfdee952b76c91ad87ea5209bdf75702aae01d5b2a7614a857a2c44df08"
---

# Now available: Depot ephemeral registries

All Depot projects now have access to our latest feature, ephemeral registries. An ephemeral registry allows you to save built images for later use. Here is everything you need to know about to use them and how they work 👇


"Ephemeral registries reduced our usage by about 70% when looking at October vs November. That's just amazing. It makes Depot (even more) a tool we can trust. 💖"


[— Paul D'Ambra, Senior Software Engineer at PostHog](https://posthog.com/)


## How to use an ephemeral registry


When you use` depot build` , you can now pass a new parameter,` --save` , to save your built image to your project's registry. You can use the` --metadata-file` parameter to save the build ID of the build to a file that you can use later.


```text
depot   build   --save   --metadata-file=build.json   .
```


This example builds and stores your image in your project's ephemeral registry. The build ID is stored in the` build.json` file so we can reference it later.


```text
{
"depot.build"  : {
"buildID"  :   "your-build-id"  ,
"projectID"  :   "your-project-id"
}
}
```


As part of this, we've released a` depot pull` command that allows you to pull your image back down from your project's ephemeral registry. You can use the build ID from the` build.json` file to pull your image back down.


```text
depot   pull   $(  cat   build.json   |   jq   -r   .  \[\"  depot.build  \"\]  .buildID  )
```


You can use it to pull the image down into a CI workflow to run integration tests or use it locally to run the image in your local Docker environment.


We've also introduced a` depot push` command that allows you to push your built image directly from the ephemeral registry to your own registry.


The combo of all three together allows you to store built images, pull them down for testing, and then push the built image onto your own registry once it's passed all your tests. Without having to run the Docker image build more than once.


```text
depot   push   $(  cat   build.json   |   jq   -r   .  \[\"  depot.build  \"\]  .buildID  )
```


To see all the flags and documentation surrounding these new commands, see our[ephemeral registry guide](https://depot.dev/docs/container-builds/how-to-guides/ephemeral-registry) .


## Using ephemeral registries in GitHub Actions


We've also released a new GitHub Action,[depot/pull-action](https://github.com/depot/pull-action) , to pull back a built image from the ephemeral registry. Here is an example of using it in your GitHub Actions workflow.


```text
permissions  :
contents  :   read
id-token  :   write


steps  :
-   uses  :   depot/setup-action@v1


-   uses  :   depot/build-push-action@v1
id  :   image-build
with  :
project  :   <your-depot-project-id>
save  :   true


-   uses  :   depot/pull-action@v1
with  :
build-id  :   ${{ steps.image-build.outputs.build-id }}
tags  :   |
org/report:tag
```


**Note:** This example assumes you've configured a[trust relationship](https://depot.dev/docs/cli/authentication#oidc-trust-relationships) for connecting your Depot project to your GHA workflow.


## How does it work


Imagine a CI workflow with a matrix that spawns 20+ jobs, each needing the built image to run a set of integration tests.


Without the ephemeral registry, each job must run` depot build --load` to pull the image back down. This would result in 20+ Depot builds to pull back down the same image already built earlier. It forced you to use Depot build minutes to pull back an entirely cached image. This was a waste of time and money.


The ephemeral registry eliminates this by allowing you to build the image once and pull it down as many times as needed without ever starting a builder or using a build minute.


We do it by persisting your built image to an ephemeral registry based on the project and build ID it belongs to. We then store the layer blobs that make up your image in a CDN that allows for fast and cheap retrieval of the image layers.


When you run` depot pull` , we use the build ID to find the image in the registry and pull the image layers from the CDN. This allows us to pull the image back down without ever starting a builder or using a build minute.


### Pricing and limits


We will charge for the storage used in the registry at the same cache storage rate we've been using for the Docker layer cache. This is $0.20 per GB/month today, with your first 50GB free.


The ephemeral registry can hold up to 20k images per project, and the oldest ones get evicted after 7 days. If you'd like that to be configured to a different amount of time,drop us an email and let us know.


## Conclusion


The ephemeral registry prevents you from rebuilding the same image more than once. You can build the image once and quickly reuse that built image across your different CI and local development workflows. Avoiding launching Depot builders to rebuild the same image repeatedly will save you time and money.


Once you're happy with the results, you can push the image directly from the ephemeral registry into your final registry. This is a great way to optimize your CI workflow and only push images to your final registry once they've passed all your tests.


If you want to get started with Depot,[sign up today and try it out for free](https://depot.dev/start) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
