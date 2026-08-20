---
schema_version: "1.0.0"
document_id: "7768ef906e3317f0e70b6d5f398bc5316783ef4556ae07b63ffc737c72c2fb98"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-v1-4-2/"
published_at: "2021-03-14T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:bc27a4b5573570073f898af68ac728b972309451df6db2d30ef27a2f236acba2"
---

# Sails 1.4.2 release

A patch release of Sails -[v1.4.2](https://github.com/balderdashy/sails/releases/tag/v1.4.2) was cut out recently. This release merges a PR which fixes a Node warning you may have experience if you are runnning Sails on Node 14 or higher.


## Getting this release


In order to upgrade to this release, you will need to do the following:


### Upgrade the Sails CLI


You need to upgrade the Sails CLI so new Sails projects you generate via the CLI will have the latest version of Sails and it’s dependencies. Simply run the below command in your terminal:


```text
npm i -g sails
```


Then run:


```text
sails -v
```


This should output:


```text
1.4.2
```


### Upgrading existing Sails project


Most likely you have an existing Sails project you’d like to upgrade to this release.


To do this, you will need to ugrade the` sails` dependency to version` 1.4.2` and the` sails-hook-orm` to version` 3.0.2` . Do this by running:


```text
npm i  [email protected]    [email protected]
```


Another way is to simply remove the entries for` sails` and` sails-hook-orm` in your` package.json` and run:


```text
npm i sails sails-hook-orm --save
```


Or you can simply do:


```text
npm i sails@latest sails-hook-orm@latest
```


To get the latest major versions of both` sails` and` sails-hook-orm`


### Upgraded


Now if you’ve done the above upgrading steps you can run` sails lift` in Node version >= 14 without the Circular dependency warning.


Happy Sailing…
