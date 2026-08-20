---
schema_version: "1.0.0"
document_id: "90566920337d07615c669a12183f0f25af1d610b2079551a2754e2421f4a9758"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/registry-integration-testing-load-save"
published_at: "2025-03-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:05f0df507aafafe6c64a3232c40d07352816729c0a4b72acfce5b622bb8496a7"
---

# How to reuse builds across integration test workflows

Running tests in a CI pipeline involves more than simply executing test files.


In the simplest case, we may have a singular workflow where we want to create a build and run a single set of integration tests. But even in this simplest case, there's a base amount of setup to get your environment in a state that’s ready for tests to be run. For example, we have to make sure the code the tests execute against is running on the machine correctly. Then, the relevant code needs to be built, which typically requires a series of steps.


And remember: That's just the simplest, single-workflow case. In many cases, there's more than just a single test workflow. In this scenario, creating a build for each workflow can be expensive both in the time it takes, and monetary costs associated with build times.


There is a solution, though. Rather than creating a build for each workflow, you can create your build once, save it to a registry, and then pull it from the registry for each additional workflow.


In this post, I’ll demonstrate how to do just that with[Depot Registry](https://depot.dev/docs/registry/overview) .


One thing to note: For these examples, I'll be utilizing OIDC trust Relationships for GitHub Actions as the authentication method. This requires the permission` id-token: write` , which is not represented in the following examples. There are multiple ways to authenticate with Depot; you can see our[authentication docs](https://depot.dev/docs/cli/authentication) for more options.


[The Depot Registry is a full-featured container registry for storing, managing, and distributing your Docker images. It provides a complete solution for image management throughout your development lifecycle. Get Started →](https://depot.dev/docs/registry/overview)


## One workflow


Let’s begin by walking through that simplest use case: the single workflow. In this case, we want to build our image and then use it to run tests.


In a workflow file, we can use Depot to run our build and download it onto the machine. By default, Depot leaves the build in the remote build cache, so adding the` --load` flag or` load: true` to our action configuration ensures that the build is both created and accessible on the machine running this command:


```text
name  :   Integration Tests


on  :
push  :
branches  :
-   main
workflow_dispatch  : {}


jobs  :
build-and-run-tests  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   uses  :   depot/setup-action@v1
-   name  :   Build and save the image
uses  :   depot/build-push-action@v1
id  :   build
with  :
load  :   true
project  :   <your-project-id>
-   name  :   Setup and Run tests
... run the tests
```


Once you add any other setup steps needed for executing your tests, your workflow is likely complete and you’re ready to be on your way.


In the case that you're using another registry and want to have your build end up there, you can utilize the Depot Registry along with the` depot push` command. The` depot push` command allows us to take a build from the Depot Registry and push it to another registry. We just need to add the` --save` flag or` save: true` configuration to our command, and it will be saved there and accessible for future use. Simply add` depot push` to transfer the build directly from the Depot infrastructure to your target registry.


```text
name  :   Integration Tests
...
jobs  :
build-and-run-tests  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   uses  :   depot/setup-action@v1
-   name  :   Build and save the image
uses  :   depot/build-push-action@v1
id  :   build
with  :
save  :   true
load  :   true
project  :   <your-project-id>
-   name  :   Setup and Run tests
... run the tests
-   name  :   Registry login
... log in to your target registry
-   shell  :   bash
run  :   depot push --project <your-project-id> --target <your-target> --tag org/repo:tag ${{ steps.build.outputs.build-id }}
```


## Multiple workflows


The single workflow case is pretty straightforward, but oftentimes our setup is more complex when we're running multiple workflows. We don’t want to have to create a new, identical build for each workflow, so let's walk through how to save a build once and then use it multiple times.


### Save your build once


To utilize a build from the registry across a number of other workflows, we first need to actually run a build and save it. Like we did with the setup earlier for a single workflow, we want to create a Depot build and save it. This time, however, we’ll use the Depot cli to make use of the new custom` save-tag` feature.


The` save-tag` flag is a newly implemented feature, which lets us use a friendly name to reference a build rather than the build id.


We can specify a` save-tag` and reference the build directly from the registry using the` save-tag` flag as follows:` registry.depot.dev/<project-id>:<my-friendly-name>` .


In the following example, I’ll use the` github.sha` for the` save-tag` value so that it’s accessible from both workflows.


```text
name  :   Save Build


on  :
push  :
branches  :
-   main
workflow_dispatch  : {}


jobs  :
build  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   name  :   Build and save the image
shell  :   bash
run  :   depot build ./yaml-commit-scraper --save --save-tag ${{ github.sha }} --project <your-project-id>
```


Once this build is created and saved we can use the` save-tag` value in the other workflows to reference our saved build.


### Using the build


With GitHub Actions, we can set up multiple workflows that depend on the success of another before being triggered. To ensure that the workflows using the build don’t start before the build is completed, we need to configure these workflows to wait for the build workflow to complete.


We can do this using the[workflow_run event](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#workflow_run) . Assuming the name of the workflow that creates and saves the build is “Save Build”, the following will configure our subsequent workflows to wait to be triggered until the "Save Build" workflow has completed. This is configured to only run the` pull-build-and-test` job if the parent workflow completes successfully.


```text
name  :   Integration Tests


on  :
workflow_run  :
workflows  : [  Save Build  ]
types  :
-   completed


jobs  :
pull-build-and-test  :
runs-on  :   depot-ubuntu-24.04
if  :   ${{ github.event.workflow_run.conclusion == 'success' }}
```


Once the build workflow is completed, we should be able to pull down the build directly from the registry for use in subsequent test workflows. This will make the build available on the machine that the workflow is running allowing you to run integration tests against it.


You can add the steps to the job as follows:


```text
jobs  :
pull-build-and-test  :
runs-on  :   depot-ubuntu-24.04
if  :   ${{ github.event.workflow_run.conclusion == 'success' }}
steps  :
-   shell  :   bash
run  :   docker pull registry.depot.dev/<project-id>:${{ github.event.workflow_run.head_sha }}
...
-   name  :   Setup and Run tests
...
```


By using the same build in each of these workflows, we’re ensuring the same build image is used across test workflows to ensure consistency as well as speeding up the process so we don't have to wait for a build to complete each time.


At this point in your workflows, once the build has been pulled down, you can add steps to complete your setup and then actually run the integration test. You can have a number of these integration tests that follow these same patterns and run after the build workflow is complete; they just need to be able to pull the build after the build is saved.


One thing to note with this setup is we haven't pushed the build to another registry after the integration tests complete successfully. Currently the GitHub Actions` workflow_run` event will be triggered if any of the specified workflows meets our criteria. There is not currrently a way to specify that we want to wait for all of the workflows specified in our` workflow_run` event to complete before triggering this workflow. Due to this limitation, if you want to push a build to a different registry while utilizing this multi-workflow setup, we recommend to do the push step as a part of your build rather than after the integration tests have run.


```text
jobs  :
build  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   name  :   Registry login
... log in to your target registry
-   name  :   Build and save the image
shell  :   bash
run  :   depot build ./yaml-commit-scraper --save --save-tag ${{ github.sha }} --project <your-project-id> --push
```


### Full workflow files


We've included the full workflow files for both the single and multiple worklfow examples for reference.


#### Single workflow


```text
name  :   Integration Tests
on  :
push  :
branches  :
-   main
workflow_dispatch  : {}


jobs  :
build-and-run-tests  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   uses  :   depot/setup-action@v1
-   name  :   Build and save the image
uses  :   depot/build-push-action@v1
id  :   build
with  :
save  :   true
load  :   true
project  :   <your-project-id>
-   name  :   Setup and Run tests
... run the tests
-   shell  :   bash
```


#### Multiple workflows


```text
name  :   Save Build
on  :
push  :
branches  :
-   main
workflow_dispatch  : {}


jobs  :
build  :
runs-on  :   depot-ubuntu-22.04
steps  :
-   uses  :   actions/checkout@v3
-   name  :   Build and save the image
shell  :   bash
```


```text
name  :   Integration Tests


on  :
workflow_run  :
workflows  : [  Save Build  ]
types  :
-   completed


jobs  :
pull-build-and-test  :
runs-on  :   depot-ubuntu-24.04
if  :   ${{ github.event.workflow_run.conclusion == 'success' }}
steps  :
-   shell  :   bash
...
-   name  :   Setup and Run tests
...
```


## Wrapping Up


Whether you're optimizing a single workflow or managing multiple test suites, the approach described here helps improve efficiency while maintaining the reliability of your builds. And as Depot continues to enhance its registry features, integrating these best practices into your CI/CD pipeline will only become more seamless.


Start optimizing your workflows today by reusing builds with Depot Registry!


## Related posts


- [Now available: Depot Registry](https://depot.dev/blog/introducing-depot-registry)
- [Comparing GitHub Actions and Depot runners for 2x faster builds](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds)
- [Uncovering Disk I/O Bottlenecks in GitHub Actions](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci)


Iris Scholten


Staff Engineer at Depot
