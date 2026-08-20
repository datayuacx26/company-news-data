---
schema_version: "1.0.0"
document_id: "8498cd150d0ae2ec3078034a07a86b5822e4d1d16ae0c35c8fcabb2a535a885a"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/redux-doghouse-reusable-react-redux-components-through-scoping/"
published_at: "2016-11-14T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:29.470006+00:00"
content_hash: "sha256:1ca2b17d86fe3f27b2bf0c49171e711d6bc3d81068de98d20b9425cf70ec7ab8"
---

# Redux-Doghouse: Creating reusable React-Redux components through scoping

Zacqary Adam Xeper


Today, we’re open-sourcing[Redux-Doghouse](https://github.com/datadog/redux-doghouse) , a library for[Redux](http://redux.js.org/) that helps you create **Scoped Actions** and corresponding **Scoped Reducers** . This pattern is useful for creating components with Redux which can be reused multiple times in multiple contexts, but with a **scope** that prevents them from conflicting with one another.


## What Doghouse does


At Datadog, we love Redux. It’s a fantastic way of managing the complex, inter-dependent states of multiple bits and pieces of a UI across an application. Redux breaks down the state of your application — and the ways in which that state updates in response to the user — into easily reusable, composable functions. Then, Redux stores all of that state in one place, which suddenly makes it easy to maintain code in which user input on one part of your UI can affect other, vastly different parts of it.


We also love[React](https://facebook.github.io/react/) , which is great for building reusable UI components that can be placed into almost any context and still look, feel, and behave the same way. Redux was built to work together with React, and it definitely works well. But we ran into a snag when we needed to take some Redux code that we had written for one UI component, and then duplicate that UI element multiple times inside a larger Redux application.


Because of the way Redux responds to **actions** of a certain` TYPE` , clicking a button on one instance of that component would affect *all* of the other instances of that component, as if the user had clicked the button on all of them at the same time! As stated before, what makes Redux great is that you *can* have a single action propagate changes to your entire UI — but this isn’t how we wanted to do it.


Rather than refactor the existing code, we decided to put those instances in doghouses.


Redux-Doghouse allows you to build reusable UI components with corresponding Redux logic that are completely unaware that they’re children of a larger Redux application. When you write a` <ReduxUI>` component to dispatch and reduce for a certain` MY_ACTION` type, it works fine when it’s the only instance of itself. But if you try to create multiple instances of it in a larger app and connect them all to the same Redux store, suddenly the actions of that same` MY_ACTION` will affect *all* instances of the UI element.


With Redux-Doghouse, you can compose a unique **scope** onto each instance of the component’s` actionCreators` and` reducers` , and only allow` MY_ACTION` s to hit the` reducers` of the` <ReduxUI>` instances that actually dispatched them.


But you still get all the benefits of an interconnected Redux application, allowing you to respond to` MY_ACTION` s and access the internal states of each` <ReduxUI>` instance at a higher level. Think of it almost as **extending** the behaviors of your` <ReduxUI>` components without overriding them.


## Our use case


If you go into a Datadog dashboard and edit a graph tile, you’ll see a **Query Editor** for each metric on that tile. Each one of those Query Editors is like a little miniature Redux app embedded into the page, rendered into a React component.


We rebuilt the Query Editor this way with the intention of converting the surrounding UI elements to React and Redux later, and to allow us to easily reuse the Query Editor in other parts of the application. Before, Query Editors could only live in the Dashboard’s graph editor, but now it’s fairly easy to reuse them in the Monitor editor, the Notebook editor, or wherever else in the app we need to put a Query Editor.


We ran into the problem when we needed to build a React/Redux version of what we call the **Expression Editor** — the Advanced Mode tool which allows you to combine multiple metric queries into one.


The Expression Editor needs to:


-


Create an arbitrary number of Query Editors, each representing a single metric query


-


Show an Expression Composer, which is the little text box allowing you to combine queries like` a + b / c` , but error out if you try to type a letter corresponding to a query that doesn’t exist (` a + d` would be invalid in the pictured example)


-


Enforce that every query has **either** the same *group by* value — in this case, the box that says` host` — **or** has an empty *group by* value (i.e. queries A and B can group by` host` and C by nothing, but A and B can’t group by` host` and C by` device` )


This kind of stuff — examining the data structure of multiple child components, having an action taken in one child cause a reaction in its siblings — is Redux’s bread and butter. The question was how to deal with actions.


-


Opening the group list and clicking a new one will cause the Query Editor to dispatch a` SET_GROUP` action. Its reducers then listen for, and react to, this` SET_GROUP` action.


-


This works fine if a Query Editor is the only Query Editor in the whole Redux app. But we want Query Editor A to only listen to a` SET_GROUP` action from Query Editor A, and Query Editor B to only listen to a` SET_GROUP` from Query Editor B


-


But we **also** want the Expression Editor as a whole to listen for a` SET_GROUP` action, and then enforce the *group by* rules across the entire expression


-


*And* we can’t modify the Query Editor to the point where it’s no longer able to operate independently in other contexts; the Query Editor shouldn’t know that its parent Expression Editor exists


This is where Redux-Doghouse comes in. Using Doghouse’s helpers, the Expression Editor adds a little bit of metadata to each action dispatched by each Query Editor: a **scope** , like A, B, C, D, etc. The Expression Editor also wraps each Query Editor’s reducers with a function to catch that metadata, and only route each **scoped action** to its corresponding **scoped reducer** .


## Why Doghouse over the alternatives


The Redux-Doghouse pattern helps you keep complicated pieces of your UI code separate from one another, so that you can split your Redux reducers and actions into modules the same way that you split your UI components. It can make things a lot easier from a development standpoint if everything is split up by component, rather than view (React) code being split one way and model (Redux) code being split another.


However, had we ported our Expression Editor to React and Redux from the top down — rather than building the Query Editor first and later building the Expression Editor — it’s possible that we could have built a structure of actions and reducers that wasn’t split at the level of each individual Query Editor. (Although, in our case, the Query Editor is an incredibly complicated piece of code, so it’s a sensible point of separation anyway.)


Development of a large application doesn’t always happen from the top-down. It especially never happens in a 6-year-old codebase for a product that hundreds of thousands of power users depend on every day — the only way we can realistically implement new technologies like Redux is piece by piece, bit by bit. Our requirements are changing every day, and sometimes existing code from one piece of the app becomes useful for other new features in ways we never imagined before.


That is why it is invaluable to be able to put multiple instances of a Redux-enabled component next to one another in the same structure, and have them all behave as expected. The more reusable our code is in more contexts, the faster and better we can work.


## Check out the repo


You can get started with Redux-Doghouse by having a look at its[Github repo](https://github.com/datadog/redux-doghouse) . You’ll find API documentation, a usage example, and installation instructions. Give it a try in your Redux project, and feel free to use the Issues section for any questions, comments, or bug reports.


-
-
-
