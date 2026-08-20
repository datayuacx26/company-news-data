---
schema_version: "1.0.0"
document_id: "a412de8004d7c57fb32163572bc4119927606601d5239df8e721c6c4978134c6"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/build-docker-images-in-gitlab-ci"
published_at: "2023-11-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:35b1ef769ca3465de96c0d0c351a3481234ea621e526a7af7c837ff3ca6bfea6"
---

# Building Docker Images in GitLab CI with Depot

Today, there are three different options for building Docker images in GitLab CI, and each of them comes with tradeoffs between security, performance, and complexity. Depot fixes many of these tradeoffs and can be used to build images in GitLab CI quickly and securely.


## Existing options for building Docker images in GitLab CI all have downsides


The three existing options for building images in GitLab CI/CD jobs unfortunately all have limitations. Let's look at them in detail.


1. You can use the shell executor on your own GitLab runners.


This requires configuring Docker Engine and granting the gitlab-runner user full root permissions to invoke Docker commands.


Downsides: It requires you to manually configure your own runners and grant them root permissions. Running your own runners is additional work, and root permissions might be a security risk.


1. You can use Docker in Docker (dind) with registered runners.


This uses the dind image provided by Docker, so it contains all the Docker tools. To build new containers with dind, you must run the runner container in privileged mode.


Downsides: This requires that each build job get its own instance of Docker Engine, so jobs are slower because there is no layer caching. In addition, privileged mode is still required, which can be a security risk.


1. You can bind to the Docker socket by bind mounting` /var/run/docker.sock` into the container where your build is running.


Downsides: This requires you to bind mount the Docker socket into your container, which exposes the underlying host and any other processes on that host to privilege escalation. Did someone say “security risk”?


While it's possible to work around the limitations, it's additional work and complexity that, in our opinion, isn't reasonable for most teams to take on.


So we built a better alternative.


## Alternative: build Docker images in GitLab CI with Depot


With Depot, you can build your Docker images from GitLab CI/CD jobs without needing to configure shell executor runners, work around slow builds with dind, or bind mount the Docker socket. Instead, the depot CLI builds the container using Depot's remote builders, and can optionally push the resulting image to a registry.


The Depot CLI is a drop-in replacement for the` docker build` command. You can install the Depot CLI via a` before_script` :


```text
before_script  :
-   curl https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR=/usr/local/bin sh
```


After the CLI is installed, you can build your image with the` depot build` command. The syntax is the same as the corresponding` docker` command:


```text
script  :
-   depot build -t org/image:tag .
```


Most likely, you will want to push your image to a registry with the` depot build --push` flag. Depot uses the local Docker registry credentials when pushing, so you will need to configure those credentials using one of two options: using the Docker CLI or creating the configuration manually.


## Example GitLab CI config for building a Docker image with Depot


Here's how a simple GitLab CI config looks like for building a Docker image with Depot:


```text
image  :   docker:20.10.16
services  :
-   docker:20.10.16-dind
variables  :
DOCKER_HOST  :   tcp://docker:2376
DOCKER_TLS_CERTDIR  :   '/certs'


build-job  :
before_script  :
-   apk add --no-cache curl   # install curl as it is not available in the docker:20.10.16 image
-   curl https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR=/usr/local/bin sh
script  :
-   docker login registry.gitlab.com --username your-username --password $REGISTRY_TOKEN
-   depot build -t registry.gitlab.com/your-username/myimage:$CI_COMMIT_SHA . --push
variables  :
DEPOT_TOKEN  :   $DEPOT_TOKEN
```


With this workflow, you can build native multi-platform images directly from GitLab CI and push them to a private registry. The depot CLI is installed via curl and the dind service is configured with TLS enabled. This allows you to run` docker login` to authenticate to a private registry, but leave the build part to Depot.


The Docker-in-Docker service is only used for authentication and not for the actual build.


With this approach:


- The build happens on a Docker host managed by Depot. You don't have to enable privileged mode on your GitLab CI runners or otherwise expose your underlying hosts — which means fewer security risks.
- All builds use a persistent Docker layer cache on fast SSDs.
- As an additional benefit, you can build multi-platform images ([including ARM](https://depot.dev/blog/docker-arm) ) directly from GitLab CI without the need to actually host runners for multiple platforms.


Depot is a paid service (see[pricing](https://depot.dev/pricing) ), but the first 60 build minutes and 50 GB of cache per month are free, and you only pay for what you use after that.


To give Depot a try,[sign up](https://depot.dev/sign-up) and get your` $DEPOT_TOKEN` to add to the config above.


### Another registry auth option: ~/.docker/config.json


If you already have a Docker` config.json` file with your registry credentials, you can skip the` docker login` step and provide the config contents directly via a` DOCKER_AUTH_CONFIG` environment variable. Your workflow can save the contents of that variable to` $HOME/.docker/config.json` , and Depot will read from that file when pushing. This removes the need for the Docker CLI and dind service at all in your image build:


```text
build-image  :
before_script  :
-   curl https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR=/usr/local/bin sh
-   mkdir -p $HOME/.docker
-   echo $DOCKER_AUTH_CONFIG > $HOME/.docker/config.json
script  :
-   depot build -t registry.gitlab.com/repo/image:tag . --push
variables  :
DEPOT_TOKEN  :   $DEPOT_TOKEN
```


Before the` depot build` runs, the contents of the` DOCKER_AUTH_CONFIG` environment variable are saved as` $HOME/.docker/config.json` .


You may be able to generate a` config.json` file locally by running` docker login` and then copying the contents of the` $HOME/.docker/config.json` file and saving it as` DOCKER_AUTH_CONFIG` if you have static authentication credentials for your registry.


## Conclusion


There are three existing options for building Docker images in GitLab CI, which, as we showed, come with security, complexity, and performance tradeoffs. These limitations are not impossible to work around, but they are inconvenient and cause a lot of friction. This friction can manifest as slow builds, security concerns, or having to maintain complex build infrastructure.


But this is unnecessary. Instead, you can use Depot to build Docker images in GitLab CI quickly and securely, using Depot's remote builders, without needing to configure shell executor runners, work around slow builds with dind, or bind mount the Docker socket, and all without having to grant root access to your CI runners.


Check out our[GitLab CI docs](https://depot.dev/docs/container-builds/integrations/gitlab-ci) , or get started for free below.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
