---
schema_version: "1.0.0"
document_id: "7e93520b369f795667bd4c95628b2905c9c1fbaa5c2f07d40bd4da9813a6f495"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/how-to-create-a-figma-design-system"
published_at: "2022-10-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:8aa98e8da57ad843cd63f0f08c6309c5d2dbed7a2d1fe60342482ac4c8f79bf3"
---

# How to Create a Figma Design System

So you’ve decided you want to create your own Figma design system, or maybe you’re just trying to understand what all the fuss is about — either way, we’ve got you covered! Before going into how you can create a design system, it’s important that we got over what we’re trying to do.


We’ve talked before about how design systems mean different things to different teams and people. But there are a couple of things industry leaders have agreed on that should be included in your design system. So let's go over what we need to include, what tools we’ll be using, and how to create it.


## What should you include in your design system?


Back in 2020, the Material team in collaboration with Clarity surveyed some of the biggest companies about design systems — they asked: what’s included in a design system? The results confirmed what most people already assumed that most design systems include these same artifacts.


Percentage of what design system artifacts users had in their design systems.


However, most of these companies likely already built their design systems over time, you might be looking at this list and feel overwhelmed. Not to worry though — every great tree started out as an acorn and all that.


It’s clear very few teams have the resources to build everything at once. Maybe you already have some of these artifacts as siloed projects, but even if you don’t, you need to start somewhere. Which acorn you decide to start with largely depends on what issues you and your team are facing and what you’re hoping a design system will help you do. So without further ado, let's take a look at the different approaches you can take to creating your Figma design system.


## How to create your Figma design system


### Design


Chances are that if you’re interested in a design system the ‘design’ part is pretty significant. We’ve talked before about how design systems are more than just a collection of design artifacts, but they still play a pivotal role.


At its core, typically most design systems will have **design tokens** — acting as representations of “design decisions”, **components** — being reusable collections of tokens serving as building blocks for your product, and **assets** — which include everything from icons to images. For starters let’s look at what’ll need out of design tokens.


#### **Design tokens**


When building any system from the ground up you need to know what your most atomic element is. In the case of a design system, those elements are design tokens and they include but aren’t limited to: colors, typographies, radii, spacing, sizing, and shadows.


You’re probably already using most of these in your designs, but creating a design system is about categorizing and attributing correctly. The goal of creating a design system isn't to use the color blue for your branding background but to correctly categorize background-main-color: #0047AB to ensure consistency across your product and any other branding material. With every decision you’re making, you’re building your design system. Here’s an example of a set of color design tokens that will be a primary part of our design system for this guide.


Another good example of design tokens is typography, here’s another example from our demo design system.


#### **Assets**


To go along with your design tokens and components, you will need a central hub for all your design assets. For our example, we’ll be using icons.


Add your icons to a sheet with proper naming conventions to categorize them in your design system correctly.


#### **Components**


When designing a product, for example, a home screen, you’re probably thinking about it in terms of components instead of design tokens. Components are reusable blocks that make up your product and they play a huge part in the design system as well. Let's take a button as an example.


This button is a collection of different elements — a pill-shaped layer, an icon, a string of text, a background color, typography, and text color. And since it will be used throughout your product you need consistency — by building a design system using components like these. But naturally as a designer your product might have different variations of the same component. This is why design systems need more than just the component, but also the guidance on how to use or alter it. Taking our button again as an example, here’s a breakdown example.


These kinds of guidelines and style guide principles are key to fleshing out your design system. However, it’s not really efficient to document all your guidelines and principles, style guide, voice and tone, and whatever else in Figma pages. So let’s take a look at how to create your design system documentation.


### Documentation


Documenting an organization's visual language is one of the first steps in a design systems development - State of design systems 2020


Now that you’ve added the design elements you’ll be using in your design system, it’s time to document how they’ll be used. We’ll be using Supernova for this part of the guide, but before we begin, check out how to get started with Figma and Supernova.


With your design data now synced in Supernova, you can document your design system. Similar to your designs, what your documentation looks like depends completely on your needs. Typically for any design system you’d want at least a style guide and guidelines for your components and assets.


Supernova’s documentation site behaves like any familiar block-focused editor such as Notion.


#### **Style guide**


Your style guide helps you break down your different design elements and explain how to use them. This helps you promote consistency and makes it easier for you team to interact with your designs. From Supernova you can create sections depending on your design tokens, assets, or any other categories that suit your design system. Using Figma content blocks you can integrate specific design data and document them thoroughly. Here’s an example from our demo design system explaining the use of colors.


Create the same for different areas of your design system, keep in mind your audience. This could be developers looking to implement your designs in the product, designers looking to replicate or create new designs, or even marketeers looking for brand guidelines.


#### **Component guidelines**


As we already know, components play a huge role in your design system. It’s important to document how and when to use them, their alterations, the code needed to implement them, and anything else that might be relevant to your team.


With your documentation now starting to take shape, you can publish it for internal or external use depending on who you want using your design system.


##### You’ve now created your own design system with Figma and Supernova. With all your design data in one place and documentation that contextualizes all of it, you can now start to use your design system. In our next article, we’ll go over how to manage your design system so you and your team can make the most of it.
