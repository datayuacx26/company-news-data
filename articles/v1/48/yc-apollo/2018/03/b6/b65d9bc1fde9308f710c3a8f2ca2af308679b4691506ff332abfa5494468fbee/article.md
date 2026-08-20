---
schema_version: "1.0.0"
document_id: "b65d9bc1fde9308f710c3a8f2ca2af308679b4691506ff332abfa5494468fbee"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-apollo-bot-a2a6f9e0f4f9"
published_at: "2018-03-08T21:27:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:f12841d24ca5afa18356326fab4a5011bf97db9319cb8aa6d4a84ca83af51bf3"
---

# Introducing Apollo Bot 🤖

Two years ago, Apollo started as a tiny open source project with just a handful of contributors. Today it has expanded to many repositories supported by an incredible community:[graphql-tag](https://github.com/apollographql/graphql-tag) ,[apollo-angular](https://github.com/apollographql/apollo-angular) ,[reason-apollo](https://github.com/apollographql/reason-apollo) ,[apollo-android](https://github.com/apollographql/apollo-android) ,[apollo-link-rest](https://github.com/apollographql/apollo-link-rest) to name a few. The existence of these projects and sustained growth are driven by community suggestions and feature contributions. Because Apollo wouldn’t exist without the active community, we want to make it much easier to get started as an open source contributor, and increase people’s ability to get involved on a deeper level.


Today, as our first step to empower the community, we are introducing[Apollo Bot](https://github.com/apollographql/apollo-bot) . Apollo Bot will help you engage with our open source codebases and the surrounding ecosystem. The first feature of Apollo Bot will be to allow contributors to label issues and pull requests, but that’s just the start and we’re excited to expand the functionality as we learn more about what will be helpful.


We hope this and future additions create an environment that is easy to get started with and productive for contributors and maintainers. Keep reading to see how it works and what’s in store!


Apollo Bot in its natural habitat


## Labelling your own issues and PRs


As of today, all new Github Issues and Pull Requests on Apollo repositories will include a section that allows you to add any of four labels:` has-reproduction` ,` feature` ,` blocking` , and` good first issue` (or` good first review` ).


These labels have been carefully selected to provide quick actionable information for everyone involved.` has-reproduction` ,` feature` , and` blocking` labels contributors to determine the importance and possible reaction to an issue or pull request.` good first issue` indicates that the feature, bug, doc change is possible with minimal prior experience. Here is a breakdown of each of these labels individually:


New Labels and Descriptions


### Using these labels


The labels will provide contributors and maintainers the opportunity to filter by what you might like to learn or help build. If you’re looking to help our with an issue, the best place to start is the[has-reproduction flag](https://github.com/search?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+org%3Aapollographql+archived%3Afalse+label%3Ahas-reproduction&type=) . On the other hand, if you are curious about how you can add to the next generation of Apollo Client, then filtering by the feature tag and jumping into a discussion works well. If you’re not sure where to start, keep reading since Apollo Bot has something for you!


## Helping others


Contributing to an open source project without a place to start can be difficult and scary. For anyone interested in making a project better in the[Apollo organization](https://github.com/apollographql) , Apollo Bot lowers this barrier to entry by providing the ability to add labels on any issue or pull request. To make use of this, you simply add a new comment on the issue or pull request with a` /label` command on its own line. Some common labels that you may want to add along with these four are` docs` ,` discussion` ,` can-close` , or the particular package that is affected. Here is an example:


Using a /label command


Note: you may include other text in the comment, as long as the` /label` command is on its own line!


### Why label issues and PRs?


Adding labels on a repository is a great way to get started helping out with the project. To understand the history of a project and the current development efforts you can start looking from the oldest issues to the newest. This will provide invaluable context on the project’s gaps and what help will be most appreciated by the community. One great place to start is` /label` issues with the new labels introduced above. Be sure to reference the original if you close as duplicate!


## To infinity and bot on


The most exciting part about Apollo Bot is the opportunity for integrations with the rest of Apollo’s ecosystem, such as our[Slack](https://www.apollographql.com/#slack) . For example, when an issue is labelled` good first issue` , the bot could notify the corresponding channel and help bootstrap anyone who is interested. With another Slack integration, Apollo Bot could recognize the most active contributors by aggregating weekly activity around the community.


Every week we host a maintainers meeting to discuss upcoming features and releases. With the new system, part of this meeting could be devoted to issues that are` blocking` with reproductions or PRs that are highly discussed. In addition to placing these topics on the agenda, Apollo Bot could invite the interested members to the meeting to discuss possible solutions or the various blockers.


### Using Apollo Bot Yourself


You are also able to include Apollo Bot in your own repositories to take advantage of the` /label` commands. Simply install the GitHub[application](https://github.com/apps/apollo-bot) on your preferred account.


### Improving Apollo Bot


If you have any ideas that will make your open source experience more productive or want to contribute, please open an issue or pull request (with labels 🙏) on the Apollo Bot[repository](https://github.com/apollographql/apollo-bot) .


### Call for avatar


We are looking for an avatar for the new bot! If you want to suggest one, please tweet or DM[@evanshauser](https://twitter.com/evanshauser) on Twitter.


Written by


Evans Hauser


[Read more by Evans Hauser](https://www.apollographql.com/blog/author/evans)
