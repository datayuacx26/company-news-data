---
schema_version: "1.0.0"
document_id: "09c8a0a81fd3a771c2a59b88cc9a8397eb281b3715ee008294f61ca34d608a6f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-configure-docker"
published_at: "2023-07-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:e7b348c724936258cf5a98420ebf178b6df3df98a635bfcae8eb33c715dfdc06"
---

# Now available: Configure Docker to run builds with Depot by default

**We've expanded the functionality of the` depot` CLI to allow you to configure` docker build` and` docker buildx build` to use Depot for all image builds by default. Check out our[Docker Compose announcement post](https://depot.dev/blog/depot-with-docker-compose) for more details.**


We are excited to announce that[depot v2.24.0](https://github.com/depot/cli/releases/tag/v2.24.0) and above introduces a new command so you can now use Depot to build Docker images anywhere you use` docker build` without changing a single line of code 🎉


We've introduced a new command,` depot configure-docker` , that installs Depot as a Docker CLI plugin and makes Depot the default builder for` docker build` and` docker buildx build` commands.


## How to set it up


With the latest version of the Depot CLI installed, you can run the following command:


```text
depot   configure-docker
Successfully   installed   Depot   as   a   Docker   CLI   plugin
```


The` configure-docker` command installs Depot as a plugin to the Docker CLI and sets the default builder to Depot. The plugin routes your usual` docker build` command to Depot builders. Configuring which Depot project your builds should go to can be accomplished via any of the methods below:


1. You can set a` DEPOT_PROJECT_ID` environment variable to the project ID of the project you want to use for builds.
2. You can run` depot init` in the root of the repository in which you're building your image, creating a` depot.json` file that our plugin will detect.


After running the command,` docker build` is routed through Depot!


## New GitHub Action:` depot/use-action`


We have released a new GitHub Action,[depot/use-action](https://github.com/depot/use-action) , that you can now use in your workflows. The action allows you to build your Docker images with Depot without changing anything else in your workflow. It does the following steps:


1. Installs the` depot` CLI in the GitHub Actions environment
2. Installs Depot as a Docker CLI plugin
3. Sets the Depot plugin as the default Docker builder


The GitHub Action needs to resolve a Depot project ID to use for builds. This can be done via the` DEPOT_PROJECT_ID` environment variable, our` depot.json` file, or you can specify the project ID directly in the action.


```text
jobs  :
job-name  :
steps  :
-   uses  :   depot/use-action@v1
with  :
project  :   <your-project-id>
```


You can authenticate to Depot via our recommended[OIDC trust relationships](https://depot.dev/docs/container-builds/integrations/github-actions#docker-hub) just as you can with our other GitHub Actions.


```text
jobs  :
job-name  :
permissions  :
contents  :   read
id-token  :   write
# optional, if you need to push to ghcr.io
packages  :   write


steps  :
-   uses  :   depot/use-action@v1
```


You can now switch your Docker image builds to leverage Depot by adding a single action to your GitHub Actions workflows. You can add` depot/use-action` above your existing` docker build` or` docker/build-push-action` , and Depot will take over the build.


```text
jobs  :
job-name  :
steps  :
-   uses  :   actions/checkout@v3
-   uses  :   depot/use-action@v1
with  :
project  :   <your-project-id>
-   uses  :   docker/build-push-action@v4
with  :
push  :   true
tags  :   |
...
```


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
