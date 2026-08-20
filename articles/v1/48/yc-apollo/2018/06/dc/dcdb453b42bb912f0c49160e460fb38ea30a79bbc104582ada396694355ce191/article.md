---
schema_version: "1.0.0"
document_id: "dcdb453b42bb912f0c49160e460fb38ea30a79bbc104582ada396694355ce191"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/evolving-apollo-repository-maintenance-a2f26f319211"
published_at: "2018-06-07T00:03:50+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:20812b990895e4ae966e76c080bdf564747f82f02a9a3a757bf94ed8548cb7ce"
---

# Evolving Apollo repository maintenance

If you’ve been following Apollo’s progress on this blog, you’ve seen a steady stream of major new features announced across the Apollo project spectrum. While new feature announcements are exciting, behind the scenes contributors and maintainers are constantly working on incremental improvements, to make your tools better.


As a quick example, we’ve recently released[Apollo Client 2.3.2](http://github.com/apollographql/apollo-client) . This version includes numerous tweaks and bug fixes, submitted by Apollo’s amazing community of contributors:


Thank you everyone! ❤️


A few important highlights:


- Fixed SSR issues when using the` cache-and-network` fetch policy (I[ssue #2119](https://github.com/apollographql/apollo-client/issues/2119) ,[PR #3372](https://github.com/apollographql/apollo-client/pull/3372) by[dastoori](https://github.com/dastoori) ).
- Fixed an issue where the` updateQuery` method passed to` ObservableQuery.fetchMore` was receiving the original query variables, instead of the new variables that it used to fetch more data ([Issue #2499](https://github.com/apollographql/apollo-client/issues/2499) ,[PR #3500](https://github.com/apollographql/apollo-client/pull/3500) by[abhiaiyer91](https://github.com/abhiaiyer91) ).
- Fixed cache invalidation for inlined mixed types in union fields within arrays (` apollo-cache-inmemory` ,[PR #3422](https://github.com/apollographql/apollo-client/pull/3422) by[dferber90](https://github.com/dferber90) ).


For more details around the Apollo Client 2.3.2 release, refer to the various Apollo Client changelog’s (` apollo-client`[CHANGELOG](https://github.com/apollographql/apollo-client/blob/master/packages/apollo-client/CHANGELOG.md) ,` apollo-boost`[CHANGELOG](https://github.com/apollographql/apollo-client/blob/master/packages/apollo-boost/CHANGELOG.md) ,` apollo-cache-inmemory`[CHANGELOG](https://github.com/apollographql/apollo-client/blob/master/packages/apollo-cache-inmemory/CHANGELOG.md) ,` apollo-utilities`[CHANGELOG](https://github.com/apollographql/apollo-client/blob/master/packages/apollo-utilities/CHANGELOG.md) ,` graphql-anywhere`[CHANGELOG](https://github.com/apollographql/apollo-client/blob/master/packages/graphql-anywhere/CHANGELOG.md) ).


Releases like the above make us excited about the health of the Apollo ecosystem. Today we’d like to tell you about exciting new improvements coming to Apollo’s overall repository management approach, that will help facilitate even more community contributions.


---


## Issue and PR triage 📈


We here at Apollo HQ absolutely love the amazing community of users and contributors who have rallied around Apollo! Every day we’re blown away by the community’s level of knowledge, thoughtfulness, dedication and amazing willingness to help out those who are just getting started in the Apollo GraphQL space. We’ve been working really hard on exciting additions to the GraphQL landscape, but while keeping our heads down getting things done, we’ve seen the list of reported issues and PRs continue to grow.


Dealing with issues and PRs is definitely one of the most challenging aspects of an open source project. Letting issues and PRs sit without any feedback is something that we want to try to avoid as much as possible. We couldn’t be happier to get community PR’s, and really want to make sure people know their contributions are welcome and appreciated.


## Apollo repo cleanup: Plan of attack 🎯


Moving forward, we’re going to put in a focused effort into making sure issues and PRs get the priority they deserve. Over the next while, we’ll be implementing several public repo management changes, such as:


- **Updating all Apollo repos to include new (and accurate) issue/PR workflow and triage guidelines.** This will help make it easier for community members to understand how issues/PRs are going to be reviewed and worked on in the future.
- **Dedicating resources to work on reviewing repo issues and PRs.** We’re working on a process to make sure issues are triaged appropriately, and outstanding PRs receive some form of feedback. PRs that are close to being merged will be finished and merged, PRs that are no longer relevant will be closed ( *sorry!* ), and PRs that still need a fair amount of work done will be marked as works in progress (and contributors will be notified of what’s missing).
- **Outlining a regular release schedule and making sure it’s clearly visible in the project README.** This will let community members know when they can expect to see merged PRs show up in a release. This won’t be added to every repo; just the more popular ones that see regular updates.
- **Create new contributor ramp-up guides for the most popular Apollo projects, to make it easy for future contributors to hit the ground running when it comes to submitting PRs.** These guides will outline how a project’s source is designed/structured, where the major chunks of functionality lie, how to build the project, how to add and run tests, how to integrate the project into a sample application while developing, where the changelogs are and how they should be updated, etc.
- **Streamlining our changelog process.** Right now the changelogs for various Apollo projects are a bit spread out, making it difficult to ramp-up quickly on what has changed in a certain release. The source for the Apollo Client project, for example, is structured such that a main` apollo-client` package is defined, that depends on other packages in the same project (e.g.` apollo-cache` ,` apollo-cache-inmemory` ,` apollo-utilities` , etc.). These packages are all managed using[lerna](https://lernajs.io/) , and are published to NPM as their own standalone packages. Each of these packages currently has their own changelog. When a bug fix is applied to the Apollo Client project, it’s possible that bug fix was actually made in say the` apollo-cache-inmemory` package, which means its specific changelog is updated with the fix details. Someone looking for an overview of recent` apollo-client` changes will not see this fix in the main` apollo-client` changelog. Needless to say, we’re definitely aware that this can cause problems for people, and will be consolidating changelogs.
- **Getting the community more involved!** 🎉 I think we all know that the Apollo community is absolutely amazing! We’ve received so many excellent library feature requests, bug reports (sometimes with multi-hour created reproductions), PRs for super complex and difficult to work on edge case bugs, amazing documentation sections, the list goes on. Moving forward, we’re going to be much more involved with getting interested community members involved with Apollo projects. Anyone who has shown an interest in a specific Apollo project, has submitted a few accepted PR’s, and is interested in helping triage issues and/or PRs, will be welcomed with open arms. We’ll get you elevated repo privileges and add you to an internal triager Slack channel, where we can all help steer the future direction of Apollo.


## Making progress 🏁


The good news is that we’ve already started on some of the above changes, tackling the Apollo Client project first. Apollo Client is our most popular library, and has been in need of some tender loving care. Over the past couple of weeks we’ve been working on clearing out the backlog of open issues, and have just about covered every outstanding PR. We’ve moved to a once a week release cycle for` <a href="https://github.com/apollographql/apollo-client" target="_blank" rel="noreferrer noopener">apollo-client</a>` and have been happily working with community members to get code changes in place. Once we’re satisfied with Apollo Client’s overall repo health (and have a process in place to make sure we don’t fall behind on issues/PRs again), we’ll be tackling` <a href="https://github.com/apollographql/react-apollo" target="_blank" rel="noreferrer noopener">react-apollo</a>` next.


A repo brick wall tagged by a passionate, and accurate, community member (photo:[My Life Through A Lens](https://unsplash.com/@bamagal?utm_source=medium&utm_medium=referral) ).


We’ll have more repository changes to announce in the future. **A BIG THANK YOU** again to everyone who has helped make[Apollo](https://www.apollographql.com/) what it is today! ❤️ If you’re interested in helping out further, please reach out to me anytime via[Apollo Slack](https://www.apollographql.com/slack) or any of the[Apollo GitHub](http://github.com/apollographql/apollo-client) repos (` <a href="https://github.com/hwillson" target="_blank" rel="noreferrer noopener">hwillson</a>` in both locations).


I’m happy to discuss any issues that are currently blockers that you think should receive more attention, and can help point you towards issues that we need help with, if you’re interested in contributing. We also host a weekly (remote) contributor meeting for anyone who’s interested in helping plot Apollo’s future course. If you’d like to join in on the calls, let us know in the Slack` #contributing` channel.


---


### GraphQL Europe 2018


Can’t get enough GraphQL?[GraphQL EU 2018](https://www.graphql-europe.org/) — Europe’s biggest gathering of GraphQL enthusiasts — is just around the corner. Join us on **June 15 in Berlin** and see expert speakers from top GraphQL companies as well as the GraphQL creators Lee Byron and Nick Schrock.


[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----a2f26f319211----------------------) from the Apollo team will be presenting there as well, and you don’t want to miss her talk!


Get 20% off your ticket with this special discount code:` apollo-at-graphql-eu`


Written by


Hugh Willson


[Read more by Hugh Willson](https://www.apollographql.com/blog/author/hwillson)
