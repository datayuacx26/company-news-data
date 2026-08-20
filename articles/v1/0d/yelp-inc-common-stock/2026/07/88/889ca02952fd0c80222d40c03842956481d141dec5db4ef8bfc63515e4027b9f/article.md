---
schema_version: "1.0.0"
document_id: "889ca02952fd0c80222d40c03842956481d141dec5db4ef8bfc63515e4027b9f"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/07/training-orchestrator-unifying-model-training-at-yelp.html"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:18:26.702617+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:630ac926ee1d411074bb9cd93fa987d856ad9ae6e64311e7d2c9b22d81abbd54"
---

# Training Orchestrator: Unifying Model Training at Yelp

# Training Orchestrator: Unifying Model Training at Yelp


- Ying Wang and Nathan Sponberg, Software Engineer


- Jul 14, 2026


At Yelp, we train many machine learning models on different schedules. Applied machine learning teams all have their own set of Spark-based training batches, scripts, and configurations. Over time, these diverged, leading to duplicated code, subtle inconsistencies, and a growing maintenance burden.


Yelp’s Core Machine Learning Team has developed excellent tooling across our ML ecosystem over the years: feature stores for reproducible data, a unified training library for neural networks and gradient-boosted trees, seamless Spark integration, and MLflow services for model tracking and deployment. But there was still one key piece missing right in the middle: a standardized way to orchestrate model training itself.


That’s where Training Orchestrator comes in.


# The Challenge That Motivated Us


Before this project, most training ran as monolithic scripts launched on a Spark cluster. Execution was tightly coupled to the cluster runtime and job launcher.


This caused several issues:


- **No local runs** : Couldn’t run pipelines on a local Spark cluster, since the execution was tied to container and cluster submission.
- **Little testability** : Hard to write unit/integration tests because training logic was intertwined with job execution. Every small change kicked off the same slow cycle: submit a Spark job, wait for Docker containers to build and clusters to spin up, watch it fail, and start over.
- **Boilerplate and drift** : Repeated scaffolding across pipelines; inconsistent configuration structures and logging patterns.
- **Scattered validation** : Checks spread across scripts, causing inconsistent error timing and redundant logic.
- **Scattered monitoring** : Teams maintained fragile custom scripts for training progress tracking and notifications.
- **Limited reproducibility across runs and environments** : Inconsistent configurations and missing provenance made exact reruns across environments difficult.


*Figure 1 - Status quo: Each team maintained its own custom training batch implementation, with duplicated orchestration and custom configuration management.*


# The Vision: Configuration-Driven Simplicity


We set out to build Training Orchestrator with a clear vision: let model developers focus on their model training logic, not on orchestration boilerplate.


The core insight was simple: most training workflows follow a similar pattern — load data, prepare it, train feature pipelines and models, evaluate, explain, rinse and repeat. What varied was the specific logic at each step and the parameters that controlled it.


So we asked ourselves: what if you could make orchestration fully declarative? What if you could describe your entire training workflow in a configuration file, plug in your custom training functions where needed, and let the framework handle the rest?


These questions shaped our design principles:


- **Flexibility Without Fragmentation**


- *Decoupled* - Run the same configuration locally, in Jupyter, or in production.
- *Declarative and modular* - Describe reusable training steps and their types, don’t hardcode orchestration logic.


- **Fast Feedback, Easy Debugging**


- *Observable and debuggable* - Step-level logs, tracing, and debugger-friendly local runs.
- *Unified notifications* - Automatic progress updates and error alerts, no custom scripts needed.
- *Testable by design* - Unit and integration tests without needing a Spark job submission process.


- **Reliability and Confidence**


- *Type-safe* - Static type checking catches errors before runtime; configurations are validated early and completely.
- *Unified validation* - Centralized validation of step input and output schemas ensures data flows correctly between steps, eliminating scattered checks.
- *Reproducible* - Every run can be replayed from a single configuration.


- **Developer Productivity**


- *Opinionated defaults and scaffolding* - Templates and conventions that minimize boilerplate.


# The Architecture: Stepwise Orchestration


Training Orchestrator creates a clean separation between what you want to run and how it gets executed. Figure 2 shows this layered architecture.


*Figure 2 - Training Orchestrator Architecture: a unified orchestration layer standardizes configuration, execution, and MLflow logging across training pipelines.*


## Configuration Layer: Type-Safe Declarations


In Figure 2, at the top, the Pydantic Configuration and Validation layer (purple section) is where pipelines are defined. This includes:


- **Orchestrator config** : Top-level settings for the entire pipeline.
- **MLflow Run config** : Specific settings for the MLflow run associated with the orchestrator job.
- **Step configuration hierarchy** : Individual step configurations for each step in the orchestrator job.
- **Global config** : Shared parameters accessible across all steps.


Every configuration is validated at instantiation through Pydantic models. Training Orchestrator validates step input and output schemas at runtime, ensuring that the data types each step produces match what downstream steps expect to consume. For example, steps that output datasets connect to steps that expect datasets, and steps that output models connect to steps that expect models. This catches configuration mismatches in seconds rather than discovering them hours into expensive training runs. By the time your orchestrator job starts executing, you know all the parameters are valid and all dependencies are satisfied.


## Global Context: Execution Environment


The Spark and MLflow context provides the runtime environment. This is what enables:


- Distributed compute via Spark
- Automatic experiment tracking and logging via MLflow
- Consistent logging and metadata collection


This context is injected into steps, so the same step definitions work locally or in production.


## Orchestrator Execution Layer: The DAG Engine


This is the heart of the system (beige section), where the configuration becomes a running model training pipeline. Here’s what happens:


### DAG Construction


The orchestrator reads your step configurations and their declared dependencies to automatically build a Directed Acyclic Graph (DAG) — essentially a flowchart that shows which steps must run before others, with no circular dependencies. Think of it like a recipe: you can’t frost a cake before baking it, and the DAG ensures steps are executed in the right order. Each green box in the diagram represents a step instance, a runnable unit with inputs, outputs, and user-defined logic.


### Step Execution


Steps execute in topological order, meaning each step waits for all its prerequisites to finish before starting. The orchestrator:


- Waits for upstream steps to complete before starting downstream ones
- Passes outputs from one step as inputs to the next
- Logs progress and metrics to MLflow at each stage (blue boxes below steps)
- Optionally sends real-time updates to user-configured Slack channels for visibility
- Handles failures gracefully with clear error context


### Inside a Step


The bottom section in the diagram shows what is inside each step. Each step has:


1. **Inputs** : Data from previous steps or external sources.
2. **User-defined code** : Your training logic, feature engineering, or model code.
3. **Step config** : Step basic metadata (e.g., what a step is, where it gets its inputs, etc.), and user-defined parameters controlling the behavior (e.g., file paths, split ratios, number of epochs, etc.).
4. **Outputs** : Results passed to downstream steps.


The same step type can execute different logic by simply swapping the user-defined code reference. Step types are uniquely defined by their input/output schema (data they consume and produce). For example, a` PrepareDataStep` doesn’t care what preparation function you use; instead, it just calls whatever function you configured and passes the results forward.


## A Simple Example


Let’s see how easy it is to build a complete training pipeline. We will use the Iris dataset to make a three-step workflow.


*Figure 3 - A three-step Iris training pipeline*


### Step 1: Write Your Training Logic


To plug your code into a Training Orchestrator step, you provide two pieces:


- Your Settings class (parameters for your function)
- Your function (the actual logic)


Figure 4 provides an example of creating a` PrepareData` step by defining the settings class and the user-defined function.


*Figure 4 - Breakdown of the PrepareDataStep*


Before composing all steps together, make sure you have created all of your steps following the pattern shown above.


### Step 2: Compose Steps


Once all steps are defined, you can then compose them into a pipeline. You can define a pipeline with a custom Pydantic model that holds the configuration for all steps in your workflow. In this configuration, each step can declare dependencies on previous steps, which chains the steps together.


```text
# Compose steps together
step_configs    =    MyStepConfigsModel  (
load_data  =  UrlLoadDataStepConfig  (
step_id  =  "  load  "  ,
source_urls  =  "  s3://my-data  "  ,
),
prepare_data  =  PrepareDataStepConfig  [  PrepareIrisDatasetSettings  ](
step_id  =  "  prepare  "  ,
input_step_id  =  "  load  "  ,     # ← Declares dependency
prepare_function_args  =  PrepareIrisDatasetSettings  (  split_ratio  =  0.66  ),
),
train_model  =  TrainStepConfig  [  TrainIrisModelSettings  ](
step_id  =  "  train  "  ,
input_step_id  =  "  prepare  "  ,     # ← Chains to prepared data
train_function_args  =  TrainIrisModelSettings  (
max_depth  =  6  ,
),
),
)
```


Figure 5 shows what the composition and the resulting DAG look like:


*Figure 5 - Configuration declares dependencies, orchestrator builds the DAG*


### Step 3: Run It


To execute the orchestrator job, you just need to configure the basic metadata of the orchestrator and let it know where your step configurations are (from the previous step).


*Figure 6 - Build and execute the orchestrator*


The orchestrator validates your configuration upfront, builds the execution DAG, then runs each step in dependency order with clear logging at every stage. When your orchestrator job is running, here’s what you see:


```text
All steps match their configurations
Beginning Steps Execution...
Executing LoadDataStep "load"...
Loading data from: s3://my-data
Step "load" executed successfully
Executing PrepareDataStep "prepare"...
Step "prepare" executed successfully
Executing TrainStep "train"...
Step "train" executed successfully
Orchestrator run completed!
```


### Mix and Match


The two-part, settings-and-function contract simplifies step swapping in many different scenarios. Here are several scenarios that you might encounter in your ML training pipeline development.


#### Scenario 1: Different Preprocessing Logic


If you want to try a different preprocessing method, you can provide a different settings + function pair to the same orchestrator job. You only need to make essential changes such as the settings for the method input argument and the core logic of your custom preprocessing method. The rest, including other step definitions and configurations, stays the same.


#### Scenario 2: Test Two Models in Parallel


If you want to compare algorithms of two different models, then you can have the same data loading and prep steps but different training steps.


#### Scenario 3: Reuse Your Function Across Batches


Your function and settings are portable and decoupled from orchestration. This means that you can use the same custom logic in multiple orchestrator batches.


#### Scenario 4: Dev/Prod - Same Code


You can use different configurations in different environments without changing code. For example, in local development you want to load data from a local file, but in prod you want to use data in an S3 bucket, so you can just update the file path in your` LoadDataStepConfig` .


# The Wins That Keep On Giving


As teams started adopting Training Orchestrator, we saw tangible outcomes:


## Testing Became Actually Feasible


Before Training Orchestrator, testing even a small change meant submitting a Spark job, waiting for the cluster to spin up, watching it fail, and starting over. The time-consuming feedback loop made testing impractical.


Since Training Orchestrator decouples the orchestrator job from infrastructure, developers can run a full job locally with sample data, iterate until the logic is solid, and get faster feedback loops. Teams can also write unit tests for individual steps more easily, which helps move training code toward stronger and more consistent test coverage. Additionally, with automatic schema validation, configuration errors that used to surface hours into training runs now fail in seconds during the validation phase. Training Orchestrator’s input caching for data loading steps can also significantly reduce integration test time when running on a small sub-sample of training data.


## Configuration Became a Single Source of Truth


Before Training Orchestrator, each job had its own config, scattered validation, sometimes undocumented CLI args, and inline defaults that drifted over time.


Now Training Orchestrator adopts Pydantic-based configuration where user-defined settings classes contain parameters, validation rules, defaults, and documentation in one place:


```text
class    IrisDataPrepareSettings  (  BaseSettings  ):
split_ratio  :    float    =    Field  (
default  =  0.66  ,
ge  =  0.0  ,
le  =  1.0  ,     # Validation rule
description  =  (
"  The ratio for splitting the dataset into training and testing sets.   "
"  Valid range: 0 <= split_ratio <= 1.0  "
),
)
```


There are more bonuses that Pydantic brings:


- Developers get IDE autocomplete as they type.
- CLI flags generate automatically with help text.
- Validation happens at instantiation, which surfaces configuration errors early.


## Reproducibility Across Environments


Inconsistent configurations and missing provenance made reproducing past runs difficult. Different teams had their own way to log and restore configurations and metadata. Now with Training Orchestrator, every training run captures its complete configuration and logs it with the MLflow run automatically. Therefore, the entire pipeline state becomes a versionable artifact. If a developer needs to reproduce a run, then they just need to reload the configuration from an MLflow run.


## MLflow Integration


Previously, users needed to manually write boilerplate to manage MLflow-related run creation, logging, and tracking. Now, all of this happens automatically. If users configure MLflow run parameters for their jobs, the Training Orchestrator initializes an MLflow tracking context that spans every step. This allows user-defined training code to call MLflow logging APIs directly without additional setup or context management.


## Real-Time Notifications Made Simple


Before Training Orchestrator, teams wrote their own Slack notification scripts — fragile shell commands that parsed log files and broke easily. Every team had their own version of complex scripts that missed failures and provided inconsistent updates. Now teams get comprehensive Slack notifications with just a few configuration parameters. Users specify their Slack team channel and username, and the Training Orchestrator handles the rest automatically.


# The Road Ahead


While Training Orchestrator has proven its value in production, we’re continuing to evolve it based on user feedback.


Today, teams write custom code to evaluate models, compare results, explain predictions, and register artifacts. We plan to build first-class, declarative steps for these common patterns — allowing teams to configure them instead of implementing them. This means less boilerplate and consistent evaluation reports across teams.


Lineage tracking is critical for reproducibility, debugging, and governance. Combined with feature lineage tracking, we can create complete visibility from raw data through features, models, and predictions. Because Training Orchestrator centralizes functionality, we can implement lineage tracking once at the orchestration layer.


This centralized orchestration architecture is designed to scale with us. When we add capabilities like lineage tracking or other steps, every existing pipeline benefits automatically. Teams get new functionality without migration work, and the platform becomes more powerful with each addition.


# Conclusion


If you’re building ML infrastructure at scale, here’s what we learned from unifying training orchestration at Yelp:


- **Make testing feasible.** Without orchestration, training code often goes untested because the friction is too high. By making steps independently runnable with local Spark, teams can easily implement unit tests and iterate on their workflow faster.
- **Decouple orchestration from execution.** In other words, separate what to run from how to run it. When training logic isn’t tied to infrastructure (Spark clusters, containers, etc.), it becomes easy to do local development, unit tests, and environment parity.
- **Declarative is better than imperative for workflows.** Writing “Step C needs output from step B” is much clearer and more maintainable than “run A, then B, then C, and don’t forget D.” Dependency-driven DAGs let the framework handle execution order. Teams can focus on their batch logic, while the orchestrator handles the rest.
- **Good configuration helps a lot.** We transformed scattered validation and undocumented CLI args into self-documenting, type-safe schemas with Pydantic. This moves configuration checks to instantiation time, so developers can catch many errors upfront instead of waiting for them to surface deep into a training run.
- **Make it reproducible.** We took this into account from day one to log job configuration as versioned artifacts in MLflow. Every run captures its complete state, making production debugging and experiment reproduction much more reliable.


Machine learning at scale isn’t just about algorithms and models. It’s about the infrastructure that lets teams move fast while maintaining quality and confidence. Training Orchestrator is our contribution to making that infrastructure better.


# Acknowledgements


We would like to thank the Model Platform team for their support throughout this project, especially Yunhui (Austin) Zhang and Jason Sleight for their insightful feedback.


[Tweet](https://twitter.com/share)


### Become a Software Engineer at Yelp


Want to help us make even better tools for our full stack engineers? If you're interested, apply!


[View Job](https://www.yelp.com/careers/job-openings/bd07a618-9b6f-4920-91c6-99280f1b268d?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
