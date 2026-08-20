---
schema_version: "1.0.0"
document_id: "4ca8b9f4513501c40e1963026e78bae6fc8dc9ecb960c163cb37690d806ec403"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/zed-v0-30-2-release"
published_at: "2025-05-01T11:12:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:0ad1123fc55e7b4e68cfa5f125e642c87bef90ea851f81d03ced237e128b495c"
---

# Zed v0.30.2 Release

[Zed](https://github.com/authzed/zed) is the command line interface (CLI) tool that you can use to interact with your SpiceDB cluster. With it you can easily switch between clusters, write and read schemas, write and read relationships, and check for permissions. It can be launched as a standalone binary or as a Docker container. Detailed installation options documented[here](https://authzed.com/docs/spicedb/getting-started/installing-zed) .


## Improvements in v0.30.2


Over the last few months we’ve been making many improvements to it, such as:


- Adding support for compilation and validation of[composable schemas](https://authzed.com/docs/spicedb/modeling/composable-schemas)
- Adding automatic retries
- Adding a new` zed backup` command
- Publishing the package to[Chocolatey](https://community.chocolatey.org/packages/zed) for all Windows users (currently in review)


And many other small fixes that are too many to list here. We are happy to announce that last week we[released zed v0.30.2](https://github.com/authzed/zed/releases/tag/v0.30.2) , which includes all of these changes.


In the near future we expect to be adding support for a new test syntax in schema files, which will allow you to validate that your schema and relationships work as you expect them to. Stay tuned!


As you can see, we are continuously making improvements to zed. If you see anything not working as expected, or if you have an idea for a new feature, please don’t hesitate to open an issue in[https://github.com/authzed/zed](https://github.com/authzed/zed) . Also, while you’re at it, please give us a star!
