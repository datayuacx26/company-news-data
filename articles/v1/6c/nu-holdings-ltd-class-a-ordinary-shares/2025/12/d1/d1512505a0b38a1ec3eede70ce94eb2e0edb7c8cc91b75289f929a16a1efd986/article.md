---
schema_version: "1.0.0"
document_id: "d1512505a0b38a1ec3eede70ce94eb2e0edb7c8cc91b75289f929a16a1efd986"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/building-elegant-interfaces-with-clojurescript-react-and-uix/"
published_at: "2025-12-23T15:26:47+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:37130c9c936f6afb791c8c8637e2fc1e4d315ef0005b4935ccfce35619a80ac6"
---

# Building elegant interfaces with ClojureScript, React, and UIx

During Clojure South, João Lanjoni, Software Engineer at Nubank, addressed a central challenge of modern web development: how to combine the ergonomics of ClojureScript with the maturity of React to build scalable, high-performance interfaces.


According to João, the solution is UIx, a tool that represents the new generation of bridges that further aligns the Clojure universe with the React ecosystem. In his session, he detailed the context, the limitations of previous approaches, and the value of UIx as a new, efficient entry point for React developers into ClojureScript.


## From 2013 to today: React and ClojureScript in perspective


Since its launch in 2013, React has redefined the structure of frontend applications by introducing concepts like consistent reactivity. The ClojureScript community quickly responded with idiomatic interfaces like Reagent, which became the de facto standard due to its solidity, providing a minimalistic interface between ClojureScript and React, using a Hiccup-like syntax to define components. With the arrival of functional components and hooks, starting around 2019, new interfaces came up to provide a direct way of using functional components (instead of old class-based components).


However, as React continuously evolved towards modern patterns, including concurrent rendering, functional components, and new ways to manage component state, Reagent remained tied to class-based components, mainly for backward compatibility. This mismatch resulted in some limitations like performance limitations in large codebases (due to Hiccup parsing in runtime), issues with functional components (as users may have to declare every functional component usage even when they were defined as a React standard), and hindered interoperability with modern React libraries, such as Material UI, Mantine, and Ant Design, widening the gap between the two ecosystems.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## What UIx changes in your code


UIx emerges to resolve this divergence. Acting as a thin interface between ClojureScript and modern React, its focus is technical and pragmatic: it offers a minimal abstraction layer, more predictable performance, and the direct use of functional components and hooks. Furthermore, it ensures native interoperability with the React ecosystem, allowing the lifecycle and state management to be handled directly by React itself.


> “If React already handles state and lifecycle management well, why not let it do that?”
>
>
> João Lanjoni, Software Engineer at Nubank


Instead of creating a complete framework or adding unnecessary abstractions, UIx is a lightweight bridge, leveraging what modern React does best, resulting in a ClojureScript codebase with idiomatic syntax but identical behavior to modern React.


## UIx component structure


In practical terms, UIx centralizes component construction around two elements: *defui* for declaring React components and *$* for rendering elements in an explicit and lightweight way. Component bodies process props identically to React. Hooks such as *useState* are exposed using idiomatic ClojureScript conventions, like *use-state* , with UIx handling the translation to native React APIs. This ergonomics combines the best of ClojureScript syntax with the React architecture, which, according to João, eliminates the need to train React developers in the internal details of layers like Reagent or Re-frame, keeping the mental model aligned with the React mainstream.


## Performance in figures


A highlight of the presentation was the chart, created by Roman Liutikov – the UIx maintainer –, comparing the call stack depth when rendering a simple component in pure React, UIx, and Reagent. React exhibits the shortest path; UIx, by adding only a thin layer, follows closely. In contrast, Reagent, due to Hiccup being interpreted at runtime, shows a significantly deeper call stack. While the difference is minimal in small applications, the impact on predictability and performance becomes notable and increases in products with hundreds or thousands of components.


## Who is already using UIx in production


João presented three real-world examples, all highlighted on the project’s official page:


- **Metosin** , one of the largest Clojure consultancies in Europe;
- **Pitch** , an AI presentation platform with amazing slide decks;
- **Cognician** , an e-learning platform for personal development.


The Pitch case is particularly impressive.


The team migrated 2,500 components from Reagent to UIx, maintained compatibility with Re-frame, and saw improvements in predictability and performance.


Metosin, meanwhile, employs Juho Teperi, one of the main contributors to Reagent, who also made an example project for a full-stack app using Clojure and ClojureScript and chose UIx to build the web interface, also using Material UI as the component library without any special wrapper.


When someone who helped build the previous tool begins to advocate for the new approach, it says a lot about the current moment of the technology, even more with the launch of a new version of Reagent introducing default functional components and a thinner hooks wrapper (inspired also by UIx).


## Reducing the developer learning curve


UIx’s value extends to the hiring and development of engineers, which opens a path for more professionals to enter the ClojureScript ecosystem without the requirement of mastering the intricacies of Reagent, Re-frame, or the atom-based state model from day one. It represents a pragmatic approach to lowering barriers without sacrificing the benefits of a functional and declarative language.


> *“The greatest value of UIx is allowing React developers to write ClojureScript with a minimal learning curve.”*
>
>
> *João Lanjoni, Software Engineer at Nubank*


## When UIx is the best choice


UIx is especially recommended for modern and complex front-end applications and teams already familiar with React. It is ideal for codebases that rely heavily on hooks and for projects requiring interoperability with the latest React libraries, with a view toward strong long-term growth potential. The library, intentionally simple, does not attempt to reinvent global state management, maintaining compatibility with mature React libraries like Zustand and Jotai, instead of adding unnecessary layers, or even using a custom hook that subscribes to a Clojure atom to manage a global state (similar to those cited libraries).


In essence, UIx does not seek to replace React but rather to act as a thin, modern, and pragmatic bridge. Its goal is to allow teams to build scalable front-ends with the power of React, while preserving the expressiveness and elegance of the Clojure philosophy and syntax. For complex and modern projects in ClojureScript, UIx may be the missing link.
