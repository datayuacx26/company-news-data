---
schema_version: "1.0.0"
document_id: "4c158a5e3c5108d95a1365954ab85909411c058b7d556739a8c5e891dab6198f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-matrix-strategy"
published_at: "2024-04-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:98baf18cde17e571c83da7532606a2d88eb6596e21d850cac5af7fb35bd289e1"
---

# How to leverage GitHub Actions matrix strategy

GitHub Actions for continous integration has one of the most powerful features to help you get maximum concurrent builds for your jobs that you want to neatly execute in parallel against different configurations. It's called the matrix strategy.


In this post, we'll explore the GitHub Actions matrix strategy, how to leverage it in your workflow, how to use different matrix configurations, and some best practices for handling errors and controlling concurrency.


## What is a GitHub Actions matrix strategy?


The matrix strategy in GitHub Actions allows you to define a matrix of values that will run the same job multiple times with different configurations. This is useful when you want to run the same job with different versions of a language, different operating systems, or different inputs based on what your job is doing.


### Defining a matrix in your GitHub Actions workflow


The key,` strategy.matrix` is defined below the job ID you're currently defining. Conceptually, the matrix strategy is a dictionary of keys and values that you want to run your job against. The keys are the names of the variables you want to use in your job, and the values are the different configurations you want to run your job against.


```text
jobs  :
example-job  :
strategy  :
matrix  :
version  : [  1  ,   2  ,   3  ]
```


In the example above, we define a matrix strategy with a key` version` and three matrix values` \[1, 2, 3\]` . This will create matrix jobs, one for each value of version.


## How to use GitHub Actions matrix strategy


One great example of leveraging a matrix strategy is when you want to build multiple Docker images in a monorepo but are not currently using a bakefile to build them all in parallel. You could define a matrix strategy to build each Docker image in parallel without the switch to bake.


### Initial build matrix for parallel Docker image builds


First, you must define a job matrix for the Dockerfiles you want to build. In this example, we're building three different Dockerfiles in parallel, so we create a build matrix like the workflow below:


```text
name  :   GitHub Actions Matrix Example
on  :
workflow_dispatch  : {}


jobs  :
build  :
name  :   Build Dockerfiles
runs-on  :   ubuntu-latest
strategy  :
matrix  :
dockerfile  : [  'Dockerfile'  ,   'Dockerfile.app'  ,   'Dockerfile.db'  ,   'Dockerfile.cron'  ]
include  :
-   dockerfile  :   Dockerfile
context  :   ./default-folder
-   dockerfile  :   Dockerfile.app
context  :   ./app-folder
-   dockerfile  :   Dockerfile.db
context  :   ./db-folder
-   dockerfile  :   Dockerfile.cron
context  :   ./cron-folder
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v3


-   name  :   Set up QEMU
uses  :   docker/setup-qemu-action@v3
-   name  :   Set up Docker Buildx
uses  :   docker/setup-buildx-action@v3


-   name  :   Build image
uses  :   docker/build-push-action@v5
with  :
context  :   ${{ matrix.context }}
dockerfile  :   ${{ matrix.dockerfile }}
platforms  :   linux/amd64,linux/arm64
```


The example above lays out a build matrix for building a collection of three Dockerfiles in a monorepo project. The matrix strategy contains a` dockerfile` key with a value for each Docker image to be built.


---


#### Expanding the matrix configuration with` include`


The` include` key allows you to define additional values for the matrix strategy. In this example, we're defining the` context` key to specify the folder where the Dockerfile is located and, thus, the build context. This allows you to build each Dockerfile in parallel with a different context.


This include key is a great way to specify additional information onto the job matrix being executed. For example, we want the build context to be` ./default-folder` when building the default` Dockerfile` .


---


If you were to execute a similar GitHub Actions workflow as the one above with a matrix strategy for building multiple Docker images in parallel, you would see the output as shown in the image above.


In this example, each Docker image build inside of GitHub Actions took about 1 minute and 35 seconds on average. It's not terrible, as all those images were built in parallel.


### Bonus: Replace Docker for Depot to build images faster


We can make these concurrent Docker image builds with the matrix strategy even faster by swapping out the Docker GitHub actions for our own Depot actions. The` depot/build-push-action` accepts all the same parameters but routes the build to our remote BuildKit builders with 16 CPUs, 32 GB of memory, and up to 500 GB of persistent layer cache on real NVMe drives.


Here is the same workflow as above, but with the Docker GitHub actions swapped out for the Depot GitHub actions:


```text
name  :   GitHub Actions Matrix Example
on  :
workflow_dispatch  : {}


permissions  :
id-token  :   write
contents  :   read


jobs  :
build  :
name  :   Build Dockerfiles
runs-on  :   ubuntu-latest
strategy  :
matrix  :
include  :
-   dockerfile  :   Dockerfile
context  :   ./default-folder
project  :   projectID1
-   dockerfile  :   Dockerfile.app
context  :   ./app-folder
project  :   projectID2
-   dockerfile  :   Dockerfile.db
context  :   ./db-folder
project  :   projectID3
-   dockerfile  :   Dockerfile.cron
context  :   ./cron-folder
project  :   projectID4
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v3


-   uses  :   depot/setup-action@v1


-   name  :   Build image
uses  :   depot/build-push-action@v1
with  :
context  :   ${{ matrix.context }}
dockerfile  :   ${{ matrix.dockerfile }}
platforms  :   linux/amd64,linux/arm64
project  :   ${{ matrix.project }}
```


A few key things are happening in the workflow above that are worth touching on:


1. The` permission` block exchanges an OIDC token in GitHub Actions with Depot to authenticate your Docker image build with your Depot organization. You can configure these trust relationships for each project[in the Depot UI.](https://depot.dev/docs/cli/authentication#add-a-trust-relationship-for-github-actions)
2. The` include` block now includes a Depot project ID for each Dockerfile you are building. This is so that each Docker image build gets its own dedicated BuildKit builder to build the image. But you could also have all builds going to one project.
3. The` depot/setup-action` is used to authenticate with Depot and set up the necessary environment variables for the` depot/build-push-action` to work.
4. The` depot/build-push-action` is used to build the Docker image with the specific Depot project specified by ID.


Depot runs native Intel and Arm builders, so we can build multi-platform images in parallel without any emulation. The combination of native builders and the matrix strategy in GitHub Actions can make this simple example significantly faster.


## Additional GitHub Actions matrix strategy configurations


We've already seen how to define a basic matrix configuration on a single dimension. We've also seen how you can expand matrix jobs with the` include` key to add additional context to a given job.


But there is a lot more you can do with the matrix strategy in GitHub Actions. You can define a multi-dimension matrix, exclude certain matrix configurations, and cancel in-flight jobs on error.


### Using a multi-dimension matrix


A multi-dimension matrix has multiple keys and values. It allows you to run a job against multiple configurations of different variables.


```text
jobs  :
test-matrix  :
strategy  :
matrix  :
configuration  : [  debug  ,   release  ]
arch  : [  x86  ,   arm  ]
runs-on  :   ubuntu-latest
steps  :   ...
```


In this example, we define a matrix strategy with two keys,` configuration` and` arch` , and two values for each key. This will create matrix jobs for each combination of configuration and architecture.


### How to exclude different matrix configurations


We've already seen how you can append information onto a given matrix configuration, but how do you exclude one? You can use the` exclude` key to exclude specific matrix configurations from running.


```text
jobs  :
test-matrix  :
strategy  :
matrix  :
configuration  : [  debug  ,   release  ]
arch  : [  x86  ,   arm  ]
exclude  :
-   configuration  :   debug
arch  :   arm
runs-on  :   ubuntu-latest
steps  :   ...
```


In this example, we have the same multi-dimension matrix as before, but we've added an` exclude` key to exclude the configuration` debug` and architecture` arm` . This will create matrix jobs for each combination of configuration and architecture except for` debug` and` arm` .


### Handling errors when using a matrix strategy


When you're running a matrix job, you might want to cancel all in-flight jobs if one of the jobs fails. You can use the` fail-fast` key to cancel all in-flight jobs if one of the jobs fails.


```text
jobs  :
test-matrix  :
strategy  :
matrix  :
fail-fast  :   true
configuration  : [  debug  ,   release  ]
arch  : [  x86  ,   arm  ]
exclude  :
-   configuration  :   debug
arch  :   arm
runs-on  :   ubuntu-latest
steps  :   ...
```


By default, in a GitHub Actions job matrix, any failure will cancel any in-flight and queued jobs. In short,` fail-fast` defaults to true if not specified. But you can set it to` false` if you want to run all jobs in the matrix, even if one of them fails.


### Controlling concurrency with a matrix strategy


When using a matrix strategy with GitHub Actions, the default is to fan out to maximum concurrency based on runner availability. But, left unchecked, you could fan out in such a way that it is detrimental to what you're doing.


You can control the concurrency of your matrix jobs by using the` max-parallel` key. This key allows you to specify the maximum number of jobs that can run in parallel.


```text
jobs  :
test-matrix  :
strategy  :
matrix  :
fail-fast  :   true
max-parallel  :   5
configuration  : [  debug  ,   release  ]
arch  : [  x86  ,   arm  ]
exclude  :
-   configuration  :   debug
arch  :   arm
runs-on  :   ubuntu-latest
steps  :   ...
```


This can be slower than running all jobs at once, but it can be helpful to control the number of jobs in flight at once.


## Conclusion


The matrix strategy in GitHub Actions is a powerful tool for creating reusable workflows that fan out jobs based on different inputs and configurations. You can use a job matrix to fan out similar steps of your build pipelines into concurrent runs that can be processed in parallel.


In this post, we dove into the basics of the matrix strategy and how to define a matrix in your GitHub Actions workflow using Docker image builds with and without Depot as an example. We discussed how to define a multi-dimensional matrix to execute jobs based on multiple keys and values. You also got an idea of how you can exclude jobs in a matrix configuration, handle errors, and control concurrency.


As we've seen, integrating Depot accelerated container image builds with a GitHub Actions matrix can streamline your CI/CD pipelines, making your builds orders of magnitude faster. If you're looking to improve your build performance significantly, consider checking out Depot.


To get started, we offer a[7-day free trial](https://depot.dev/start) for both of our products, accelerated Docker image builds that are up to 40x faster, and our recently launched managed GitHub Actions runners that are up to 10x faster than GitHub hosted runners.


## Related posts


- [Depot managed GitHub Actions Runners](https://depot.dev/products/github-actions)
- [Running GitHub Actions jobs in a container built with Depot](https://depot.dev/blog/github-actions-jobs-in-a-container)
- [How to build multi-platform Docker images in GitHub Actions](https://depot.dev/blog/multi-platform-docker-images-in-github-actions)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
