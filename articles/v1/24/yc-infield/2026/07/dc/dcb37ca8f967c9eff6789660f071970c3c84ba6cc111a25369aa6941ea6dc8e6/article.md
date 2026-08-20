---
schema_version: "1.0.0"
document_id: "dcb37ca8f967c9eff6789660f071970c3c84ba6cc111a25369aa6941ea6dc8e6"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/infields-data-pipeline"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:2a910b3cebd5ce73d2c8b0f92081ae0e243c216b52d65f021725b3d7cf2ea881"
---

# Infield's Data Pipeline

As part of Infield’s mission to make upgrading open source dependencies easy and safe, we’re building a comprehensive dataset of open source packages and metadata about each package version. We use LLMs and human-in-the-loop changelog review to categorize breaking changes for each version, then enrich this data with upgrade experience data from the community. This post will go into more detail about how we’re gathering this data.


Many programming languages have a centralized database where information about packages are stored. For example, Javascript has the[npm registry](https://www.npmjs.com/) and Ruby has the[RubyGems database](https://rubygems.org/) . We fetch our package information from these repositories, but how we fetch this information differs by ecosystem.


The RubyGems website provides a nightly SQL dump of all of their gem data. We stay current by fetching this data every night and updating our own package and package version data accordingly. The npm registry doesn’t have an equivalent way to download information for all of its packages, but it does provide an API to fetch a single package’s information. Since there are over 2 million packages on the npm registry, we have decided to not preemptively fetch every single package. Instead, when a user installs Infield for a Javascript repository, we fetch any npm package that doesn’t already exist in our database. Similar to Ruby, we run a nightly task that refreshes every npm package we store, so that we always know about new versions and patches.


Once we have package-level information, we can grab the changelogs so that we can analyze breaking changes. We consider a package version to be fully researched when we have classified all of a version’s changes according to our own classifications, the primary of which is classifying the change as “Breaking” or “Non-Breaking”. We also give the change a type classification, such as “Added”, “Removed”, “Changed”, “Deprecated” or “Fixed”. At this point, Infield is able to recommend upgrades that include this package version, since we can show users the exact breaking changes, if any, they need to worry about.


In order to do this research, Infield first needs to find the relevant changelogs for a package. This information is often stored in a file in the project repository titled something like “CHANGELOG.MD” or “CHANGES.MD”. In these cases, it is simple for us to make a request to the GitHub API (or whichever code repository service the project uses) to fetch the file. While this is a simple process for the average changelog that follows common standards like the[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format, it becomes more complex for changelogs stored in idiosyncratic formats. As we come across different ways developers store or write their changelogs, we update our system to read these patterns automatically.


After fetching the changelog we need to read it programmatically. This is where we use large language models (LLMs), currently OpenAI’s GPT-4. To get the changelogs ready for classification by the LLM, we first parse the changelog text file and split it into different chunks for each individual version of the package. Now we can feed this version-specific changelog chunk into the LLM, along with a prompt that explains to the LLM how it should classify each change and how we want the output structured. For every change within the changelog, this will output a summary of the change and the “Breaking / Non-Breaking” classification, among other things. If we do this for every version of a package, then we will be able to confidently suggest any upgrade for that package, letting users know which breaking changes, if any, they need to worry about.


In the interest of strengthening the open source ecosystem, we freely display all of this package research on our public package pages. For any package we have research on, you can select a version range and get a comprehensive list of every breaking change between those two versions. If you are planning on doing an upgrade, you can check the public package page on Infield to aid you in researching and planning your upgrade.
