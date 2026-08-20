---
schema_version: "1.0.0"
document_id: "c92ef7ec777bb360b6b8cc3ce69ca978e1b986662c2508c2ac33a7bcb2d0ba4c"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/ai-for-bi-write-definitions-as-code"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:47b4a01642ba771d577bb191c2a79263cf060b7d859200eb578105c503eba085"
---

# AI for BI: write definitions as code

Documentation provides context for humans, but it should also be a contract for machines to execute.


## Introduction


In the[first article](https://www.photoroom.com/inside-photoroom/why-ai-for-bi-fails) , we explained why AI fails when meaning is ambiguous. (Every time AI has to interpret the meaning of question, it is as if it had to choose between several branches of a decision tree. The more ambiguity, the more branches, the less likely it is to pick the right path.)


The fix starts with definitions, but just documenting is not enough, because most metric docs are for humans. (Revenue = money we make. Thank you.) Humans fill in gaps with context. Machines can't, and when the column “revenue” is not available as is, they have to make assumptions, guess, and risk providing an answer with a lot of confidence, despite having to guess part of the meaning.


The more precise and unambiguous the definitions, the more deterministic and repeatable the path. In other words, if you manage to narrow down your semantic space, with precise definitions, you make the output of the AI more predictable.


The goal is clear: Definitions must be machine-readable.
But we want to go beyond just good definitions. We want a system, an architecture that guarantees that we have unambiguous definitions.


In this article, we cover what it looks like in practice.
And if you skip to the second part of the article, I even made a 10-step checklist to follow to get there.


### What "machine-readable" actually means


A definition must be precise enough so that there is no possible ambiguity when reading it.
→ concepts that can be used with many different meanings are not tolerated.
One of the best practice that we have enforced for this is to add prefix whenever the word used could be a little too generic.


For instance:` job_name` could very well be the name of one of our dbt jobs. It could also be the title associated to an open position in Ashby. If we see it, we will make sure to disambiguate it by naming our columns` dbt_job_name` and` ashby_job_name` .


### Our approach: 3 pillars


We have come up with a simple solution to create clarity in our definitions, and in our code. The principle is simple


-


Central definitions live in a **dictionary** (what things mean)


-


We have a **repository** listing every definition, and where they are called (where things live)


-


We have a rule that forces a 1:1 relationship between one concept and one definition.


An intentional semantic design, with strong enforcement, is the best thing you can do to increase AI reliability for BI questions. To enforce ours, we rely on dbt[docs blocks](https://docs.getdbt.com/docs/build/documentation?version=1.10) : a reusable piece of documentation you write once and reference anywhere.


Let’s look at each pillar in detail:


1.


**Each definition is written in 1 place only** :` definitions.md`


We chose to have 1 unique md file for all company definitions, listed alphabetically.


This is very important as:


1.


it enforces the single source of truth: if you need to edit a definition, you edit it in 1 place and it trickles down everywhere the definition is called.


2.


it gives a good observability of all the definitions that we use in our BI


3.


You can easily parse this document and turn it into a table. Data teams love tables.


We currently have 2500+ definitions (coming from ~40 data sources and models on top).


2.


**Every YML of documentation then simply refers to central definitions**


*Note: is it even useful to document a column, when all we do is simply repeat its name? dbt needs it to properly propagate the definition to our database, but you can build a macro to auto-fill this YML using this rule.*


I find this view very comforting. Every definition has a description that is centrally listed. Things are clean and in order. This feels good.


3.


**We ensure that 1 column name has exactly 1 definition.**


We have 2 processes that ensure that 1 column has only 1 definition (1 would be enough, but I prefer belt and suspenders). Call it opinionated data modeling.


1.


Every column of the codebase is listed in datadictionary.yml.
We use[dbt-datadict](https://github.com/TasmanAnalytics/dbt-datadict) , an open-source dbt package developed by Tasman, a boutique analytics consultancy, to generate this file. (Tasman set up the foundation of our data stack at Photoroom). This package is an absolute must if you want to have a complete and clean documentation.


In the file datadictionary.yml, we can see:


-


all the models that call a given column name


-


all the definitions that a column has. We only allow 1 column by definition.


Why this is great: it provides observability on where each notion is being used.


2.


We also have checks in the CI that guarantee that column_name = entry_name in the YML file. This guarantees a 1:1 relationship.
We cover our CI checks in Article 3.


### How to get there in 10 steps.


How to go from the current state of your codebase to the state we are at today?
For the example below, I used the dummy project provided by dbt when initiating a dbt project.


1.


Make sure you have 1 YML file per SQL file (ask your coding assistant to split them if needed)


2.


Install dbt-datadict:` pip install dbt-datadict`
And make sure you also have dbt-labs/codegen in your packages.yml


```text
packages:
- package: dbt-labs/dbt_utils
version: [">=1.1.0", "<2.0.0"]
- package: dbt-labs/codegen
version: [">=0.12.0", "<0.14.0"]


```


then run` dbt deps`


3.


run` datadict generate` in your terminal
For this example, I have removed a few columns from a YML.
As you can see, it picked up that 3 columns were missing, and it added them to the yml.


4.


Then, run` datadict apply`
this creates the` datadictionary.yml` file centralizing all definitions, the models they are called in, and their different descriptions


As you can see, we have 2 kinds of problems


1.


in orange: some columns are missing definitions. It is confusing.


2.


in green: some columns don’t have the same definition. It is also confusing.


5.


Be brave


This is not very fun, but you will be very happy once it is done.


1.


Add definitions everywhere they are missing. Spend time investing in good definitions. Everything that is not crystal clear has to be documented. Ambiguity is technical debt.


2.


for ambiguous definitions (columns sharing the same name, even though they don’t represent the same thing): either merge definitions, or prefix the column to remove ambiguity, and make sure this gets correctly propagated to children models. (ask your code assistant, and check).


6.


Do one more check: make sure you have no more than 1 definition per column (easy to check: do a Cmd + F of description_versions. If you only have 1 definition, you shouldn’t have description_versions at all.)


7.


At this moment in time, there may be a difference between your datadictionary.yml and your YML files that document each model.


8.


It is ok, there is a command that propagates the content of your datadictionary.yml
To propagate “clear definitions”, run the command` datadict apply` .


9.


At this stage, the codebase already looks much cleaner.
1 concept = 1 definition, and this is enough.
**You can stop here if you are tired!**


But I like a clean codebase, and I love centralization. So I do the unnecessary step - that makes life so much nicer for many future use cases - of centralizing the definitions in docs blocks. I HIGHLY recommend you do it too.


For this, I asked Claude:


```text


Using dbt docs blocks, create a definitions.md file that centralizes all the definitions that are written in datadictionary.yml


The entry name of the definition should match EXACTLY the name of the column. For instance


name: customer_id
description: "Unique identifier for a customer."


should become, in datadictionary.yml
name: customer_id
description: '{{ doc("customer_id") }}'


and in definitions.md
{% docs customer_id %}
Unique identifier for a customer.
{% enddocs %}


Make sure all entries in definitions.md are in the alphabetical order


```


*All the definitions are written in definitions.md, inside doc blocks.*
*The datadictionary.yml references the doc blocks.*
*But for the moment, the yml file of documentation of each model still show the full definitions, and do not reference the doc blocks.*


10.


Once, again, we’ll have a difference between datadictionary.yml and our documentation YML, that we’ll solve by running a` datadict apply` .


*The yml file now references the central documentation with doc blocks instead of the full definition.*


### Closing


Definitions have to be more than documentation, they are infrastructure. Treat them like API contracts between your codebase and the AI that will query it.
But good definitions can still drift over time. Once you have done the cleaning part, and you are happy with the state of your codebase, you have to make sure that the quality is maintained and you need processes to validate that nothing decreases this quality over time.


In the next article, I'll show how we use CI checks to prevent ambiguity from reaching production, and maintain this quality of documentation.
