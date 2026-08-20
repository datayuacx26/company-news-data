---
schema_version: "1.0.0"
document_id: "66d49ba504de6f9200492402148524d956f60c761afcce6181907c4de97a5af0"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/"
published_at: "2024-09-06T10:36:32+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:5ce7dc7403a119bf6ad057359fe5ad4d33d4d0895a52ae39179b4d0af5cfd5c0"
---

# The death of VBScript

Several users have expressed concerns over the[announcement by Microsoft that VBScript would be deprecated](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/vbscript-deprecation-timelines-and-next-steps/ba-p/4148301) in the near future. How does that decision impact OL Connect Workflow users?


## Scripting in Workflow


Scripting capabilities have always been an integral part of Workflow. Whenever processes need functionality that goes beyond what’s available through native tasks, scripts are a versatile means of achieving just about anything.


Because Workflow runs as a service, it must have access to a scripting engine that can be called from that service, using the proper service credentials. That’s where the Windows Scripting Host (WSH) comes into play: the WSH is natively integrated in all versions of Windows and can be called by any application. It also provides mechanisms for hosting applications to augment the Scripting Engine’s feature set: for instance, Workflow embeds the` Watch` object inside the scripting engine, allowing users to access Workflow-specific features that are otherwise unavailable to any other application that uses the WSH, or to any external JavaScript engine like NodeJS.


By default, the WSH understands two scripting languages: VBScript and JScript (the latter is Microsoft’s version of JavaScript), which are collectively known as *Active Scripting languages* . Over the years, several 3rd party organizations have provided additional active scripting languages – sometimes for free, sometimes for a fee – that can be registered in Windows for use inside the WSH. Perl and Python are examples of such languages that can also be used by Workflow (when they are registered), by way of the WSH.


## Does Workflow natively use scripting?


Yes it does. Many of the plugins that have been added to Workflow over the years use Active Scripting to perform their tasks. However, only one of these plugins still uses VBScript as its backend engine: the *Mathematical Operations* plugin. All other script-based plugins use JScript or Enhanced JScript – the latter being a *poly-filled* version of JScript that implements many of the more modern JavaScript features that were never implemented in Microsoft’s version.


Some other current and legacy functionalities also rely on VBScript. For instance, the Update Manager relies on VBScript in order to display information about the new versions of the application that are available for download. Also, Excel Business Graphs hosted inside PlanetPress Suite templates require VBScript for some of their internal functionality. Our R&D department has been asked to compile a list of all such instances where VBScript is required and to come up with mitigation measures (including the Math Operations plugin).


However, some of our partners and customers have created their own custom plugins for Workflow that may be based on VBScript. It will be up to them to determine how they wish to address the VBScript deprecation issue. In most cases, re-writing the scripts so they use JScript instead of VBScript should be a fairly simple task, but it may take a while for those updated plugins to be deployed to all the locations where they are used. Again, this is something that the custom plugin developers will have to work out.


## The future of Active Scripting as a whole


Microsoft has been extremely tight-lipped about their plans for Active Scripting. We know that VBScript is eventually going away, but there have been no announcements on either JScript or support for 3rd party Active Scripting languages. So at this stage, we just don’t know how long Active Scripting is going to be supported. But given its ubiquitous presence in many of Microsoft’s own products, it is unlikely to be going anywhere any time soon.


A more pertinent question for Microsoft would be about their plans for eventually offering an long-needed upgrade (or alternative) to their antiquated JScript engine. But that, unfortunately, is not something they seem willing to address.


## Is there urgency in dealing with this issue?


Not quite. Microsoft’s announcement clearly states that VBScript is not going away before 2027 at the earliest. Between now and then, it will be made an optional feature in newer versions of Windows, but that can be managed easily by simply installing the missing component on new installs of Windows (existing installations of Windows that are being upgraded to a newer version should not be impacted).


And by the time 2027 comes around, Upland OL will have put in place all the mitigation measures required to make this a non-issue.


Still, if you have written VBScript tasks in your implementations, you should start looking – sooner rather than later – at updating them to use JScript instead. The language itself is more powerful (especially for string and array manipulations!) and the sheer number of examples that are available on the web is unmatched. Granted, many of those examples use the modern JavaScript syntax instead of the more limited JScript version, but adapting the code to JScript is a relatively minor effort compared to trying to achieve the same thing with VBScript.


When and if any new and relevant information on the topic becomes available, we will publish it on this blog.


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/#respond)


##### All Comments (2)


- Peter September 09, 2024


It would be more than nice if you used a modern Javascript engine such as node.js or similar. And please don't forget to integrate the api's (Alambic, Repository, Watch-object etc.). I would already switch to Node.js, but then I always lack this OL components


[Reply](https://olresourcecenter.uplandsoftware.com/blog/the-death-of-vbscript/?replytocom=325#respond)
