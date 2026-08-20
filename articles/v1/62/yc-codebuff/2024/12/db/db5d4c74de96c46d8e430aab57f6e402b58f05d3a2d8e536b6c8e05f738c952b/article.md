---
schema_version: "1.0.0"
document_id: "db5d4c74de96c46d8e430aab57f6e402b58f05d3a2d8e536b6c8e05f738c952b"
company_key: "yc-codebuff"
company: "Codebuff"
source_id: "yc-codebuff-rss-bef55ad83d13"
canonical_url: "https://news.codebuff.com/p/agentic-cli-code-generation-has-arrived"
published_at: "2024-12-03T00:19:51+00:00"
first_seen_at: "2026-07-27T08:33:23.704221+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:7a97fc1c60d22663f390be63cdbc636e6dc9e7c2ba41a3fa708b550a91b89a2c"
---

# Agentic CLI Code Generation Has Arrived 🚀

# Agentic CLI Code Generation Has Arrived 🚀


### Watch the future of software being built – literally


[Codebuff](https://substack.com/@codebuff)


Dec 03, 2024


Hello everyone!


Big news - we've just leveled up AI-driven coding with something we're super excited about:


**agentic workflows** . Imagine having an AI that doesn't just spit out code once, but keeps refining and improving it until it's


*chef's kiss* perfect. That's what we've built.


### **Agentic Workflows**


So what exactly are agentic workflows? Think of it like having two AI teammates working together to crush complex coding challenges. We've created a dynamic duo:


-


**Coding Agent:** Generates code based on the reviewer agent’s demands.


-


**Reviewer Agent:** Evaluates the coding agent’s output and reprompts it as necessary. Basically, it runs the agent in a loop on your behalf.


You can start using Codebuff's agentic workflows today. To trigger it, use phrases like “


**keep going until <x>** ”


**** or “


**work on <y> until you’re satisfied** ”. Feeding language like this into Codebuff spins up its Reviewer agent.


So why did we even build agentic workflows? We had originally built Codebuff with only the coding agent, but wanted to


burn more tokens make a few more things possible:


-


**Execute open-ended instructions** like "Polish up this feature to make it more user friendly". Agentic workflows enable this by dividing high-level tasks into bite-sized, sequential steps for the coding agent.


-


**Produce higher quality code** by having the coding agent pass through files multiple times.


The original coding agent is good at spitting out code in a single step, but we needed more. To get closer to enabling truly autonomous feature-building using today's technology, we needed agentic workflows.


Side note — the new reviewer agent might seem like a demanding master, but does a damn good job managing the coding agent. Hopefully this doesn’t have real world parallels.


### **Minecraft in the Browser**


We recently decided to put Codebuff to the ultimate test:


Could it build a browser version of Minecraft using only open-ended instructions?


We started off the exercise by giving it a few open-ended prompts. We then gave a super open-ended prompt:


"Please develop the game to be more fleshed out with more stuff and interaction and instructions. Go until you are satisfied."


This spun up the Reviewer agent. The Reviewer and Coding agents then went back-and-forth for a little while, all by themselves. Here's an interesting example of what the Reviewer agent told the Coding agent during execution:


*"CONTINUE. Still need to add particle effects when breaking blocks to complete the user's request for a more polished game. This will provide satisfying visual feedback when destroying blocks."*


It’s funny, we found agentic workflows work well when it resembles how actual product teams collaborate. You might even notice that when starting an agentic workflow, the "product manager" agent creates a product design review.


Back to Minecraft. The reviewer and coding agent continued to collaborate until the reviewer agent was satisfied. With just our single prompt, the reviewer agent coached the coding agent to generate:


-


**Hover effects to preview block placements.**


-


**Particle effects for breaking blocks.**


-


**An interactive overlay for block selection.**


Here's the final output of what these dynamic duos created:


We put this in front of about a dozen people, who each added a new feature/idea, and ended up with this by the end:


### **Customer Love**


Other than the agentic workflows, we also wanted to share what a customer told us recently. Let us know if you have similar or differing feedback! He said:


"Codebuff can take an idea from 0 to 1 very quickly with structure and MVP working features. I tried a few different projects out, and found the best use case was rapidly iterating on the UI."


We hear this a lot. People love Codebuff for generating tons of code quickly and speeding up the iteration process. It creates initial scaffolding for a new feature, test, or codebase within minutes.


However, we've also heard from developers that they still need to review the code quite a bit after prompting Codebuff. The same customer mentioned:


"Codebuff is a great tool, but do not let it replace basic problem solving. The best use case is for large output and moving fast. You should always check out a problem before blindly asking for a solution."


It’s something we’ve been mulling over as well. Even though Codebuff is likely the best in class for code quality today, we’ve still got a ways to go.


It's one of the reasons why we're excited to get your feedback on agentic workflows. The new reviewer agent double-checks the coding agent's output. We think you’ll see a noticeable improvement in code quality because of it.


### **2 Minute Agentic Workflow Tutorial**


Quick shill: for every person you refer,


[you’ll both earn 500 credits per month](https://codebuff.com/referrals) (for the time being).


Ok now for the tutorial. Hopefully the Minecraft example hinted at how to run agentic workflows, but here's what you do:


Run the following in your terminal (make sure to hit “enter” after each command before moving on):


```text
npm i -g codebuff
mkdir minecraft-codebuff && cd minecraft-codebuff
codebuff
```


Once it starts up, log in to claim your credits:


```text
login # this generates a link to log you in and give you more credits
```


Now you can tell it


**“generate a basic minecraft clone that runs in my browser that lets me move around in the world, explore it, and mine blocks. keep going until it's very recognizable as a minecraft game”**


Then, watch the magic happen. Beware, this will use a lot of credits! We’re building features around managing credit spend for these long agentic tasks, but we couldn’t wait for those to be done before showing this to you!


### **Towards our Autonomous Coding Future**


We've always wondered about the day when development teams can offload day-to-day feature creation tasks. Today, engineers copy code from AI. In the future, AI will build entire features. Software developers will only have to make decisions on relevant feature selection and product strategy based on domain knowledge.


Codebuff’s new agentic workflow feature gets us one step to that longer-term reality. Today, Codebuff is still the


*only* codegen tool that:


-


**Chooses which files to read and edit automatically, by default** — other AI coding assistants require you to specify which files to edit.


-


**Harnesses your existing tools, scripts, and packages without needing explicit approvals** — Codebuff installs packages, runs terminal commands, executes tests, etc. Whatever awk/grep/pip install stuff you used to do, Codebuff now does it for you instead.


-


**Runs in** ***any*** **development environment —** There’s no lock-in. You can bring your IDE: VSCode, Cursor, Vim, Emacs, Replit, or plain text editor (if you’re a monster).


And we’re cementing our lead even further by adding agentic workflows to the list! Please try it out and let us know what you think.


Alright that's it for this newsletter. Thanks for reading! Until next time.


- James & Brandon


Thanks for reading Codebuff! Subscribe for free to see what we’re building.
