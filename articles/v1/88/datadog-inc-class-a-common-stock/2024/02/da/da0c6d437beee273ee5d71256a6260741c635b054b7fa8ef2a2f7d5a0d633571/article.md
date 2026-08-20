---
schema_version: "1.0.0"
document_id: "da0c6d437beee273ee5d71256a6260741c635b054b7fa8ef2a2f7d5a0d633571"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/how-we-use-vale-to-improve-our-documentation-editing-process/"
published_at: "2024-02-14T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:2906e1a3f3de34da268d0af4b1c95972a2a84cfc1f69fd8bde129db12b0dd9cf"
---

# How we use Vale to improve our documentation editing process

Austin Lai


Heston Hoffman


Kari Halsted


*“I notice that you use plain, simple language, short words and brief sentences. That is the way to write English—it is the modern way and the best way.”* - **Mark Twain**


Just as development teams adopt linters such as Prettier in their workflows to flag errors and style issues in their source code, documentation teams use linters such as Vale to enforce style guidelines and maintain a standard for clear and concise prose.


Crafting prose can be tricky—for example, consider this update from an external contributor: “ *Command options can also be set via environment variables* (` DATADOG_API_KEY="...", DATADOG_WORKER_CONCURRENCY="15", DATADOG_DNS_USE_HOST="true"` ). *For options that accept multiple arguments, JSON string array notation should be used* (` DATADOG_TESTS_DNS_SERVER='\["8.8.8.8", "1.1.1.1"\]'` )”.


A technical writer may rephrase this as: “ *You can customize the launch command with environment variables such as*` DATADOG_API_KEY="...", DATADOG_WORKER_CONCURRENCY="15", DATADOG_DNS_USE_HOST="true"` *in the JSON configuration file for your private location. For options that accept multiple arguments, use JSON string array notation. For example:*` DATADOG_TESTS_DNS_SERVER='\["8.8.8.8", "1.1.1.1"\]'` ”.


Which wording do you prefer, and why?


In the last three years, the Documentation team at Datadog has[doubled from 7 to 14 writers](https://www.youtube.com/watch?v=gdsmHokNdIs&list=PLZAeFn6dfHpneQPsDWa4OmEpgW4pNiaZ2&index=13) . Even so, we operate on a 200 developer to 1 writer ratio, wading knee deep in docs-as-code, opening and reviewing pull requests on GitHub, and collaborating with developers and product managers to maintain the quality and standards for an open source repository with over 1,400 internal and external contributors containing documentation for 35 products (and counting).


We writers know to avoid using malapropisms, jargon, mismatching tenses, gendered language, and more when we write. But how do we impart this knowledge to contributors making an update to the documentation for the first time, and how do we ensure writers reviewing content updates can remember all of our guidelines? Of course, the answer is automation and continuous integration (CI)!


Even if contributors are upping their writing skills by using AI to write better first drafts, the LLMs are likely unaware of the style choices of the Datadog documentation team, such as using serial commas, avoiding latinate words like *via* , or writing timelessly by not using words like *currently* .


This is the story of how we, the Datadog Documentation team, automate technical content copy editing, shifting the effort of providing high-quality, consistently styled docs updates ever left, closer to the moment the author of the change puts fingers to keyboard. And not just for our documentation, but also for our entire organization and beyond, by open sourcing the solution.


## The best of problems: So many contributors


To illustrate the amount of documentation work the team processes, in 2023, we merged over 20,000 pull requests to improve the docs for over 30 products, 65 API endpoints, 95 Marketplace integrations, 400 security compliance rules, 400 workflow actions, and 650 integrations—that’s a lot of pages of content, and we’re continuously publishing new documentation every day.


Part of managing the large workload comes down to solid processes. Writers on our team participate in an on-call schedule, triaging questions on Slack and reviewing pull requests on GitHub. On average, an on call writer reviews over 40 pull requests per day. We’re looking for any advantage to help us plow through all this work. Automating some style and consistency rules is part of it.


## The solution: Documentation style linting


To automate the enforcement of our style guidelines, we adopted Vale, an[open source command-line linting tool](https://vale.sh/) into our authoring environment and CI workflow. Vale has helped us cut down on editing time, reduced the mental toll on writers, and even enabled contributors to amend their own contributions before we get to reviewing their pull request.


To have Vale run the rules on our GitHub PRs, we set up a[GitHub Action](https://github.com/errata-ai/vale-action) in our documentation repository. The action specifies the location of the vale.ini file in the repository so that when Vale executes, it’s able to find the location of the style rules as well as which rules it should run on the HTML and Markdown files.


An example .vale.ini file in the Documentation repository. An example .vale.ini file in the Documentation repository.


When someone creates a PR, they can see and address automated comments provided by the action in the Files Changed tab. For example, our rules tell them when their sentences are very long, or they’ve chosen a wordy phrasing:


Comments flagged by Vale in a pull request. Comments flagged by Vale in a pull request.


Or when they are carrying over habits formed on typewriters:


Vale flagging the use of two spaces instead of one. Vale flagging the use of two spaces instead of one.


## Codifying our style guide


Once upon a time, writers would update our team’s editorial and contributing guidelines in an internal Confluence space, then add these guidelines to a guide on reviewing documentation pull requests, and finally update the contributing guidelines and various pages on the` Datadog/documentation` repository wiki.


With[datadog-vale](https://github.com/DataDog/datadog-vale) , we converted our existing style guidelines for reviewing and writing prose into linter rules. We add new style rules without having to document them in multiple places. Our CI pipeline actively checks PR contributions against our style rules, before a writer takes a look at the pull request.


### Useful rules


We used Vale’s YAML specification to set up rules for the content we want to validate in Markdown and HTML, and added regular expressions to specify areas in the Markdown content we don’t want to validate (such as Hugo shortcodes).


Vale flagging on sentence length and the use of a temporal word. Vale flagging on sentence length and the use of a temporal word.


Mark Twain once said, “Writing is easy. All you have to do is cross out the wrong words.” Well, you can customize Vale to do exactly that! For example, if you create a[words.yml file](https://github.com/DataDog/datadog-vale/blob/main/styles/Datadog/words.yml) and specify a list of jargon to avoid, Vale flags a warning when it detects cruft such as “easily” or “simply” in your docs.


If you’re a staunch proponent for the Oxford comma like the rest of us, the[oxfordcomma.yml rule](https://github.com/DataDog/datadog-vale/blob/main/styles/Datadog/oxfordcomma.yml) is a good first rule to start getting your feet wet with Vale.


Vale flagging a missing Oxford comma. Vale flagging a missing Oxford comma.


In this rule configuration, as shown below, regex defines when Vale should alert on a sentence that ought to use the Oxford comma, providing a message that describes what needs to be fixed, and how the Oxford comma should be applied correctly.


```text
1   extends  :   existence    2   message  :   "Use the Oxford comma in '%s'."    3   link  :   "https://github.com/DataDog/documentation/blob/master/CONTRIBUTING.md#commas"    4   scope  :   sentence    5   level  :   suggestion    6   tokens  :    7        -   '(?:[^,]+,){1,}\s\w+\s(?:and|or)'
```


You can create a rule file titled` abbreviations.yml` that flags when someone uses Latin abbreviations instead of plain English, and provides alternatives to replace the abbreviation:


```text
1   extends  :   substitution    2   message  :   "Use '%s' instead of abbreviations like '%s'."    3   link  :   "https://github.com/DataDog/documentation/blob/master/CONTRIBUTING.md#abbreviations"    4   ignorecase  :   true    5   level  :   error    6   nonword  :   true    7   action  :    8       name  :   replace    9   swap  :    10       '\b(?:eg|e\.g\.|eg\.)[\s,]'  :   for example    11       '\b(?:ie|i\.e\.|ie\.)[\s,]'  :   that is    12       '\b(?:etc)[\s\n,.]'  :   and more
```


Anyone writing technical documentation can use the Vale linter to ensure consistent product terminology, vocabulary, and prose. By incorporating the linter as you would a code linter like Prettier in a CI/CD pipeline, people can author technical documentation that adheres to your style guide, and can address suggestions from Vale before an approver reviews the pull request.


### Shifting it even further left: Vale in the development environment


When Vale is in the CI pipeline, authors get feedback after they create a PR. We can get them that feedback even earlier, though, by integrating Vale into their IDE where they are writing the documentation change in the first place.


At Datadog, our writers mainly use Visual Studio Code, and have a clone of the` datadog-vale` repository downloaded. We run the rules locally to lint content as we write it. For example, you can lint a file located in a directory by running` vale content/en/folder_name/file_name.md` in a terminal. You can lint a whole folder of Markdown files by running` vale content/en/folder_name` .


Running the Vale command on the Agent Logs documentation in a terminal window. Running the Vale command on the Agent Logs documentation in a terminal window.


In addition to the Vale command line interface, you can get Vale warnings and errors reported within the IDE. There are Vale integrations for popular IDEs such as[Jetbrains](https://vale.sh/docs/integrations/jetbrains/) and[VS Code](https://github.com/chrischinchilla/vale-vscode) . In VS Code, the Vale integration highlights style issues inline, and also lists them in the **Problems** tab.


Inline style issues on the Problems tab in VS Code. Inline style issues on the Problems tab in VS Code.


## Challenges


Keeping up with the pace of product development in a high-velocity organization like Datadog can be challenging, so the Datadog Documentation team accepts docs updates and contributions from outside the core team–from product managers, developers, support engineers, customers, open-source enthusiasts: anyone! It’s a convenient way to constantly improve and keep our docs up-to-date. But reviewing documentation updates contributed by people who haven’t memorized our style guide can be labor intensive.


After all, technical writers put on a different hat while editing, and the process can take more brain power than writing does. Not only are we providing constructive criticism on the way information is being communicated, but also we’re proposing a rewording of language patterns and syntax, all while trying not to change the technical accuracy of the information. On top of that, often this is an external contributor’s first pull request in the documentation repository or their first pull request anywhere, ever. We want to create an encouraging and safe environment for them to feel like contributing to open source is something they may want to do again. That starts with helping them make the highest quality contribution they can, on their own.


### CI workflow setup


As we set up Vale and defined our rules to check our documentation PRs, we ran into some challenges. For example, Vale rules were alerting on content in image shortcodes, which we expected the linter to ignore. Also, consuming the Vale styles from a separate repo in our GitHub Action turned out to be a bit challenging and resulted in some hackery! We’ve addressed strange behaviors that writers noticed, and we regularly fine tune the configuration of our rules by consulting the[Vale documentation](https://vale.sh/docs/) . Vale provides a prose linting tool that not only works well, but also integrates easily into our workflows.


## What’s next


Today,` datadog-vale` is the single source of truth for documentation style and prose at Datadog. This means any internal team can contribute their product-specific terminology to the repo and benefit from both their custom rules and the existing rules provided by other teams. For example, the Security Engineering team maintains a large collection of pages that we single-source into our documentation, and they’ve added[industry-specific vocabulary](https://github.com/DataDog/datadog-vale/tree/main/styles/Vocab/Security) and[terminology](https://github.com/DataDog/datadog-vale/tree/main/styles/SIEM-Names) to the` datadog-vale` repository to use alongside our style rules.


Besides encouraging teams to contribute to the rules, we’re also helping teams add Vale to their CI, ensuring that contributors to other repos at Datadog get similar style feedback. For example, we are adding Vale into the integrations publishing workflow. Soon, we will be able to uncover any prose issues that may be lurking in the shadows.


Like most documentarians, we are excited about the potential of LLMs to make our documentation better and our processes quicker. We balance how LLMs use our data (particularly the not-yet-published content that we don’t want stored anywhere externally) with the incredible editing power that they have the potential to bring. We anticipate exploring the integration of LLMs into our processes and are confident they will play a role in streamlining our efforts to maintain plain, simple language in the product documentation. A good first step is having a crisp set of computer-understandable style rules already created.


Interested in using Vale to write clear, concise product documentation? Check out the[official documentation](https://vale.sh/docs/) and customize Vale into your workflow. Lastly, in the spirit of open source, we invite you to open a pull request in the` documentation` repository today!


-
-
-
