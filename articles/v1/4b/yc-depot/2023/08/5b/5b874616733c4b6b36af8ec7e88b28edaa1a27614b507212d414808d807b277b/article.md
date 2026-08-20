---
schema_version: "1.0.0"
document_id: "5b874616733c4b6b36af8ec7e88b28edaa1a27614b507212d414808d807b277b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-with-docker-compose"
published_at: "2023-08-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:eac1504c310f6aa35d67c0156e04cd982f218816b7efef98b1913b92cc591152"
---

# Now available: Use Docker Compose with Depot

A month ago, we[announced our configure-docker command](https://depot.dev/blog/depot-configure-docker) , and we're excited to announce an expansion of it that covers Docker Compose and other tools that leverage` docker buildx build` !


Our latest release of the Depot CLI expands our new command that installs Depot as a Docker CLI plugin to configure a default` buildx` driver to use Depot as well. Allowing you to leverage Depot for any workloads that use` docker buildx build` , like Docker Compose. All without having to change a single line of code 🎉


## How it works


First, you must install the latest version of the[Depot CLI](https://github.com/depot/cli) . See our[CLI installation docs](https://depot.dev/docs/cli/installation) for more details.


Once you have the latest version of the CLI installed, you can run the following command:


```text
depot   configure-docker
Successfully   installed   Depot   as   a   Docker   CLI   plugin
```


Behind the scenes, in addition to installing Depot as a Docker CLI plugin, the command configures a default` buildx` driver that routes builds to Depot. Allowing any` docker buildx build` commands to use Depot, in addition to the existing` docker build` command.


### Using Depot with Docker Compose


Once the` configure-docker` command is executed, you can use Depot with Docker Compose without changing a single line of code. All you need to do is ensure you have a Depot project configured for your build. You can do this via:


1. Setting a` DEPOT_PROJECT_ID` environment variable to the project ID of the project you want to use for builds.
2. Running` depot init` in the root of the repository in which you're building your image. The command creates a` depot.json` file that our plugin will detect.


If you have a Depot project configured, you can run` docker compose build` , and Depot will build the images in your Docker compose file. You can see this in action in the following example.


Here is our example Docker compose file with a single service:


```text
services  :
app  :
build  :
dockerfile  :   Dockerfile.app
platforms  :
-   'linux/amd64'
-   'linux/arm64'
tags  :
-   repo/app:123
```


The Dockerfile that it is building looks like this:


```text
FROM   node:16   AS   build


WORKDIR   /app
COPY   package.json yarn.lock tsconfig.json ./
COPY   src/ ./src/
RUN   yarn install --immutable
RUN   yarn build


FROM   node:16
WORKDIR   /app
COPY   --from=build /app/node_modules /app/node_modules
COPY   --from=build /app/dist /app/dist
COPY   gifs-to-upload/ dist/gifs-to-upload/
ENV   NODE_ENV production
CMD   [  "node"  ,   "--enable-source-maps"  ,   "./dist/index.js"  ]
```


Now, all you have to do is call` build` or` up` just as you usually would.


```text
docker   compose   build
#1 [app depot] build: https://depot.dev/orgs/123/projects/456/builds/000
#1 DONE 0.0s
#2 [app depot] build: https://depot.dev/orgs/123/projects/456/builds/111
#2 DONE 0.0s
# ...continued output from Docker Compose
```


You can see that the Docker is building with Depot. It creates a separate build for each architecture and service defined in the` compose.yml` .


If you want to use Depot with Docker Compose but prefer to use the` up` command, you can also do that. Just run` docker compose up` , and Depot will build your images.


```text
docker   compose   up
[+] Building 33.0s (  19/19  )
=  > [app   depot]   build:   https://depot.dev/orgs/123/projects/456/builds/222                                                                    0.0s
=  > [app   depot]   launching   arm64   machine                                                                                                                                                           2.0s
=  > [app   depot]   connecting   to   arm64   machine                                                                                                                                                       6.9s
=  > [app   internal]   load   build   definition   from   Dockerfile.app
=  >   ...continued   output   from   Docker   Compose
```


#### Note on Docker Compose


Docker Compose creates a new build for each service and architecture defined in the` compose.yml` . If you have a` docker-compose.yml` that defines 3 services, and you're building for 2 architectures, you will have 6 builds created in Depot.


## Power of Depot for all tools


We've got a lot of requests to integrate Depot with other developer tools out there that need to build containers on demand. Tools like[Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers) and[goreleaser](https://github.com/goreleaser/goreleaser) are great examples of this.


Before this command, these tools couldn't integrate with Depot. They call` docker build` or` docker buildx build` directly, and there was no way to configure them to use a different builder.


Now, with` depot configure-docker` , all tools using Docker or` buildx` internally can route their image builds to Depot. We have an integration guide and documentation for how this works with[Dev Containers](https://depot.dev/docs/container-builds/how-to-guides/devcontainers) and[docker build and docker buildx build in general](https://depot.dev/docs/container-builds/how-to-guides/docker-build) .


We're excited about what this new command will unlock for folks! We will put out some new integration guides for other tools that need to build a Docker container on demand, so stay tuned for those. You can also hop in our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know which tools you'd like us to integrate with next!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
