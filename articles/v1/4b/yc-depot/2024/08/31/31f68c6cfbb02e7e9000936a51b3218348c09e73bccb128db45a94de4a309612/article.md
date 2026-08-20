---
schema_version: "1.0.0"
document_id: "31f68c6cfbb02e7e9000936a51b3218348c09e73bccb128db45a94de4a309612"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-to-build-with-depot-on-fly"
published_at: "2024-08-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:4797bf68b65296fbb952aa3e096a9138038ecfba025d12884e9c794e4f04687f"
---

# Deploy to Fly using a Depot builder

# Deploy to Fly using a Depot builder


Have you ever been playing Rock, Paper, Scissors against an API endpoint and thought to yourself, wow, I wish this would load faster in Australia? No, of course not, but you have more interesting services and apps than Rock, Paper, Scissors.


Yet, that is what we are going to do today, and we are going to do it fast, from quick builds to low latency deployments.


You may already know,[Fly.io](https://fly.io/) is a cloud platform that makes it easy to deploy globally distributed applications with zero config load balancing. Fly runs your code close to the user for minimal latency. Supply your code, optionally provide a Dockerfile, and Fly will take care of the rest.


Part of deploying an application fast, is building it fast. We[recently announced our partnership with Fly.io](https://depot.dev/blog/fly-builds-powered-by-depot) 🎉, and now Fly users can immediately take advantage of[Depot's accelerated Docker builders](https://depot.dev/products/container-builds) to build and deploy their applications faster than ever, no Depot account required.


In this quick walkthrough, I'll show you how to build and deploy a TypeScript service to Fly.io using the new Depot builder.


## Why build with Depot on Fly?


Depot builds faster. Over the past two years, Depot has developed and optimized a custom build engine based on Docker’s BuildKit, excelling in the cloud. Depot automatically manages distributed build caches with perfectly incremental builds, so you can build your Docker images in seconds, not minutes.


When paired with Fly's blazing-fast Firecracker VM based infrastructure, Depot and Fly provide a powerful combination for building and deploying your applications globally with speed.


## Sample Project Setup


Clone or download our sample app from the[GitHub repository](https://github.com/depot/fly-depot-nest-api-demo) . Inside you'll find a simple[NestJS](https://nestjs.com/) TypeScript API service. It has a single endpoint for playing Rock, Paper, Scissors, and a Swagger UI for testing and documentation


```text
git   clone   git@github.com:depot/fly-depot-nest-api-demo.git
cd   fly-depot-nest-api-demo
```


## The Dockerfile


Notice, we have a Dockerfile in the root of the project. This Dockerfile is designed to build an optimized Docker image for our TypeScript service, taking advantage of multi-stage builds and optimized caching. We have a great full-detailed guide on how to write an optimized Dockerfile for Node with pnpm[here](https://depot.dev/docs/container-builds/optimal-dockerfiles/node-pnpm-dockerfile) . Using Fly, it isn't required that you provide a Dockerfile, but we can often craft a more optimized build process when you do.


Let's take a quick look at our Dockerfile


```text
FROM   node:lts-alpine   as   base


FROM   base   AS   deps
RUN   corepack enable
WORKDIR   /app
COPY   package.json pnpm-lock.yaml ./
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm fetch --frozen-lockfile
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm install --frozen-lockfile --prod


FROM   base   AS   build
ENV   NODE_ENV=production
RUN   corepack enable && npm install -g @nestjs/cli
WORKDIR   /app
COPY   package.json pnpm-lock.yaml ./
RUN   --mount=type=cache,id=pnpm,target=/root/.local/share/pnpm/store pnpm install --frozen-lockfile
COPY   . .
RUN   pnpm build


FROM   base
ENV   NODE_ENV=production
WORKDIR   /app
# Copying the package.json is necessary to set the type to module
COPY   package.json ./
COPY   --from=deps /app/node_modules /app/node_modules
COPY   --from=build /app/dist /app/dist
EXPOSE   3000
CMD   [  "node"  ,   "./dist/main.js"  ]
```


Based on our[Node pnpm Dockerfile guide](https://depot.dev/docs/container-builds/optimal-dockerfiles/node-pnpm-dockerfile) mentioned above, our Dockerfile is organized into three stages.


We start with an optimized base image, like` node:lts-alpine` to minimize additional files in the image layers, reducing the final size of the built image. The default` node` image based on Debian is` 1.09GB` vs` node:lts-alpine` at` 132MB` .


```text
REPOSITORY                                     TAG            IMAGE   ID         CREATED          SIZE
node                                           lts-alpine     d9946f265131     13   days   ago      132MB
node                                           lts            4efd4a4c24f5     4   weeks   ago      1.09GB
```


From our base image, we create a series of stages to optimize the build process for further enhancing the cache ability of the build layers.


## Local Testing


Before deploying to Fly, let's test our service locally and explore the Swagger UI, so we know what to expect when we deploy it.


We're going to build the Dockerfile. If you have a Depot account you can use the` depot build` command to take advantage of our powerful remote builders, otherwise you can use` docker build` .


```text
docker   build   -t   rock-paper-scissors   .
```


Now we can run the Docker container and test the service.


```text
docker   run   -p   3000:3000   rock-paper-scissors
```


### Hello World!


Open up your browser and navigate to[http://localhost:3000/](http://localhost:3000/) and verify you see a` Hello World!` message.


### Swagger UI


The Swagger UI is available at[http://localhost:3000/api](http://localhost:3000/api) . Here you can see documentation about the available endpoints in the service and even test them out with a nice UI.


[Swagger UI Rock Paper Scissor game](https://depot.dev/images/swagger-rock-paper-scissors.webp)


Try hitting the "Test it out" button and then "Execute" to play a game of Rock, Paper, Scissors with your selected "Move".


Example response:


```text
{
"player"  :   "rock"  ,
"computer"  :   "scissors"  ,
"result"  :   "win"
}
```


## Deploy to Fly


Now that we have confirmed the service is working as expected, let's deploy it to Fly using the new Depot builder.


The project already comes with a pre-defined` fly.toml` . You can use or edit what is included, or delete it and run` fly launch` to generate a new config file pointing to your local region and select an app name automatically. If you use the included` fly.toml` , make sure to replace` <app-name>` with a unique name for your service.


Use the` --depot` flag to specify the Depot builder should be used to accelerate the build process.


```text
fly   deploy   --depot
```


Behind the scenes, Depot is running accelerated container image builders with instant caching across builds inside of Fly. You can watch the build logs in your terminal as the service is built and deployed. On your first deployment you likely Depot builds and caches the initial layers, and subsequent builds automatically reuse previous layers that are valid in your Docker layer cache.


Once the build is complete, you'll see a message with the URL to access your service.


```text


Visit your newly deployed app at https://<app-name>.fly.dev/
```


Go ahead and open the URL in your browser to` /api` and you should see the Swagger UI for the Rock, Paper, Scissors service.


## Try Depot on Fly


Congratulations 🎉! You've successfully deployed a TypeScript service to Fly using the new Depot builder. We'd love to know how it went! For Fly-specific questions, check out the[announcement post on the Fly.io forum](https://community.fly.io/t/depot-remote-builder-support-in-flyctl-is-now-in-beta/21087) .


Have a question about Depot? Let's chat on Discord! Join us in the[Depot Discord server](https://depot.dev/discord) and let us know how we can help you build and deploy faster.


Kyle Tryon
