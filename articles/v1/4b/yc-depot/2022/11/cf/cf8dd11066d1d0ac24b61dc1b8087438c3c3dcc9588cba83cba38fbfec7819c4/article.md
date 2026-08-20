---
schema_version: "1.0.0"
document_id: "cf8dd11066d1d0ac24b61dc1b8087438c3c3dcc9588cba83cba38fbfec7819c4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/asdf-depot-plugin"
published_at: "2022-11-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:05d34c580d6c0f5cc9dfe1f3fe014928e38e0fe92a3b4d30724548fcc270e26f"
---

# Now available: A Depot plugin for asdf

Managing different project runtimes and tool versions can be a mess when you are working across different code bases that have different dependencies. Before[asdf](https://asdf-vm.com/) , you had to manage the different versions of tools via disparate version managers for the different tools, languages, and frameworks you make use of.


But with` asdf` , you can manage all of your different runtimes and tools in one place with one CLI. In addition, it has a plugin system that allows you to define specific versions of various tools that you use and therefore centralizes teams on a single version for each tool they use.


This week, we rolled out our own` depot` CLI plugin for the` asdf` version manager. We got the feedback earlier in the week that it would be helpful if we had a plugin to allow folks using` asdf` to not have to leave their existing method for managing tool versions. So, we got everything configured in an open-source repo,[depot/asdf-depo](https://github.com/depot/asdf-depot) , to make our CLI available as a plugin 🎉


To install the plugin, you can run the following command:


```text
asdf   plugin   add   depot   https://github.com/depot/asdf-depot.git
```


If you want to list all the versions available for` depot` , you can run the following command:


```text
asdf   list-all   depot
```


To install a specific version of` depot` , you can run the following command:


```text
asdf   install   depot   1.2.0
```


Lastly, if you wish to install a specific version of Depot globally, you can run the following command:


```text
asdf   global   depot   latest
```


We are excited to keep adding new ways developers want to interact with Depot to get faster Docker image builds. We hope that you find this helpful if you're a user of` asdf` , and we look forward to hearing your feedback. You can contribute to this plugin or open issues over on our[depot/asdf-depo repo](https://github.com/depot/asdf-depot) .


If you have another tool that we could integrate Depot into to make your life a bit easier, pleasereach out and let us know .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
