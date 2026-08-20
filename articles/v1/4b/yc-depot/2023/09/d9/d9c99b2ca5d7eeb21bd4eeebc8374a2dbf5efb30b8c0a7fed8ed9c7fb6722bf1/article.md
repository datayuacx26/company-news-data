---
schema_version: "1.0.0"
document_id: "d9c99b2ca5d7eeb21bd4eeebc8374a2dbf5efb30b8c0a7fed8ed9c7fb6722bf1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/dockerfile-linting-issues"
published_at: "2023-09-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:f557336a60675f10f6a84d1bcfe7378a7c9ec93d1b7cd29cf59a502ed01a409f"
---

# Top 10 common Dockerfile linting issues

We recently announced the ability to use a Docker linter on build in our recent[lint & build blog post](https://depot.dev/blog/dockerfile-lint-on-build) via` hadolint` and Semgrep.


Running a Dockerfile linter on a Docker image we want to build can allow us to follow some of the best practices around writing efficient Docker images. Efficient could mean faster builds or smaller image sizes.


This post covers the ten most common Docker linting issues we've seen flowing through Depot to date. We expect these to change over time, but hopefully they can give everyone a good starting point for improving their Dockerfiles. We'll cover each issue, why it's a problem, and how to fix it.


## How to lint a Dockerfile


With Depot, we make use of two Docker linters,[hadolint](https://github.com/hadolint/hadolint) and a set of Dockerfile linter rules that[Semgrep has written](https://semgrep.dev/p/dockerfile) to make a bit smarter.


To lint a Dockerfile on-demand with Depot, we can pass the[--lint flag](https://depot.dev/docs/cli/reference/container-builds#depot-build) during a build. The Docker linter runs before the build.


Of course, we can also run` hadolint` ourselves locally without Depot with our own specific rules and config file. Or even use the[hadolint Dockerfile linter UI](https://hadolint.github.io/hadolint/) . To run hadolint locally you can either install it via brew or use the Docker image and pipe your Dockerfile into it:


```text
hadolint   Dockerfile
# or use the Docker image
docker   run   --rm   -i   ghcr.io/hadolint/hadolint   <   Dockerfile
```


## 1. Multiple consecutive` RUN` instructions


Also known as lint error` DL3059` from hadolint.


This is the most common issue we see with Dockerfiles flowing through Depot. It's present in **nearly 30% of all Dockerfiles** we've seen. The problem is that multiple` RUN` instructions are in a row that could be condensed. For example:


```text
RUN   download_a_really_big_file
RUN   remove_the_really_big_file
```


It's helpful to know how[Docker layer caching works](https://depot.dev/blog/fast-dockerfiles-theory-and-practice) to understand why this might be problematic. In short, each new` RUN` statement in a Dockerfile results in a new layer in the final image.


In this example, we create a new layer when we download the big file and another layer when we remove it. Both layers will be present in the final image. So, the final image will contain the big file in the first layer, making the final image larger than it needs to be.


However,` DL3059` can also be problematic if we use two different` RUN` statements to install packages. For example:


```text
RUN   fetch_package_registry_list
RUN   install_some_package
```


The first` RUN` statement will fetch the package registry list in this example. The second` RUN` statement will install the package. But if the package registry list changes between the first and second` RUN` statements, then the package registry list will be out of date when we install the package.


### Solution to` DL3059`


When working with large files that we add and remove during a` docker build` , combining those operations into one atomic` RUN` statement is helpful.


```text
RUN   download_a_really_big_file && \
remove_the_really_big_file
```


This reduces the final image size by removing the intermediate layer that contains the big file as we download and remove it in the same` RUN` statement. Note that this can have cache implications if you combine` RUN` statements with things that can be cached with things that frequently invalidate the cache. In those situations, you likely want to keep the portion that can be cached in its own` RUN` statement.


For the package registry example, we want to combine the fetch registry list with the install package into one` RUN` statement.


```text
RUN   fetch_package_registry_list && \
install_some_package
```


This ensures that the package registry list is updated when we install the package instead of potentially being outdated.


## 2. Pin versions during` apt-get install`


A more controversial Dockerfile linting issue is` DL3008` from hadolint. This issue is also present in **30% of all Dockerfiles** . The problem arises when not pinning versions during` apt-get install` . For example:


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package
```


When you don't version pin, you're not forcing the` docker build` to verify it has a specific version and thus the required packages you may need. This can lead to unexpected behavior when you build your Dockerfile or run the resulting image if you inadvertently installed a newer version of a package than you expected.


### Solution to` DL3008`


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package=1.2.*
```


By pinning the version of` some-package` , the build is forced to retrieve the particular version. This allows you to build up guarantees about the packages you're installing in your Dockerfile and the dependencies of those packages.


The reason it's controversial is because version pinning runs the risk of needing to catch up on security updates. For example, suppose you pin a package version with a security vulnerability. In that case, you risk not getting your security update when you build your Dockerfile until you change the version to a new one. This is why it's essential to understand the packages you're installing and the security implications of pinning versions.


## 3. Use` --no-install-recommends` to avoid installing unnecessary packages


Another widespread linter error is` DL3015` , installing unnecessary packages with` apt-get` . This is present in **22% of all Dockerfiles** . The issue arises when we're not using the` --no-install-recommends` flag during` apt-get install` . For example:


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package
```


When you don't use the` --no-install-recommends` flag, you install all the recommended packages for the package and the package itself. Potentially increasing the final size of your Docker image by installing packages you don't need.


### Soltuion to` DL3015`


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package --no-install-recommends
```


The solution is to pass the flag` --no-install-recommends` to` apt-get install` . This will prevent the installation of recommended packages and reduce the final size of your container image. It's essential to understand the recommended packages for the packages you're installing to ensure you're getting all the dependencies.


## 4. Avoid using the cache directory when using` pip install`


Docker layer caching comes in again when we're talking about` pip install` during a Docker build. Hadolint error` DL3042` is present in **18% of all Dockerfiles** . The issue arises when we're not telling` pip install` not to use a cache directory in our Dockerfile. For example:


```text
FROM   python:3.11
RUN   pip3 install mysql-connector-python
```


When you don't tell` pip install` not to use a cache directory, it will install the package and keep a cache directory for that package, which creates an unnecessary cache entry for every package you've installed via` pip` in that layer. When you have lots of packages, this can increase your final Docker image size.


### Solution to` DL3042`


```text
FROM   python:3.11
RUN   pip3 install --no-cache-dir mysql-connector-python
```


We don't need a cache directory for our` pip` packages because we don't need to reinstall packages when building a Docker image. The Docker layer cache can be used instead. Turning off the cache directory makes our final image smaller.


## 5. Remove the` apt-get` lists after installing packages


As we explored in our post around[reducing Docker image sizes](https://depot.dev/blog/how-to-reduce-your-docker-image-size) , keeping container image sizes down often returns to the actual` docker build` process. Hadolint error` DL3009` is present in **16% of all Dockerfiles** . The issue arises when we're not removing the` apt-get` lists after installing packages. For example:


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package --no-install-recommends
```


Our earlier example for` DL3015` , shown here, can be optimized further to keep the final image size down. By not cleaning up the` apt-get` cache, it's written into the layer for that` RUN` statement. We are taking up valuable space in our final image.


### Solution to` DL3009`


```text
FROM   ubuntu:22.04
RUN   apt-get update && \
apt-get install -y some-package --no-install-recommends && \
apt-get clean && \
rm -rf /var/lib/apt/lists/*
```


Here, we are combining the installation of` some-package` with the clean-up of the` apt-get` cache so that installing and clean-up happen in one atomic` RUN` statement. This keeps the final image size down by removing the` apt-get` cache from the final image and doesn't introduce another layer into the final image.


## 6. Make use of` WORKDIR` instead of` RUN cd some-path`


Another common Dockerfile linter issue is` DL3003` , using` RUN cd` instead of the` WORKDIR` statement. This is present in **14% of all Dockerfiles** . Here is a typical example:


```text
FROM   ubuntu:22.04
RUN   cd /usr/src/app && git clone git@github.com:depot/some-repo.git
```


Each` RUN` statement executes inside its own shell, and most commands can work with absolute paths.


### Solution to` DL3003`


```text
FROM   ubuntu:22.04
WORKDIR   /usr/src/app
RUN   git clone git@github.com:depot/some-repo.git
```


When changing directories, you can use the` WORKDIR` statement, which spawns the shell in your specified directory. The only exception is when you need to do something inside the subshell; in that scenario, you still need to use` cd` .


## 7. Pin versions when installing packages via` pip`


Like` DL3008` , the Dockerfile linter issue` DL3013` is the same idea but applied to` pip install` instead of` apt-get install` . This is present in **13% of all Dockerfiles** . Here is a typical example:


```text
FROM   python:3.11
RUN   pip3 install --no-cache-dir mysql-connector-python
```


When you don't version pin, you're not forcing the` docker build` to verify it has a specific version and thus the required packages you may need. As we saw in` DL3008` , this can have unexpected behavior if we install a different version than what we originally installed when we created the Dockerfile.


### Solution to` DL3013`


```text
FROM   python:3.11
RUN   pip3 install --no-cache-dir mysql-connector-python==8.1.0
```


By pinning the version of` mysql-connector-python` , the` docker build` is forced to retrieve the particular version regardless of what may be in the Docker layer cache.


## 8. Use JSON notation for` CMD` and` ENTRYPOINT` arguments


This Dockerfile lint error,` DL3025` , comes down to correctness when running the image. It's present in **12% of all Dockerfiles** . Here are typical examples for both statements where this comes up:


```text
FROM   ubuntu:22.04
ENTRYPOINT   foo run-server
```


```text
FROM   ubuntu:22.04
CMD   foo run-server
```


When we don't use JSON notation for` CMD` and` ENTRYPOINT` arguments, the executables referenced won't receive signals from the OS correctly. This is particularly relevant when talking about how to signal to a running container that it is being shut down (i.e., a` SIGTERM` ).


### Solution to` DL3025`


```text
FROM   ubuntu:22.04
ENTRYPOINT   [  "foo"  ,   "run-server"  ]
```


```text
FROM   ubuntu:22.04
CMD   [  "foo"  ,   "run-server"  ]
```


By using JSON notation, the executable will be the containers PID 1 and, therefore, receive signals from the OS. Two additional things to note about this notation:


1.


` CMD` doesn't process environment variables in shell form (i.e.,` $FOO_BAR` ) because of the side effect of how` sh -c` is used as the default entry point. So, we must handle environment variables ourselves outside the` CMD` statement.


2.


The` CMD` statement is parsed as a JSON array, so we **must use double quotes ("") instead of single quotes('')** to correctly pass our arguments.


## 9. Use` apt-get` or` apt-cache` instead of the user facing` apt`


The command,` apt` , is meant to be an end-user tool and not to be used in Dockerfile` RUN` statements. So,` DL3027` flags this Dockerfile lint error when we use` apt` instead of` apt-get` or` apt-cache` . This is present in **9% of all Dockerfiles** . Here is a typical example:


```text
FROM   ubuntu:22.04
RUN   apt install -y some-package=1.2.*
```


### Solution to` DL3027`


```text
FROM   ubuntu:22.04
RUN   apt-get install -y some-package=1.2.*
```


The interface of` apt` is not guaranteed across versions by Linux distributions. So it's better to use` apt-get` or` apt-cache` , which are more stable.


## 10. Pin versions when installing packages via` apk add`


As we've seen in` DL3008` and` DL3013` , pinning versions is also important for` apk add` in Alpine-based Dockerfiles. This is present in **8% of all Dockerfiles** . Here is a typical example:


```text
FROM   alpine:3.7
RUN   apk --no-cache add some-package
```


### Solution to` DL3018`


```text
FROM   alpine:3.7
RUN   apk --no-cache add some-package=~1.2.3
```


The rationale is the same: version pinning forces the` docker build` to fetch the pinned version regardless of what may be in the Docker layer cache. An important thing to note for Alpine-based images is that we are using partial pinning here via the` ~` syntax. We can pin to a specific version via` some-package=1.2.3` , but this will fail the build if this package is removed.


## Conclusion


In this post, we looked at the top 10 most common Dockerfile linting issues we're seeing as builds are flowing through Depot. As we saw, they can vary in severity and impact. But they all have the potential to improve your Dockerfiles and your builds. Each issue comes with its own set of pros and cons.


For example, pinning versions can guarantee a specific state when building Docker images but have the downside of potentially missing security updates. Or using` --no-install-recommends` can avoid making your image bigger for dependencies you don't need or use. But it can also mean you miss a dependency that you need.


This post has given you some ideas on improving your Dockerfiles and your builds via linting. If you want to learn more about how Depot can help you improve your Dockerfiles on-demand, check out our[recent post on our Docker linter](https://depot.dev/blog/dockerfile-lint-on-build) .


If you're looking to make your Docker image build process faster either for native Intel or Arm images,[sign up for an account](https://depot.dev/start) and give things a try. We make it easy to run your first build with either` docker build` or` depot build` via our[quickstart guide](https://depot.dev/docs/container-builds/quickstart) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
