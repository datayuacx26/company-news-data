---
schema_version: "1.0.0"
document_id: "316ac7f4b16e35847e0164d176da56c7c9643fa419000f3b1fa7b3f55d166ba3"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/design-patterns-to-build-ui-components-library-in-react-f547ebbd0e46"
published_at: "2020-10-13T05:22:28+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-08-20T01:50:37.555197+00:00"
content_hash: "sha256:a5b7d14de8e669e7717fa3f2e87daab08e7722a1d2f88ebd5f12191839db0e49"
---

# Design Patterns to build UI Components library in React

### Design patterns to build UI components library in React


What will we learn through this article?


- **Why do we need a UI System?**
- **Creating UI Design System**
- **Primary and Secondary Components**
- **Designing Component APIs**


### Why do we need a UI System?


To build **consistent** UI across every part of your application can be daunting. A UI system helps your users **navigate intuitively** and makes it uniform across your product, making them familiar with your **design language.** They help teams by giving them a more structured and guided way to build UI for their products. Let’s create one!


### Creating a UI Design System


The best way to start is by defining styles, colors, spacing, text sizes, etc. Design system is the skeleton for any UI library, which is single source of truth for all the styling rules.


To create a design rule system for React library we will use Styled-Components. But Design System for components is not limited to styled-components, we can also do with CSS variables or other styling library. In this case we will be using styled-components only.


Let create rules for styling:


We have defined high level configuration for styling. Let’s see one by one.


#### Colors


#### Typography


Along with the headings and body text styles, we can also define button, input, etc text styles.


#### Spacing


Let’s create a function for spacing. The size of spacing is multiple of 4.


#### Z-Index


#### Breakpoints


We are using standard screen sizes as our breakpoints. We can define more breakpoints depending on our use case.


[See this documentation](https://styled-components.com/docs/advanced) to use the theme in styled-components.


### Primary and Secondary Components


Let’s look at what are primary and secondary components for an UI library.


five distinct stages


The components at atomic level are our **primary components** . For example, inputs, buttons, img, headings. The components which can be build by assembling these atomic components are **secondary components.**


And sometimes we need a component to switch between several visual states, depending on certain conditions. Meaning you need the component to change, perhaps, color to convey different information. For example:


- A button can be normal, primary, secondary, disabled, etc.
- A list item can be selected or not selected.
- A form element can be required, with error or normal.


A good example of this visual variant pattern is[Bootstrap’s button styles](https://getbootstrap.com/docs/3.4/css/#buttons) .


Button Component Variant


### Designing Component APIs


React component APIs are the component props. Let us standardise props for all the components. There can be two types of props:


1. Global/Shared props
2. Component specific props


**Global/Shared props**


**Component specific props:** Let’s create a button component using base element


Similarly, we can create more variants for button, like ErrorButton, InfoButton, SuccessButton by extending BaseButton component.


There are no hard rules for defining component APIs. But those should be well defined and developer friendly. All shared props should be applicable on almost all the components.


In Cleartax, we built an UI library named as Zoids, to avoid custom styling and to achieve consistent UI across products.


**Why do we need to build an UI library when there are lots of open source libraries available?**
If you’re a startup, a single developer, or if the project is not too complex then I recommend picking an existing component library. However, at some point, you might re-evaluate whether it makes sense to create your component library when you find that existing component libraries don’t fit your needs anymore.


For Cleartax with complex UI products, building our own library make more sense. We used a variant based component design system, which helps resolve our need for atomic level components.


---


[Design Patterns to build UI Components library in React](https://medium.com/cleartax-engineering/design-patterns-to-build-ui-components-library-in-react-f547ebbd0e46) was originally published in[ClearTax Engineering](https://medium.com/cleartax-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
