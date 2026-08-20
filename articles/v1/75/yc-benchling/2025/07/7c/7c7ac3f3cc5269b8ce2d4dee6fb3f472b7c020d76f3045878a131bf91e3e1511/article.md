---
schema_version: "1.0.0"
document_id: "7c7ac3f3cc5269b8ce2d4dee6fb3f472b7c020d76f3045878a131bf91e3e1511"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/launching-table-mapping-in-sub-templates-blog"
published_at: "2025-07-30T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:91f843fe9aa767c1b6076f937a127bdd0e45e709ecb3080fb93238935b3d3d30"
---

# Behind the build: Launching table mapping in sub-templates

What do you wish you could do with Benchling? That’s the question at the heart of the Benchling Community Ideas portal, one way we love to get product feedback from our customers. Through the Idea Portal, the Benchling Community shares feature ideas and feedback, helping us prioritize what to build next.


One of the latest examples of this? Table mapping in sub-templates — a feature that enables automatic data flow across tables, including now in sub-templates. This new release builds on the table mapping feature that Benchling users already know and love, providing even more flexibility to automate and structure the flow of data.


We originally scoped this feature as part of a broader table mapping release but made the decision to cut it due to uncertainty around the value it would bring customers. But the community wasn’t ready to let it go. What followed was an ongoing conversation between community members and the Product team resulting in a collaborative win for all users. The release of table mapping in sub-templates is just one example of how impactful customer-driven innovation is as we build Benchling.


So how did a feature once on the chopping block get a second wind and go live? The answer is you. Here's how it happened.


## But first, what is table mapping?


Say goodbye to copying and pasting data into tables. Table mapping in Benchling lets users automate the flow of data between structured tables, no manual transfer required. Instead of retyping or copying values, users can now select data to be “mapped” to an indicated column. The best part? Mapped connections between tables persist across templates. That means whenever a new template is created, users don’t have to set it up again. For example, bioentities can now be pulled automatically into registration tables, saving time and reducing the likelihood of errors. Once the feature is configured in a template, the process runs seamlessly without needing a single extra click.


Table mapping has been available since 2023 in templates — a customizable form that structures data entry in the Benchling Notebook. Sub-templates, however, are the more granular building blocks of templates, allowing users to create reusable elements like result table configurations and standardized headers. This reduces template setup time and ensures consistency across entries. Furthermore, sub-templates provide more flexibility. Users can mix and match custom-built elements when creating entries, an especially valuable approach for processes that benefit from some standardization but don’t fit neatly into a one-size-fits-all template.


The initial introduction of table mapping in templates helped many users accelerate their work by removing the need to manually replicate information across tables. However, users who sought more control and flexibility, particularly in sub-templates, were left wanting more. As one user memorably put it, not having table mapping in sub-templates was like “getting a new pair of rollerblades and then being told by your parents you can't use them inside!”


## Once upon a time, there was a community post


In a thread that now has ~200 views, Dora Vitay, Senior Lab Automation & Data Engineer at enginzyme, posted a request for the expansion of table mapping into sub-templates. She wasn’t alone, and her post quickly gained momentum, with other Benchling users chiming in through upvotes and comments.


“It was great to see the traction it gained in the community,” Vitay shared. “I was so glad that other users could also see a use case for their workflows. And the response from the development team was splendid.”


The post quickly exceeded the upvote threshold required for internal review and was taken on by Thomas Gerard, a software engineer at Benchling who had also worked on the initial table mapping release. So when this year’s Polish Bash — a week-long sprint focused on small but impactful improvements to Benchling’s user experience — rolled around, Gerard saw the perfect opportunity to bring this feature to life.


## Meanwhile, behind the scenes at Benchling


When Thomas Gerard first built out the table mapping feature, sub-templates weren’t an obvious candidate for functionality. Given the smaller and highly customizable nature of sub-templates, he didn’t believe they would contain many dependent tables that would benefit from the addition of table mapping. That changed with Vitay’s community post and the wave of support it received. Her feedback highlighted just how sophisticated user sub-templates had become. It was clear that table mapping could provide real value in these more complex use cases.


"You know the system is working when our users intuitively head straight for the Community to flag a product limitation like this,” shared Melissa DiTucci, Benchling Community Manager. “Users know this is a place where they have the attention of our product teams, regardless of seniority or organization size. A good idea is a good idea.”


As a software engineer on the Data Input team, Gerard noted that community feedback is built into the team’s feature prioritization system which they review at weekly meetings. “We really do go through every single one, and we read everything. We don't respond to everything immediately, but it's always there in the back of our minds and often goes into our backlog of things to work on,” stated Gerard.


With a clear signal from the community, Gerard and his team successfully made the case to prioritize the feature’s addition during Polish Bash. However, the real task had just begun. While templates and sub-templates share a similar editing experience, sub-templates are unique since they act as “mini” entries nested within a larger notebook entry. The technical lift, then, was ensuring that any table mappings from a sub-template would carry over to the table mappings set in the main entry without creating inconsistencies. It was a subtle but essential piece of engineering to ensure the feature worked as intuitively as users expected based on the shared community feedback and use cases.


Just a few weeks later, table mapping in sub-templates went live and Gerard returned to the community thread to share the news, closing the loop on the feedback journey. The release of this feature demonstrated the reality of how customers enrich and power Benchling. Through their advocacy, ideas, and feedback, the community plays an active role in making Benchling better for everyone.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## It's here — table mapping in sub-templates!


Now, with table mapping extended to sub-templates, Benchling users gain even more flexibility and control when building out their Notebook entries. Sub-templates can now include mapped tables that automatically pull and populate data just like in templates. This means users no longer need to manually move data from one part of an entry to another, even when working with smaller, modular components. This is especially useful for repetitive and structured data, such as sample names, container IDs, or experimental metadata. It’s part of a broader push in Benchling to build out automation not just in large-scale analyses or across systems, but also into daily moments like the everyday act of writing a notebook entry.


Getting started is easy. To begin using table mapping in sub-templates, users simply need to configure mapped columns when creating or editing a sub-template. Once added to an entry, the mapped data flows in automatically without any extra setup. For example, in a QC workflow, a sub-template containing a standard results table can now automatically populate sample IDs from a previous registration step.


By reducing manual toil and minimizing data entry errors, table mapping is helping teams move faster and with greater accuracy. It’s a simple change that adds up to big efficiency gains, ultimately freeing scientists to focus on the science, not the spreadsheets.


## Any more ideas?


“Don't hesitate, just post the idea.” Vitay advises anyone considering submitting a feature request to Benchling. “You might even hear about a workaround or feature that already exists to accomplish your goal. It felt great to see questions pop up that we have already spent time solving ourselves and know that knowledge can be used to help fellow users.”


Even if an idea feels niche, it still holds value. As Gerard explains, “It’s a way for us to understand how Benchling is used in real life and the problems that actually exist.”


If you’re a Benchling user, we want to hear from you.[Join the Benchling Community](https://community.benchling.com/) to be a part of the conversation, submit your own feature ideas, and connect with peers using Benchling.


Not a Benchling user? Join a global community of innovators transforming R&D with Benchling.[Learn more and request a demo here.](https://www.benchling.com/request-demo)
