---
schema_version: "1.0.0"
document_id: "e663a8f1695d19bde1bba4a6f55c00d46330897b0b7a87599ebb87c46bed5e92"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/buildx-bake-deep-dive"
published_at: "2024-02-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:a5920d6d211e10d56a448956e8bffa091ffb67e90cb5fc3ee80df65b48d018c7"
---

# Buildx bake deep dive: Bake all your images with one command

In this post we will take a deep dive into how` docker buildx bake` works. We'll look at what bake is, why it's relevant to various use cases, and how you can use it to build your Docker images faster.


## What is bake?


` docker buildx bake` is a command that allows you to build multiple Docker images simultaneously. It enables you to define all of your images in a single file and build them all in parallel from a single command. Bake is like a task runner for building Docker images. It allows you to efficiently leverage the[parallelization of BuildKit](https://depot.dev/blog/buildkit-in-depth) .


In practice, we often see bake used with[Depot](https://depot.dev/start) in the following scenarios:


- **Monorepos** - If you have a monorepo with multiple services, you can use bake to build all of the images in parallel.
- **Multiple images that share dependencies** - If you have multiple images that share a common base image, you can use bake to build them all in parallel and take advantage of BuildKit's deduplication of work.
- **Programmatic builds** - If you want to parameterize your` Dockerfile` and trigger different steps in the build from the outside, bake can be a great tool to do that.
- **Docker Compose** - If you use Docker Compose to define your application, you can use bake to build all of the images in your Compose file in parallel.


## Why use bake?


Using` buildx bake` or` depot bake` allows you to leverage the parallelization and deduplication of work that BuildKit provides.


You could use other tools like` make` to place all of the Docker image builds behind a single command, but all the builds would happen sequentially. Redoing common work multiple times across builds.


With buildx bake, you can define all of these builds in a single file to run them all in parallel and get deduplication of work for free via BuildKit. Here is an example bake file written in HCL:


*Note: You can write bake files in HCL, JSON, or Docker Compose format. We'll explore more of the syntax in a moment.*


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


There are two key components in this bake file:


- **Group** - This is a top-level construct that allows you to define a group of targets. This is useful for defining a set of targets you want to build together. In this example, we have a single group called` default` containing all our targets. When you run` docker buildx bake` without specifying a group, it will use the` default` group.
- **Target** - This represents a single` docker build` or` depot build` . A target is a single Docker image that you want to build. This example has three targets:` app` ,` db` , and` cron` . Each target has a` dockerfile` that points to the Dockerfile that you want to build, a` platforms` field that specifies the platforms that you want to build for, and a` tags` field that specifies the tags that you want to apply to the image.


## How does bake work?


When you run` docker buildx bake` , it reads the bake file and builds all the targets in parallel. It uses BuildKit to build the images, which means it can take advantage of BuildKit's parallelization and deduplication of work.


By default, bake will look for the following bake file references without specifying a file:


- ` compose.yaml`
- ` compose.yml`
- ` docker-compose.yml`
- ` docker-compose.yaml`
- ` docker-bake.json`
- ` docker-bake.override.json`
- ` docker-bake.hcl`
- ` docker-bake.override.hcl`


You can also specify a bake file with the` --file` flag:


```text
docker   buildx   bake   -f   my-bake-file.hcl
```


As mentioned earlier, if you call bake without specifying a group or target, it will use the` default` group. You can specify a group or target(s) directly in the call. For example, if we only wanted to build the app and db images from the previous example, we could run:


```text
docker   buildx   bake   app   db
```


### Bake under the hood


A` target` defines a single Docker image build in a bake file. So, when you look at a bake file, you can think of it as a list of` docker build` commands. So, the number of targets in your bake file will represent the number of images you build in parallel against a single BuildKit instance.


BuildKit builds each target concurrently and deduplicates common work across targets. In practice, one target will compute the common work, and the others will just reuse that result.


#### Example of deduplicating work


The power of BuildKit deduplication can shine when you are leveraging bake. Here is an example bake file to help illustrate where deduplication comes in:


```text
target   "base"   {
dockerfile   =   "Dockerfile.base"
platforms    =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags         =   [  "repo/base:  ${  TAG  }  "  ]
}


target   "app"   {
contexts   =   {
base   =   "target:base"
}
dockerfile   =   "Dockerfile.app"
platforms    =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags         =   [  "repo/app:  ${  TAG  }  "  ]
}


target   "db"   {
contexts   =   {
base   =   "target:base"
}
dockerfile   =   "Dockerfile.db"
platforms    =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags         =   [  "repo/cron:  ${  TAG  }  "  ]
}
```


The` contexts` property on the` app` and` db` targets tells BuildKit that they depend on the` base` target. When you run` docker buildx bake app db` , BuildKit will first build the` base` target and then use that result for the` app` and` db` targets.


But here's the cool part: BuildKit will only build the` base` target once, even though there are two different targets specifying it. This is because BuildKit can deduplicate the work across targets and reuse the result of the` base` target for the` app` and` db` targets.


## Bake file syntax in HCL


We've already covered the group and target constructs in a bake file, but you can do a few more things with a bake file.


- ` target` defines the build target of an image.
- ` group` defines a group of targets.
- ` variable` exists to define build arguments and variables.
- ` function` exists for custom Bake functions.


It's generally considered best practice to write your bake files in HCL as that is where the most features are available. However, you can also write bake files in JSON or Docker Compose format.


### ` target` syntax


The` target` property represents a single` docker build` command. It has the following properties.


#### ` target.args`


This is equivalent to the` --build-arg` flag in` docker build` . It allows you to pass build arguments to the build. Here is an example of how you would use the` args` property:


```text
target   "app"   {
args   =   {
FOO   =   "bar"
}
}
```


#### ` target.annotations`


You can use this property to add annotations to images built via bake. It accepts a list of key-value pairs. By default, bake only adds annotations to image manifests, but you can use prefixes to add the annotations to additional levels like the image index.


Here is an example of how you would use the` annotations` property:


```text
target   "app"   {
annotations   =   [  "org.opencontainers.image.version=1.0.0"  ]
}
target   "app2"   {
annotations   =   [  "index,manifest:org.opencontainers.image.version=1.0.0"  ]
}
```


The` app` target will add the annotation to the image manifest, and the` app2` target will add the annotation to the root image index and the manifest. The complete list of annotation levels supported are:


- ` manifest` to annotate the image manifests
- ` index` to annotate the root image index
- ` manifest-descriptor` to annotate the manifest descriptors in the index
- ` index-descriptor` to annotate the index descriptor in the image layout


#### ` target.attest`


This allows you to add build attestations to the image being built in the target. In the image build, you can use this property to set the` --sbom` and` --provenance` flags. Here is an example of how you would use the` attest` property:


```text
target   "app"   {
attest   =   [
"type=sbom"
]
}
```


#### ` target.cache-from`


This allows you to specify a list of cache sources that the BuildKit builder will import when building the target. It's the equivalent of` --cache-from` . Here is an example of how you would use the` cache-from` property:


```text
target   "app"   {
cache-from   =   [  "type=registry,ref=repo/app:cache"  ]
}
```


#### ` target.cache-to`


This allows you to specify a cache destination that the BuildKit builder will export to when building the given target. It's the equivalent of` --cache-to` . Here is an example of how you would use the` cache-to` property:


```text
target   "app"   {
cache-to   =   [  "type=registry,ref=repo/app:cache"  ]
}
```


#### ` target.context`


This is the same as the build context positional argument in` docker build` . You can specify the local path to use as the build context or a remote path via a URL. Here is an example of how you would use the` context` property:


```text
target   "app"   {
context   =   "path/to/context"
}
```


**Note: If you don't specify a context for a target, it will default to the current working directory.**


#### ` target.contexts`


This allows you to define additional build contexts for the target. This is useful when you reference another context inside your` Dockerfile` via a` FROM` statement or` --from=name` . Here is an example of how you would use the` contexts` property:


```text
target   "base"   {
dockerfile   =   "Dockerfile.base"
}


target   "app"   {
dockerfile   =   "Dockerfile.app"
contexts   =   {
base   =   "target:base"
}
}
```


Here, the` app` target references the` base` target as a context. This means that when the` app` target is built, it will have the additional context,` base` , available. So, the` Dockerfile.app` can reference that context in a` FROM` statement.


```text
# Dockerfile.app
FROM   base
...
```


#### ` target.dockerfile-inline`


This allows you to specify the Dockerfile content inline in the bake file. This is useful to avoid creating a separate Dockerfile on disk. Here is an example of how you would use the` dockerfile-inline` property:


```text
target   "app"   {
dockerfile-inline   =   "FROM node\nRUN echo app"
}
```


#### ` target.dockerfile`


This is the path to the Dockerfile to use for the image build of the target. It's the equivalent of the` -f` flag in` docker build` . Here is an example of how you would use the` dockerfile` property:


```text
target   "app"   {
dockerfile   =   "Dockerfile.app"
}
```


**Note: If you don't specify a Dockerfile for a target, it will default to` Dockerfile` .**


#### ` target.inherits`


This allows you to inherit properties from other targets. This is useful when you want to reuse properties from another target. Here is an example of how you would use the` inherit` property:


```text
target   "base"   {
dockerfile   =   "Dockerfile.base"
platforms    =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags         =   [  "repo/base:  ${  TAG  }  "  ]
}


target   "app"   {
inherits     =   "target:base"
dockerfile   =   "Dockerfile.app"
tags         =   [  "repo/app:  ${  TAG  }  "  ]
}
```


In this example, the` app` target inherits the` platforms` properties from the` base` target. This means the` app` target will be built for the same platforms as the` base` target.


#### ` target.labels`


This allows you to set the` --label` flag for the image build. Here is an example of how you would use the` labels` property:


```text
target   "app"   {
labels   =   {
"org.opencontainers.image.version"   =   "1.0.0"
}
}
```


#### ` target.matrix`


This lets you parameterize a single target to build images for different inputs. This is particularly helpful for removing duplication of targets in your bake file. Here is an example of how you would use the` matrix` property:


```text
target   "build"   {
name   =   "build-  ${  app  }  "
matrix   =   {
app   =   [  "app"  ,   "db"  ,   "cron"  ]
}
dockerfile   =   "Dockerfile.  ${  app  }  "
platforms    =   [  "linux/amd64"  ,   "linux/arm64"  ]
tags         =   [  "repo/  ${  app  }  :  ${  TAG  }  "  ]
}
```


Here we have a generic` build` target to build images for` app` ,` db` , and` cron` . This is useful when you have multiple targets that are very similar, and you want to avoid duplicating the properties across multiple targets.


**Note: The` name` property is required when using the` matrix` property to create the unique image build for each value in the matrix.**


#### ` target.no-cache-filter`


This allows you to set the` --no-cache-filter` flag for the image build. It's handy for telling the build to skip the cache for certain stages. Here is an example of how you would use the` no-cache-filter` property:


```text
target   "app"   {
no-cache-filter   =   "build"
}
```


This will skip the cache for the` build` stage in the Dockerfile.


#### ` target.no-cache`


This allows you to set the image build's` --no-cache` flag. It's handy for telling the build to skip the cache entirely. Here is an example of how you would use the` no-cache` property:


```text
target   "app"   {
no-cache   =   true
}
```


#### ` target.output`


This allows you to set the image build's` --output` flag. You can control the export action you want after your build. Here is an example of how you would use the` output` property:


```text
target   "app"   {
output   =   "outputdir"
}
```


This tells the` app` target to export its build result to the` outputdir` directory.


#### ` target.platforms`


This allows you to set the` --platform` flag for the image build. You can use this to control the platforms you want to build for. Here is an example of how you would use the` platforms` property:


```text
target   "app"   {
platforms   =   [  "linux/amd64"  ,   "linux/arm64"  ]
}
```


This will build an image for both Intel and Arm. With` docker buildx bake` , this will default to using emulation to build for the non-native platform. With` depot bake` , it will build for both platforms in parallel on native CPUs.


#### ` target.pull`


This allows you to set the` --pull` flag for the image build. It can tell the builder whether or not to pull images during the build. Here is an example of how you would use the` pull` property to tell BuildKit to always pull images:


```text
target   "app"   {
pull   =   "always"
}
```


#### ` target.secret`


This allows you to set the` --secret` flag for the image build. It will enable you to expose secrets to the build using` RUN --mount=type=secret` . Here is an example of how you would use the` secret` property:


```text
target   "app"   {
secret   =   [
"type=env,id=SECRET_TOKEN"
]
}
```


#### ` target.ssh`


This allows you to set the` --ssh` flag for the image build. It allows you to expose SSH keys to the build using` RUN --mount=type=ssh` . Here is an example of how you would use the` ssh` property:


```text
target   "app"   {
ssh   =   [
"default"
]
}
```


#### ` target.tags`


This allows you to set the` --tags` flag for the image build. It's the equivalent of the` -t` flag in` docker build` . Here is an example of how you would use the` tags` property:


```text
target   "app"   {
tags   =   [  "repo/app:test"  ]
}
```


#### ` target.target`


This allows you to set the` --target` flag for the image build. It will enable you to specify a specific target for the build in a multi-stage Dockerfile. Here is an example of how you would use the` target` property:


```text
target   "app"   {
target   =   "build"
}
```


This tells the build to build everything up to the` build` stage in the Dockerfile and create a final image up to that stage.


### ` group` syntax


A group defines a set of targets to build at once. They take precedence over any targets that share the same name. Here is an example of multiple groups:


```text
group   "default"   {
targets   =   [  "app"  ,   "db"  ,   "cron"  ]
}


group   "app-db"   {
targets   =   [  "app"  ,   "db"  ]
}
```


### ` variable` syntax


When writing a bake file in HCL, you can define variable blocks that can be used in your Dockerfile or for property values in your targets. Here is an example of how you would use the` variable` block:


```text
variable   "TAG"   {
default   =   "latest"
}


target   "app"   {
tags   =   [  "repo/app:  ${  TAG  }  "  ]
}
```


If you invoke this from the outside with the` TAG` variable set, it will override the default value:


```text
TAG  =  production   docker   buildx   bake   app
```


#### Built-in variables


There are two variables always available to you in a bake file:


1. ` BAKE_CMD_CONTEXT` - Contains the main context when building from a remote bake file.
2. ` BAKE_LOCAL_PLATFORM` - This tells you the platform of the host running the bake command.


## Using bake with Docker Compose


You can also use a Docker Compose file as a bake file. It has limitations, like not supporting` inherits` or variable blocks. But it's a great way to build multiple services in parallel. Here is an example of a Docker Compose file used as a bake file:


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


When you run` docker buildx bake -f compose.yaml` , it will build all of the services in parallel as separate targets.


You can also specify a` .env` file next to your compose file to pass in environment variables to the build. Here is an example where our` app` target uses an environment variable:


```text
services  :
app  :
build  :
dockerfile  :   Dockerfile.app
platforms  :
-   linux/amd64
-   linux/arm64
tags  :
-   'repo/app:${TAG}'
```


```text
# .env file
TAG  =  latest
```


## Using bake in GitHub Actions


Docker has published an action,` docker/bake-action` , that allows you to use` docker buildx bake` in your GitHub Actions workflows. Here is an example of how you would use the` docker/bake-action` in a GitHub Actions workflow:


```text
name  :   bake images
on  :
push  :
branches  :
-   'main'


jobs  :
bake  :
runs-on  :   ubuntu-latest
steps  :
-   name  :   Checkout
uses  :   actions/checkout@v4
-   name  :   Set up Docker Buildx
uses  :   docker/setup-buildx-action@v3
-   name  :   Build and push images in bake file
uses  :   docker/bake-action@v4
with  :
push  :   true
```


This will run` docker buildx bake` on every push to the` main` branch and push the images to the registry.


## Even faster bake with Depot


Depot is a remote container build service running an optimized version of BuildKit on fast cloud VMs for Intel & Arm with automatic persistent caching across builds. We've optimized the build process to be even faster than running` docker buildx bake` on your local machine.


Our CLI,[depot](https://depot.dev/docs/cli/reference) , is a drop-in replacement for the build options available in Docker. Including build and bake. You can use Depot by switching` docker` for` depot` in your commands.


```text
- docker buildx bake
+ depot bake
```


Behind the scenes, Depot builds your images on 16 CPU core machines with 32 GB of memory. We also persist your cache to fast NVMe storage that is instantly available across builds. We also run native Intel & Arm BuildKit builders so that you can build images on native CPUs instead of relying on emulation.


**Depot builds your Docker images up to 40x faster with a single line code change.**


If you're interested in getting an even faster bake, you can[sign up for our 7-day free trial](https://depot.dev/start) and start using Depot today.


## FAQ


How do I build multiple Docker images at once?


Use` docker buildx bake` to define all your images in a single file and build them in parallel with one command. You write a bake file (in HCL, JSON, or Docker Compose format) with multiple targets, where each target represents one Docker image. BuildKit builds all targets concurrently and automatically deduplicates shared work, so if multiple images use the same base, it's built once and reused.


What is the matrix property in docker buildx bake?


The` matrix` property lets you parameterize a single target to build multiple similar images without duplicating your configuration. For example, if you have app, db, and cron services with nearly identical Dockerfile patterns, you can define one target with` matrix = {(app = \['app', 'db', 'cron'\])}` and use` ${app}` variables in your dockerfile and tags properties. You must include a` name` property when using matrix to create unique builds.


Can I reference one target as a dependency in another target?


Yes, use the` contexts` property to reference another target as a build context. Set` contexts ={' '} {(base = 'target:base')}` in your target, and BuildKit will build the base target first, then make it available in your Dockerfile via` FROM base` . BuildKit deduplicates this work automatically, so even if multiple targets depend on the same base, it's only built once.


How do I pass secrets to builds in a bake file?


Use the` secret` property in your target with the format` secret = \["type=env,id=SECRET_TOKEN"\]` . This exposes secrets to your build steps, which you can then mount in your Dockerfile using` RUN --mount=type=secret,id=SECRET_TOKEN` . The secret is never written to the image layers and is only available during the build process.


## Related posts


- [BuildKit in depth: Docker's build engine explained](https://depot.dev/blog/buildkit-in-depth)
- [Docker buildx explained](https://depot.dev/blog/docker-buildx-explained)
- [How to build Arm and multi-architecture containers today](https://depot.dev/blog/building-arm-containers)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
