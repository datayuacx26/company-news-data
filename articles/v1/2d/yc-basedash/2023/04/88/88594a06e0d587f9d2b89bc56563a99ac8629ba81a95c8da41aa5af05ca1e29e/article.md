---
schema_version: "1.0.0"
document_id: "88594a06e0d587f9d2b89bc56563a99ac8629ba81a95c8da41aa5af05ca1e29e"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/our-typescript-monorepo-setup/"
published_at: "2023-04-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:b600eb55f55ed3c8463e28ea6a43057a91d1b1a2aeb9506149ca661f4db497f7"
---

# Our TypeScript monorepo setup

Our Basedash app is set up as a TypeScript monorepo. That means that our client (React app) and our server (node.js app) both live in the same repository. We like the monorepo approach because it makes it easy to make changes between the server and client in one pull request, simplifies our deployments, and makes code sharing between client and server more manageable. This article is going to go over the following details:


- monorepo structure
- how we use Yarn workspaces to share code
- how we are using a shared` tsconfig.json` file
- how we’ve set up code linting to use a shareable config
- how we use tRPC to get type safety on both the server and client for API requests
- how we use Turborepo to build apps and packages and run tasks
- future improvements we would like to make in our setup


Breakdown of the languages used in our monorepo. Can you tell we love TypeScript?


## Monorepo structure


Here’s a simplified overview of how our monorepo code is structured.


```text
apps/
client/
server/
packages/
constants/
eslint-config-basedash/
prisma/
types/
utils/
e2e/
package.json
```


- **apps:** The code for our client react app and our node.js server
- **packages:** Shared code is found within this directory. The code is shared between apps and can also be shared between other packages. The reason we have our[Prisma](https://www.prisma.io/) code in a` prisma` package instead of keeping it within the` server` app directory is because we use the generated Prisma types on both the client, server, and other shared packages. This is especially useful for[Prisma enums](https://www.prisma.io/docs/concepts/components/prisma-schema/data-model#defining-enums) .
- **e2e:** End-to-end tests. The only shared code it uses is our shared eslint config (` eslint-config-basedash` )
- **package.json:** The root` package.json` contains information about how all the code is related and it contains scripts used to run Turborepo tasks


## Code sharing with yarn workspaces


[Yarn workspaces](https://classic.yarnpkg.com/lang/en/docs/workspaces/) is how we manage to share code between our apps and packages. Here’s a look at the` package.json` in the root of the monorepo where we specify the Yarn workspace config:


```text
{
"private"  :   true  ,
"workspaces"  : {
"packages"  : [
"apps/*"  ,
"packages/*"
],
"nohoist"  : [
"**/jest"  ,
"**/prisma"  ,
"**/@prisma/client"
]
}
}
```


The` private` option must be set to` true` since it is a requirement for yarn workspaces. The` nohoist` is required to get Prisma and Jest working properly where it is used within the underlying packages and apps. You can read more about what` nohoist` does[here](https://classic.yarnpkg.com/blog/2018/02/15/nohoist/) .


For each package in our` packages` directory, there is a` package.json` file where the name of the package is specified. For example, our` constants` package has a name of` @basedash/constants` :


```text
{
"name"  :   "@basedash/constants"
}
```


All our package names start with` @basedash/` except for our eslint config which is named` eslint-config-basedash` . This is because ESLint requires that shareable config names are prefixed by` eslint-config-` ([docs on shareable ESLint configs](https://eslint.org/docs/latest/developer-guide/shareable-configs) ).


In order to use a package within an app or another package, the package needs to be specified in the consuming app’s/package’s` package.json` as a dependency (or devDependency) with` *` as the version (which means “match any version”).


As an example, our client app references all the packages as well as the server by having the following in its` package.json` :


```text
{
"dependencies"  : {
"@basedash/constants"  :   "*"  ,
"@basedash/types"  :   "*"  ,
"@basedash/utils"  :   "*"  ,
"@basedash/prisma"  :   "*"  ,
"@basedash/server"  :   "*"
}
}
```


Then in a` .tsx` file in the client, we can import stuff as we normally would for any other npm package:


```text
import   { SOCKET_EVENTS }   from   '@basedash/constants'  ;
```


## TypeScript configuration setup


Each package and app needs to have its own` tsconfig.json` file in order to use TypeScript files. We have a root` tsconfig.json` file at the root of the monorepo that has default TypeScript configuration options use by all of our packages and apps. Each package and app extends from the root` tsconfig.json` file and can specify additional TypeScript configuration options.


Here is our root` tsconfig.json` file:


```text
{
"compilerOptions"  : {
"target"  :   "es6"  ,
"strict"  :   true  ,
"noUncheckedIndexedAccess"  :   true  ,
"lib"  : [  "ES2019"  ,   "ES2021.String"  ],
"esModuleInterop"  :   true  ,
"allowJs"  :   true  ,
"allowSyntheticDefaultImports"  :   true  ,
"moduleResolution"  :   "node"  ,
"module"  :   "commonjs"  ,
"resolveJsonModule"  :   true  ,
"skipLibCheck"  :   true  ,
"isolatedModules"  :   true  ,
"declaration"  :   true
},
"exclude"  : []
}
```


And here’s how one of our packages (e.g. our` constants` package) can extend from the root` tsconfig.json` file:


```text
{
"extends"  :   "../../tsconfig.json"  ,
"compilerOptions"  : {
"outDir"  :   "dist"  ,
"rootDir"  :   "."  ,
},
"include"  : [
"**/*.ts"
]
}
```


With a common shared config, we can more easily maintain consistency of code in our TypeScript monorepo without needing to duplicate configuration options.


## ESLint configuration


As mentioned earlier, we have a shared ESLint package named` eslint-config-basedash` and each app and package extends from this shared ESLint config by having the following in each app’s/package’s` .eslintrc` file:


```text
{
"root"  :   true  ,
"extends"  : [
"basedash"
]
}
```


This allows for all our code to use a common set of ESLint rules, while still allowing for additional customizations on a per-app or per-package basis.


Here’s how the` .eslintrc` file looks like in our` server` app directory:


```text
{
"root"  :   true  ,
"extends"  : [  "basedash"  ],
"rules"  : {
"no-restricted-imports"  : [  "error"  , {
"name"  :   "utils/logger"  ,
"importNames"  : [  "logger"  ],
"message"  :   "Use the logger instance on req.logger since it holds information like the user's ID and email"
}],
"no-console"  :   "error"
}
}
```


The server’s ESLint config adds custom ESLint rules that throw errors if there are any usages of` console.log` or if someone tries to import the` logger` from` utils/logger` .


## Type-safe APIs using tRPC


We are using[tRPC](https://trpc.io/) for most of our API endpoints (in tRPC lingo, endpoints are defined as[procedures](https://trpc.io/docs/server/procedures) ). This allows us to make API calls via the tRPC client, which ensures that all the passed arguments are properly type-checked and that the data received from the API is also fully typed. Because our tRPC procedures are defined in our` server` app directory, we are required to transpile/build our server code in order to allow our` client` app to use the generated tRPC types.


The flow looks as follows:


1. Define a tRPC endpoint/procedure ([tRPC docs on how to do this](https://trpc.io/docs/server/procedures) )
2. Transpile/build the server code
3. Use the tRPC client in the` client` app


The` client` app uses the tRPC server types as follows:


```text
import   type   { TrpcRouter }   from   '@basedash/server'  ;
import   { createTRPCProxyClient, httpBatchLink }   from   '@trpc/client'  ;


export   const   trpcClient   =   createTRPCProxyClient  <  TrpcRouter  >({
links: [
httpBatchLink  ({
url:   `/trpc`  ,
}),
],
});
```


` TrpcRouter` is a type exported from the` server` app that contains all the typing corresponding to the tRPC API endpoints/procedures.


```text
export   type   TrpcRouter   =   typeof   appRouter;
```


Where` appRouter` is the tRPC router on which all the endpoints/procedures are defined.


### A note regarding superjson


One thing to note about tRPC is that it incorporates well with[superjson](https://github.com/blitz-js/superjson) , which allows you to send stuff like` Date` objects or` Map` and` Set` objects from your server and have them still represented as` Date` s,` Map` s, and` Set` s on the client. Normally, if you don’t use a tool like superjson to serialize and deserialize your API payloads, your` Date` objects will be converted to` string` s because you can’t send` Date` objects via HTTP requests.


We tried using superjson, but found that we were having to manually convert most of our` Date` objects to strings anyways in order for them to be stored in our Redux store ([see Redux docs on avoiding non-serializable values](https://redux.js.org/style-guide/#do-not-put-non-serializable-values-in-state-or-actions) ). Therefore, we use tRPC without superjson.


## Turborepo


We use[Turborepo](https://turborepo.org/) to build our apps and packages as well as run other “tasks” in our apps and packages, such as linting, testing, and typechecking.


Here’s a look at our` turbo.json` config file:


```text
{
"pipeline"  : {
"build"  : {
"outputs"  : [
"dist/**"  ,
"build/**"
],
"dependsOn"  : [
"^build"
]
},
"test"  : {
"outputs"  : [],
"dependsOn"  : []
},
"lint"  : {
"dependsOn"  : [],
"outputs"  : []
},
"typecheck"  : {
"dependsOn"  : [],
"outputs"  : []
},
"clean"  : {
"cache"  :   false
}
}
}
```


And here are the scripts defined in the root` package.json` that are used to run the Turborepo tasks:


```text
{
"scripts"  : {
"build"  :   "turbo run build"  ,
"build:packages"  :   "turbo run build --filter=@basedash/* --filter=!@basedash/client"  ,
"lint"  :   "turbo run lint"  ,
"test"  :   "turbo run test"  ,
"clean"  :   "turbo run clean && rm -rf node_modules"  ,
"typecheck"  :   "turbo run typecheck"
},
"devDependencies"  : {
"turbo"  :   "^1.2.9"
}
}
```


Here’s an explanation of each of our tasks:


- **build:** Transpiles TypeScript code into JavaScript code for all our apps and packages. For the` client` app, builds the optimized/minified HTML and JavaScript files used for a web app.
- **tests** : Runs unit tests
- **lint:** Lints code using ESLint
- **typecheck:** Looks for type errors using` tsc`
- **clean** : Deletes all the` node_modules` directories as well as any build outputs (i.e.` dist` or` build` directories). Used when debugging things and needing to start from a *clean* slate.


The way tasks work is that when a task is run, such as` turbo run lint` , Turborepo will go through all the` lint` commands specified in the` scripts` of each package’s` package.json` file. Also, as long as` "cache": false` isn’t specified in the` turbo.json` config for the task, then Turborepo will cache the result of running the task for each package. This means that if` turbo run lint` is ran twice in a row without making any changes to the files in the yarn workspace packages, then Turborepo will simply use the same lint result found from the cache.


Here’s an example of a portion of the output shown as a result of running` turbo run lint` for the first time. Notice how turbo repo says “cache miss”, indicating that it doesn’t have anything to read from the cache.


It took 29.62s to run the lint task on our monorepo. Now if` turbo run lint` is ran again, turborepo will notice that no files have changed, so it will “replay” the output and not run the` lint` command for each package again (turbo repo calls this “full turbo” when everything is cached).


This is most noticeable for us when running` turbo run build` since building our packages is what takes the longest amount of time in our CI. Now, if we make a pull request that only affects code in the` client` app, then Turborepo will only build the client app and get the builds for all the other packages from a remote cache. To get remote caching setup, see Turborepo’s[documentation page](https://turborepo.org/docs/core-concepts/remote-caching) .


The great thing about this is that the remote caching also works within docker, which is important for us since when we are building our packages on our CI, we do so within docker. Here’s the line/instruction in our` Dockerfile` that runs the Turborepo build task:


```text
RUN   yarn build   --  token  =  $TURBO_TOKEN   --  team  =  $TURBO_TEAM
```


The token and team arguments are ultimately read from environment variables configured on our CI that specify where the Turborepo remote cache is found.


Before Turborepo, the best we could do with caching our docker builds was to enable[Docker Layer Caching](https://circleci.com/docs/docker-layer-caching) on CircleCI (our CI provider). Docker Layer Caching caches the results of running instructions in your` Dockerfile` , but when it reached the` RUN yarn build` instruction (prior to turborepo), it would only read from the docker cache if ALL the files in ANY of the packages/apps haven’t been modified. This is of course not useful since it’s highly likely that when making a code change in our monorepo, we will have changed code in one of our packages (the only exception being if the code change only affects end-to-end tests or CI config files). With Turborepo, we can read from the remote Turborepo cache and only build what needs to be built.


If interested, you can read more about how the docker build cache works here:[https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache) . Read more about the turbo repo caching here:[https://turbo.build/repo/docs/core-concepts/caching](https://turbo.build/repo/docs/core-concepts/caching)


## Future improvements


One of the biggest annoyances that remains is having to always rebuild our` server` code when making a change to a tRPC endpoint. This could be solved if we were to **migrate our app to Next.js** since the server code and client code could co-exist together (i.e. wouldn’t need to be linked together via yarn workspaces). This is something we’ve been considering, but the migration from a single-page app built with[parcel](https://parceljs.org/) to a Next.js app seems daunting. That being said, some people have done it (see[this Twitter thread](https://twitter.com/brotzky_/status/1599933305092460544) ) and it might be doable for us without too much work.


I’d also like to migrate away from yarn and instead **use[pnpm](https://pnpm.io/)** . I sometimes get some strange issues with installing packages using yarn and pnpm seems to be faster and the latest gold standard for package managers. We’ve tried to migrate once already but ran into some blockers.


---


## Twitter thread


Here’s a Twitter thread summarizing the main points of the article:


1/7: Basedash uses a TypeScript monorepo setup for efficient code sharing and builds between client and server.


2/7: The monorepo structure consists of “apps”, “packages”, and “e2e”.


3/7: Code sharing is managed through yarn workspaces, which allows for easy reference to shared packages.


4/7: Shared TypeScript configuration options are maintained using a common` tsconfig.json` file.


5/7: We use a shared ESLint package and extend it for customizations on a per-app or per-package basis.


6/7: Type-safe APIs are created via tRPC, which ensures all passed arguments are properly type-checked.


7/7: Turborepo is used to build apps and packages and run tasks such as linting, testing, and typechecking, with the ability to cache results.
