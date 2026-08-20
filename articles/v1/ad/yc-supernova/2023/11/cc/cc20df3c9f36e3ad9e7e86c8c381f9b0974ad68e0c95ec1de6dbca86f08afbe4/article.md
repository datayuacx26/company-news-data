---
schema_version: "1.0.0"
document_id: "cc20df3c9f36e3ad9e7e86c8c381f9b0974ad68e0c95ec1de6dbca86f08afbe4"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/understanding-the-differences-between-figma-variables-and-design-tokens"
published_at: "2023-11-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:717d998c2d3ea6bfe12a4d4a32097c645a73ce551f1b2cbeaec2cea73d79cc9a"
---

# Understanding the Differences Between Figma Variables and Design Tokens

[Raquel Pereira](https://www.linkedin.com/in/raqper/) *is doing Design Ops at*[Volkswagen Digital Solutions](https://www.vwds.pt/) *, working for*[MAN Truck at Bus](https://www.man.eu/corporate/en/homepage.html) *. On a daily basis, she enhances design and research processes, advocating for a better design culture, not only at MAN but also in the VW group. She also created and teaches an online remote intensive course at*[TheStarter](https://www.thestarter.io/) *-*[Implement Design Systems at Scale](https://www.thestarter.io/design-systems-implementacao-em-escala-thestarter) *- preparing the next generation of design professionals in Portugal and Brazil. Last but not least, she's a Community Advocate at*[Friends of Figma Portugal](https://friends.figma.com/portugal/) *where they organize events, talks, and workshops. She proposed the*


*a live stream series where we have fun exploring design systems made on Figma.*


As many in the design community are aware, Figma announced a significant update in June, introducing their answer to tokens with Figma Variables, among other features. This has spurred a wave of exploration and adoption, with some users eagerly integrating the new tool into their workflows and sharing their insights. Meanwhile, others remain cautious, deliberating whether it's premature to embrace the change. Amidst this buzz, a recurring question arises: what exactly differentiates design tokens from Variables?


## Defining Figma variables and design tokens


According to the[Design Tokens Technical Reports from W3C Community Group](https://tr.designtokens.org/) , “ *design tokens are indivisible pieces of a design system such as colors, spacing, typography scale* ”. In a more technical definition, a design token “ *is information associated with a name, at minimum a name/value pair* .”


[Example from the same report:](https://tr.designtokens.org/format/#name-and-value)


[‍](https://tr.designtokens.org/format/#name-and-value) Here we define 1 design token with the following properties:


- Name: "token name”
- Value: "#fff000"
- Type: "color"


However, when we want to define[variables](https://help.figma.com/hc/en-us/articles/15339657135383-Guide-to-variables-in-Figma) in Figma, they are “ *design store reusable values that can be applied to all kinds of design properties and prototyping actions.* ” As Carly Ayres mentioned in this[article](https://www.figma.com/blog/the-future-of-design-systems-is-semantic/) from Shortcut (Figma’s blog), “ *more than just codifying design decisions, variables also literally vary to unlock design theming and dynamic prototyping logic* ”.


[Samantha Gordashko](https://www.linkedin.com/in/Sgordashko/) explains this very well


in case you would like to listen with some visual support.


So what does that tell us? **Design tokens are not the same as variables, but variables can store design tokens.**


## Best practices for naming conventions


According to the[W3C Community Group](https://tr.designtokens.org/format/#types) , we define tokens as being singular when they have a single value, and composite when there is a combination of singulars into one token:


Based on this, we can plan and brainstorm with designers and developers on how to create the best formula to name our variables and/or design tokens.


What’s most important is that your token naming convention works for your company and, therefore, your teams, so don’t feel pressured to copy past the formula of big companies before you give a chance to try it out first to create a solution for your reality.


As Figma[explains](https://help.figma.com/hc/en-us/articles/18490793776023-Update-1-Tokens-variables-and-styles#token-organization) it, “each type communicates the **what** (primitive), **how** (semantic), and **where** (component-specific) of a token.”


We usually start to name all values that exist in our reality — primitive or global tokens. After that, we create a purpose for those when we apply it in our components — semantic tokens. Here’s a reference of this logic applied on an example on Figma’s YouTube video, “


”, with[Luis Ouriach](https://www.linkedin.com/in/luisouriach/) and[Jack Miller](https://www.linkedin.com/in/jjcms/) .


You can still create another level — component tokens — but it’s common to only exist defined on the code side since they're already too technical to be on Figma. However, these levels will live in your world, depending on your naming convention formula will work out when you start to scale.


## Difference between variables and styles


The design community also asked: what about styles, and what are the main differences? Besides some technical differences, the main feature when it comes to variables is that **when you want to use design tokens,** **a style isn't guaranteed to work as a source of truth, while a variable can** **do so, which is one of the core goals of having a design system.**


Here’s an example, focused on color variables:


I’ve made some simple components inspired by Domino’s Pizza. One group of components has variables, and the other one has styles.


First I created some color styles and variables, corresponding to global/primitive values.


With the variables, you can create a global collection. The best approach when you have multiple types of global values is to rate them to scale your palettes. For instance, you might have Yellow-100, Yellow-90, Yellow-80, etc, from darker to lighter tons inside the yellow family.


When you don’t have any token mindset or concern, you would simply apply colors as they are, like this:


For our use case where we don’t have any token logic — styles and variables have similar behaviors. If I update a style, it would act the same as when I update a variable:


However, once we start to implement some token logic, creating an alias between global with semantic tokens, we now need to create a style token library to respect the token naming convention. This might be most common scenario for multiple companies and teams at the moment. Here’s the application when you use semantic tokens, with styles and variables:


The main difference is that if I change the value of a variable that is an alias to another — in this example, updating a global variable that is an alias to a semantic one — it will update in all places where they're applied because they're connected. If I update the global style, independent from the semantic one, it won’t make a difference:


For people who are using Tokens Studio with a free account, you'll have to go through more steps and effort to guarantee consistency when updating your components. Since you can only import or create styles, and it’s only possible to sync in one Figma file, you might need to have all things in one Figma file. Therefore, having them in one library (colors, typography, components, etc) — which in can be a problem for certain teams. However, with the paid account, not only you can sync in multiple files but also create and maintain variables as well, besides styles.


With Figma variables, this becomes a huge advantage to maintain consistency since you can save a lot of steps when updating and releasing components, since you can update and manage variables in multiple files. And if variables are well constructed, the updates are instant and consistent with multiple assets. You also have the feature of “hiding for publishing” when you’re editing variables, so you can avoid publishing variables libraries for those you don’t need to see — which is also a huge feature.


## Variables are still in beta, but there’s more to come


At the moment, the available variable types are:


For number variables, you can define min and max sizes, spacing, and corner radius values. For creating components, this has a lot of potential to keep UI patterns in multiple assets.


String variables can be useful to patronize UX writing in multiple components: you can define, for instance, the 3 major CTAs that you would usually have in a button, preventing someone would using a different text:


Boolean variables are the most challenging ones to see their value on components since they are more focused on use with modes or in prototyping because you cannot apply them to the boolean properties of your components yet.


Another major feature when you’re editing your variables is that you can choose where these variables should appear when it’s time to apply. Instead of having multiple values in a big list, you can decide which variables make sense to choose as an option for each type, helping consistency. This feature appears when you’re editing a variable:


Last but not least, on each variable you can even specify their code syntax for the platforms that you need: Web, iOS, or Android.


Be aware that variables are in beta, but there are already plans for future variable types including:


- Typography
- Stroke width
- Effects color (like gradients or shadows)
- Layer opacity
- Grid
- Image
- Component property type


## How can we use variables in our design system?


### With Modes


We can see modes(or themes) as the next level of variables: a combination of multiple variables can compose a mode. The first example that comes to mind regarding modes is creating dark and light modes, for instance. The current pain point is that you can only use up to 4 modes on Figma unless you have an Enterprise Plan, which then you will have 40 modes.


The most common use cases using modes so far are for:


- **Theming:** you can have light and dark modes but also other themes that might fit your business needs.
- **Languages:** in case you work in a multi-language business, this might be interesting to apply and see the implications when using English with German, for instance.
- **Sizes:** you can also use modes for different resolutions, preparing your design system to be more responsive, which is usually a big pain point for library maintainers.
- **Brands:** It’s also common for multi-brand design systems to use modes for the same component applied in different brands.
- **Frameworks/Code source:** In case you work in a company with multiple solutions for coding, but you want to use the same component as much as possible and create some adjustments for each one of them, you can use modes so the designer is aware of which option can use.


Image from Figma


Modes can be applied on:


- Layers
- Frames
- Components and component sets
- Sections
- Groups
- Pages


Image from Figma


### With Collections


Collections are usually used to create multiple levels of tokens: global, semantic, or component tokens, etc. However, in case you don’t have an Enterprise Plan, you can use collections to try to replace what modes can provide: Collections by brands, themes, sizes, etc. It might multiply the number of collections or it won’t replace the mode functionality, but it’s a possible solution to explore if it fits your needs.


The main difference between modes and collections is with modes we can switch the UI much faster, keeping the variables' naming more consistent, and with much less effort.


## When should you start using variables?


### **If you don’t have any design token logic implemented**


Start right away! With time, map your current needs among designers and developers, and explore the naming conventions together in workshops to brainstorm proposals.


### **If you already use Figma styles with some code naming convention**


Try to slowly build your collections and modes, fitting your company's needs — usually, it’s easier to implement step by step than all at once. Try not to depend on these variables yet for your current components and create testing libraries so you can see how it goes. In the meantime, Figma will launch more types of variables, and afterward, you can also add those.


In case you are considering upgrading your styles to variables, try the[“Styles to Variables converter” plugin](https://www.figma.com/community/plugin/1253585487427690087/styles-to-variables-converter) . Not only will it convert your styles into variables, but also associate each style with the respective variable. You might have to adjust the alias between global and semantic collections, but it’s already a lot of time spared.


You can also[import a JSON file via this Figma plugin](https://www.figma.com/community/plugin/1253424530216967528/variables-import) to see how it works out.


### **If you use Tokens Studio**


Design Tokens Studio has way more variety when it comes to creating tokens, which is one of their strengths at the moment since it’s very code-friendly as well. In case you already have a lot there defined, since variables are still in beta, it’s understandable that you don’t want to jump right away.


In case you don’t have the Figma Enterprise Plan, it’s understandable that you think it’s not reasonable to use variables since it’s the rest API is not available, and you will lose the code sync that Design Tokens Studio offers even with a free account, which is a major advantage.


In case you’re in the Enterprise Plan on Figma, it gives the advantage of fewer steps when you update and manage components as we’ve seen during this article. You can start defining a transition plan internally and evaluating the priorities with your team.


### **If you want to invest in Figma Variables 100%**


Making a business case can help you justify the investment to upgrade your design system can help you. In case you don’t have the Enterprise Plan, you cannot use the variables REST API, which is a big con for most small/medium companies, and may not be a realistic approach. So ask yourself and your team if you need to be Enterprise to use variables 100%, from design to code? Maybe the Organisation plan should be worth it to be eligible as well in the future.


## How do Variables work on the code side?


We shouldn’t forget the final round — your design system also lives in a code repository, where designs become reality.


Figma has 3 types of API: Plugin, widget, and REST. You can see the main differences in more detail[here](https://www.figma.com/developers/compare-apis?fuid=1123195745429566946) , but when it comes to variables, the REST API is the only one we can sync our variables to our code repository.


One of the great benefits is that you can use this API in multiple files, which is something that with Tokens Studio in a free account you can’t yet.


However, this code sync with variables is only available for the Enterprise Plan plan at the moment, so this could be a major pain point for multiple teams and companies to consider implementing in case they have Organisation or other types of Figma plans. But that doesn't mean there's no solution.


## Are there any other tools to sync Figma Variables with code?


Yes, and not only can do that, but also all the other needs that a design system requires:[Supernova](https://www.supernova.io/your-design-systems-partner) . It works as a source of truth for your design systems, having an all-in-one platform.


Supernova is code-friendly for design system maintainers, since you can build and edit your[documentation](https://www.supernova.io/documentation) without knowing any code, and you can even see your code live and make your design system documentation interactive. They even have a great feature to check your design decisions, which is a component checklist, to make sure that you respect your patterns every time you create something new.


But with more focus on[code automation](https://www.supernova.io/code-automation) , you can create a pipeline to connect your design data to your codebase, which has a huge potential to keep consistency, especially if you are considering using Figma variables — no more asking developers to push token updates!


You can have multiple tech stacks: CSS, React Web, React Native, Flutter, iOS, Android, and more, and integrate into your code repository, whether you use GitHub, Azure DevOps, Gitlab, BitBucket, or customized REST end-point.


Improving your workflows on Figma, they even have their plugin to make your life easier to sync variables.


So, depending on your current situation, in case you need to invest in your design system to keep up with the market trends, Supernova might be a great solution, since it offers not only an easy way to maintain[Figma variables](https://www.supernova.io/blog/how-to-manage-figma-variables-with-supernova) , but all the other needs that a design system requires: documentations, assets, code, etc.[Sign up](https://cloud.supernova.io/signup) for a free account to try it for yourself.
