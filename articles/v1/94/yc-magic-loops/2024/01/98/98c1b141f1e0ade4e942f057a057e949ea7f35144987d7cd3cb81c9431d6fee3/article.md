---
schema_version: "1.0.0"
document_id: "98c1b141f1e0ade4e942f057a057e949ea7f35144987d7cd3cb81c9431d6fee3"
company_key: "yc-magic-loops"
company: "Magic Loops"
source_id: "yc-magic-loops-rss-5d6a51a0bf61"
canonical_url: "https://blog.magicloops.dev/posts/how-to-build-a-magic-loop/"
published_at: "2024-01-18T05:53:25+00:00"
first_seen_at: "2026-07-24T10:22:08.394696+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:901df77c7d848aa205b3bc238a8f6879bbed0d3da4f2cdfe16c8411e9d4c5045"
---

# How to build your first Magic Loop

## **tl;dr**


> There are 3 ways to create your first Loop: chat with the Loop Creator, use Manual Loop mode, or copy a Public Loop.


## Background


Magic Loops are AI-enabled automations that allow you to use natural language to build simple programs (notifications, scrapers, REST endpoints, background jobs, and more!)


There are 3 primary ways to create a Magic Loop:


1. Using the Loop Creator
2. Creating a Loop manually
3. Copying an existing Public Loop


## **The Loop Creator**


When you first land on Magic Loops, you’ll notice a simple text box:


By entering your request here (e.g. text me when the tides are low), the Loop Creator will kick off a conversation about what you’re trying to build.


### Loop Creator Chat


Once you’ve sent your request, the Loop Creator will process it and create a “Loop Outline”.


The Loop Outline is essentially an overview of the Loop that you will create, broken down into steps (known as “Blocks”).


In the chat with the Loop Creator, you can review the Loop Outline, clarify your request, and add missing details.


> Humans are notoriously bad at specifying what a computer should do, the same applies here!


When the outline seems right, just click “Create Loop” to kick off automatic Loop Generation.


### Loop Generation


This screen can be a bit overwhelming, but essentially the Loop Creator will now try and build your Loop.


Block by block, the LC will go through and create, update, and validate the Block to get the desired behavior.


> Note: If something is wrong, you can stop it at any time and adjust the parameters.


If the Loop Creator runs into any issues, it may pop open the chat and ask a clarifying question.


### Test: Success ✅


Assuming all goes well, the Loop Creator will finish the Loop.


Feel free to test the created Loop once more to make sure it works.


If you’re happy, simply click “Activate” in the top right corner.


That’s it! 🎉


> Please note: The Loop Creator is technically in “alpha” — so don’t be surprised if it doesn’t get your Loop right on the first try!


## **Manual Loop Creation**


For more advanced Loops, it often makes sense to create a Loop *manually* .


We give you full control over the loop, allowing you to change and modify it until it fits your needs.


### How does it work?


Simply click “Manual Loop” in the Loop sidebar menu, and get started.


You can add Blocks as you see fit, and tweak their prompts until they do what you want.


As you add Blocks, simply click “Test” on each Block, or on the Loop as a whole, to simulate a run (known as a “Test Run”).


> Warning: Any API calls or Fetch Blocks will actually run. During a Test Run only Notify blocks are muted.


### Code Blocks


One of the most useful aspects of Magic Loops are the Code Blocks.


Code Blocks allow you to define specific details of a Block, and we use GPT-4 to generate code for that Block.


The Code Blocks automatically receive the previous blocks output, the Loop Variables, and more context to better facilitate your request.


### Test: Success ✅


Once you’ve tested your Manual Loop, simply click “Activate” to turn it on.


## Public Loops


On the Magic Loops[homepage](https://magicloops.dev/) you’ll find a section for Public Loops.


Public Loops are Loops created by other users and shared with the world.


If you find a Loop that you like, simply click “Use this Loop” at the top right.


> Oftentimes Public Loops rely on Loop Variables, make sure any Loop Variable values are defined before trying to Activate your Loop.


Feel free to adjust the Loop as needed for your usecase.


### Sharing your Loops


Like a Loop that you’ve built? Want to share it with others?


Simply click “Options” and then “Share”.


You’ll see a pop-up where you can change the configuration to “Discoverable under Shared Loops”:


## **Summary**


There are three primary ways to create a Magic Loop:


1. Working with the Loop Creator
2. Manually building Loops by hand
3. Using an existing Public Loop


Regardless of the path you choose, you can always fall back to Manual mode, or ask the Loop Creator for help in modifying your Loop.


## Get started today!


[Make your first Magic Loop](https://magicloops.dev/) and discover how easy it is to program useful automations with AI!
