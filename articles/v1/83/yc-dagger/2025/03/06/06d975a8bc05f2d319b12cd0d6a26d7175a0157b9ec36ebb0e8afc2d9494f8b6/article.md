---
schema_version: "1.0.0"
document_id: "06d975a8bc05f2d319b12cd0d6a26d7175a0157b9ec36ebb0e8afc2d9494f8b6"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/a-shell-for-the-container-age-introducing-dagger-shell/"
published_at: "2025-03-26T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:be98ec1ad917fa886150dc975f06a0d9a142f012fda7a6049cefa29961fefe91"
---

# A Shell for the Container Age: Introducing Dagger Shell

The Unix shell is over 50 years old, but it still defines how programmers use their computers. We type a few words in a terminal, and milliseconds later an ephemeral factory comes online: the Unix pipeline. Data streams through a network of simple programs working concurrently, like robots on the factory floor, executing a computational choreography we composed seconds ago. Its job done, the factory vanishes. Onto the next command. That loop built the internet, and still runs it today.


The design principles of Unix are timeless: write simple programs, compose them with standard interfaces. But 50 years is a long time… the software stack has expanded, and Unix interfaces are buried under layers of abstraction. We still use the shell, but it’s no longer the universal composition system it was meant to be. What if we changed that?


What if the Unix shell took the best ideas from docker, make, powershell and nix? Native support for primitives like containers, secrets and service endpoints; typed objects; declarative execution; content-addressed artifacts; everything sandboxed and cached by default. What if those all became standard shell features, available with standard shell syntax, and backed by a modern API? Would we still need to learn a dozen DSLs to automate the basic tasks of shipping software? Or would the complexity of the modern software stack melt away, leaving only two fundamental elements: shell and code? Let’s find out!


Today we’re introducing Dagger Shell: a bash syntax frontend for our state-of-the-art runtime and composition system, the Dagger Engine. Use it for builds, tests, ephemeral environments, deployments, or any other task you want to automate from the terminal. It’s also a great way to[compose AI agents](https://x.com/solomonstre/status/1895671390176747682) …but more on that later.


To get started, just[install the latest version of Dagger](https://docs.dagger.io/install) , and type` dagger` . Or keep reading for examples.


#### A complement to your system shell


Dagger Shell isn’t meant to replace your system shell, but to complement it. When a workflow is too complex to fit in a regular shell, the next available option is often a brittle monolith: not as simple as a shell script, not as robust as full-blown software. The Dagger Shell aspires to help you replace that monolith with a collection of simple modules, composed with standard interfaces.


```text
container |
from alpine |
with-exec apk add git
```


#### Shell and code are all you need


When a script becomes too complex and outgrows the shell syntax, why learn a weird DSL when you can write real code?


Unix was built on the twin pillars of its shell and C programming environment. Dagger follows the same model, with SDKs available for Go, Python, Typescript, Java and PHP. Pick a language, write a function…Congratulations, you just extended Dagger with your own primitive! No matter how complex your system, you should only ever need two building blocks: shell and code.


#### Shell, meet API


Dagger Shell is just another Dagger API client, giving you typed objects, built-in documentation, and access to a[cross-language ecosystem of reusable modules](https://daggerverse.dev/) .


That’s right, Dagger Shell can load any of the thousands of modules in the Daggerverse, inspect their API, and run their functions. Here we explore a[Trivy module](https://daggerverse.dev/mod/github.com/jpadams/daggerverse/trivy) :


#### Sandboxed by default


Dagger Shell commands run as sandboxed functions, accessing host resources (files, secrets, services) only when explicitly provided as arguments.


This can make commands slightly more verbose, but also more repeatable, giving you confidence to iterate quickly without second-guessing.


For example, here’s a dev environment using a secret, local directory, and database access:


```text
container |
from alpine |
with-secret-variable POSTGRES_PASSWORD op://dev/db-password/credential |
with-directory /src ~/src/myapp |
with-service-binding db tcp://localhost:5432 |
terminal
```


#### Simple container build


This creates an Alpine container, drops in a text file with your message, sets it to display that message when run, and publishes it to a temporary registry. All in one pipeline - no context switching between Dockerfile creation, build commands, and registry pushes.


#### Test environments


A common problem in CI is creating ephemeral test environments: you need to containerize the software being tested, and connect it to its dependencies. Dagger makes this easy with native support for service bindings.


For example, let’s create an environment with several live instances of the Dagger docs hooked up, and “test” them with curl.


This also highlights how third-party modules can be seamlessly mixed and matched.


```text
# Build a wolfi linux container with curl, then test connection to stable and dev docs
github.com/dagger/dagger/modules/wolfi | container --packages=curl |
with-service-binding docs-stable $(github.com/dagger/dagger/docs@v0.17.1 | server) |
with-service-binding docs-dev $(github.com/dagger/dagger/docs@main | server) |
with-exec curl http://docs-stable |
with-exec curl http://docs-dev
```


#### Multi-Stage Builds


Implement complex build workflows with clear, modular syntax. Maintain visibility and control over each stage of your pipeline. Save and reuse parts of your pipeline. Why juggle between editing Dockerfiles, running build and push commands, when you can do it all in one go?


```text
repo=$(git https://github.com/dagger/hello-dagger | head | tree)


env=$(container | from node:23 | with-directory /app $repo | with-workdir /app)


build=$($env | with-exec npm install | with-exec npm run build | directory ./dist)


container | from nginx | with-directory /usr/share/nginx/html $build | terminal --cmd=/bin/bash
```


This example pulls source code, creates a Node.js build environment, runs the build process, and packages just the built files into a minimal nginx container. Each stage is a named variable you can inspect, modify, or reuse - the state is explicit, not hidden in layers of a Dockerfile or build cache.


Get a dev build of Dagger, fresh from our repository:


```text
container |
from golang:latest |
with-directory /src $(git https://github.com/dagger/dagger | head | tree) |
with-workdir /src |
with-exec go build ./cmd/dagger |
file ./dagger |
export ./dagger
```


#### What’s Next?


We’d love for you to try out Dagger Shell and share your feedback. Learn more in the[Dagger Shell documentation](https://docs.dagger.io/features/shell) , join our[Discord](https://discord.com/channels/707636530424053791/1318344234511892490) and let us know how Dagger Shell improves your development workflow!


Happy hacking! 🚀
