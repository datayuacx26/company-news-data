---
schema_version: "1.0.0"
document_id: "86f792ebaaa4133028054876321579eda3d07e2b21b5cb94774deb7ed6525de2"
company_key: "yc-cloudcruise"
company: "CloudCruise"
source_id: "yc-cloudcruise-news-import-0286802dbbee"
canonical_url: "https://cloudcruise.com/blog/prompt-engineering-at-cloudcruise"
published_at: "2025-06-11T00:00:00+00:00"
first_seen_at: "2026-07-23T05:51:05.060428+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:6312188f38a3a97be963ac0580c8a95604e561972a7c3b36d289d0b7d8be7e88"
---

# Prompt Engineering at CloudCruise

#### Our Approach to Prompting


At CloudCruise, we are building the developer platform for browser agents. This year, we saw our platform’s usage increase by 1,000%, and we are now serving more than 100,000 browser automations per week!


While the increase in traffic was very welcome, this revealed a new challenge: as we started to scale, so did our errors. Our browser agents repeatedly encountered random survey popups, website UI layouts were constantly changing, and incorrect inputs were given to our agents, all of which disrupted agent performance. Manually fixing every agentic workflow was no longer sustainable.


A few months ago, we started working on a[maintenance agent](https://www.cloudcruise.com/blog/genesis) - an agent that is designed to diagnose and recover from our browser workflow errors. We wanted users to be able to kick off browser automations and trust the maintenance agent to handle predictable errors, only looping in humans when necessary.


What optimistically started as a two-week sprint to productionalize this agent turned into a months-long process iterating on prompts, re-designing our agentic architecture, and refactoring our entire backend (more on that in later blog). We faced many challenges with prompting, both with the actual prompts themselves and the orchestration of all these prompts, and wanted to share our learnings with you.


#### Our First Attempt


Let’s start with our very first prompt:


```text
You   are   an   expert    in    error   analysis  .


You    are   given   an   error   message  ,    a   screenshot   of   a   website  ,    and   other   relevant   information  .
Your    task   is   to   analyze   the   error   and   provide   a   detailed   analysis   of   the   error  .  The    error   message   given   to   you   might   be   incorrect  .  You    need   to   determine   if    the   error   message   is   correct   or   not  .
You    will   be   given   a   list   of   possible   error   categories  .  Choose    the   most   likely   error   category   or   create   a   new    one   if    it   doesn  't exist.


Here   is   the   list   of   possible   error   categories :


{  error_categories  }


Here   is   the   error   message :


{  error_message  }


Here   is   the   other   relevant   information :


{  other_relevant_information  }


Here   is   the   output   json   data   model :


{  output_data_model  }
```


```text
You   are   an   expert    in    error   analysis  .


Here   is   the   list   of   possible   error   categories :


{  error_categories  }


Here   is   the   error   message :


{  error_message  }


Here   is   the   other   relevant   information :


{  other_relevant_information  }


Here   is   the   output   json   data   model :


{  output_data_model  }
```


```text
You   are   an   expert    in    error   analysis  .


Here   is   the   list   of   possible   error   categories :


{  error_categories  }


Here   is   the   error   message :


{  error_message  }


Here   is   the   other   relevant   information :


{  other_relevant_information  }


Here   is   the   output   json   data   model :


{  output_data_model  }
```


```text
You   are   an   expert    in    error   analysis  .


Here   is   the   list   of   possible   error   categories :


{  error_categories  }


Here   is   the   error   message :


{  error_message  }


Here   is   the   other   relevant   information :


{  other_relevant_information  }


Here   is   the   output   json   data   model :


{  output_data_model  }
```


While this prompt is relatively straightforward and clear, two things clearly caused it struggle:


1.


**There was not enough** *right* **context in the inputs.** Typically, we see that either the LLM underperforms or the instructions to the LLM are inadequate. However, it is almost always the latter. And really, how we think about it is: “If I were given the same inputs and instructions as the LLM, could I reach the desired conclusion?” If the answer is no, then you have a prompting or input issue. The internet in general has a lot of edge cases: random popups, websites go down, and buttons move around. So even today, we are constantly working on the quality of our inputs and what those inputs are.


2.


**The agent was trying to do too much all at once.** It had to:


1.


Trace back the history of actions (which could be hundreds of steps)


2.


Gather information about the point of error (e.g.. the dom, xpath selector, system error, etc.)


3.


Categorize the error


4.


Determine where in the workflow we would have to restart


As you can imagine, there was room for improvement.


Quite literally, we’ve spent hundreds of hours investing time into prompting, evaluations, and improving data quality. Besides “making the prompt more clear and precise,” here’s everything we did to iterate on the initial prompt. As a disclaimer, we decided to focus on accuracy, prioritizing its importance above latency and cost.


#### Breaking Prompts into Smaller Subtasks


While pre-trained LLMs are inherently powerful in tackling and automating tasks, it is still very challenging for them to accurately execute complex workflows based on a single prompt. Breaking up the workflow into smaller tasks (akin to modularizing code) can greatly increase accuracy, even at the cost of latency. In order to address overwhelming the agent, we created separate prompts for each subtask and strung them together:


1.


Reasoning if the initial error thrown by our system was correct


2.


Analyzing screenshots taken during execution


3.


Prompts for video analysis


4.


Finding the root cause analysis


5.


Categorizing the root cause


6.


Pinpointing where to recover in the workflow


Let’s focus on one of these smaller prompts:


```text
// We assign roles to the LLMs, framing the rest of the prompt in a specific context
#  ROLE
You   are   ** Root  - Cause   Bot  ** ,    a   senior   QA   analyst   for    a   browser  - automation   agent  .


// For high level instructions, we format prompts using Markdown
#  TASK


1.   ** Pinpoint   the   first   true    failure  ** :
•   Scan   the   entire   timeline   ** once  **  —   oldest   →   newest   —   and   pick   the   earliest   action   whose   failure   plausibly   propagates   forward   and   makes   later   steps   impossible  .
•    Consider   both   the   full   chain   of    `action_results`   ** and  **  each   action’s   * Description  *.
-  Treat   the   Description    as   the  action’s   own   “expectations   sheet” :    it   outlines   what   the   node   aims   to   see   or   accomplish   and   lists   any   known   pre  - conditions   or   edge   cases  .  When    the   observed   UI   diverges   from   these   stated   expectations  ,    flag   that   mismatch    as   a  likely   failure   signal  .
•    Prefer   a   step   whose   failure   statement   directly   references   a   blocker   still   visible    (  or   still    consequential )    in    later   screenshots  .
•    Output   that   action’s    `action_id`    as   ** `failure_node_id`  **.
•    If   evidence   is   insufficient  ,    set    `failure_node_id = null`   ** and  **  `confident = false`  .
…
// For more specific conditional logic, we leverage XML
< logic_block   id  = "xpath_gate"  >
<  condition    name  = "xpath_only"  >
xpath_analysis?.xpath_correct === false
&& (newest_node == failing_node)
&& (all_prior_nodes.mechanical_executed == true OR diagnostic)
&& (overlay_present == false)
</  condition  >


< instruction  >
If   < xpath_only  >  is   TRUE   →   XPath  / selector   may   be   root   cause  .
Otherwise    →   never   blame   XPath  ;    prefer   popup  ,    UI  - missing  ,    timing  ,    or   data   issues  .
< /instruction>
< /logic_block>
```


```text
#  ROLE


// For high level instructions, we format prompts using Markdown
#  TASK


1.   ** Pinpoint   the   first   true    failure  ** :
…
// For more specific conditional logic, we leverage XML
< logic_block   id  = "xpath_gate"  >
<  condition    name  = "xpath_only"  >
xpath_analysis?.xpath_correct === false
&& (newest_node == failing_node)
&& (all_prior_nodes.mechanical_executed == true OR diagnostic)
&& (overlay_present == false)
</  condition  >


< instruction  >
< /instruction>
< /logic_block>
```


```text
#  ROLE


// For high level instructions, we format prompts using Markdown
#  TASK


1.   ** Pinpoint   the   first   true    failure  ** :
…
// For more specific conditional logic, we leverage XML
< logic_block   id  = "xpath_gate"  >
<  condition    name  = "xpath_only"  >
xpath_analysis?.xpath_correct === false
&& (newest_node == failing_node)
&& (all_prior_nodes.mechanical_executed == true OR diagnostic)
&& (overlay_present == false)
</  condition  >


< instruction  >
< /instruction>
< /logic_block>
```


```text
#  ROLE


// For high level instructions, we format prompts using Markdown
#  TASK


1.   ** Pinpoint   the   first   true    failure  ** :
…
// For more specific conditional logic, we leverage XML
< logic_block   id  = "xpath_gate"  >
<  condition    name  = "xpath_only"  >
xpath_analysis?.xpath_correct === false
&& (newest_node == failing_node)
&& (all_prior_nodes.mechanical_executed == true OR diagnostic)
&& (overlay_present == false)
</  condition  >


< instruction  >
< /instruction>
< /logic_block>
```


For most of our prompts, we strive to follow industry best practices: assigning a role to the LLM, leveraging XML (especially for conditional logic) and using markdown to delineate each section for the prompt clearly. The biggest improvement was using o3 for the root cause analysis step - and OpenAI cutting the price by 80%! While the other steps like analyzing screenshots or categorizing the error were specific tasks that GPT 4.1 performed well on, determining the root cause using multi-modal inputs was certainly a task for o3.


To iterate on the prompts themselves, I created a project in ChatGPT, and uploaded[OpenAI’s Prompting Guide](https://cookbook.openai.com/examples/gpt4-1_prompting_guide) as context, and meta-prompt heavily within that project.


#### Move Deterministic Logic out of the Prompt


Another unlock was moving as much of the LLM’s responsibilities into code as we could. This involved two main tasks:


1.


**Evaluating if the XPath Selector was incorrect.** Not only was this expensive as large DOMs could burn through your tokens, but there are XPath engines that you could leverage to evaluate XPath expressions on the fly.


2.


**Determining the Failure Point in the Browser Agent’s Actions.** We originally used tool calls to repeatedly go further back in history until the maintenance agent felt it had found the failure point. We found that the tool calling was unreliable and the latency increase was just too much for this to be productionalized. So, we developed a heuristic based on time or action slices (e.g., every 5 seconds or every 5 actions) to determine whether the agent had failed at any point, by comparing the state before and after each action.


#### Using Azure OpenAI vs OpenAI


We initially migrated to Azure to make use of our remaining credits. However, to our surprise, the switch also cut our aggregate latency in half. While onboarding came with some challenges, the performance gains made it worthwhile. Our team has experimented with various combinations of models from Anthropic, Google, and OpenAI, and we encourage others to do the same. For instance, Claude excelled at managing agentic tool usage, Gemini handled long context windows particularly well, and GPT-4.1 consistently performed best on narrowly scoped tasks.


#### Using an LLM Observability Platform


Because we have a multi-step and multi-modal agent, we decided to design the inputs and outputs in a way that we can easily evaluate. Rather than passing a large set of inputs into the maintenance agent and testing only the final output, we structured intermediate outputs at each step and built evaluation datasets for every major stage in the process.


###### For example:


1. Browser Agent History Evaluation Dataset


2. Root Cause Evaluation Dataset


3. Error Categorization Dataset


We leverage a platform called[Langfuse](https://langfuse.com/) which hosts and versions not only our prompts, but makes it extremely easy to run evaluations for our LLMs (would highly recommend).


There’s still a ton of work left, but the maintenance agent is already live - recovering production runs by fixing broken XPaths, closing pop-ups, and rolling the app back to the exact point of failure before retrying!


If you have highly repetitive workflows that you want to automate, feel free to reach out to us (we’ve done a ton of work automating prior authorizations, claim statusing and more).
