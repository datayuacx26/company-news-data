---
schema_version: "1.0.0"
document_id: "0c736ddea871a0ce2b3de32c6e0f66bbb6c434b89ef145b7325adf9ddd0f5945"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/phoenix-heex-and-resource-exhaustion-56d69efd13f9"
published_at: "2024-01-04T16:09:42+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:c5f6ed54ca6f04b1d18774a933ca5b609a17ddf06714604e7c42befbf0d1c167"
---

# Phoenix HEEX and resource exhaustion

Elixir


Bugs


Telemetry


Phoenix


# Phoenix HEEX and resource exhaustion


[Christian Koch](https://medium.com/@ckochx?source=post_page---byline--56d69efd13f9---------------------------------------)


9 min read


·


Dec 8, 2023


--


## Bottom line up front (BLUF)


Don’t pass the whole socket assigns to a HEEx template. Set a per-template assigns for every HEEx or` sigil_H` /` ~H""` template.


Related: don’t store your template in the assigns.


Related-related: don’t add the assigns into the assigns.


## Background


During a regular production deploy one subset of app pods began restarting regularly. Not continually, but frequently enough to suggest the presence of a regression in the release.


The deploy was cancelled and “yesterday’s” release was left in place while we started investigating.


## Graphs


The resolution that we have available for these graphs softens some of pain.


Press enter or click to view image in full size


Per pod CPU usage


CPU shows some spikiness which doesn’t seem great.


Press enter or click to view image in full size


Per pod memory usage


Memory usage doesn’t look terrible but that spike hits the hypervisor limit and the pod is OOMed.


Press enter or click to view image in full size


Per pod restarts


Restarts are much higher than we would like. (But this is often not a user facing issue in elixir apps.)


Press enter or click to view image in full size


Total latency


The latency spike directly correlates with the user pain and the manifestation of the bug issue.


## Investigations begin


The first thing we have to determine is why are the pods even failing? The answer is they are using all their allotted memory and are getting terminated by the control plane. This manifests as an Out Of Memory failure, also called an OOM-kill.


OOM is a tricky error state, since the termination comes from outside the application, debug artifacts like crash dumps are never generated nor is there a running instance from which to gather additional diagnostic or forensic information.


Initial investigation reveals no useful information and the decision is made to deploy a canary deployment. This will provide some real production traffic levels and usages to the pod in a hope to trigger the OOMs and gather more information about what is going wrong.


We run` :observer_cli.start()` on the canary to try to find processes that are consuming a lot of resources, specifically memory, since that is the factor that is causing the control plane to restart the pod. The resolution of this is not ideal and we can only see processes fly by the status screen rapidly. The closest we get to a clue are LiveView processes that have more than 1GB of memory. Unfortunately we only see` LiveViewModule.han` in the window which is likely one of the` handle_...` callbacks but which one?


There is a parallel effort to diff the prod release. This doesn’t yield any obvious red flags but combined with the` :observer` observations we can try to replicate the failures on the the suspect` LiveView` page.


*Eventually* we are able to replicate the spiking memory usage locally, identify, and revert the offending PR. This gets us to a state where we can deploy again, which we immediately don’t do as it is too late in the day.


## Investigations continue


Rested and partially recuperated, we return to the original PR in an effort to determine why this seemingly innocuous heex conversion causes the pod to run out of memory. Running` :observer` locally gives us the` :observer` GUI and more readily available info about what is happening to memory. We also don’t have any hypervisors that will restart the app locally so hopefully we can get more info about the failure state.


After triggering many local failures and seeing the memory usage spike to 10 and sometimes 100+GB, we resort to analyzing the size of the` assigns` to try to find any red flags.


```text
assigns   |> Enum.map(fn {key, value} -> {key, :erts_debug.flat_size(value)} end)  |> Enum.sort_by(fn {_key, value} -> value end)
```


Three things quickly jump out as bright red flags.


1. ` assigns` has an` assigns` key.
2. The size of the assigns grows with the lifespan of the LiveView.
3. There are two other keys (` thing1_notification` and` thing2_notification` ) that grow ~2X and ~4X (respectively) to the size of the assigns.


The values stored in the fastest growing keys are both changed in the regressing PR. This finally provides a confirming piece of evidence.


## Analysis


*What the hell is happening here?*


### PROBLEM 0- (not really a problem unless you make it one)


***The framework:***


HEEx (HTML + EEx) or` sigil_H`[https://github.com/phoenixframework/phoenix_live_view/blob/v0.20.1/lib/phoenix_component.ex#L785](https://github.com/phoenixframework/phoenix_live_view/blob/v0.20.1/lib/phoenix_component.ex#L785) requires an attribute named` assigns` and it must be a map. This value is passed into` EEx.compile_string/2`


` EEx.compile_string/2` is greedy and just compiles all the key-value pairs into your template. ([See docs](https://hexdocs.pm/eex/EEx.html#compile_string/2) )


LiveView is then a bit clever and only sends the values that are needed to render your template across the wire. But these key-values are still stored in the socket state, which means they get evaluated to calculate a diff for any template updates.


### PROBLEM 1- Assigns in assigns:


An async` Graph Workflow` ([named Pacer,](https://hexdocs.pm/pacer/Pacer.Workflow.html) courtesy of my Cars.com engineering colleagues) needs` assigns` to calculate many things. But using and setting do not both need to happen.


A` Pacer Workflow field(:assigns)` sets the input assigns into the Pacer output. The output is merged back to the socket` assigns` so now there is an` assigns` stored in the` assigns` .


### PROBLEM 2- Assigns in assigns in the template:


If you combine Problem 0 and problem 1 you can see that our template could be a problem. Before and after the HEEx conversion the notification templates were using the naive (socket)` assigns` , which on this LiveView can be kind of large. All the` assigns` are getting compiled into the template. (This is also happening for the` sigil_E` template, AFAICT).


The difference with HEEx is that now the assigns are diffed to calculate changes that need to get sent to the client, thus making the` assigns` a LiveView and runtime concern. Both of the notification functions noted above are using the full socket assigns and then getting set into the` assigns` courtesy of the Pacer Workflow.


### PROBLEM 3- The template with assigns in assigns is in the assigns:


The output from the first notification function


` thing1_notification/1` embeds the` assigns` in the template and then the template is added to the` assigns`


### PROBLEM 4- The template with assigns with the template with assigns in assigns is in the assigns:


The output from the second notification function:


## Get Christian Koch’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


` thing2_notification/1` embeds the` assigns` (now containing the` thing1_notification` template) in the template and is this template is also added to the` assigns` .


## The KABOOM 💣


These nested` assigns` create a geometric expansion. (I am pretty sure it’s not exponential, but it hardly matters as it rapidly grows beyond the system RAM). The expansion is nevertheless bounded and will (eventually) return, if you are patient enough to wait for your system to calculate 10s of gigabytes of diffs. The` assigns` expands more as the LiveView process lives longer and serves more requests over the same socket. This makes some sense as the assigns is always updating itself back into the` assigns.`


As noted in the investigation, the memory pressure from this deeply nested, deeply duplicated` assigns` overwhelms the system RAM, either causing an OOM or running out of system memory and into swap, which is so slow the crash would be preferable.


mistakes were made


## What we did wrong


### Wrong 0:


Both of the notification functions:


` thing1_notification/1` and` thing2_notification/1`


are using the full socket` assigns` , which is part of the reason they grow so large so fast. They each only need one or two values from the assigns.


Instead of passing in the full` assigns` , it would be much more performant to define a template assigns that sets only the keys that are needed for the template. A fix with only this change is sufficient to address the memory spiking.


### Wrong 1:


The` LiveView` notification functions return a template and set those templates into the` assigns` . This is far from optimal.


A better design would be to set some values into the` assigns` and use those values to populate a template when needed.


### Wrong 2:


` assigns` in` assigns` .


I hope this doesn’t need too much explanation. Having the` assigns` in the` assigns` is the virus on the bacterium on the mite on the bug that tips this whole infection into a terminal condition.


## The fix(es):


### An assigns of their own:


The two notification functions in the` LiveView` get targeted assigns that only have the values they need.


### Don’t assign the whole template:


This will be a follow up PR to remove the template from the` assigns` and refactor the search expansion notification(s) into an on-demand template.


### Don’t assign assigns in assigns:


Make assigns virtual` field(:assigns, virtual?: true)` . Pacer supports evaluation-only fields that are only used for evaluation but not set in the output of the Pacer workflow.


## Analysis:


Like all the really interesting failures this failure had several origins and each alone would be insufficient to cause full system instability. It took a confluence of changes over time to tip this system into a meta-unstable state where it was susceptible to the added memory pressure from merely invoking the Search Expansion message.


The speed of memory growth also hampered our efforts at analysis as the system was restarted from outside, by the control plane, before it could emit any telemetry that might have helped us to better diagnose the issues.


The dangerous memory usage only became an issue on longer lived processes, as the` assigns` keeps updating itself and the templates. This helps explain why this issued caused intermittent failures. Any navigation that caused a new socket would reset the` assigns` back to a normal level. There was an added “safety” margin as only certain live navigation paths would invoke the code that called for` thing1_notification/1` and` thing2_notification/1.` The HEEx conversion also tells us why this was not an issue under EEx; the EEx templates never needed to get reëvaluated.


## Looking forward:


### A canary:


Creating a canary deployment was crucial to our debugging efforts. While it’s not ideal to subject our users to a potentially unstable experience, sometimes it’s the only way to discover an idiosyncratic error state. We are working with the cloud team to ensure we have documented and operationalized this process. Having the ability to deploy a canary is an important debugging tool that we hopefully use infrequently but will likely need again at some point. We are working to operationalize this technique so we have a better means of testing faulty deploy artifacts in production.


### Circumspect assigns:


Store as little as necessary in the assigns. It’s very easy for single use values to slip into assigns. See if there is a way to solve to problem without adding anything to the assigns.


Don’t put the` assigns` in the` assigns` .


### Heex conversions:


When converting EExor` sigil_E` to HEEx, be sure to create a purpose built` assigns` . Passing the full socket assigns to a template is a strong anti-pattern.


Note there are several ways to do this unexpectedly. This is a copy of a note from José Valim:


> Question: “would it be better to pass an assigns in the header with a default empty map so that we don’t pass the whole assigns every time and we don’t have to edit the function calls themselves?”
>
>
> Hi team! 👋
> Indeed, you don’t want to pass all existing assigns. Instead, you must build an assign from the current parameters. For example, if you have this function today:


```text
def render_something(foo, bar) do   ~L"""   <%= foo %> <%= bar %>   """  end
```


> Then you should rewrite it to this:


```text
def render_something(foo, bar) do   assigns = %{foo: foo, bar: bar}  ~H"""   <%= @foo %> <%= @bar %>   """  end
```


> In the above, you will still call it as <%= render_something([@foo](http://twitter.com/foo) ,[@bar](http://twitter.com/bar) ) %>.
>
>
> Then, when you want to call this function as a function component, you change only the first two lines:


```text
def render_something(assigns) do   ~H"""   <%= @foo %> <%= @bar %>   """  end
```


> And call it as:


```text
<.render_something foo={@foo} bar={@bar} />
```


> Alternatively, you can rewrite the functions to the final function-component aware version already:


```text
def render_something(assigns) do   ~H"""   <%= @foo %> <%= @bar %>   """  end
```


> But then you must call it as this in non-HEEx templates:


```text
<%= render_something(%{foo: @foo, bar: @bar}) %>
```


> As you can see, the goal is to convert the existing parameters into a map. This means callers from LEEx template are a bit uglier now but it means it should just work if you use it as a function component. Perhaps that’s the best route.


Cars Commerce is actively searching for new talent to help grow the tech team. If you or anyone you know is interested in working on similar projects at cars commerce head over to our[careers](https://www.cars.com/careers/#job-listings) page and browse the current opportunities!
