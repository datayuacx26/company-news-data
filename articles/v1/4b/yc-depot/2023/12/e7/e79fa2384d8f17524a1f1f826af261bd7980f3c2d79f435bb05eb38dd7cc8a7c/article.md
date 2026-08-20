---
schema_version: "1.0.0"
document_id: "e79fa2384d8f17524a1f1f826af261bd7980f3c2d79f435bb05eb38dd7cc8a7c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/faster-docker-image-builds-in-aws-codebuild"
published_at: "2023-12-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:86691bb68bb015e84476bcef1036ff79caaa9db2eae59dc212b86c5bd118da51"
---

# Faster Docker image builds in AWS CodeBuild

In this post, we will look at how to build Docker images in CodeBuild, with both the EC2 and Lambda compute types, and how to make those builds faster by routing the Docker image build to Depot.


## Building Docker images in AWS CodeBuild


Building an image with CodeBuild can be done with a single step inside a` buildspec.yml` file at the root of our repository.


```text
version  :   0.2


phases  :
build  :
commands  :
-   docker build .
```


This configuration requires a couple things to be configured in your AWS CodeBuild project:


1. The environment for your project **must** be configured to use the managed AWS CodeBuild image and the EC2 compute type.
2. To access Docker inside your build, You must check the box for **Privileged** mode.


With your CodeBuild project configured with those two conditions, you can run the build with the configuration above, and you will see a Docker image get built.


It's functional. But, if we look at the build phase details, we see some things that could be improved.


In this basic example, the phase details reveal a lot of inefficiencies during the build:


- We spent **130 seconds** waiting in the queue for our build to run.
- We then waited for another **40 seconds** provisioning a runner for our build.
- Then, when we actually ran the Docker image build, it took **39 seconds** .


From when we started the build to when it finished, it took **209 seconds** . That's a long time to wait for a build to complete. We will come back to these provisioning inefficiencies in the Lambda compute section.


Another problem is that we can't reuse our Docker layer cache across builds in this basic example. So, if we rerun this build, the actual build of the image will take 39 seconds, even if nothing changes. The Docker layer cache is[fundamental to building Docker images fast](https://depot.dev/blog/fast-dockerfiles-theory-and-practice) .


To work around this problem, you have two options:


1. You can configure a[local cache](https://docs.aws.amazon.com/codebuild/latest/userguide/build-caching.html#caching-local) in CodeBuild that will store the Docker layer cache on the build host. This allows you to reuse that cache across builds if you use the same build host.
2. You can configure an[Amazon S3](https://docs.aws.amazon.com/codebuild/latest/userguide/build-caching.html#caching-s3) cache for storing your Docker layer cache back to S3. This allows you to reuse that cache across builds and different build hosts.
3. The third option is to use an ECR registry as your Docker layer cache. We would add a step to our` buildspec.yml` file to pull the previous image and use it as a cache source. This allows us to reuse the cache across builds and different build hosts. Below is an example of this:


```text
version  :   0.2


phases  :
pre_build  :
commands  :
-   echo Logging in to Amazon ECR...
-   aws ecr get-login-password --region <your-aws-region> | docker login --username AWS --password-stdin <your-ecr-registry>
-   docker pull <your-ecr-registry>:latest || true
build  :
commands  :
-   docker build --cache-from <your-ecr-registry>:latest .
post_build  :
commands  :
-   docker push <your-ecr-registry>:latest
```


### Problems with caching options in AWS CodeBuild


Option one, the local cache, is a good idea but doesn't go across build hosts. So even if our Docker layer cache is saved to the local cache of the build host, we likely won't get to use it across builds as we will be assigned another build host.


The second option, Amazon S3, and the third option, ECR as a layer cache, are both viable options. But they both have the same problem. They require us to transfer the layer cache from S3 or ECR to the build host. This network penalty can negate any performance gains we would get from using the layer cache.


## Faster Docker image builds with Depot


This is where Depot can come in. Depot is a remote container build service that runs Docker image builds on native Intel & Arm CPUs. A given builder has 16 CPUs, 32 GB of memory, and a 50 GB persistent SSD cache.


With Depot, you can replace` docker build` with` depot build` , and we can build the Docker image up to 40x faster with automatic caching. It's a matter of installing the[Depot CLI](https://depot.dev/docs/cli/reference) and then swapping out the build step:


```text
version  :   0.2


env  :
secrets-manager  :
DEPOT_TOKEN  :   '<your-project-token-secrets-manager-arn>'


phases  :
pre_build  :
commands  :
-   echo Installing Depot CLI...
-   curl -L https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR="/usr/local/bin" sh
build  :
commands  :
-   depot build -t test .
```


A few things to note here:


1. You must generate a[Depot project token](https://depot.dev/docs/cli/authentication#project-tokens) and store it in AWS Secrets Manager. This token will be used to authenticate your build to your Depot project.
2. We set the` DEPOT_TOKEN` environment variable by referencing the project token in secrets manager via the ARN of the secret.
3. We install the` depot` CLI in the` /usr/local/bin` , which is already in the` $PATH` of the CodeBuild environment when using the EC2 compute type.


Now, if we rerun our build, we see a much different output when we get to the build phase:


Instead of the build taking 39 seconds, it took 4 seconds. Why? Because the majority of the build was cached. So, the first build using Depot can take the initial 39 seconds. However, subsequent builds will be much faster as the existing Docker layer cache is immediately available. The net result is that the build is 10 times faster than the original.


Depot uses the native Docker layer cache without the network penalty of transferring the cache from S3 or ECR. The layer cache is persisted to a fast SSD on the remote builder.


## Build Docker images in AWS CodeBuild with Lambda compute type


As we initially saw, building a Docker image in AWS CodeBuild requires the EC2 compute type. You need root-user permissions to access Docker in the CodeBuild environment. So, you have to use the EC2 compute type and check the **Privileged** box to get access to Docker.


But, the EC2 compute type is *slow* to provision and expensive as you scale up.


What if you could build a Docker image in AWS CodeBuild using the Lambda compute type? The provisioning would be faster, and the cost would likely be cheaper if you could simultaneously make your builds faster via Docker layer caching.


With Depot, this is possible. You need to change the install directory for the CLI to` /tmp/codebuild/bin` , and then you can use the Lambda compute type. Here is an example` buildspec.yml` file:


```text
version  :   0.2


env  :
secrets-manager  :
DEPOT_TOKEN  :   '<your-project-token-secrets-manager-arn>'


phases  :
pre_build  :
commands  :
-   echo Installing Depot CLI...
-   curl -L https://depot.dev/install-cli.sh | DEPOT_INSTALL_DIR="/tmp/codebuild/bin" sh
build  :
commands  :
-   depot build -t test .
```


If we run a build with the Lambda compute type and this buildspec, we see the following in our phase details:


When we dig into the phase details in AWS CodeBuild, the results differ from what we saw with the EC2 compute type.


EC2 with` docker` Lambda with` depot`


time waiting in the queue 130s < 1s


provisioning time 40s 2s


build time 39s 2s


total time


209s


8s


**The net result of using Depot with AWS CodeBuild and the Lambda compute type is that you can build Docker images up to 26x faster than the EC2 compute type with Docker.**


### What about pushing Docker images to ECR with Lambda in AWS CodeBuild?


Using the Lambda compute type, we can build Docker images faster by routing the Docker image build to remote build compute in Depot. In fact, Depot is a literal drop-in replacement for` docker build` and` docker buildx build` . So, you can use the same flags, like` --push` , to push your Docker image to ECR from a Depot builder.


But with the Lambda compute type, we don't have access to` docker login` to log into our ECR registry. So, how do we push our Docker image to ECR?


We write the Docker auth configuration file (i.e.,` $HOME/.docker/config.json` ) ourselves. Here is an example` buildspec.yml` file that does this:


```text
version  :   0.2


env  :
secrets-manager  :
DEPOT_TOKEN  :   '<your-project-token-secrets-manager-arn>'


phases  :
pre_build  :
commands  :
-   ecr_stdin=$(aws ecr get-login-password --region eu-central-1)
-   registry_auth=$(printf "AWS:$ecr_stdin" | openssl base64 -A)
-   mkdir $HOME/.docker
-   echo "{\"auths\":{\"<your-registry-url>\":{\"auth\":\"$registry_auth\"}}}" > $HOME/.docker/config.json
build  :
commands  :
-   depot build -t <your-ecr-registry-url>:latest --push .
```


The magic happens in the` pre_build` phase. We use the AWS CLI to get the ECR login password, and then we use` openssl` to base64 encode the username and password. We then write the Docker auth configuration file to` $HOME/.docker/config.json` with the base64 encoded username and password.


This is effectively recreating what` docker login` is doing for us behind the scenes. We then use the` depot` CLI to build and push our Docker image to ECR. The Docker auth configuration file is automatically picked up and shipped to the builder as part of the build context.


**Note:** There are still limitations to using the Lambda compute type in AWS CodeBuild. For example, you can't use the` --load` flag to load the image into the local Docker daemon. Because there is no local Docker daemon unless you use the EC2 compute type.


## Conclusion


In this post, we looked at how to build Docker images inside AWS CodeBuild with both the EC2 and Lambda compute types.


We analyzed some of the performance tradeoffs of EC2 and Lambda, the options for Docker layer caching, and how to leverage Depot in both configurations.


We saw that with Depot, we can swap` docker build` for` depot build` and get automatic cache sharing across builds. We also noticed that because Depot isn't reliant on the Docker daemon, we can build Docker images in CodeBuild with the Lambda compute type.


If you want to try out Depot in your AWS CodeBuild environments, you can[sign up for a free trial](https://depot.dev/start) and get started today.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
