---
schema_version: "1.0.0"
document_id: "ca90f31c79061e128c7f73d225ac6c65c0496186a5d4e4e1c9a7eeb017b2dd4e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/dockerfile-lint-on-build"
published_at: "2023-05-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:cf58eabccf04c5a665649497da97eae433ab2d4feb20427cd1c9f15e99317d87"
---

# Lint a Dockerfile on every build

Now available in[depot v.2.17.0](https://github.com/depot/cli/releases/tag/v2.17.0) is the ability to lint a` Dockerfile` on any Docker image build you do with Depot. This means you can now run a Dockerfile lint on every build with Depot! No more configuring a Dockerfile linter outside of your actual image build.


## How it works


The new Dockerfile linter uses the popular open-source project,[hadolint](https://github.com/hadolint/hadolint) , which helps you build best practice Docker images. It's a fast, lightweight, and easy-to-use linter already used in the Docker community. We're also very excited to be sponsoring the project.


### Dockerfile linting with Depot


To use the new Dockerfile linter, you can add the` --lint` flag to your` depot build` or` depot bake` commands:


```text
depot   build   --lint   .
[+] Building 3.9s (  16/16  ) FINISHED
=  > [depot] launching arm64 builder                                                                                                                                                          0.5s
=  > [depot] connecting to arm64 builder                                                                                                                                                      0.4s
=  > [lint]                                                                                                                                                                                   1.3s
=  > =  >   INFO   Dockerfile:7   DL3059:   Multiple   consecutive   `  RUN  `   instructions.   Consider   consolidation.                                                                                              1.3s
=  > [internal] load build definition from Dockerfile                                                                                                                                         0.3s
=  > =  >   transferring   dockerfile:   436B                                                                                                                                                           0.3s
=  > [internal] load .dockerignore                                                                                                                                                            0.3s
=  > =  >   transferring   context:   116B                                                                                                                                                              0.3s
=  > [internal] load metadata   for   docker.io/library/node:16-alpine                                                                                                                            0.4s
=  > [build   1/6]   FROM   docker.io/library/node:16-alpine@sha256:f1657204d3463bce763cefa5b25e48c28af6fe0cdb0f68b354f0f8225ef61be7                                                                  0.0s
=  > =  >   resolve   docker.io/library/node:16-alpine@sha256:f1657204d3463bce763cefa5b25e48c28af6fe0cdb0f68b354f0f8225ef61be7                                                                        0.0s
=  > [internal] load build context                                                                                                                                                            0.2s
=  > =  >   transferring   context:   421B                                                                                                                                                              0.2s
=  >   CACHED   [build   2/6]   WORKDIR   /app                                                                                                                                                            0.0s
=  >   CACHED   [build   3/6]   COPY   package.json   yarn.lock   tsconfig.json   ./                                                                                                                            0.0s
=  >   CACHED   [build   4/6]   COPY   src/   ./src/                                                                                                                                                        0.0s
=  >   CACHED   [build   5/6]   RUN   yarn   install   --immutable                                                                                                                                            0.0s
=  >   CACHED   [build   6/6]   RUN   yarn   build                                                                                                                                                          0.0s
=  >   CACHED   [stage-1   3/5]   COPY   --from=build   /app/node_modules   /app/node_modules                                                                                                                 0.0s
=  >   CACHED   [stage-1   4/5]   COPY   --from=build   /app/dist   /app/dist                                                                                                                                 0.0s
=  >   CACHED   [stage-1   5/5]   COPY   gifs-to-upload/   dist/gifs-to-upload/                                                                                                                             0.0s
WARN[0001]   No   output   specified   with   depot   driver.   Build   result   will   only   remain   in   the   build   cache.   To   push   result   image   into   registry   use   --push   or   to   load   image   into   docker   use   --load


1   linter   issue   found:
INFO   Dockerfile:7   Multiple   consecutive   `  RUN  `   instructions.   Consider   consolidation.
More   info:   https://github.com/hadolint/hadolint/wiki/DL3059


--------------------
5   |       COPY   src/   ./src/
6   |       RUN   yarn   install   --immutable
7   |   >>>   RUN   yarn   build
8   |
9   |       FROM   node:16-alpine
--------------------
```


The linter runs before the build starts. It outputs any lint errors, warnings, or info messages at the end of the Docker image build. For example, we see here it's complaining about my Docker image build having back-to-back run instructions.


The Dockerfile linting doesn't fail the build by default. But, you can enable this behavior via the` --lint-fail-on` flag:


```text
depot   build   --lint   --lint-fail-on   info   .
[+] Building 2.6s (  3/3  ) FINISHED
=  > [depot] launching arm64 builder                                                                                                                                                          0.6s


1   linter   issue   found:
More   info:   https://github.com/hadolint/hadolint/wiki/DL3059


--------------------
5   |       COPY   src/   ./src/
6   |       RUN   yarn   install   --immutable
7   |   >>>   RUN   yarn   build
8   |
9   |       FROM   node:16-alpine
--------------------
```


Here we can see that the build failed because we enabled the` --lint-fail-on` flag with the` info` level. This value can be` info` ,` warning` , or` error` depending on your goal.


### Linting a Dockerfile in CI


If you're not using GitHub Action for continuous integration, you can use the new flags as shown above.


For GitHub Actions, we've updated our[depot/build-push-action](https://github.com/depot/build-push-action) and[depot/bake-action](https://github.com/depot/bake-action) actions to support these new flags. You can enable the Dockerfile linter by setting the` lint` input to` true` :


Here is an example of linting a Dockerfile using our` build-push-action` . It does a` git clone` , sets up our CLI, and builds the docker image with Dockerfile linting enabled:


```text
jobs  :
build  :
name  :   Build
runs-on  :   ubuntu-20.04
steps  :
-   name  :   Checkout repo
uses  :   actions/checkout@v3


-   uses  :   depot/setup-action@v1


-   name  :   Bake Docker images
uses  :   depot/build-push-action@v1
with  :
lint  :   true
lint-fail-on  :   error
```


Dockerfile lint functionality works with our` bake-action` as well. This allows you to build best practice docker images for all images in a build definition file. See our post on[building many images via bake](https://depot.dev/blog/buildx-bake-with-depot) for more details.


```text
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
lint  :   true
lint-fail-on  :   error
```


## Why we built this


Linting a Dockerfile is a great way to catch errors during static analysis. It improves the quality of your images and helps you build best practice Docker images. It's one of those best practices that improve code quality and catches mistakes during a code review. But it's an extra hop away today. You have to run` hadolint dockerfile` outside your actual build process.


With Depot, we are already providing you with a faster experience for building your Docker container, so adding the ability to lint your Dockerfile on every build was a natural next step for us. We want to make it as easy as possible for you to build high-quality Docker images as fast as possible, and this is an excellent step in that direction.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
