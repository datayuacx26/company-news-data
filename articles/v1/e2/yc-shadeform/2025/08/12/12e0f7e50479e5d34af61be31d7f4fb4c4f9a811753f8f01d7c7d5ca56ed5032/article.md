---
schema_version: "1.0.0"
document_id: "12e0f7e50479e5d34af61be31d7f4fb4c4f9a811753f8f01d7c7d5ca56ed5032"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/tutorials/evaluate-clouds-with-shadeform--langfuse"
published_at: "2025-08-13T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:7698dc7f1fd67f58efe1e1a1e6a67f7319eba1a985b622c11d1215747af52a75"
---

# Evaluate Clouds with Shadeform + Langfuse

## Self-Hosted Models are on the Rise


While many AI startups have built their applications around closed source model APIs, a growing number are turning to self-hosted open source or custom models—especially in sectors that handle sensitive data like healthcare and legal.


Self-hosted models allow companies to retain full control over their data and fully customize the model to their application, making it an attractive choice for both privacy and competitive edge.


## How to Self-Host a Model


When self-hosting, companies can either deploy the model in their own datacenter (if they have the capital), or (more commonly) deploy the model in the cloud.


However, if you deploy in the cloud, how do you go about choosing which cloud is best for your deployment?


Many will opt for a hyperscaler like AWS, GCP, or Azure, but will suffer prices that have been marked up 10-12x what you could be paying elsewhere.


For companies who are capital sensitive, running in one of the 50+ new dedicated AI clouds like Lambda Labs, Nebius, Crusoe, or others is going to be the smartest choice.


## Evaluating Clouds for Self-Hosting


When evaluating these new clouds, outside of pricing, we care most about how our model is going to perform for the end user once deployed—latency, throughput, etc.


There are many factors that contribute, from GPU specs to storage and networking configurations. However, it’s almost impossible to gauge model performance by looking at a cloud’s stat sheet.


The only reliable way to determine how your deployment will perform is to test it yourself. Luckily, we’ve put something together to make that process much simpler.


## Shadeform x Langfuse: Automated Cross-Cloud Benchmarking


We’ve teamed up with[Langfuse](https://langfuse.com/) , an open source model tracing and evaluation platform, to put together an[automated benchmarking script](https://platform.shadeform.ai/templates/cd968d1e-a2c0-405b-b4f7-04025131f14b) that will help you easily measure self-hosted model performance across clouds.


With[Shadeform](https://shadeform.ai/) ’s multi-cloud GPU marketplace and launch template feature, we can easily pre-load and deploy the script across multiple cloud environments to evaluate them.


**Here’s how the script works:**


- Creates a local deployment of Langfuse inside the GPU instance.
- Starts a vLLM server––the current gold standard inference engine––with a specified Hugging Face model.
- Feeds a series of prompts to the model.
- Langfuse traces each chat completion and records latency, tokens, and tokens/second.


Once the script completes, you’ll be able to view the results from the Langfuse UI.


## Getting Started


### Step 1: Create a Shadeform account and go to the template


If you don’t have one already, create a Shadeform account[here](https://platform.shadeform.ai/) .


Once you have an account and can access the platform, you can find the benchmarking template[here](https://platform.shadeform.ai/templates/cd968d1e-a2c0-405b-b4f7-04025131f14b) .


### Step 2: Save the benchmarking template as a copy


Before we begin, we’ll want to save the template as a copy.


To do this, click “Save as a Copy” in the top right corner.


Your copy of the template can be found in the “Templates” tab on the navigation bar under “My Resources”.


### Step 3: Customize your template


To customize your template, click on your copy of the template, and then click “Edit” in the top right corner.


Scroll down to the section titled “Environment Variables”.


Start by replacing the` HUGGINGFACE_ACCESS_TOKEN` value with your own.


Next, replace the` MODEL_ID` value with the id of the Hugging Face model you want to evaluate, e.g.` deepseek-ai/DeepSeek-R1` .


**(Optional)** If you want to customize the batch of prompts used to benchmark the model, scroll down to the “Startup Script” section.


Find the section of the script titled` — BENCHMARKING SCRIPT SETUP —` . Here, you’ll find a list of prompts that you can customize to your needs.


When you finish configuring the script, click “Update Template” at the bottom.


### Step 4: Select a GPU instance from the cloud you want to evaluate


Click “Compute Marketplace” on the navigation bar, scroll down, and filter the results by “Cloud”.


Next, find a GPU instance you want to test on (e.g. 8x H200) and click “Launch”.


### Step 5: Deploy the template and wait for the script to complete


Find the field titled “Template” towards the top of the launch page and select your copy of the benchmarking template.


Once your configurations populate, click “Deploy”.


The script should take around 15-30 minutes to complete depending on the model and GPU(s) used. You can check the script’s status by clicking on the “Running Instances” tab on the navigation bar, clicking on the instance, and clicking “Logs”.


You should see the following message once everything has finished:` “Benchmarking complete, view the results at: <instance-ip>:3000"`


### Step 6: Access the Langfuse UI to see the benchmark results


Once the script has completed, go to` <instance-ip>:3000` to view the results.


When you get to the Langfuse UI, you’ll be met with an authentication screen.


A user is provisioned by default so you don’t have to create a new user.


> - **Username:**ops@shadeform.ai
> - **Password:** 12345678


Alternatively, any user created will automatically bind to the default organization with the pre-configured benchmarking project.


Once you've authenticated in the Langfuse UI, click "Go to project" under the project titled "Benchmark Run 1".


Next, click "Tracing > Traces" on the left-hand navigation bar.


Here, you will see a table with each trace labeled "Prompt n" based on the order of the list of prompts.


The table will show you the latency (time it took to generate), the total tokens used, as well as the tokens per second.


Clicking on a trace will show the details of each response.


### (Optional) Step 7: Run additional tests


If you want to run additional benchmarks in the same session, you’ll need to SSH into the instance.


To SSH, click on the instance in the “Running Instances” tab.


Scrolling down, you’ll find a “Get Private Key” button.


Click this button and follow the instructions on screen.


Once you have successfully SSH’d into the instance, you can copy the python benchmarking script from the template into a new .py file and edit as needed before running.


### Step 8: Repeat steps 5 & 6 for the next cloud


Because you saved the template as a copy in step 2, it’s ready to be deployed on any other cloud instance for evaluation.


Simply repeat steps 5 & 6 for the next cloud you want to evaluate.


## Summary


Self-hosted models offer significant advantages over closed source API solutions, especially for companies handling sensitive data. But, choosing the right cloud to deploy your model in can be both time and resource intensive. This automated benchmarking script simplifies performance evaluation across cloud environments, enabling companies to confidently select the optimal deployment solution based on real-world metrics.


For more information on Langfuse, visit their[website](https://langfuse.com/) and take a look through their extensive[documentation](https://langfuse.com/docs/) .
