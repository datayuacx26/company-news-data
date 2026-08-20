---
schema_version: "1.0.0"
document_id: "963a788bf08c6c0578ee773094346d719aa65f30fa133c78abd3f20918cade17"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-jobs-in-a-container"
published_at: "2024-04-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:a99557b46a3fd822bc6e243dee157609a49383a37193364f17ba6a0cec91a491"
---

# Running GitHub Actions jobs in a container built with Depot

We recently released our[Depot-managed GitHub Actions Runners](https://depot.dev/blog/depot-github-actions-runners) , and there have been many exciting use cases already! One that a community member found is combining our accelerated Docker image builds, ephemeral registry, and our runners to build and run GitHub Actions jobs in a container.


In this post, we'll walk through what it looks like to run a given GitHub Actions job in a container and then how you can build that container image with Depot and run the job in that container in the same workflow. Let's dive in!


## Running GitHub Actions jobs in a container


The` container` key in the job definition allows you to specify a container image for a given GitHub Actions job to run in. Containers are helpful if you need to run your job in a specific environment or with particular tools or dependencies installed.


Here's an example of a GitHub Actions workflow file with a job that runs in a container:


```text
name  :   Run job in a container
on  :
push  :
branches  :   main
jobs  :
job-in-container  :
runs-on  :   ubuntu-latest
container  :
image  :   node:18


steps  :
-   name  :   echo node version
run  :   node --version
```


This example uses the public` node:18` Docker image to run the job in. But you can also run jobs in container images you build and host in your own private registry by specifying registry credentials:


```text
name  :   Run job in a private container image


jobs  :
job-in-container  :
runs-on  :   ubuntu-latest
container  :
image  :   ghcr.io/myorg/myimage
credentials  :
username  :   ${{ github.actor }}
password  :   ${{ secrets.GITHUB_TOKEN }}
```


Here, you can specify the` credentials` key to provide the username and password for your private registry. The credentials passed in are the same authentication credentials you would use when using` docker login` . This example demonstrates authenticating to the GitHub Container Registry (GHCR) using the GitHub token made available to the job workflow.


### Additional container configuration for a job


You can also specify additional configuration for the job's Docker container, such as mounting volumes, exposing network ports, and setting environment variables. Here's an example of a job that mounts a volume and sets an environment variable:


```text
name  :   Run job in a private container image


jobs  :
job-in-container  :
runs-on  :   ubuntu-latest
container  :
image  :   ghcr.io/myorg/myimage
credentials  :
username  :   ${{ github.actor }}
password  :   ${{ secrets.GITHUB_TOKEN }}
volumes  :
-   /path/to/host/dir:/path/to/container/dir
env  :
MY_ENV_VAR  :   my-value
```


## GitHub Actions job container images with Depot


Now that you know how to run a job in a container, let's discuss how to build your own container images with Depot and run your job in that container using the same GitHub Actions workflow file.


For this example, we will build the container image in one job and then run a second job in that container by leveraging the` needs` key.


```text
name  :   Build container job image and run job with image


jobs  :
build-job-container  :
runs-on  :   ubuntu-latest
outputs  :
buildId  :   ${{ steps.build.outputs.build-id}}
token  :   ${{ steps.pull-token.outputs.token }}
steps  :
-   name  :   Checkout code
uses  :   actions/checkout@v2
-   uses  :   depot/setup-action@v1
-   name  :   build and save the image
uses  :   depot/build-push-action@v1
id  :   build
with  :
save  :   true
-   name  :   export pull token
id  :   pull-token
run  :   echo "token=$(depot pull-token)" >> "$GITHUB_OUTPUT"


job-in-container  :
needs  :   build-job-container
runs-on  :   ubuntu-latest
container  :
image  :   registry.depot.dev/<your-project-id>:${{ needs.build-job-container.outputs.buildId }}
credentials  :
username  :   x-token
password  :   ${{ needs.build-job-container.outputs.token }}
steps  :
-   name  :   run in container
run  :   echo "running in container"
```


**Note: This example workflow assumes you have run[depot init](https://depot.dev/docs/cli/reference/container-builds#depot-init) to generate a` depot.json` file and you are building a Dockerfile at the root of your repository.**


Note: This example workflow assumes you have run depot init , have a depot.json file, and are building a Dockerfile at the root of your repository.


### build-job-container job explanation


The` build-job-container` job builds a container image with Depot and saves it to the Depot ephemeral registry. The image is built via the[depot/build-push-action](https://github.com/depot/build-push-action) Docker container action that we provide. The image is saved to the ephemeral registry by specifying the` save` input as` true` .


The final step in the job is to export a pull token to pull the image from the Depot registry in the` job-in-container` job. This is done by running the` depot pull-token` command and saving it to the job's output.


The` outputs` key defines the build ID and pull token produced by the` job-in-container` job so that you can reference them in the` job-in-container` job.


### job-in-container job explanation


The` job-in-container` job runs in the container image built in the` build-job-container` job. The container image is specified using the` image` key in the` container` key of the job definition. The image is pulled from the Depot ephemeral registry using your Depot project ID and` buildId` to form the image url:


```text
```


The final piece to pull the container image for the job is to provide the pull token generated in the` build-job-container` job. The authentication credentials use` x-token` for the` username` and the` password` is the pull token:


```text
credentials  :
username  :   x-token
password  :   ${{ needs.build-job-container.outputs.token }}
```


After that configuration, the GitHub Actions workflow will run the build job first to build the container image. Using Depot to build the image quickly and efficiently and saving the built image in a temporary registry. The subsequent job can then authenticate to the registry and pull the image to run the job in the container.


## Bonus: Using Depot GitHub Actions runners for even faster builds


The example above uses Depot to build the container image up to 40x faster by persisting your layer cache automatically across builds, using compute tailored to Docker images, and building images on native CPUs for both Intel & Arm. When you combine that with our ephemeral registry, you can build the container for your job and run it all within the same GitHub Actions workflow.


But, you can take this even more performant and drastically reduce the network transfer by combining this with[Depot-managed GitHub Actions Runners](https://depot.dev/products/github-actions) .


To do that, you first need to connect your GitHub organization to Depot, and then you can use Depot runners for your GitHub Actions jobs. You can connect your GitHub org from the GitHub Actions tab in your organization. If you don't have an account, you can[sign up for Depot](https://depot.dev/start) and connect your GitHub organization.


Once you have connected your GitHub organization to Depot, you can use any of our GitHub Action runner labels to route your jobs to Depot. Here's an example of the workflow above that uses the` depot-ubuntu-latest` runner label:


```text
name  :   Build container job image and run job with image


jobs  :
build-job-container  :
runs-on  :   depot-ubuntu-latest
outputs  :
buildId  :   ${{ steps.build.outputs.build-id}}
token  :   ${{ steps.pull-token.outputs.token }}
steps  :
-   name  :   Checkout code
uses  :   actions/checkout@v2
-   uses  :   depot/setup-action@v1
-   name  :   build and save the image
uses  :   depot/build-push-action@v1
id  :   build
with  :
save  :   true
-   name  :   export pull token
id  :   pull-token
run  :   echo "token=$(depot pull-token)" >> "$GITHUB_OUTPUT"


job-in-container  :
needs  :   build-job-container
runs-on  :   depot-ubuntu-latest
container  :
credentials  :
username  :   x-token
password  :   ${{ needs.build-job-container.outputs.token }}
steps  :
-   name  :   run in container
run  :   echo "running in container"
```


### Advantages to using Depot GitHub Actions runners


Our managed runners have 30% faster compute, 10x faster caching, 1 GB/s networks, and are half the cost of GitHub hosted runners. Combining that with our accelerated Docker image builds and ephemeral registry allows you to build and run your jobs faster and more efficiently than ever before.


For this specific use case, building the container image with Depot and running the job in the container with Depot runners will save you time and money by reducing the network transfer and speeding up the build and run times.


Our accelerated image builders live in the same network as our runners. So when you build an image with Depot and run a job inside it with a Depot runner, you save all the network transfer time and cost of pulling the image from a remote registry.


## Conclusion


This blog post explored the exciting possibilities of running GitHub Actions jobs in containers and leveraging Depot to optimize your workflows. By running jobs in containers, you gain flexibility in managing dependencies and environments efficiently.


We learned how to specify container images for GitHub Actions jobs, customize container configurations, and even build and run jobs in custom container images using Depot. This approach streamlines workflows and enhances performance and cost-effectiveness.


Furthermore, we discussed the advantages of integrating Depot-managed GitHub Actions runners into your workflows. Depot runners improve build times with faster compute, accelerated caching, and efficient network speeds.


By combining Depot's accelerated image building, ephemeral registry, and managed runners, you can supercharge your GitHub Actions workflows, making them faster, more efficient, and ultimately more cost-effective.


Ready to optimize your GitHub Actions workflows with Depot? Start your 7-day free trial today with no credit card required 👉[depot.dev/start](https://depot.dev/start)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
