---
schema_version: "1.0.0"
document_id: "1e50c9fd920a66691299c94cecca913400794cb4b6b0c695ffe3ab7c5267289b"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/how-to-manage-figma-variables-with-supernova"
published_at: "2023-11-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:d18837cc9d012409c396250b2037bd571134041e5626cb36ced9da6a83cf25e1"
---

# How to Manage and Document Figma Variables With Supernova

Variables are Figma's new way of creating design tokens. Part of Supernova's continued mission is to be the best tool for streamlining the management, documentation, and delivery of your[Figma design system](https://www.supernova.io/blog/how-to-create-a-figma-design-system) . To help you do that, we're excited to add support for Figma Variables in our[latest release](https://www.supernova.io/blog/simplify-your-design-token-pipeline-november-2023-release) .


With a plugin to help you bridge the gap between Figma and Supernova, you can revolutionize how you work with design tokens. This blog post will guide you through importing, managing, documenting, and delivering your Figma Variables in Supernova.


## Import your **Figma Variables into Supernova**


Getting started with Supernova and Figma Variables takes a few simple steps:


### **1. Install the Supernova plugin in Figma**


Head to the Figma marketplace and install the[Supernova plugin](https://www.figma.com/community/plugin/1303357900761384370/supernova-figma-variables-sync) designed for Figma Variables.


### **2. Log in to your Supernova account**


Once the plugin is installed, open it and log in with your Supernova account.


If you don't have a Supernova workspace yet, you can[sign up](https://cloud.supernova.io/signup) and get started for free.


### **3. Select your Supernova workspace**


Once you've logged in, you'll gain access to all your design collections in Figma. Select which Supernova workspace you want to import to and manage your Figma Variables from.


### **4. Import and sync Variables with Supernova**


Once you select your workspace and collection, you can import and see your Figma Variables in Supernova. All your Figma Variables will automatically be organized and arranged in your Supernova workspace as design tokens — here's what we'll sync:


- All variables from the selected local collections.
- All variables from other local collections if there's a cross-collection alias.
- All modes from selected collections — automatically converting them into themes.


## Manage your **Figma Variables**


With your Figma Variables now imported to Supernova as design tokens, you can start using them in our design system. Here's how your Variables will be mapped inside Supernova's Tokens view:


After importing, you can manage various aspects of your Variables from the Tokens view — including token names, copying values, applying filters, adding or editing custom properties, and providing detailed descriptions.


## Document your **Figma Variables**


Beyond management, it's crucial to create a source of truth for all your designs, not just Variables.


Figma Variables often have specific guidelines, revision histories, and best practices. With Supernova, you can embed your Figma Variables directly within your documentation, helping you create visually engaging guides that help ensure brand consistency within your team. By embedding Figma Variables in your documentation, team members can easily access and reference them — making it the central pillar of your design system.


From the Documentation section in your Supernova workspace, you can start creating pages for your Figma Variables. Using token blocks, you can embed automatically updating references to your Figma Variables. There are multiple[token blocks](https://learn.supernova.io/latest/documentation/types-of-blocks/token/tokens/general-84NRgxGl) to choose from, and the options help you tailor your documentation to your needs.


## Delivering your **Figma Variables to code**


When the time comes to translate your Figma Variables into code, Supernova offers a streamlined code automation process.


### **Setting up code delivery**


With Supernova's[Hooks](https://learn.supernova.io/latest/code-integration/exporters/automating-code-delivery/hooks-duxrtgvx) and[Exporters](https://learn.supernova.io/latest/code-integration/exporters/introduction-to-exporters-nq3dafut) , you can establish a seamless code delivery process. This process can be automated to fit seamlessly into your existing workflow.


#### Export to any codebase


[Exporters](https://learn.supernova.io/latest/code-integration/exporters/introduction-to-exporters-nq3dafut) are packages (similar to an npm package) that transform design system data into production code. They enable you to convert your Figma Variables to the codebase of your choice.


#### Set up an automation pipeline


Combined with[Hooks](https://learn.supernova.io/latest/code-integration/exporters/automating-code-delivery/hooks-duxrtgvx) , you can set up a pipeline that listens for triggers and automatically updates your code in Github, Bitbucket, Gitlab, or Azure.


To create a Hook from your **Code automation** section in Supernova, you need to:


- Configure an automatic observer that watches for **events** in the design system
- Select the **Exporter you would like to run** when this change happens
- Set a **delivery URL** for the generated code


---


Building on our mission to help you build the best design system possible more effectively and efficiently, we want to empower users to use the latest and greatest tools. Figma Variables is the next step in Figma's journey, and combined with Supernova, you can build a connected single source of truth to help your organization build better, more consistent products.


Learn more about how Supernova's latest release makes sure it's your ideal[design system partner](https://www.supernova.io/your-design-systems-partner) . For the latest news on Supernova, follow us on[Twitter](https://twitter.com/supernova_io) or join our[Slack](https://join.slack.com/t/supernovafriends/shared_invite/zt-234oqqkfp-JW0MXmL7RS2lzJUUuWBy7g) community.
