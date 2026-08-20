---
schema_version: "1.0.0"
document_id: "dd7d1ddefbcc45688f9476e2b9ea25e0891c5ccf6865e2a69adf4afd63fd3727"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/dagger-functions-for-depot"
published_at: "2024-02-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:b2228b060a4c8c4820ffdc0db4fa2efb5ca0e093718ae8ce3699a194da7a7077"
---

# Now available: Dagger Functions for Depot

We're excited to announce that we have released a set of[Dagger Functions](https://dagger.io/blog/introducing-dagger-functions) for Depot that allows you to offload any` build` or` bake` operation to Depot and use the resulting image in your own Dagger code. The power of Depot combined with Dagger unlocks a lot of exciting use cases that will be incredibly valuable in the future as Dagger adoption continues to ramp up.


## How it works


We've launched our own[Depot module](https://daggerverse.dev/mod/github.com/depot/daggerverse/depot) , which is a collection of Dagger Functions that interface with our existing` depot build` and` depot bake` commands.


We can create a very simple Dockerfile to demonstrate the power of Dagger with Depot:


```text
FROM   cgr.dev/chainguard/node:latest
ENTRYPOINT   [  "echo"  ,   "chainguard node"  ]
```


You can leverage the module in your Dagger setup in two ways. The first is to invoke the Dagger functions in the module directly from the Dagger CLI:


```text
dagger   -m   github.com/depot/daggerverse/depot   call   \
build   --token   env:DEPOT_TOKEN   --project   <  depot-project-i  d  >   --directory   .   container
```


The CLI here invokes the` build` function inside the Depot module, passing in the usual required flags for the` depot build` command. Notably, the project token,` --token` , comes from an environment variable called` DEPOT_TOKEN` . By default, the function will then build and return the image via our[ephemeral registry](https://depot.dev/blog/depot-ephemeral-registry) . Given our basic Dockerfile above, we can see the module in action:


The second way to use the Depot module is to leverage it in your own Dagger module. We recommend following the[Dagger module quickstart](https://docs.dagger.io/quickstart/428201/custom-modules) to get started with a Dagger module.


Here is an example of a Dagger module that uses the Depot module inside:


```text
package   main


import   (
"  context  "
)


type   DaggerModuleTest   struct  {}


func   (  m   *  DaggerModuleTest  )   CheckCVEs  (  ctx   context  .  Context  ,   depotToken   *  Secret  ,   project   string  ,   directory   *  Directory  ) (  string  ,   error  ) {
artifact   :=   dag.  Depot  ().  Build  (depotToken, project, directory,   DepotBuildOpts  {Sbom:   true  })
sbomFile   :=   artifact.  Sbom  ()
return   dag.
Container  ().
From  (  "anchore/grype:latest"  ).
WithFile  (  "/mnt/sbom.spdx.json"  , sbomFile).
WithExec  ([]  string  {  "sbom:/mnt/sbom.spdx.json"  ,   "--fail-on=high"  }).
Stdout  (ctx)
}
```


This example shows the additional power you get when you combine Depot for the image build with the ability to chain modules and functions in Dagger.


The` DaggerModuleTest` module exposes a function,` CheckCVEs` , that builds the image via the Depot module, asking for an SBOM to be generated, and then runs[Grype](https://github.com/anchore/grype) on the resulting bill of materials, failing the build if any high severity CVEs are found.


From within the` DaggerModuleTest` module, we can call the` CheckCVEs` function via the Dagger CLI:


```text
dagger   call   checkCves   \
--depot-token   env:DEPOT_TOKEN   \
--project   <  depot-project-i  d  >   \
--directory   .
```


Here is what leveraging the Depot module in a new Dagger module looks like in action:


We see that our build didn't fail because we don't have any high-severity CVEs in our base image, but if we were to change the base image to something like` node:16-alpine` , we would see the build fail:


You can see more examples of how you can use the Depot module with Dagger for quite a few other use cases in our[Daggerverse repo](https://github.com/depot/daggerverse/tree/main/depot#cli-examples) .


## What's next


We're excited to see what the community does with the new Dagger Functions for Depot! There are quite a few exciting new use cases that combining Depot with Dagger unlocks, and we can't wait to see what you build.


We will be exploring new modules we can build that allow you to codify more of your CI/CD pipeline in Dagger while combining the features of Depot to make your CI/CD pipeline faster and more efficient. You can stay tuned on those ideas in our[Discord Community](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
