---
schema_version: "1.0.0"
document_id: "d0bf3e12da4de6455f13feb5ffae0b8f19500894b3511169d4ff3d32250dbff7"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/introducing-simplekiq-a-lightweight-orchestration-framework-for-ruby"
published_at: "2023-05-03T11:28:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:17be29f262b7982d59691915a9bcf8001dc5c1293d57f695eb8c3f5aacd945fb"
---

# Introducing Simplekiq: A Lightweight Orchestration Framework for Ruby

Sidekiq is an amazing background job framework for Ruby with a long history of the best kind of minimalism - one of performance and succinctness. However, a downside of this is that long workflows built from dozens of background jobs queueing subsequent background jobs can be difficult to consistently design and maintain.


We especially struggled with refactoring these types of structures since one has to read through all the code for the jobs to understand when and from where each job might get queued. The added complexity of manually defining Sidekiq Pro batches leads to difficult-to-diagnose triage in more creative workflows, especially when batch callbacks are involved.


## Introducing Simplekiq: A Solution to Our Challenges


We solved our issue by building[Simplekiq](https://github.com/doximity/simplekiq) (special thanks to[Daniel Pepper](https://github.com/dpep) for freeing the rubygems[simplekiq](https://rubygems.org/gems/simplekiq) name for us) —a lightweight background job workflow framework built on top of[Sidekiq Pro](https://sidekiq.org/products/pro.html) batching. Simplekiq solves the issue of having to follow long chains of jobs that queue other jobs ad nauseam.


Workflows that span many files and job classes are inherently difficult to understand, diagnose and refactor. Simplekiq flattens out these complex, multi-file workflows into declarative, single-file ones which we call orchestrations. Each orchestration can contain a mix of serial and parallel execution so that even the most complex workflow can be expressed in an efficient, readable syntax.


## Comparing Sidekiq Pro and Simplekiq with an Example


It’s easier to demonstrate this with an example: making an apple pie. The late, great Carl Sagan[taught us](https://www.youtube.com/watch?v=BkHCO8f2TWs) that to make an apple pie from scratch we must first invent the universe. That’s a bit out of budget for most bakeries, but in the spirit of simplicity let’s explore how a workflow for baking an apple pie from basic ingredients looks in standard Sidekiq Pro structures and compare it to the flattened Simplekiq paradigm. Our example will also show how refactoring differs between the two.


### Implementing a Workflow with Sidekiq Pro


If we wanted to bake an apple pie using a series of Sidekiq jobs which we can start out with running one after the other in series, then we might come up with the following job classes:


```text
PrepareButterJob
PrepareWhiteSugarJob
PrepareGroundCinnamonJob
PrepareThinlySlicedApplesJob
PreparePieCrustJob
PreheatOvenJob
FillPieJob
BakePieJob


```


We can leave the basic implementation of the jobs to your imagination, but let's assume they all take a workstation id as an argument and perform an intermediate step that takes the baking workstation one step closer to a finished pie.


Without Simplekiq, it can be time-consuming to set these up to run one after the other. The most straightforward approach would be to simply queue up the next job at the end of each job:


```text
class    PrepareButterJob
include    Sidekiq  ::  Worker


def    perform  (  workstation_id  )
workstation    =    Workstation  .  find  (  workstation_id  )


cow    =    workstation  .  find_cow
milk    =    cow  .  milk
butter    =    workstation  .  churn  (  milk  )
workstation  .  store  (  butter  )


# This next `PrepareWhiteSugarJob` will run after this current `PrepareButterJob` completes
PrepareWhiteSugarJob  .  perform_async  (  workstation_id  )
end
end


```


But then we realize that many of the intermediate ingredients can be prepared independently, which means we can parallelize them for greater overall pie throughput.


This requires a lot of code changes to our "straightforward" approach of daisy-chaining jobs since we now have to change all of the jobs involved. Instead of calling each other, we have to queue them together and queue the next step in a success callback. There's also no clear place to put this since they're the first jobs in our workflow chain, so if we want the entry point to still be a job then we'll have to make a new one just for this:


```text
class    PrepareIntermediatePieIngredientsJob
include    Sidekiq  ::  Worker


def    perform  (  workstation_id  )
batch    =    Sidekiq  ::  Batch  .  new
batch  .  on  (  :success  ,    PrepareIntermediatePieIngredientsJob  ,    workstation_id:   workstation_id  )
batch  .  jobs    do
# The following jobs will all run in parallel
PrepareButterJob  .  perform_async  (  workstation_id  )
PrepareWhiteSugarJob  .  perform_async  (  workstation_id  )
PrepareGroundCinnamonJob  .  perform_async  (  workstation_id  )
PrepareThinlySlicedApplesJob  .  perform_async  (  workstation_id  )
end
end


def    on_success  (  status  ,    options  )
# The following job will run after the parallelized jobs above complete successfully
PreparePieCrustJob  .  perform_async  (  options  [  :workstation_id  ])
end
end


```


Yuck. Now we have a whole new class and file just to handle a performance adjustment and need to modify all four of the jobs involved (plus their tests) since they will no longer be queueing the next job like they were before.


### Implementing a Workflow with Simplekiq


Let's take a look at how this is different when we use Simplekiq to orchestrate our workflow from the start:


```text
class    MakeApplePieFromScratchJob
include    Simplekiq  ::  OrchestrationJob


def    perform_orchestration  (  workstation_id  )
# The following jobs will run in series
run    PrepareButterJob  ,    workstation_id
run    PrepareWhiteSugarJob  ,    workstation_id
run    PrepareGroundCinnamonJob  ,    workstation_id
run    PrepareThinlySlicedApplesJob  ,    workstation_id
run    PreparePieCrustJob  ,    workstation_id
run    PreheatOvenJob  ,    workstation_id
run    FillPieJob  ,    workstation_id
run    BakePieJob  ,    workstation_id
end
end


```


Already we can see how much easier it is to understand at a glance how this workflow functions. There's no need to manually daisy-chain jobs. Since the framework is just a thin wrapper on top of native Sidekiq features the jobs themselves are regular Sidekiq jobs that only need to adhere to the classic Sidekiq interface. You can write normal Sidekiq jobs that either finish successfully or fail by raising exceptions.


### Refactoring with Simplekiq


So how do we change those first few jobs to be parallelized? Easy, use Simplekiq's` in_parallel` block:


```text
class    MakeApplePieFromScratchJob
include    Simplekiq  ::  OrchestrationJob


def    perform_orchestration  (  workstation_id  )
in_parallel    do
# These jobs will run in parallel
run    PrepareButterJob  ,    workstation_id
run    PrepareWhiteSugarJob  ,    workstation_id
run    PrepareGroundCinnamonJob  ,    workstation_id
run    PrepareThinlySlicedApplesJob  ,    workstation_id
end


# The following jobs will run in series after all the parallel jobs above complete successfully
run    PreparePieCrustJob  ,    workstation_id
run    PreheatOvenJob  ,    workstation_id
run    FillPieJob  ,    workstation_id
run    BakePieJob  ,    workstation_id
end
end


```


No need to modify or make new jobs; the new block is the only change required. Behind the scenes it's doing a very similar thing to what we did in our initial example, but the verbosity and complexity are hidden behind the metaprogramming of the Simplekiq DSL. The code we’re maintaining is as focused on the business logic flow as it can be, with as much of the repetitive workflow configuration logic stripped out as possible.


### Breaking Down Jobs and Parallelizing with Simplekiq


Let's say we learn that our pie throughput is limited by preparing the white sugar. It turns out white sugar manufacturing is a complex process so we first break` PrepareWhiteSugarJob` up into a series of jobs by turning it into an orchestration:


```text
class    PrepareWhiteSugarJob
include    Simplekiq  ::  OrchestrationJob


def    perform_orchestration  (  workstation_id  )
# The following jobs will run in series
run    HarvestSugarCaneJob  ,    workstation_id
run    SqueezeCaneJuiceJob  ,    workstation_id
run    AddHydratedLimeJob  ,    workstation_id
run    FilterOutInsolubleCalciumOrganicCompoundsJob  ,    workstation_id
run    RemoveExcessLimeViaCarbonationJob  ,    workstation_id
run    BoilAndEvaporateExcessWaterJob  ,    workstation_id
run    GatherSugarCrystalsWithCentrifugeJob  ,    workstation_id
end
end


```


Then we identify` SqueezeCaneJuiceJob` as the bottleneck in the process due to slow juicers so we decide to parallelize this step. Without Simplekiq, the changes required to achieve this would be something like:


- Add a new` SqueezeIndividualCaneJuiceJob` designed to handle each cane
- Pick apart the cane retrieval and iteration logic from the individual cane processing logic and move the latter into this new job
- Weave` Sidekiq::Batch` management logic into the original cane retrieval and iteration logic
- Add a callback to the batch and a method to handle it to one of the cane squeezing job classes
- Move the queueing of the next job (` AddHydratedLimeJob` ) to this callback


Luckily it's much easier with Simplekiq. After we've found our bottleneck we just need to modify that one` SqueezeCaneJuiceJob` to be a` BatchingJob` . Here’s what that might look like with contrived ActiveRecord-like behavior:


```text
class    SqueezeCaneJuiceJob
include    Simplekiq  ::  BatchingJob


# Instead of `perform` like a normal job, we start in `perform_batching`.
# You need to queue all your batches by the time this job returns.
# Once it returns, it will automatically wrap the queued jobs in the new batch.
def    perform_batching  (  workstation_id  )
workstation    =    Workstation  .  find  (  workstation_id  )


workstation  .  sugar_cane_stalks  .  find_each    do    |  stalk  |
# This is how the jobs get queued, this leads to `perform_batch`
queue_batch  (  workstation  .  id  ,    stalk  .  id  )
end
end


# Every `queue_batch` invocation leads to a job which uses this method as
# their ingress with their respective parameters as assigned via `queue_batch`.
def    perform_batch  (  workstation_id  ,    stalk_id  )
workstation    =    Workstation  .  find  (  workstation_id  )
stalk    =    SugarCaneStalk  .  find  (  stalk_id  )


juice    =    workstation  .  juice  (  stalk  )
workstation  .  store  (  juice  )
end


# Once all batched jobs complete successfully this job will be considered
# complete and the orchestration one layer up will continue to the next step.
end


```


The outer` PrepareWhiteSugarJob` will automatically wait for the batching of` SqueezeCaneJuiceJob` to finish before moving onto the` AddHydratedLimeJob` step, and the` MakeApplePieFromScratchJob` will wait for the` PrepareWhiteSugarJob` to finish before considering its` in_parallel` block to be finished—exactly what we wanted and with only minor file modifications needed for each change.


### Adding Error Handling with Simplekiq


Lastly, let's say that we've been running into some quality control issues and we want to start aborting apple pie workflows more aggressively. We’ll need widespread and consistent error handling for that. Without Simplekiq, we’d have to add error handling to every class. If we then want to change the error-handling interface we need to make a tedious and mistake-prone update to every job.


With Simplekiq, instead we can simply add an` on_death` callback handler to the top-level orchestration:


```text
class    MakeApplePieFromScratchJob
include    Simplekiq  ::  OrchestrationJob


def    perform_orchestration  (  workstation_id  )
in_parallel    do
run    PrepareButterJob  ,    workstation_id
run    PrepareWhiteSugarJob  ,    workstation_id
run    PrepareGroundCinnamonJob  ,    workstation_id
run    PrepareThinlySlicedApplesJob  ,    workstation_id
end


# The following jobs will run in series if/when the above parallel jobs finish successfully
run    PreparePieCrustJob  ,    workstation_id
# The next job will run if and only if the prior job finishes successfully
run    FillPieJob  ,    workstation_id
# Same as above...
run    PreheatOvenJob  ,    workstation_id
run    BakePieJob  ,    workstation_id
end


def    on_death  (  status  ,    options  )
# This method will run if and only if any of the jobs recursively within this orchestration raise
workstation_id    =    options  [  "args"  ].  first
Workstation  .  find  (  workstation_id  ).  abort_pie
end
end


```


And that's all we need to get some behavior which will get triggered on a failed job no matter where it is in the tree of nested orchestrations.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
