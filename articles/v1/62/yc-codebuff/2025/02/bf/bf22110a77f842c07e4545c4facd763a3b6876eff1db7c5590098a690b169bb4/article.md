---
schema_version: "1.0.0"
document_id: "bf22110a77f842c07e4545c4facd763a3b6876eff1db7c5590098a690b169bb4"
company_key: "yc-codebuff"
company: "Codebuff"
source_id: "yc-codebuff-rss-bef55ad83d13"
canonical_url: "https://news.codebuff.com/p/codebuff-is-live-with-o3-mini"
published_at: "2025-02-04T20:05:28+00:00"
first_seen_at: "2026-07-27T08:33:23.704221+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:8b7fdf2917d46d1f47d2c5af5b6e5ff9bb63336055b074ac1209ab0ecc8223b8"
---

# Codebuff is live with o3-mini!

# Codebuff is live with o3-mini!


### Plus, a new Community Codebuff repo, higher performance & budget modes (--max and --lite), tab completion, and we're hiring 💼


[Codebuff](https://substack.com/@codebuff)


Feb 04, 2025


After raising our pre-seed round this December 🔥, we’re back with a bunch of awesome features and updates.


## o3-mini is live in Codebuff!


We launched our o3-mini integration on Friday, a few hours after it was first available. You can trigger it by simply asking Codebuff to do something complex.


Thanks for reading Codebuff! Subscribe for free to get the latest updates.


Codebuff will then enter a “deep thinking” mode where it triggers o3-mini with effort level “high”.


We think this is the best way to leverage reasoning models: as architects that plan the best solution. We expect this combination of o3-mini + Sonnet 3.5


[to top the LLM leaderboards](https://aider.chat/docs/leaderboards/) shortly.


## Community GitHub Repo


We’ve just launched the


[community-codebuff repo](https://github.com/CodebuffAI/codebuff-community) on GitHub (give us star!), featuring:


1.


Starter templates for new Codebuff projects


2.


Community showcase projects


3.


Knowledge! Organized in knowledge files on different topics


**Starter templates**


First, you can now create new projects from our set of starter templates! Each starter template is configured in proper Codebuff form with a knowledge file.


For example, to create a NextJS project, run the following:


```text
# Creates a new nextjs project in directory my-app.
codebuff --create nextjs <my-app>
```


You can choose from a variety of templates including nextjs, vite, remix, convex, node-cli, python-cli, and chrome-extension, with more to come!


**Showcase**


Secondly, after so many of you have wrote in with projects of your own creation, we decided to try to collect them into a showcase.


We have a minecraft clone up, and thanks to our users


` narthur` and


` Lachlan` for their contributions as well! You can use the --create command to clone any of these projects and try them out locally!


**Knowledge**


Finally, we want to collect and share knowledge for the Codebuff agent. Upload knowledge files that describe a technology you are using — for example, a component library you want it to use, or how to optimize react re-renders. We hope to eventually allow Codebuff to read from this knowledge via a tool call so it can access it when needed.


Take advantage of this open source content to improve your own use of Codebuff. And then, give back to the community by


[contributing to the Codebuff community repo](https://github.com/CodebuffAI/codebuff-community?tab=readme-ov-file#contributing) with your own starter templates, showcase projects, and knowledge files.


## Performance Leap: New Model Integrations 🚀


We’re excited to announce two major modes that make Codebuff outputs higher quality or more cost-effective:


1.


` codebuff --lite` **:** For more efficient code generation and still surprisingly good performance.


-


We’re using DeepSeek’s first reasoning model, R1, as the main agent as well as DeepSeek V3 for finding files.


-


It can often match the performance of


` o3-mini` and


` Sonnet 3.5` , but at


**one-tenth** the cost—enough to run a full demo for just $0.02 in credits.


2.


` codebuff --max` **:** For the highest quality code generation, using better models to find files and plan changes.


-


Finds files with Sonnet instead of Haiku for an improved ability to get the right context


-


When o3 comes out, we’ll use it for planning instead of o3-mini.


We’ve also fixed a bug that prevented Codebuff from finding all your files, so it should now run more smoothly than ever!


## Quality of Life Improvements


The new code search tool


We’ve introduced several updates to enhance customization and usability:


-


**Tab Completion**


Hit the “tab” key to quickly autocomplete folders/files, just like in a standard terminal. It also works for names of classes and functions in your codebase.


-


**(Finally!) Add new lines within Codebuff**


Press the space bar twice to insert two new lines (a workaround for


[archaic terminal limitations](https://stackoverflow.com/a/76117154) ).


-


**Global Rules via Knowledge Files**


You can now set universal rules across all Codebuff projects by placing a


` knowledge.md` (or


` .knowledge.md` ) file in your home directory. For instance, add a single line to enforce Poetry usage in all Python projects—no extra setup required.


This feature is still in alpha, so let us know if it’s helpful in your workflow!


-


**Code Search!**


Codebuff can now search your codebase and get back file results. The agent crafts a regular expression that is executed by the blazing fast rust library


[ripgrep](https://github.com/BurntSushi/ripgrep) . This makes Codebuff even better as a junior software engineer that does whatever you tell it.


-


**Bug fixes**


-


Codebuff is much less likely to run disruptive commands like git push, run a dangerous script, or install a package globally without permission.


-


We’ve squashed a pesky cursor bug that forced terminal restarts.


-


Ctrl-C now kills the currently running terminal process.


-


Overflowing the context window should be rarer.


-


…and much more


You should now have a much smoother experience overall using Codebuff.


## Nosu Hackathon


We also sponsored a


[hackathon](https://nosu-ai-hackathon.devpost.com/) hosted by


[Nosu](https://www.nosu.io/) . A huge shoutout to the winners


[Hannah Rauch](https://devpost.com/hrauch3404) ,


[Phil-Ho Combatir](https://devpost.com/hocombatir) ,


[noor sahel](https://devpost.com/noorsahell99) , and


[Chan Dinh](https://devpost.com/andinhc254) .


They made


[Project Paemon](https://devpost.com/software/name-5ftgnj#updates) : a fun questionnaire that lets you hatch your own “pæmon” based on your answers. Be sure to check it out, we hope it sparks new ideas for projects you’ll share with the community!


## Stay tuned!


We have many more exciting updates in the works. Codebuff is now rivaling and in some cases exceeding the best other codegen products on the market. We’re coming for you, Cursor!


Our vision is right: Developers want a simple interface to direct an agent to do work quickly and locally.


We accomplish this with more intelligence than anyone else: a fleet of LLM’s, including up to 7 different models today, that solve each subproblem for you. We are well on our way to the world’s best coding agent!


## BTW, we’re hiring


If you’re intense about wanting to help us build the best coding agent (or know someone who is!), send your resume/LinkedIn to


jobs@codebuff.com .


Thanks for reading! We’re always interested in your feedback. Email us at


founders@codebuff.com .


Until next time,


James + Brandon


Thanks for reading Codebuff! Subscribe for free to get the latest updates.
