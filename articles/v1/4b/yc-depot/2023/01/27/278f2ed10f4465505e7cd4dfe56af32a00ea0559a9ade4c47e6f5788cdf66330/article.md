---
schema_version: "1.0.0"
document_id: "278f2ed10f4465505e7cd4dfe56af32a00ea0559a9ade4c47e6f5788cdf66330"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/buildx-bake-with-depot"
published_at: "2023-01-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:d425439f3511a3a38b660892f8b6b5f96e74ad82d9af2b33f66d46ac57ab3c48"
---

# Build all of your Docker images concurrently from a file with bake

The ability to build multiple Docker images from a single file is now available in our latest` depot` CLI version. With the` depot bake` command, you can now build multiple images from a single HCL, JSON, or Docker Compose file. It's a fantastic way to build all the images related to your application with a single build request to Depot.


## What is Docker bake?


In a nutshell,` bake` is a high-level build command that allows you to build multiple images from a single file via BuildKit. The` bake` command allows you to issue a single build request with the file containing the image definitions to be built. All of the images get built concurrently against a single BuildKit builder.


With Depot, we are orchestrating BuildKit builders for every build you route to us. So a builder and its persistent cache are spawned for each architecture you request in a build. But a single Depot builder (i.e., a project) can handle multiple builds concurrently.


## Build multiple images concurrently


Before Docker bake was supported, you would often need to run multiple` depot build` commands to build multiple images with a single builder. A lot of our users have reached for a` Makefile` to do this:


```text
build  :
depot build --project 1234567890 -f Dockerfile.app
depot build --project 1234567890 -f Dockerfile.db
depot build --project 1234567890 -f Dockerfile.cron
```


But this results in the images building serially. First, build the app image, then the DB, and finally, the cron image. It works, but you have to issue a separate build request for each image you want to build.


With` bake` , you can build all these images concurrently with a single build request. To do it, you define a file that contains how all the images should be built; it can be written in an HCL, JSON, or Docker Compose file. Here is an example of a` docker-bake.hcl` file that produces the same images as the` Makefile` above:


```text
group   "default"   {
targets   =   [  "original"  ,   "db"  ,   "cron"  ]
}


target   "app"   {
dockerfile   =   "Dockerfile.app"
platforms   =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags   =   [  "repo/app:test"  ]
}


target   "db"   {
dockerfile   =   "Dockerfile.db"
platforms   =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags   =   [  "repo/db:test"  ]
}


target   "cron"   {
dockerfile   =   "Dockerfile.cron"
platforms   =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags   =   [  "repo/cron:test"  ]
}
```


The entire build definition from the` Makefile` above is now contained in a single file. Bake files can specify all of the parameters you would pass to an image build, notice how the file above is using the` platforms` and` tags` parameters, much like` --platform` and` --tag` when running` depot build` .


With this file, you can now run` depot bake` to build all the images in the file:


```text
depot   bake   --project   1234567890   -f   docker-bake.hcl
```


What you see in the output is multiple images being built concurrently. So, for example, you can see that the` app` image is being made for both` linux/amd64` and` linux/arm64` at the same time as the` db` and` cron` images are being built for both architectures.


```text
=  > [cron   linux/arm64   2/3]   WORKDIR   /app                                                                                                                                                                                                                                                              11.7s
=  > [cron   linux/amd64   2/3]   WORKDIR   /app                                                                                                                                                                                                                                                              12.5s
=  > [app   linux/arm64   build   2/6]   WORKDIR   /app                                                                                                                                                                                                                                                          6.2s
=  > [app   linux/amd64   build   2/6]   WORKDIR   /app                                                                                                                                                                                                                                                          6.6s
=  > [app   linux/arm64   build   3/6]   COPY   package.json   yarn.lock   tsconfig.json   ./                                                                                                                                                                                                                          0.0s
=  > [db   linux/arm64   3/3]   RUN   echo   "postgres build"                                                                                                                                                                                                                                                    0.2s
=  > [app   linux/arm64   build   4/6]   COPY   src/   ./src/                                                                                                                                                                                                                                                      0.0s
=  > [app   linux/arm64   build   5/6]   RUN   yarn   install   --immutable                                                                                                                                                                                                                                          9.3s
=  > [app   linux/amd64   build   3/6]   COPY   package.json   yarn.lock   tsconfig.json   ./                                                                                                                                                                                                                          0.1s
=  > [db   linux/amd64   3/3]   RUN   echo   "postgres build"                                                                                                                                                                                                                                                    0.2s
=  > [app   linux/amd64   build   4/6]   COPY   src/   ./src/                                                                                                                                                                                                                                                      0.0s
=  > [app   linux/amd64   build   5/6]   RUN   yarn   install   --immutable                                                                                                                                                                                                                                          9.5s
=  > [app   linux/arm64   build   6/6]   RUN   yarn   build                                                                                                                                                                                                                                                        4.2s
=  > [app   linux/amd64   build   6/6]   RUN   yarn   build                                                                                                                                                                                                                                                        4.4s
=  > [app   linux/arm64   stage-1   3/5]   COPY   --from=build   /app/node_modules   /app/node_modules                                                                                                                                                                                                               1.6s
```


### Using bake with Docker Compose


You can also use Docker Compose files with` depot bake` . This allows you to build multiple images from a single compose file. Here is an example` docker-compose.yml` file that builds the same images as the` docker-bake.hcl` file above:


```text
services  :
app  :
build  :
dockerfile  :   Dockerfile.app
platforms  :
-   linux/amd64
-   linux/arm64


db  :
build  :
dockerfile  :   Dockerfile.db
platforms  :
-   linux/amd64
-   linux/arm64


cron  :
build  :
dockerfile  :   Dockerfile.cron
platforms  :
-   linux/amd64
-   linux/arm64
```


This allows you to define a single file that contains multiple Docker images with multiple Dockerfiles and build them all concurrently with a single` depot bake` command.


## ` depot/bake-action` GitHub Action


In addition to the` depot bake` command, we released[depot/bake-action](https://github.com/depot/bake-action) GitHub Action that implements the same inputs and outputs as the` docker/bake-action` .


Like our[depot/build-push-action](https://github.com/depot/build-push-action) , you can use GitHub's OpenID Connect tokens via a trust relationship, so your builds can authenticate with Depot projects without needing any static access tokens. Here is an example GitHub Action workflow that builds images from a` docker-bake.hcl` file:


```text
name  :   Run Depot bake
on  :   push


permissions  :
id-token  :   write
contents  :   read


jobs  :
build  :
name  :   Build
runs-on  :   ubuntu-20.04
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v3


-   uses  :   depot/setup-action@v1


-   name  :   Bake Docker images
uses  :   depot/bake-action@v1
with  :
project  :   1234567890
file  :   docker-bake.hcl
```


## Conclusion


Building multiple images via` depot bake` is a great way to speed up your image builds even more. With` bake` , you can leverage BuildKit's ability to build multiple images concurrently and deduplicate work across multiple images specified in a single build request.


Define all the images that compose your application in a single HCL, JSON, or Docker Compose file and run` depot bake` to build them all at once. No more waiting for each image to build one at a time. Instead, you can build them all concurrently on the same builder.


For those of you leveraging GitHub Actions, you can use the[depot/bake-action](https://github.com/depot/bake-action) to build images from your file inside your existing Actions workflows. Just swap in` depot/bake-action` for` docker/bake-action` , and you're good to go.


If you're not using Depot, we offer a free tier to let you try things out in your existing workflows to see if we can make your image builds faster. We are the only remote container build service that offers native multi-platform image support, and we make caching a breeze because it's automatically persisted for you across builds. These two features usually make image builds 15x faster in CI environments.


Sign up today for our free trial and swap` docker build` for` depot build` to get instantly faster builds:[https://depot.dev/sign-up](https://depot.dev/sign-up) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
