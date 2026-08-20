---
schema_version: "1.0.0"
document_id: "07632e4f2d6c32294564ef1979195c8330122323fb3f3ddea92f80a1780a6851"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/guide-to-debugging-github-actions"
published_at: "2025-07-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:48c43e3d261bdea3e5f5cec0fde04cb18b748e03079a817bd028f6156ac0794e"
---

# A practical guide to debugging GitHub Actions

GitHub jobs fail for reasons that make no sense all the time. You haven't changed anything in your code, yet the workflow breaks anyway. It's frustrating because you just want your changes to build and deploy. Debugging action runners can be a headache, but here's how to start untangling what's going wrong.


## GitHub Actions failures


GitHub and GitHub actions are a huge part of so many engineers' workflow. It’s something that people rely upon heavily in their day to day work life. We want to not have to think about how a job is getting run for the most part and just see everything go green so changes can be merged and deployed.


So when there are issues, it’s very disruptive to teams.


### The team impact


Not only can issues with actions and jobs just generally be frustrating, but they can be pretty[costly to your team](https://depot.dev/blog/self-hosting-github-actions) . If it’s an error that doesn’t seem relevant to the change, a lot of people’s first reaction can be to re-run the failed jobs. For flaky issues, this may fix this particular instance of a problem, but it can also mean waiting a long time just for another failure to occur.


These failures, especially if a test suite is running on the job, may cause people to stop trusting that particular job / check. This may be a problem down the line if this failure is at some point identifying a real issue. Additionally, these failures might be occurring when a change needs to get in quickly. If you need to put out a quick fix for something and a job failure is causing you to be unable to deploy the change, this is going to be a big problem especially if this is during an incident or some other high stakes circumstance.


## Debugging GitHub Actions


In many cases these jobs are running a test suite, so sometimes the problem is actually the change being made. Other jobs may end up with build failures which can also possibly be the fault of the change being made. However, what about the case where the problem isn’t actually the code change? The issue may be that the runner your job is executing on is running out of memory or utilizing all of its CPU. There is also the case where your jobs aren’t failing but they are taking longer to run but it’s not clear as to why.


### Utilizing GitHub Logs


In the GitHub UI, you have the ability to look through the job logs to see what is going on with your job. In the case that there is a really explicit error, this UI does a good job at scrolling you to and highlighting what the error that caused the job to fail. This is often useful for identifying whether the issue is related to the code change.


If it’s not clearly related to the code change, it can be used as a starting off point to investigate what the actual issue is. Sometimes the error that causes the job to fail is not as obvious and requires more investigation to determine the problem.


#### Log point in time resource usage


When GitHub Actions jobs fail unexpectedly, memory exhaustion is often the culprit, but the symptoms aren't always obvious in the logs. Rather than guessing, you can add simple monitoring steps to your workflows that capture resource usage before and after critical operations. For memory diagnostics, adding` free -h` and` df -h` commands provides immediate visibility into RAM and disk utilization at key points in your build process. If CPU bottlenecks are suspected, logging system information with` nproc` ,` lscpu` , and` uptime` reveals processor constraints and load averages. Wrapping expensive operations with the` time` command shows exactly where delays occur.


```text
-   name  :   Check memory usage
run  :   |
echo "Memory usage before:"
free -h
df -h


# Your actual build/test steps here


echo "Memory usage after:"
free -h


-   name  :   Monitor CPU usage
run  :   |
echo "CPU info:"
nproc
lscpu
echo "Load average:"
uptime
# Time your critical steps
time make build
```


This approach requires no external dependencies or API keys, just a few extra workflow steps that output directly to your job logs, making it perfect for quick debugging sessions or permanent monitoring of resource-intensive builds. The downside of this approach is that you now have to spend time searching through your logs to try and find where these values were outputted and also keep track of them if there are multiple of these outputs.


### Utilizing an observability tool


Looking at logs is simple enough if you have very few locations you’re logging this data. You may want to collect this data more than just a single time which will require searching through GitHub logs to find all the instances of these values. While identifying the failure-inducing error in GitHub's logs works rather well, viewing other more mundane logs can be tedious and slow. The logs for each non-errored step begin in a collapsed view and don't return in search results until you uncollapse the step and then alter the filter. Typically to speed up this search, I utilize viewing the raw logs so I can more quickly search through them to find the information I seek, but this does forgo a lot of nice formatting that makes the logs easier to parse. A more practical alternative to trudging through these logs would be to send this usage data as metrics to an external observability platform.


#### Report point in time resource usage to an external observability tool


If you want to get these same point in time metrics but have them viewable beyond just searching through your GitHub logs, you could opt to collect and send these metrics to an external observability tool like DataDog for managing and viewing usage metrics you collect. For GitHub-hosted runners, you can implement point-in-time monitoring by sending custom metrics to observability platforms like DataDog during workflow execution. This involves capturing system metrics (memory usage, CPU load, disk utilization) at key moments such as before builds, after tests, or continuously during long-running processes and posting them via HTTP API.


Point-in-time metrics can be effective enough for identifying resource bottlenecks, tracking performance trends over time, and correlating system state with job failures. The implementation is straightforward: collect metrics using standard Unix commands, format them as JSON, and send via curl to DataDog's series endpoint with appropriate tags for filtering and analysis.


Here's a simple example that captures and sends memory utilization during a workflow:


```text
-   name  :   Send metrics to DataDog
run  :   |
# Simple curl commands
MEM_USED=$(free | grep Mem | awk '{print ($3/$2) * 100.0}')
curl -X POST "https://api.datadoghq.com/api/v1/series" \
-H "DD-API-KEY: ${{ secrets.DD_API_KEY }}" \
-H "Content-Type: application/json" \
-d '{
"series": [{
"metric": "github.actions.memory.percent",
"points": [[$(date +%s), $MEM_USED]],
"tags": ["repo:${{ github.repository }}"]
}]
}'
```


#### Agent with a self hosted runner for continuous usage metrics


If you're using GitHub-hosted runners, you're limited to point-in-time metrics collection during job execution. However, self-hosted runners offer the opportunity for more continuous monitoring by installing observability agents directly on the runner infrastructure. For example with a DataDog agent running consistently on your self-hosted runners, you get baseline system metrics including CPU, memory, disk, network collected every 30-60 seconds. This provides much richer insights into runner performance and its resource utilization.


Install the agent on your runner:


```text
DD_API_KEY  =  your_api_key   bash   -c   "$(  curl   -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"
```


Add a step to your workflow to tag the metrics emitted from the agent:


```text
-   name  :   Tag DataDog metrics for this job
run  :   |
# Add GitHub-specific tags to the runner's metrics
curl -X POST "https://api.datadoghq.com/api/v1/tags/hosts/$HOSTNAME" \
-H "Content-Type: application/json" \
-H "DD-API-KEY: ${{ secrets.DD_API_KEY }}" \
-d '{
"tags": [
"github_job:${{ github.job }}",
"github_repo:${{ github.repository }}",
"github_run_id:${{ github.run_id }}"
]
}'
```


This is going to provide a lot of data that can be used to help identify your issues but is likely a more heavy handed approach than many would take. This requires you both to manage your own self-hosted runners as well as utilize an external observability tool. If you’re not already managing self hosted runners or utilizing an observability tool, this is likely a far too heavy and expensive approach for debugging this.


## Our approach: built-in observability


Similarly to the last option, we use an agent for collecting job data. Our setup involves deploying an OpenTelemetry (OTEL) collector directly on[Depot action runners](https://depot.dev/products/github-actions) that continuously captures observability data while the job executes. We store the following to be graphed and viewed in the Depot app:


- Memory utilization metrics with sub-second granularity throughout the job lifecycle
- CPU utilization metrics with detailed timing information
- Complete job logs with step associations
- Process-level resource utilization for granular visibility
- Out of memory (OOM) events with detailed timing information


The goal of this approach is to collect the information that commonly provides insights around what the problem was without requiring users to make changes to their workflows or to adopt an observability or data visualization tool to access this data which is sometimes prohibitively expensive. Out of memory indicators provide very clear feedback that the problem is memory constraints. The peak resource utilization for CPU and memory values provide insights into whether or not one of these is a constraint on the system, ultimately being the culprit for the job failure or slowdown. Process level metrics for a particular step allow you to more deeply identify which process may be hogging resources and causing problems. Your job logs and their timings can be used to further identify where other slow downs occurred while also providing an easier interface for searching through and identifying relevant events.


## Taking action


Now that you have more data to be able to identify what is actually causing the failures or slow downs, you can take targeted actions to alleviate the issue.


### High memory usage


#### Immediate solutions


- Upgrade runner types: Use a larger runner with more memory. This has the tradeoff of being more expensive.
- Implement build caching: Cache dependencies and build artifacts to reduce memory overhead during subsequent runs.
- Set memory limits: Prevent memory runaway by only allowing a limited amount of memory for a particular process.


#### Optimize resource-heavy operations


- Docker builds: Use multi-stage builds and layer caching to reduce memory requirements.
- Large jobs: Break monolithic jobs into smaller sequential jobs that consume less memory.


#### Identify memory leaks


If you see sudden memory spikes between "before" and "after" measurements, investigate:


- Test suites that don't properly clean up resources
- Build processes that accumulate temporary files
- Applications that don't release memory between operations


### CPU bottlenecks


#### Immediate solutions


- Upgrade runner types: Use a larger runner with more CPU cores. This has the tradeoff of being more expensive.
- Limit parallel operations: Avoid overwhelming the runner's CPU cores.


#### Split and distribute work


- Job matrices: Use matrix strategies to distribute CPU-intensive work across multiple runners simultaneously.
- Sequential jobs: Chain dependent jobs instead of running everything in parallel to reduce peak CPU load.


## Conclusion


When job failures happen, they can be frustrating and time-consuming. Debugging them means understanding the context so you can separate the real cause from the noise. The key to effective GitHub Actions debugging is picking the right strategy for your team’s scale and needs.


Start simple with logging for one-off issues, and as your CI/CD usage grows, consider adding an external observability tool.


If you're already using Depot as your job runner, you get[built-in metrics in the job details](https://depot.dev/blog/introducing-github-job-details-observability-for-your-cicd-pipeline) to kickstart this investigation. The next time a job fails unexpectedly, you'll be equipped to pinpoint whether it's a code issue or a resource constraint and fix it faster.


## FAQ


Why do GitHub Actions jobs fail even when my code hasn't changed?


Because many failures are caused by the environment, not your code. Common causes include resource limits (memory, CPU, disk), flaky network dependencies, outdated cache layers, or GitHub-hosted runner instability.


How can I tell if the failure is a real bug or just a flaky runner?


Start by checking the logs for consistent errors. If rerunning the job produces different results or passes without changes, it's likely a flaky issue caused by resource constraints, timing, or network hiccups.


What's the simplest way to debug a flaky job?


Add lightweight resource logging (free -h, df -h, uptime) around critical steps and compare runs. This shows if memory or CPU is maxing out. For deeper analysis, use an observability tool or a runner with built-in metrics (like Depot).


Can I SSH into a GitHub Actions runner to debug a failed job?


You can open an SSH session while a job is still running by adding an interactive debugging action such as[action-tmate](https://github.com/mxschmitt/action-tmate) to the workflow. You cannot reconnect to a GitHub-hosted runner after the job has finished because the runner is ephemeral. Depot GitHub Actions runners are compatible with action-tmate; follow the[SSH troubleshooting guide](https://depot.dev/docs/github-actions/troubleshooting#ssh-access-to-depot-github-actions-runners) to start a session and inspect the live environment.


How do I check memory and CPU usage during a GitHub Actions job?


Add monitoring steps to your workflow that capture resource usage before and after critical operations. For memory diagnostics, use` free -h` and` df -h` commands. For CPU bottlenecks, log system information with` nproc` ,` lscpu` , and` uptime` . Wrapping expensive operations with the` time` command shows exactly where delays occur. This approach requires no external dependencies, just a few extra workflow steps that output directly to your job logs.


What are the tradeoffs between logging resource usage to GitHub logs versus sending metrics to an observability tool like DataDog?


Logging to GitHub logs is simple and requires no external dependencies or API keys, but you have to spend time searching through logs to find outputted values and keep track of them across multiple runs. Sending metrics to an observability tool like DataDog provides easier viewing and management of usage data over time, with the ability to track performance trends and correlate system state with job failures. The tradeoff is added complexity and cost of adopting an external tool.


## Related posts


- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Claude Code sessions in Depot](https://depot.dev/blog/now-available-claude-code-sessions-in-depot)
- [Introducing GitHub Job Details: Observability for your CI/CD pipeline](https://depot.dev/blog/introducing-github-job-details-observability-for-your-cicd-pipeline)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Iris Scholten


Staff Engineer at Depot
