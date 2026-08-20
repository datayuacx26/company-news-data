---
schema_version: "1.0.0"
document_id: "4c3f0931e71c5c08dfc7fa84390cc1281c823dfa376cc687af2666c5345e9fae"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-bake-ephemeral-registry"
published_at: "2024-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:1c3f9d0cb2f7628094e9dd59ebb7b9e0ea2cf0c0d7d1f29b0acda627ab9bba44"
---

# Now available: Depot ephemeral registry for bake

You can now leverage Depot's ephemeral registries inside your` depot bake` commands to save built images for later use. This is a great way to optimize your CI workflow and only push images for your targets to your final registry once they've passed all your tests.


When we first announced[ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry) , we made it available for` depot build` commands first so you could easily save a given built image into our temporary registry and use` depot pull` or` depot push` to pull it back down or push it to your own registry without having to start another build.


We've extended that functionality to` depot bake` as well. This means you can save built images for multiple targets for later use in your CI workflows via` depot pull` or push select targets to your own registry with` depot push` .


Here is how it all works 👇


## How to use an ephemeral registry with` depot bake`


You can now pass a new parameter,` --save` , to save the images built for the different targets in your bake file to your project's registry. You can use the` --metadata-file` parameter to save the build ID of the build to a file that you can use later.


```text
depot   bake   -f   docker-bake.hcl   --save   --metadata-file=build.json
...
=  >   merging   manifest   list   registry.depot.dev/projectid:buildID-db                                                                                                        7.7s
=  >   merging   manifest   list   registry.depot.dev/projectid:buildID-app                                                                                                       7.0s


Saved   targets:   cron,app,db
To   pull:   depot   pull   --project   projectID   buildID
To   push:   depot   push   --target   <  TARGE  T  >   --project   projectID   --tag   <  REPOSITORY:TA  G  >   buildID
```


The` build.json` file contains the build ID for that build and the targets that were built.


```text
{
"depot.build"  : {
"buildID"  :   "your-build-id"  ,
"projectID"  :   "your-project-id"  ,
"targets"  : [  "app"  ,   "db"  ,   "cron"  ]
}
}
```


This example build shows that we had three targets in our bake file that were built:` app` ,` db` , and` cron` . From the bake output, you can see that we can pull **all of the targets** from the ephemeral registry with a simple call to` depot pull` :


```text
depot   pull   --project   <  your-project-i  d  >   <  your-build-i  d  >


[+] Pulling images registry.depot.dev/projectID:buildID-app, registry.depot.dev/projectID:buildID-db, registry.depot.dev/projectID:buildID-cron 3.9s (  3/3  ) FINISHED
=  >   pulling   repo/cron:latest                                                                                                                                                                        3.9s
=  >   pulling   repo/app:latest                                                                                                                                                                         3.9s
=  >   pulling   repo/db:latest                                                                                                                                                                          3.6s
```


This pulls all of the images that were built from your bake file. By default, the images are pulled down with the same tags they were built with via the` tags` property on the target. We default to the Docker Compose format if no tags are specified.


If you only want to pull down specific targets from the ephemeral registry instead of all of them, you can use the` --target` parameter to specify which targets you want to pull down:


```text
depot   pull   --project   <  your-project-i  d  >   --target   app,db   <  your-build-i  d  >
```


---


### Shortcut for grabbing the build ID


The` --metadata-file` parameter is optional, but it's helpful for quickly storing metadata about your build, like the Depot build ID in a file you can parse later. You can use something like[jq](https://jqlang.github.io/jq/) to quickly parse out the build ID from the metadata file to use in your` depot pull` command:


```text
depot   pull   --project   <  your-project-i  d  >   \
$(  cat   build.json   |   jq   -r   .  \[\"  depot.build  \"\]  .buildID  )
```


---


You can also use` depot push` with the` target` parameter to push specific targets to your upstream registry:


**Note: You'll need to be logged into your registry before calling push**


```text
depot   push   --project   <  your-project-i  d  >   --target   app   \
--tag   your-registry-tag   \
$(  cat   build.json   |   jq   -r   .  \[\"  depot.build  \"\]  .buildID  )
```


This is a great way to push the image to your own registry once it's passed all your tests and you're happy with the results.


## Updated GitHub Actions


### ` depot/bake-action` to use the ephemeral registry


We've also updated our GitHub Action,[depot/bake-action](https://github.com/depot/pull-action) , to allow you to save images for your targets by specifying the` save: true` parameter:


```text
permissions  :
contents  :   read
id-token  :   write


steps  :
-   uses  :   depot/setup-action@v1


-   uses  :   depot/bake-action@v1
id  :   image-build
with  :
project  :   <your-depot-project-id>
save  :   true
```


**Note:** This example assumes you've configured a[trust relationship](https://depot.dev/docs/cli/authentication#oidc-trust-relationships) for connecting your Depot project to your GHA workflow.


### ` depot/pull-action` to use the ephemeral registry


We've also updated our GitHub Action,[depot/pull-action](https://github.com/depot/pull-action) , to default to pulling all targets from the ephemeral registry when the` build-id` was a bake operation.


```text
permissions  :
contents  :   read
id-token  :   write


steps  :
-   uses  :   depot/setup-action@v1


-   uses  :   depot/bake-action@v1
id  :   image-build
with  :
project  :   <your-depot-project-id>
save  :   true


-   uses  :   depot/pull-action@v1
with  :
build-id  :   ${{ steps.image-build.outputs.build-id }}
project  :   <your-depot-project-id>
```


There is also the` targets` parameter that you can use to specify which targets you want to pull down from the ephemeral registry:


```text
permissions  :
contents  :   read
id-token  :   write


steps  :
-   uses  :   depot/setup-action@v1


-   uses  :   depot/bake-action@v1
id  :   image-build
with  :
project  :   <your-depot-project-id>
save  :   true


-   uses  :   depot/pull-action@v1
with  :
build-id  :   ${{ steps.image-build.outputs.build-id }}
project  :   <your-depot-project-id>
targets  :   app,db
```


## Conclusion


Bringing the Depot ephemeral registry to` depot bake` is a great way to save built images for later use in your CI workflows, to share with your team, or to push to remote registries. It saves you time by giving you the option to build your targets *once* and then pull them down or push them to your own registry as needed.


For more information on how to use the ephemeral registry with` depot bake` , check out the[Ephemeral Registry documentation](https://depot.dev/docs/container-builds/how-to-guides/ephemeral-registry) .


If you want to get started with Depot,[sign up today and try it out for free](https://depot.dev/start) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
