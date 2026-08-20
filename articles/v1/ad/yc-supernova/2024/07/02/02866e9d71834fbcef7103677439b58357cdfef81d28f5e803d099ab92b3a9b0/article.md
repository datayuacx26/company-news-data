---
schema_version: "1.0.0"
document_id: "02866e9d71834fbcef7103677439b58357cdfef81d28f5e803d099ab92b3a9b0"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/what-are-figma-variables-modes-collections"
published_at: "2024-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:d4a5eefee378274993d8e1962c01ecd74e74f92089a80dc010c0096a5ef32211"
---

# What Are Figma Variables, Modes, and Collections?

Figma variables are all the rave and you’ve probably heard a lot of product designers talk about them. You might even hear design system folks use “Figma variables” interchangeably with “design tokens” *(Note: They’re not exactly the same thing!*[Read this article](https://www.supernova.io/blog/understanding-the-differences-between-figma-variables-and-design-tokens) *to understand the differences.)* or mention modes and collections. But what are these Figma features and how can you use them for your design system? Let’s find out.


## What are Figma variables?


[Figma variables](https://help.figma.com/hc/en-us/articles/15339657135383-Guide-to-variables-in-Figma) are a powerful feature that allow designers to create reusable values for properties like colors, text styles, typography, and more. These variables help maintain consistency throughout a design project and enable easy updates across multiple components.


## Modes in Figma Variables


Modes in Figma variables refer to different states or versions of a variable that can be switched based on specific conditions or contexts. For instance, you can define different color modes for light and dark themes or different brands.


This allows for seamless theme switching without manually adjusting each element or creating different variants of your components. You can also use modes with strings (eg. localization) and numbers (eg. spacing, screen resolution).


Here's an example of how modes work:


- **Light vs Dark Mode:** Define variables for colors, shadows, and other properties suited for a light theme vs dark theme.
- **Copy strings:** Define a different set of variables for English vs Portuguese.


Switching between these modes will automatically apply the corresponding variable values to your design, making theme management more efficient.


*💡 Tip: Use*[this file as a playground](https://www.supernova.io/blog/understanding-the-differences-between-figma-variables-and-design-tokens) *for Figma variables and watch our*


*where we take a deep deeper dive into variables and modes!*


## Collections in Figma Variables


Collections are groups of related variables that help organize and manage them more effectively. For example, you can create collections for colors, typography, spacing, etc. This categorization simplifies the process of finding and updating variables.


By grouping related variables into collections, you ensure that your design system remains organized and scalable. You can easily navigate through collections to locate and modify specific variables as needed.


*💡 Tip: Use*[this sample file](https://www.figma.com/community/file/1380497988101489689) *as inspiration for testing out Figma variables and connecting them to Supernova.*


## Connecting Figma Variables in Supernova


Integrating Figma variables with Supernova can enhance your design system management. Here's a quick summary and simplified steps to connect Figma variables with Supernova:


1. **Install Plugin** : Install[the Supernova plugin](https://www.figma.com/community/plugin/1303357900761384370/supernova-figma-variables-sync) from the Figma marketplace.
2. **Log In** : Open the plugin and log in to your Supernova account.
3. **Select Workspace** : Choose your Supernova workspace.
4. **Sync Variables** : Import and sync Figma collections and modes.


## Expanding the power of Figma variables with Supernova


Once your variables are imported, you can start managing, documenting, and delivering them within Supernova.


### Managing Figma Variables


\[


\]([https://www.supernova.io/design-tokens](https://www.supernova.io/design-tokens) )


In the Design Tokens section of Supernova, you can manage various aspects of your variables, such as names, values, filters, and descriptions. This centralized management ensures consistency across your design system.


### Documenting Figma Variables


\[


\]([https://www.supernova.io/documentation](https://www.supernova.io/documentation) )


Supernova allows you to embed Figma variables directly within your documentation. This feature helps create a source of truth for your design system, making it easier for team members to understand usage and reference variables.


### Delivering Figma Variables to Code


\[


\]([https://www.supernova.io/code-automation](https://www.supernova.io/code-automation) )


Use Supernova’s code automations to translate Figma variables to code and ensure your design tokens are always up-to-date. You can use the out-of-the-box exporters we provide, or create custom ones.


Figma variables, along with modes and collections, provide a robust framework for managing design properties efficiently. By incorporating these features into your workflow, you can achieve greater consistency, flexibility, and scalability in your design projects.


Start exploring the power of Figma variables today and see how they can transform your design process!
