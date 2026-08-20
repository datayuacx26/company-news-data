---
schema_version: "1.0.0"
document_id: "614babb746737e69bdb66e6a185a55eedab48f118a1a486a0685b2ae429ceadd"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-build-image"
published_at: "2024-08-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:00539c83f06d3eb80dcc9ee0b55dc838533f74bec02727531a45ad7d996375f1"
---

# The complete guide to getting started with building Docker images

Packaging applications and services into containers has been around for a while. Docker was a technology that came out of another idea called DotCloud in 2013. So, even the Docker containers we know and love today are a decade old. But it's important to remember that the underlying technology of a Docker container is even older.


## What is Docker?


The underlying technologies backing Docker containers are low-level Linux kernel components like cgroups, namespaces, and a union-capable file system like OverlayFS. These technologies are what allow Docker containers to be so lightweight and portable. Combined, they allow a single Linux VM to run multiple containers.


### Installing Docker


To get started with Docker, you need to install it first. Depending on what you're running containers on, there are multiple ways to do that. Here are three Docker installation guides:


1. [Install Docker for Linux](https://docs.docker.com/desktop/install/linux-install/)
2. [Install Docker for Mac](https://docs.docker.com/desktop/install/mac-install/)
3. [Install Docker for Windows](https://docs.docker.com/desktop/install/windows-install/)


Each Docker installation guide ultimately installs Docker Desktop and configures the Docker engine on the given operating system.


## What is a Docker image vs. a Docker container?


When getting started with Docker, a common question is, what is the difference between a Docker image and a Docker container? A Docker image is a series of layers stacked on each other that form the dependencies and source code needed to run your application. During a Docker image build, all those layers get packaged together to produce a final Docker image.


A Docker container is a runnable instance of a Docker image. You can run multiple containers with the same image to run multiple copies of your application or service.


A Docker image is the source code and dependencies packaged together, and the Docker container is the running instance of that image.


## So, what is a Dockerfile?


A Dockerfile is a file that contains instructions for how to build a Docker image. It's a text file that includes a series of instructions that are executed in order to build a Docker image. The Dockerfile is the recipe that produces our Docker image.


As we will see in a minute, a Dockerfile is executed from top to bottom during a given docker image build. Instructions are invoked in order, and each instruction generally maps to an image layer. Those layers, stacked on top of each other one after the other, form our final Docker image.


### Dockerfile instructions and what they do


Several different instructions can be used in a Dockerfile. Each instruction is a command that is executed during the build process. The most common instructions are:


Instruction What it does


` FROM` Defines a new build stage and sets the base image for that stage


` RUN` Executes any commands it is given in a new layer on top of the current image that has been built up to that point


` COPY` Copies the contents from a source directory to the filesystem at the path passed in to a new layer in the image


` ADD` A more advanced version of` COPY` that supports things like local tar extraction and remote URLs


` CMD` Defines the default set of arguments that are supplied to the process that runs the container when it's launched via` ENTRPOINT`


` ENTRYPOINT` Configures the executables or commands that will run once the container is initialized


` USER` Sets the user that the container is run under, often used to run containers as non-root


` LABEL` Adds key-value labels to the image being built; note that labels are passed down from base images


` ARG` Define build-time only variables that can be used during the Docker image build


` ENV` Sets environment variables from within the Docker image that can be used during the build process or when the container is run


` EXPOSE` Defines a port that the container will listen on when the image is run as a container


` WORKDIR` Sets the working directory for the commands that follow it


` VOLUME` Creates a mount point with a specific name that is bound to a mounted volume from the underlying host or another container


#### What's the difference between` CMD` and` ENTRYPOINT` ?


It's important to remember that` CMD` and` ENTRYPOINT` are not the same thing.` CMD` defines the default set of arguments that are supplied to the process that runs the container when it's launched via` ENTRYPOINT` . We can override the arguments to` CMD` when we run the container via` docker run` .


But why do I see Dockerfile(s) without an` ENTRYPOINT` ? If you don't specify an` ENTRYPOINT` , Docker will use the default` ENTRYPOINT` of` /bin/sh -c` . This means that if you don't specify an` ENTRYPOINT` , your` CMD` will be run with` /bin/sh -c` as the` ENTRYPOINT` .


Both statements have two forms, shell command form and executable command form. In shell command form,` ENTRYPOINT /bin/echo "Hello, $name"` , goes through shell validation and processing. So` docker run -it Kyle` on this test will output` Hello, Kyle` .


The executable command form skips shell validation and processing. So` ENTRYPOINT \["/bin/echo", "Hello, $name"\]` will output` Hello, $name` because the shell is not processing the command.


## Building a Docker image


Now that we have a solid foundation, we can build a Docker image and see how a Docker image build works with a sample application. For this example, we will use an example[Fastify API](https://fastify.dev/) that uses TypeScript and` pnpm` for package management. The example project can be` git clone` on our[GitHub](https://github.com/depot/examples/tree/main/node/pnpm-fastify) .


After cloning the project, we can run` pnpm install` and` pnpm build` from the root of the example to install our dependencies and build our TypeScript source code.


```text
pnpm   install   &&   pnpm   build
```


After building the code, we should see a` dist` directory with our compiled code.


```text
ls   dist/
index.js
index.js.map
```


We can now run the example API outside a Docker container to see if it works as expected. We use` curl` to hit a` /health` endpoint on that API that returns a simple JSON response.


```text
pnpm   start
curl   localhost:3000/health
{  "alive"  :true}
```


### Keeping our Docker image size down


Before jumping straight into writing a Dockerfile for our example project, we need to start with a` .dockerignore` first.


A` .dockerignore` file tells the build what files and directories to ignore and exclude from the Docker build context when we run` docker build` . Our project git repositories often contain many files and folders that we don't need in our final image or the build context itself.


```text
node_modules
Dockerfile
.git
.gitignore
dist/**
README.md
```


This` .dockerignore` file tells the Docker build to ignore all of these files and directories during the build. These files will be excluded from the Docker build context and thus won't be copied via any` COPY` or` ADD` instructions.


### Writing a Dockerfile


Now that we have a` .dockerignore` file, we can write our Dockerfile. For a more advanced` Dockerfile` that is highly optimized, we can use our[best-practice Dockerfile for Node.js & pnpm](https://depot.dev/docs/container-builds/optimal-dockerfiles/node-pnpm-dockerfile) . The optimized Dockerfile uses multi-stage builds, optimized Docker layer caching, and BuildKit cache mounts to optimize the docker build image process.


#### Simple example Dockerfile


For this post, we will use a more straightforward example Dockerfile to walk through core concepts.


First, we need a base image for our Docker image to be built from. Since our example project is in Node, an official Node base image like` node:20` is an excellent place to start.


```text
FROM   node:20
```


Once we have a base image, we can install our dependencies and build our application. We first enable[corepack](https://nodejs.org/api/corepack.html) , an experimental tool for managing versions of package managers. We then copy in our` package.json` and` pnpm-lock.yaml` files and install our dependencies. Finally, we copy in our source code and build our application.


```text
RUN   corepack enable


COPY   package.json pnpm-lock.yaml ./
RUN   pnpm install --frozen-lockfile
COPY   . .
RUN   pnpm build
```


The final thing to do is to set our` CMD` instruction, which tells the container what to run when it's launched. In our case, we want to run our compiled` index.js` file, our API.


```text
ENV   NODE_ENV production
CMD   [  "node"  ,   "./dist/index.js"  ]
```


**Note: This Dockerfile is not optimized for size or build performance. It's meant to be an example to follow along with. For an optimized version that uses multi-stage builds and Docker layer caching, see our[best-practice Dockerfile for Node.js & pnpm](https://depot.dev/docs/container-builds/optimal-dockerfiles/node-pnpm-dockerfile) .**


### Building our Docker image


Now that we have a Dockerfile, we can start building our Docker image with` docker build` . We can run this command from the root of our example project. We tag our resulting image with the name` fastify-example` via the` --tag` flag.


```text
docker   build   --tag   fastify-example   .
```


If we run the` docker images` command, we should see our new image in our list of container images.


```text
docker   images
REPOSITORY                                  TAG         IMAGE   ID         CREATED            SIZE
fastify-example                             latest      7e3f51733ddd     8   seconds   ago      1.18GB
```


The docker build command is just one way to build your image. There is also the` docker buildx build` command. The docker build command is a subset of the larger` docker buildx` command. If you want to leverage extended build capabilities for BuildKit, you can use` docker buildx build` .


### Running our Docker image


Now that we have built our Docker image with the` fastify-example` tag, we can try running it locally. We can run a Docker container of our image via the` docker run` command.


We run our Docker container with the` -p` (i.e.,` --port` ) flag to specify that we want to forward traffic from port 8080 on our host machine to port 3000 in the container because our API is listening on this. We also run our container with the` -d` flag, which tells the Docker Daemon to run the container detached in the background.


```text
docker   run   -p   8080:3000   -d   fastify-example
```


We can verify our container is running via the` docker ps` command.


```text
docker   ps
CONTAINER   ID     IMAGE               COMMAND                    CREATED           STATUS           PORTS                      NAMES
5595944ea42b     fastify-example     "docker-entrypoint.s…"     3   seconds   ago     Up   2   seconds     0.0.0.0:8080  -  >  3000/tcp     peaceful_brahmagupta
```


We can also verify our Docker container is up and working by hitting the` /health` endpoint with` curl` .


```text
curl   localhost:8080/health
{  "alive"  :true}
```


We can also use other Docker CLI commands like` docker logs` to see the logs from our container. Note that the` logs` command expects the container ID or name, not the image name. From our example above, the name of our container is` peaceful_brahmagupta` .


```text
docker   logs   peaceful_brahmagupta
{  "level"  :30,  "time"  :1695637221083,  "pid"  :1,  "hostname"  :  "6e8107cd9149"  ,  "msg"  :  "Server listening at http://0.0.0.0:3000"  }
```


We can use the` docker inspect` command to get a low-level description of our container. The JSON output can be helpful for debugging and troubleshooting.


```text
docker   inspect   peaceful_brahmagupta
```


Finally, we can call` docker stop` to stop our container with a graceful shutdown, or we can call` docker kill` to kill our container, which will terminate it immediately.


### Pushing our Docker image to a registry


When we build a Docker image locally or via our[remote builders in Depot](https://depot.dev/start) , the container image, by default, is kept on the machine that ran the Docker image build. When we want to run the Docker image locally, as we saw in the earlier step, the image staying on our machine is excellent.


But, most of the time, we want to push our image to a Docker container registry so we can share it with other developers, deploy it to our production environments, etc.


There are numerous container registries like Docker Hub, Amazon Elastic Container Registry (ECR), GCP Artifact Registry, and GitHub Container Registry. For this example, we will assume we are using GitHub Container Registry.


To push to a Docker container registry, we generally need to call` docker login` to authenticate to our registry. For GitHub Container Registry, we can use the` ghcr.io` hostname, our GitHub username, and a[personal access token (PAT) to authenticate](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-with-a-personal-access-token-classic) .


```text
docker   login   ghcr.io   -u   GITHUB_USERNAME   --password   GITHUB_PAT
>   Login succeeded
```


After logging into our container registry, we can build our image with a tag that includes the registry hostname, our GitHub username, and the image name. We also specify the` --push` flag, which will push our image to the registry we've tagged it with.


```text
docker   build   -t   ghcr.io/GITHUB_USERNAME/fastify-example:latest   --push   .
```


Alternatively, we can use` docker tag` and` docker push` to push an image we've built locally to a registry.


```text
docker   tag   fastify-example   ghcr.io/GITHUB_USERNAME/fastify-example:latest
```


This tags our` fastify-example` Docker image with` ghcr.io/GITHUB_USERNAME/fastify-example:latest` , and then we can push it to our registry.


```text
docker   push   ghcr.io/GITHUB_USERNAME/fastify-example:latest
```


## Conclusion


We've covered in this post how to get started with Docker and build a Docker image from a Dockerfile. We've also covered how to run a Docker container from that image and how to push that image to a container registry. All of these are handy to know when it comes to working with containers locally and in production.


With Depot, we build the Docker image up to 40x faster and provide critical insights about how to rewrite your Dockerfile to build faster, leverage caching, and more. We remove the need to think about the artifacts Docker produces, allowing you to focus on writing your own code and getting it into production faster.


You can[sign up for an account](https://depot.dev/start) and get your first 60 minutes of build time free. If you have questions comments or want to chat more about containers, check out our[Community Discord](https://discord.gg/MMPqYSgDCg) .


## FAQ


What's the difference between a Docker image and a Docker container?


A Docker image is the source code and dependencies packaged together into layers; it's like a blueprint. A Docker container is a running instance of that image. You can run multiple containers from the same image, which is how you can run multiple copies of your application or service at the same time.


Why do I need a .dockerignore file?


A` .dockerignore` file tells Docker what files and directories to exclude from the build context when you run` docker build` . Your project repositories often contain files you don't need in the final image, things like` node_modules` ,` .git` , or` README.md` . Excluding these keeps your build context smaller and prevents unnecessary files from being copied into your image via` COPY` or` ADD` instructions.


Can I override the CMD instruction when I run my container?


Yes, you can override the arguments to` CMD` when you run the container via` docker run` . That's the whole point of` CMD` : it defines the default arguments, but you can change them at runtime. If you specified` ENTRYPOINT` , your new arguments will be passed to that entrypoint. If you didn't specify an` ENTRYPOINT` , Docker uses the default of` /bin/sh -c` .


Why is my Docker image so large even with a .dockerignore?


The size of your final image depends on your base image and what gets installed during the build. The example Dockerfile in this post produces a 1.18 GB image because it uses the full` node:20` base image and includes all dependencies. To get a smaller image, you'd want to use multi-stage builds, where you build in one stage and copy only the compiled artifacts to a smaller runtime stage. The optimized Dockerfile linked in the post shows how to do this properly.


## Related posts


- [Docker multi-stage builds explained](https://depot.dev/blog/docker-multi-stage-builds)
- [Buildx bake deep dive: Bake all your images with one command](https://depot.dev/blog/buildx-bake-deep-dive)
- [How to speed up your Docker builds](https://depot.dev/blog/speed-up-docker-builds)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
