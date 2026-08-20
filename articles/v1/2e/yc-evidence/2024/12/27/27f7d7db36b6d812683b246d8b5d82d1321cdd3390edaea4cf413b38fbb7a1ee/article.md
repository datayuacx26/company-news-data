---
schema_version: "1.0.0"
document_id: "27f7d7db36b6d812683b246d8b5d82d1321cdd3390edaea4cf413b38fbb7a1ee"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/themes"
published_at: "2024-12-11T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:425a16d2793dfa73cbe90cab65549840b82000aafb23b6bca698815eb6e973b4"
---

# Themes & Dark Mode

# Themes & Dark Mode


*Launching a theming system for Evidence, including dark mode - giving you the power to create publication-quality outputs that perfectly match your brand*


[Sean Hughes December 11, 2024 · 2 min read](https://www.linkedin.com/in/hughessean/)


Today we’re releasing themes, which make it easy to control the appearance of your Evidence app with a simple global configuration. This release also includes dark mode, which is now included in all new Evidence projects by default.


With themes, it’s easy to give your app a completely different feel. For example, the Evidence docs in the style of the[Financial Times](https://www.ft.com/) :


## Why Themes?


We get a lot of questions about custom styling/colors in our Slack community and from our customers - especially from people working on embedded analytics.


Evidence has always been deeply customizable, but until now those changes have required CSS overrides and repetitive code. After helping many organizations with customizing their reports, we’re excited to offer a global approach for creating company-branded reporting.


Themes make this process dramatically simpler, and take care of the edge cases that are hard to keep track of when building a custom app from scratch (like tooltips, links, alerts, etc.).


Theming is a crucial part of building reports for these scenarios:


- **Embedded Analytics:** Embed Evidence seamlessly into your app by matching its styling
- **Customer Portals:** Deliver a white-labeled analytics experience to your customers
- **Internal Reporting:** Match company branding and internal tool design to maximize familiarity for your report users


## Sample Themes


We’ve created a collection of[sample Evidence themes here](https://github.com/evidence-dev/themes) . You can copy-paste theme configurations into your own projects or start a new theme from scratch.


## How Evidence Themes Work


Evidence uses a small set of color tokens for all UI elements across the entire app. This allows you to create a customized look and feel with only a couple lines of configuration. Evidence will adjust colors depending on the context so that everything fits together cohesively.


Configuration is defined in` evidence.config.yaml` - this is where you can include color palettes, color scales, and custom colors you can reuse across your project.


Here’s an example for the “Forest” sample theme:


```text
# Forest theme
theme  :
colorPalettes  :
default  :
-    '#556b2f'    # Dark Olive Green
-    '#6b8e23'    # Olive Drab
-    '#8fbc8f'    # Dark Sea Green
-    '#2e8b57'    # Sea Green
-    '#3cb371'    # Medium Sea Green
-    '#66cdaa'    # Medium Aquamarine
-    '#20b2aa'    # Light Sea Green
-    '#008b8b'    # Dark Cyan
-    '#5f9ea0'    # Cadet Blue
-    '#4682b4'    # Steel Blue
colors  :
primary  :
light  :    '#32cd32'    # Lime Green
dark  :    '#3cb371'    # Medium Sea Green
accent  :
light  :    '#8fbc8f'    # Dark Sea Green
dark  :    '#556b2f'    # Dark Olive Green
base  :
light  :    '#f0fff0'    # Honeydew
dark  :    '#1a2f2f'    # Darker Slate Gray
info  :
light  :    '#3cb371'    # Medium Sea Green
dark  :    '#5f9ea0'    # Cadet Blue
```


The color tokens in that yaml file (primary, accent, etc.) determine how UI elements will appear. You have control over the configuration for both light and dark mode.


## Dark Mode


Dark mode is now included in all new Evidence projects, and can be customized using the new theming system.


We’ve all come to expect dark mode in the apps we use every day (and night), but dark mode poses such a challenge for data-intensive apps that it is still ”[on the roadmap](https://community.tableau.com/s/idea/0874T000000HB6aQAG/detail) ” for many data tools.


Dark mode poses a challenge for data apps because color is often driven by data and customized for specific visualizations. Evidence will now automatically adjust any custom colors you’ve applied in your app so that they always look presentable in dark mode.


### Appearance Switcher


You can toggle dark mode using the appearance switcher in the top right settings menu.


#### Appearance Switcher Options:


- System (default) - matches your operating system setting
- Light
- Dark


### Configuring Dark Mode


If you have an existing Evidence project, dark mode is disabled by default. To add it, include the following in your` evidence.config.yaml` :


```text
appearance  :
default  :   system
switcher  :    true
```


If you are creating a new Evidence project, dark mode is enabled by default. To disable it, you can change` evidence.config.yaml` like so:


```text
appearance  :
default  :   light
switcher  :    false
```


To check out dark mode in action, see the[Evidence docs site](https://docs.evidence.dev/) , which is built with Evidence.


## Get Started with Themes


Themes make it easy to build fully branded analytics experiences with Evidence - we’re excited to see what people build. Here are some resources to help you get started:


- [Themes Docs](https://docs.evidence.dev/core-concepts/themes/)
- [Sample Themes (Github)](https://github.com/evidence-dev/themes)
- For questions or support,[reach out in Slack](https://slack.evidence.dev/)
