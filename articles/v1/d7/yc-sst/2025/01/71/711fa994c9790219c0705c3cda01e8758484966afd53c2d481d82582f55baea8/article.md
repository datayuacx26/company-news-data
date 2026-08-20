---
schema_version: "1.0.0"
document_id: "711fa994c9790219c0705c3cda01e8758484966afd53c2d481d82582f55baea8"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/configure-autodeploy-workflow/"
published_at: "2025-01-30T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:3b727ffb891116ec83056a7a68031ad3de31d60451265b4167b3ba9581458f01"
---

# Configure Autodeploy workflow

You can now completely control the build process when auto-deploying your app through the[Console](https://sst.dev/docs/console) .


##### Background


After you’ve enabled[Autodeploy](https://sst.dev/docs/console#autodeploy) , the Console will start a build process, check out your code, detect your package manager, install dependencies, and run` sst deploy` ; all without needing a config.


However, there are times where you want to configure the build process. For example, to run your tests, or run some post-deploy script.


You can now do that with the new` autodeploy.workflow` prop.


---


### Workflow


The[autodeploy.workflow](https://sst.dev/docs/reference/config#console-autodeploy-workflow) prop allows you to completely take over control of the build process and run any commands you want.


sst.config.ts


```text
export     default     $config  ({        app  (  input  )   {   /* ... */   },        async     run  ()   {   /* ... */   },         console: {           autodeploy: {            async     workflow  (  {   $  ,   event   }   )   {              await     $  `  npm i  `  ;               event  .  action     ===     "  removed  "                ?     await     $  `  npm sst remove  `                :     await     $  `  npm sst deploy  `  ;             }           }         }    });
```


The problem with CI/CD scripts has always been that they are bash commands embedded in a YAML config. This can be hard to write and maintain.


Even though the SST config is in TypeScript, it’s still cumbersome to write shell scripts in Node.


---


#### Bun Shell


To address this we run the` workflow` function in a Bun process.` $` is the[Bun Shell](https://bun.sh/docs/runtime/shell) , a *bash-like* shell that makes it easy to write scripts in TypeScript and JavaScript.


sst.config.ts


```text
async   workflow  ({ $, event }) {        await     $  `  npm i -g pnpm  `  ;        await     $  `  pnpm i  `  ;
const {   exitCode   } = await   $  `  pnpm test  `  .  nothrow  ();        if   (exitCode   !==     0  ) {          // Process the test report and then fail the build          throw     new     Error  (  "  Failed to run tests  "  );         }
event  .  action     ===     "  removed  "          ?     await     $  `  pnpm sst remove  `          :     await     $  `  pnpm sst deploy  `  ;    }
```


This simplifies the shell commands in your deployment scripts.


---


#### Errors


In the above example, we are throwing an error after we handle a failed command.


```text
throw     new     Error  (  "  Failed to run tests  "  );
```


This will fail the build and display this error message in the Console.


---


#### Learn more


You can see the new` workflow` prop in action in how the[Console is deployed](https://github.com/sst/sst/blob/dev/www/sst.config.ts) ; since the Console auto-deploys itself!


sst.config.ts


```text
async   workflow  ({ $, event }) {        await     $  `  bun i  `  ;        await     $  `  goenv install 1.21.3 && goenv global 1.21.3  `  ;        await     $  `  cd ../platform && ./scripts/build  `  ;        await     $  `  bun i sst-linux-x64  `  ;         event  .  action     ===     "  removed  "          ?     await     $  `  bun sst remove  `          :     await     $  `  bun sst deploy  `  ;    }
```


[Learn more](https://sst.dev/docs/reference/config#console-autodeploy-workflow) about the new` workflow` prop in the docs.


---


### Other updates


We also rolled out a couple of other updates to Autodeploy:


- We implemented a workaround so you won’t get the annoying Docker Hub rate limit errors while deploying the` Service` component.
- You can now return multiple stages in[autodeploy.target](https://sst.dev/docs/reference/config#console-autodeploy-target) . This means that when you git push to your repo, the Console can deploy to multiple stages.


You can learn more about the[Console](https://sst.dev/docs/console) and[Autodeploy](https://sst.dev/docs/console#autodeploy) in the docs.
