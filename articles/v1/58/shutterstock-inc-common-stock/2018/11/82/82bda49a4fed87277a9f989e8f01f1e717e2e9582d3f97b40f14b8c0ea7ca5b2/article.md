---
schema_version: "1.0.0"
document_id: "82bda49a4fed87277a9f989e8f01f1e717e2e9582d3f97b40f14b8c0ea7ca5b2"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/11/05/gitbook-plugin-related-links"
published_at: "2018-11-05T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:8f494c8b5c03bdd9d13913f2fa93e3cf90050f3d90c303f5a90c6b2a1f91e777"
---

# Managing Related Links with an Open-Source GitBook Plugin

Sometimes it’s the little things that make working with a software tool effortless, like editors that have smart auto-complete and programs that provide precise error messages that help you work through problems.


In my first year at Shutterstock, I’ve had to set up some technical documentation systems from scratch, and that meant starting with open-source tools that may or may not have all those great features that we might be used to having. In order to bridge these gaps, we evaluate what we need the most and add the features that really save time, prevent errors, and reduce stress.


We’ve recently released a plugin for the open-source[GitBook toolchain](https://toolchain.gitbook.com/) that we wrote to add one of those small, but helpful features; it helps manage the links in a GitBook topic by retrieving the title of a link target. That way, instead of having to hard-code the title of the target page, you just put the file name in the front matter of the page.


Here’s an example from the plugin’s[README.md](https://github.com/shutterstock/gitbook-plugin-related-links/blob/master/README.md) :


```text
---
relatedLinks  :
-    myFile.md
-    http://gitbook.com
-    ../otherfiles/relatedFile.md
-    '  [My     custom     link     text](someOtherFile.md)'
---
# Here's my file that needs related links


```


At build time, before Gitbook converts the Markdown to HTML, the plugin gets the link titles from the MD files in the front matter and adds links to a related links section at the end of the page.


The resulting file looks like this:


```text
# Here's my file that needs related links


### Related links
-    [  My special file  ](  myFile.md  )
-   http://gitbook.com
-    [  My related file  ](  ../otherfiles/relatedFile.md  )
-    [  My custom link text  ](  someOtherFile.md  )


```


For more information about using the plugin, see the[README.md](https://github.com/shutterstock/gitbook-plugin-related-links/blob/master/README.md) .


We wanted this feature in our internal documentation because starting from scratch meant dealing with rapid change. As we worked out the architecture and scope for documenting internal Shutterstock systems, we had to reorganize and rename individual pages, and it was tiring to fix hard-coded links every time we renamed pages.


With this plugin, we still have to verify that the link to the target page still make sense in context, but we don’t have to copy and paste the title of the page over and over.


You can have a look at the plugin[here](https://github.com/shutterstock/gitbook-plugin-related-links) , and you can install it in your GitBook by adding` related-links` to your list of plugins in the` book.json` file:


```text
{
"title"  :     "My GitBook"  ,
"author"  :     "Me"  ,
"plugins"  :     [     "related-links"     ]
}


```


Let us know if you find this plugin useful, and happy open-sourcing!
