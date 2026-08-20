---
schema_version: "1.0.0"
document_id: "59a696207b83fcdb423bcf4a759743673510011983602718878d5bd973bbe639"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/flow-and-react"
published_at: "2017-05-22T18:07:31+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:76fef9645415c6827e5dfa92b5cfa5333116675085872684eeacb991c149a19b"
---

# Using Flow to Write More Confident React Apps

Writing your first[React](https://facebook.github.io/react/) app is simple with tools such as[Create React App](https://github.com/facebookincubator/create-react-app) , but as your application and your team grow, it becomes more difficult to write scalable, confident code efficiently. Think about the number of times you pass props around a React application and expect them to look a certain way or be of a certain type, or the times when you’ve passed the wrong type of argument into a function and rendered a page useless. Fortunately, there are many solutions for these preventable problems, and one of the tools we’ve found helpful here at Rent The Runway is[Flow](https://flow.org/en/) .


# What Is Flow?


According to[Flow’s official website](https://flow.org/en/) , Flow is a static type checker for JavaScript. After you install Flow, start the Flow background process, and annotate your code, Flow checks for errors while you code and gently alerts you to them. There are even Flow packages available for many popular text editors, which can highlight potential problems and provide explanatory text right beside the code itself.


Many times, Flow can infer how you want your code to work, so you don’t always need to do extra work, but it helps to be certain by actually using the static type annotations. In short, Flow checks how data moves through your app, ensuring that you’re using the types of data that you expect and forcing you to think twice before you reassign a variable to a different type, or try to access a non-existent property of an object.


# Basic Example


Here’s a basic example of how to add Flow type checking to a file.


This is a simple component to display and edit data for a customer’s account.


Telling Flow that it should check this file is as simple as including the \`// @flow\` flag at the top of the file. Almost immediately, Flow will begin checking the file, and you’ll notice red dots in the text editor (Sublime) warning about potential problems. When we hover over those lines, we see additional information about the error at the bottom of the text editor.


After tackling the highlighted lines one by one, we’re left with a nicely annotated file:


Here, we’re doing the bare minimum to silence the red dots, telling Flow that the \`constructor\` method accepts a \`props\` argument that should be an object. (Flow allows you to be more specific, which we’ll show in a bit, but this suffices for now). In addition, we’ve explicitly declared the properties and types of each property for the class, which is part of Flow’s Class type checking capabilities. Now, the shape of state and the types of its properties are guaranteed, and we only had to do a bit of annotation for Flow to get started.


To be more explicit, we can also declare the types of properties that an object when supplied as an argument to a function must have. Below, on line 15 of the CustomerAccountProfile component (right), we specify that the props must have a customer property.


In that component’s parent, CustomerTabAccount (left), on line 75, you can see that when we try to pass in ‘null’ as a prop, Flow gives us a warning that ‘This type is incompatible with (object type)’.


To go one step farther, you could even declare the types of all the properties within props for the component. Now, if you try to reference a property that isn’t declared with the props, Flow will let you know that the property is not found.


Although this is just a sample of the many type checks Flow is capable of, the[Flow documentation](https://flow.org/en/docs/types/) goes pretty deep into all the possibilities.


# Advantages


What’s great about Flow is that it can catch errors early and help development teams communicate better about how components work together within an application. And although React’s[PropTypes](https://github.com/reactjs/prop-types) serve a similar purpose and are arguably simpler to use, they only throw errors at runtime rather than while you’re coding. Plus, since they’re declared below the component, they seem more like an afterthought.


Beyond the basics, adding Flow to your codebase is fairly painless, since it’s opt-in. You tell Flow to analyze a file by including a \`// @flow\` flag at the top of a file. This will mark it as a Flow file, and the Flow background process will monitor all Flow files. Files without the flag will be ignored, so you get to control which files you check and you can integrate it into a new or existing codebase over time.


It also plays nicely with ES6 and Babel, which strips out all the annotations for production.


# Disadvantages


On the other hand, that same opt-in policy leaves room for neglect. Less diligent programmers can opt to forgo Flow altogether. In addition, there is a learning curve for type annotations, especially for new developers who aren’t used to type checking their code. What I find to be a bigger challenge is not getting started, but using Flow well. Basic type checking is useful, but Flow offers the ability to write very use-case specific type checks such as “Maybe” types, which check optional values and “Class” types, which allow you to use a class as a type. It’s easy to begin using Flow, but difficult to master all of its capabilities.


We began adopting Flow slowly in one of our retail applications, and as we continue building new features, we continue to convert older components to use Flow. Overall, it has helped us identify bugs sooner rather than later and has enabled easier refactoring because we can be more confident about the arguments functions accept and how different function invocations change a component’s state.


For more information on Flow and getting started, consult the[official Flow documentation](https://flow.org/en/) .


# Resources


[Official Flow Documentation](https://flow.org/en/)


[React PropTypes Versus Flow](http://stackoverflow.com/questions/36065185/react-proptypes-vs-flow)
