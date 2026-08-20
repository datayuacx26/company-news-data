---
schema_version: "1.0.0"
document_id: "09a62f2f43504e328c0e1a2c8c5717a223efa988963f5f440afa33ab69da0922"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/how-to-build-a-multi-branded-design-system-using-supernova"
published_at: "2023-09-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:7a8b8d60c436b3b599230118e5ce9ae1f547b51fdafd881acd537188d147ee9f"
---

# How To Build A Multi-Branded Design System Using Supernova

Teams build design systems for a variety of reasons. Two key benefits that underline almost all teams’ desire to build a design system are improving consistency and streamlining workflows. This is no different for companies with multiple brands and products that share overarching design principles and guidelines but differ in semantics. Instead of building and managing different design systems and teams, you can now build a multi-branded design system with Supernova.


In this article, we'll review real-world use cases for multi-brand design systems and how to build one yourself in Supernova. You can also skip ahead to the[guide](https://www.supernova.io/blog/how-to-build-a-multi-branded-design-system-using-supernova#Building%20a%20multi-branded%20design%20system:~:text=Building%20a%20multi%2Dbranded%20design%20system) .


## Multi-brand design system use cases


The idea of a multi-branded design system is simple — you have a central source of truth for your entire organization. It houses all the design elements, components, and documentation but can be separated by brand for specific differences. So why would you need such a thing? Mergers are common for huge corporations, and when an organization has multiple brands to manage, consistent design principles need to be in place. Let's take a look at a few examples.


### Air France-KLM — Airlines


Air France-KLM is an excellent example of different brands with the same underlying principles in the case of airlines. You can easily see the similarities by looking at their websites.


The underlying design principles are the same, but the icons, colors, typography, and logos are all brand-specific.


### VW Group — Auto


During their


at Into Design Systems 2022, Thorsten Jankowski, User Experience Lead, and Matthias Fritsch, Design System Lead at Volkswagen AG, shared how they created a multi-branded design system GroupUI. And how it caters to VW Group's large array of car brands. They take a different approach than Air France-KLM, with a strong focus on flexibility.


Here, we can see their model of how the design system works, with brands developing their design decisions and foundations. It would go through the framework of the GroupUI design system and result in the final UI kits.


### News UK — Media


Image credit:[NewsKit](https://www.newskit.co.uk/)


With various brands to support across different media types (news, radio, and TV), News UK's design system — NewsKit — needs to be robust. Its documentation must account for every possibility to be usable and consistent across all these products. They encourage different brands to set up their Figma library architecture with separated brands and the Theme Swapper plugin to support multiple brands.


Image credit:[NewsKit](https://www.newskit.co.uk/)


Many other examples of multi-brand design systems and other potential use cases exist. What can we learn from these three examples to build our multi-branded design system, and how can we do it in Supernova?


## Building a multi-branded design system


From the examples we shared, we can get an idea of what we need to support a multi-branded design system:


- Supporting multiple data sources
- Theme/brand switcher
- Flexible and robust documentation
- Design system manager


Let's see how you can do all that and more with Supernova in just a few simple steps.


### Multi-branded design system in Supernova


We'll follow the basic steps in the video and go into more detail on each step.


#### Prerequisite


Your only two requirements for creating a multi-branded design system are having separate[Figma](https://figma.com/) libraries for each brand and a[Supernova](https://cloud.supernova.io/signup) account on a Team, Company, or Enterprise plan.


#### Step 1 - Create your brands in Supernova


Sign in to your[Supernova Cloud](http://cloud.supernova.io/) account, and you'll be met with the default home screen after creating your design system. You'll find the brand switcher in the top left corner with the Default brand.


Click on the brand switcher, and you'll have the option to **Edit brands** or create a **New brand** . Click on **+ New brand,** and you'll see a pop-up. Add the name of the new brand and its description. In our demo, we'll call them Brand A, B, and C.


#### Step 2 - Importing your brands from Figma


After setting up our brands, we want to go to **Data sources** from the left sidebar. And you'll find the brand switcher in the same place (upper left corner), this time with our new brands. Ensure you're on the brand you want to import and click **+ New data source** in the middle. The New data source pop-up will appear.


Here we need to go to our brand library in Figma and copy the library URL from your browser. Go back to Supernova and paste the URL under **Figma file URL** . Make sure everything you want to import is checked and **Automatic updates** is toggled on. We'll get into more details on Automatic updates later on.


**Repeat Step 2 for each brand.**


#### Step 3 - Check your brand design data


You should now have your multi-branded design system set up. We want to check and make sure your design data is all there. Go to your[Tokens](https://www.supernova.io/design-tokens) page from the left sidebar. You should have a populated Tokens view. For our brands, we separated the differences using our Semantic tokens and kept a common color palette.


You can now easily switch between your brands from the brand switcher to quickly see the differences and similarities.


#### Step 4 - Documenting multiple brands


A design system is only complete with[documentation](https://www.supernova.io/documentation) . Supernova gives you complete flexibility in organizing and documenting your multi-branded design system. Your documentation is all in one central place, but you can separate it by brand using tabs. You can also add brand-specific designs to any documentation page. You can also use our guide on[documenting your design system in Supernova](https://www.supernova.io/blog/how-to-use-supernova-for-design-system-documentation) .


Go to your **Documentation** from the left sidebar, where you'll be met with the default setup for your documentation. In the **Tokens** section, make sure you have three tabs for each brand. You can easily create any number of tabs or folders to suit your design system. We want to head to one of the brand's empty design tokens page. And using the forward slash “/” to start creating a block. Write “token” and you should get options to choose from — click on **Token group** .


You can select which tokens you want to see. In this case, let's use **Color** and select the **Semantic** color group to showcase the differences between our brands. You should see a brand switcher in the top middle of your **Token group** block. Click on it and select the brand you want to showcase on this documentation page.


Feel free to add similar **Token groups** to your other brand pages. You can also add any other design tokens that you've imported, as well as icons and typographies.


#### Step 5 - Publish and preview your documentation


Now that our documentation has all the branded tokens we want to showcase. We can click **Publish** in the top right corner. We can then preview our documentation site and see our multi-branded design system.


#### Step 6 - Automatic updates


Say you wanted to change your original designs in Figma — a slight alteration to your primary color, for example. Typically, you'd have to manually import your design file again and update all the outdated documentation you have. But with Supernova, your design system is built to scale and meet your needs.


With automatic updates toggled on. All you have to do is save your Figma file, and Supernova will check for any new updates every hour. If Supernova detects a change in the Figma file, it will automatically change the primary color token to its new value in the Tokens view, and everywhere it's used in your documentation. And even if you don't have Automatic updates toggled on or want an instant refresh, you can go to the Data sources page and click **Get updates** .


---


Your multi-branded design system should now be set up. We hope you found this guide helpful. Supernova will help you easily scale your design system with many features catering to your team. Check out our[Blog](https://www.supernova.io/blog) to learn more, or find us on[Twitter](https://twitter.com/supernova_io) for all the latest news.
