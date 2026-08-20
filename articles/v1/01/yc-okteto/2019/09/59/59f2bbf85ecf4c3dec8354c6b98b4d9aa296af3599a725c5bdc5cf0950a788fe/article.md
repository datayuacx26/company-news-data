---
schema_version: "1.0.0"
document_id: "59f2bbf85ecf4c3dec8354c6b98b4d9aa296af3599a725c5bdc5cf0950a788fe"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/manage-your-kubernetes-context-directly-from-vs-code/"
published_at: "2019-09-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:da3bcad746e5cb87bf479d9fc63a273dbee0cd8b74c3f6bdb0b53595aa086db8"
---

# Manage your Kubernetes Context Directly from VS Code

# Manage your Kubernetes Context Directly from VS Code


Do you keep deploying your applications into the wrong namespace? Now you can see and manage your Kubernetes context directly from VS Code with[our new extension](https://marketplace.visualstudio.com/items?itemName=okteto.kubernetes-context) 🚀.


Visual Studio Code has taken the IDE world by storm. On[Stack Overflow’s 2019 Developer survey](https://insights.stackoverflow.com/survey/2019#technology-_-most-popular-development-environments) it already appears as the most popular development environment. I’m a big fan of it, mostly because of the extension model. These days, I spend most of my coding time in VS Code windows.


When I’m building Cloud Native applications, I use namespaces extensively. I have dedicated Kubernetes namespace for every application I’m working on, and then I also create one for every branch I’m working on. This lets me switch very quickly not only between branches, but also between different versions of an entire application.


I love working in this way, but every now and then I’ll lose track of which namespace I’m on and I’ll end up running` kubectl apply -f deployment.yaml` in the wrong place. Nothing catastrophic (gotta love this whole infrastructure as code trend), but it makes me lose my flow 😒. On my terminal I use[this plugin](https://github.com/superbrothers/zsh-kubectl-prompt) , but I couldn’t find anything like this for my IDE.


Until now…


Like everything else we do around here, we build this extension to make our lives as Cloud Native Developers easier than ever.[Install the Kubernetes context extension](https://marketplace.visualstudio.com/items?itemName=okteto.kubernetes-context) , reload your VS Code instance, and your current context name and namespace will be displayed in the bottom right of the screen. Need to change it? Click on the status bar, and select another one, without losing your flow.


I hope you find it as useful as we do. Let us know what you think about it[on Twitter](https://twitter.com/oktetohq) , or in the[Okteto community forum](https://community.okteto.com/) .


Ramiro Berrelleza


CEO & Co-founder


[View all posts](https://www.okteto.com/blog/authors/ramiro-berrelleza/)


[visual-studio-code](https://www.okteto.com/blog/tags/visual-studio-code/)


[development](https://www.okteto.com/blog/tags/development/)


[kubernetes](https://www.okteto.com/blog/tags/kubernetes/)


[cloud-native](https://www.okteto.com/blog/tags/cloud-native/)


#### Share this:
