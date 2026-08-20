---
schema_version: "1.0.0"
document_id: "ff2b93721985303b0bfc9ab5c5d0f1e81a9e672f2e3e98375b3defb95e355acc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-build-fundamentals"
published_at: "2023-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:c8dd77269522687df0fcdc1d68434bb4b19514dd215b8ca15534aaa714a49e6a"
---

# The fundamentals of building a Docker image

Docker containers and containerization are popular ways to package and deploy various applications. But, building a Docker image is a skill that begins at the very beginning. From the moment we create a Dockerfile, we should be thinking about the fundamentals of building a Docker image quickly.


In this post, we'll work with an example project to chat through four fundamentals to make building Docker images as fast as possible when using one of the docker commands; the` docker build` command.


1. Keeping our Docker build context small by excluding things in our source code for a faster image build
2. Using a minimal base image for a smaller container image
3. Leveraging a multi-stage image build for keeping our final Docker container slimmed down
4. How to use BuildKit cache mounts for a fast image build when you use cache but it invalidates


## Example project


Let's create a sample Node API project we can work with throughout to build a new docker image. We will leverage[Fastify](https://fastify.dev/) to create an API that we configure via the` fastify-cli` .


First, we initialize a project via` pnpm` or your favorite package manager. Then we install the` fastify-cli` globally via your preferred package manager:


```text
pnpm   add   -g   fastify-cli
```


Then, create a new project via the` fastify-cli` . We use the` --lang=ts` flag to create a TypeScript project:


```text
fastify-cli   generate   --lang=ts   .
```


The details of what the API does aren't that important for this post, so we can stick with what` generate` gives us. We can start the API after running` pnpm install` via` pnpm start` and test it out:


```text
pnpm   install   &&   pnpm   start
{  "level"  :30,  "time"  :1684761301504,  "pid"  :42443,  "hostname"  :  "depotmacbook.home"  ,  "msg"  :  "Server listening at http://127.0.0.1:3000"  }
```


We can test the API out via` curl` :


```text
curl   localhost:3000
{  "root"  :true}
```


## Fundamentals of building a Docker image quickly


Now that we have an example project to containerize, we can start looking at the fundamentals of` docker build` to build images fast. First, we need a` Dockerfile` and a` .dockerignore` file at the root of our git repository.


```text
touch   Dockerfile   .dockerignore
```


### Fundamental #1: Use a` .dockerignore` file to exclude files from the build context


The` .dockerignore` file excludes files from the build context during the build process. We don't want to copy over files that aren't needed to run our application or that we generate during the build.


We exclude things like` README` files or unnecessary files like our` .git` directory. We also exclude artifacts that will get generated during the build, like` node_modules` , because we will install those in our build.


```text
node_modules
.git
.gitignore
README.md
dist/*
```


### Fundamental #2: Use a minimal base image


When building Docker images, it's common to jump to a generic base image like the` ubuntu` or` node:20` . But, this has downsides that impact the build command and even` docker run` down the line.


1. Generic base images are often large, which means we have to download a lot of data before we can start building our image layers
2. Large base images *can* make our final image larger than it needs to be because they include a lot of unnecessary dependencies that we don't need
3. Large Docker images are slow to build and slow to run


So, instead of reaching for the large base image, we can reach for the` slim` version instead. Here is an updated Docker file that uses the` slim` version of the` node:20` base image:


```text
FROM   node:20-slim


ENV   PNPM_HOME=  "/root/.local/share/pnpm"
ENV   PATH=  "${PATH}:${PNPM_HOME}"


RUN   corepack enable
RUN   pnpm add -g fastify-cli


WORKDIR   /app
COPY   package.json pnpm-lock.yaml tsconfig.json ./
RUN   pnpm fetch --frozen-lockfile
RUN   pnpm install --frozen-lockfile


COPY   src/ ./src
RUN   pnpm build:ts


EXPOSE   3000
CMD   [  "fastify"  ,   "start"  ,   "-l"  ,   "info"  ,   "dist/app.js"  ]
```


This smaller base image is **244MB** instead of **951MB** . A smaller base image will make building the Docker image faster. Why? Because we will have less data to download and package up. Our final image is also smaller because we won't have unnecessary dependencies from the larger base image.


##### Why not` alpine` ?


Another option is to use the` alpine` base image. But, it's an unofficial Node runtime because it uses[musl](https://www.musl-libc.org/) to implement the C standard library. The difference can cause performance issues, bugs, and application crashes. So for this post, we stick to a` slim` image instead.


#### What's happening in the actual Dockerfile?


Our first` RUN` command is` corepack enable` so we can access` pnpm` without installing it ourselves. Next, we install the` fastify-cli` so that we can use it to start our API.


Then we use the` COPY` command to copy in our` package.json` ,` pnpm-lock.yaml` , and` tsconfig.json` files. With those files copied, we can use another` RUN` command to execute` pnpm install` to install our dependencies.


Once our dependencies are installed, we can build our TypeScript files and output them into our` dist/` folder via the final` RUN` command. Lastly, we expose port 3000 for our image to run on and set the` CMD` instruction to start our API.


We can build and run the image with the following docker commands from our command line:


```text
docker   build   --tag   fastify-example   .
docker   run   --interactive   fastify-example
{  "level"  :30,  "time"  :1685107923669,  "pid"  :1,  "hostname"  :  "7ac713c84acc"  ,  "msg"  :  "Server listening at http://0.0.0.0:3000"  }
```


##### **Copy only what you need to install dependencies**


Notice that we don't copy over our entire repository (i.e.,` COPY . .` ) and only copy the files needed to install our dependencies. If we copy over our entire repository, any code change would invalidate the cache. Invalidating the cache would mean having to reinstall our dependencies. This is a waste of time and resources.


Check out our[fast Dockerfiles theory & practice post](https://depot.dev/blog/fast-dockerfiles-theory-and-practice) for more background on how that subtle mistake could cause your` docker build` times to explode.


### Fundamental #3: Use a multi-stage build


A multi-stage build uses many` FROM` statements in our` Dockerfile` to create many images. We can copy files from one stage to another, which allows us to create a final image that only contains the files we need to run our application. We can also build stages in parallel, which speeds up our builds.


Here is what our` Dockerfile` looks like with a multi-stage build:


```text
FROM   node:20-slim   as   base


ENV   PNPM_HOME=  "/root/.local/share/pnpm"
ENV   PATH=  "${PATH}:${PNPM_HOME}"
RUN   corepack enable
RUN   pnpm add -g fastify-cli


FROM   base   as   dependencies


WORKDIR   /app
COPY   package.json pnpm-lock.yaml tsconfig.json ./
RUN   pnpm fetch --frozen-lockfile --prod
RUN   pnpm install --frozen-lockfile --prod


FROM   base   as   build


WORKDIR   /app
COPY   package.json pnpm-lock.yaml tsconfig.json ./
RUN   pnpm fetch --frozen-lockfile
RUN   pnpm install --frozen-lockfile
COPY   src/ ./src
RUN   pnpm build:ts


FROM   base


WORKDIR   /app
COPY   --from=dependencies /app/node_modules /app/node_modules
COPY   --from=build /app/dist /app/dist
EXPOSE   3000
CMD   [  "fastify"  ,   "start"  ,   "-l"  ,   "info"  ,   "dist/app.js"  ]
```


We now have four stages in our multi-stage build process:


1. ` base` - This is the base image we use to install` pnpm` and` fastify-cli`
2. ` dependencies` - We install our dependencies without dev dependencies via the` --prod` flag
3. ` build` - We install our dependencies *with dev dependencies* to build our TypeScript files
4. Our final image - We copy over our dependencies from the` dependencies` stage and our built TypeScript files from the` build` stage using the` base` stage as our base image


When we run a` docker build` now, we build each stage in parallel. Then in our final stage, we copy the files from the previous build stages into our final container image that the Docker engine can run. The earlier stages get excluded from the final image. The net result is that we have a final image that is **285MB** instead of **412MB** .


### Fundamental #4: Leverage BuildKit cache mounts


BuildKit cache mounts speed up our builds when the cache gets invalidated and a given layer needs rebuilding. The target of a cache mount is persisted across builds, assuming you have a persistent disk to keep them on. See our post on[using BuildKit cache mounts in CI](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci) for more in-depth details.


Here is our final` Dockerfile` with BuildKit cache mounts:


```text
FROM   node:20-slim   as   base


ENV   PNPM_HOME=  "/root/.local/share/pnpm"
ENV   PATH=  "${PATH}:${PNPM_HOME}"


RUN   corepack enable
RUN   pnpm add -g fastify-cli


FROM   base   as   dependencies


WORKDIR   /app
COPY   package.json pnpm-lock.yaml tsconfig.json ./
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm fetch --frozen-lockfile --prod
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm install --frozen-lockfile --prod


FROM   base   as   build


WORKDIR   /app
COPY   package.json pnpm-lock.yaml tsconfig.json ./
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm fetch --frozen-lockfile
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm install --frozen-lockfile
COPY   src/ ./src
RUN   pnpm build:ts


FROM   base


WORKDIR   /app
COPY   --from=dependencies /app/node_modules /app/node_modules
COPY   --from=build /app/dist /app/dist
EXPOSE   3000
CMD   [  "fastify"  ,   "start"  ,   "-l"  ,   "info"  ,   "dist/app.js"  ]
```


We added a` --mount` flag to our` RUN` statements that install our dependencies. This tells BuildKit to store the contents of` /root/.local/share/pnpm/store` , the virtual store directory for` pnpm` , across builds. So the next time we build our image, the virtual store will be mounted, and we won't have to download the complete list of dependencies again.


## Conclusion


From the moment we create a Dockerfile, we should be thinking about the fundamentals of building a Docker image as fast as possible.


These fundamentals will help us build images fast. They allow us to leverage as much of our previous build results as possible to avoid unnecessary work. They help us keep our image sizes small and our builds & deployments fast.


## 40x faster builds with Depot


Our CLI,` depot build` , is a drop-in replacement for` docker build` that can make your Docker image builds up to 40x faster.


We launch remote Docker image builders supporting x86 and Arm architectures. These remote builders come with 6 CPUs, 32 GB memory, and a persistent 50 GB NVMe cache disk.


With these fundamentals, you can leverage Depot to build your images even faster. We automatically persist your layer cache across builds via a local SSD. It's available instantly during builds and shared with anyone who has access to the project. A developer who builds an image can reuse the result their coworker produced minutes before.


If you want to try out Depot to accelerate your` docker build` workflow, sign up and try things out via our[quickstart guide](https://depot.dev/docs/container-builds/quickstart) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
