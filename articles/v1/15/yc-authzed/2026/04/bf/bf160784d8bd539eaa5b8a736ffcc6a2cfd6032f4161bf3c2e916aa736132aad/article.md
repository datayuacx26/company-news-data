---
schema_version: "1.0.0"
document_id: "bf160784d8bd539eaa5b8a736ffcc6a2cfd6032f4161bf3c2e916aa736132aad"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/composable-schemas-ga-and-zed-v1"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:52:37.138540+00:00"
content_hash: "sha256:e1b21175edfe744dd311da0a6df81b03fe5202542394b457141202a781183ee6"
---

# Composable Schemas is GA

In a[blog post from December 2024](https://authzed.com/blog/composable-schemas-preview) we announced that we had developed the initial foundations for composable schemas. Today, I'm excited to share that composable schemas are now generally available in SpiceDB 1.51.1.


## What are Composable Schemas?


Composable schemas allow you to better manage and modularize your large schemas, which helps organizations scale. It also enables the open source community to design reusable data models for common problems and schema structures.


In practice, large schemas tend to become monolithic. Composable schemas solve this by enabling you to split your schema into well-defined, reusable components, so teams can move faster, make changes, and scale without everything becoming entangled. Just as importantly, this modular approach unlocks new possibilities for the open source community, making it easier to share, reuse, and build on common authorization patterns rather than reinventing them from scratch.


> "AuthZed's composable schemas have been a blessing and have helped us better manage authorization at scale. Instead of wrestling with one massive monolithic schema, we're now breaking things into clean, reusable pieces that are actually readable and reflect how our systems and core components are built. It's made the development workflow faster, safer, and much easier to work with."
>
>
> — Simon Bruno, Senior Security Engineer at Forter


## How To Use Composable Schemas


The composable schemas feature required significant changes in our schema compiler around making it filesystem-aware so that, if an error occurs, we can tell you exactly which file contains the error. For similar reasons, we also updated our Language Server Protocol, which is used by the VS Code extension, to be filesystem-aware.


There are two breaking changes compared to the Preview to be aware of:


- ` use import` is now required if you want to use` import "foo.zed"` syntax.
- ` use partial` is now required if you want to declare or reference partials.


In order to enhance the developer experience, we have made a number of changes to our tooling, namely:


- We released the[Zed CLI](https://github.com/authzed/zed) to v1. The various` zed` commands now support composable schemas. For example:


- ` zed validate <root.zed>` is what you can call to validate across multiple files. It will resolve your imports and validate those files as well.
- ` zed schema compile <root.zed>` is what will combine the multiple files back into one schema that you can then write in SpiceDB.


- We released[VS Code extension](https://github.com/authzed/spicedb-vscode) 1.1.1 to work with composable schemas. For example, when you are hovering over a definition (or a caveat, or a partial) that is declared in another file, you will get the right tooltip; or if you press F12 to navigate to a definition, it will navigate to the file that declared it.


Note that our playground does not support` import` syntax as the Playground uses a singular editor (and doesn't support the notion of files). But let us know if this is something that you would like us to support!


## Thank You, SpiceDB Community


We would like to thank the community for your patience in releasing this, and for your bug reports that allowed us to make a better experience for everyone.


If you run into issues or would like to request a feature, open an issue or hit us up in our[Discord server](https://authzed.com/discord) . Happy modeling!


On this page


- What are Composable Schemas?
- How To Use Composable Schemas
- Thank You, SpiceDB Community


## Related


[Product AuthZed Cloud is Now Available! Bringing the power of AuthZed Dedicated to more with our new shared infrastructure, self-service offering: AuthZed Cloud. Aug 20, 2025 · 5 min](https://authzed.com/blog/authzed-cloud-is-now-available)[Product AuthZed Cloud is Now Available! Bringing the power of AuthZed Dedicated to more with our new shared infrastructure, self-service offering: AuthZed Cloud. Jake Moshenko · Aug 20, 2025 · 5 min](https://authzed.com/blog/authzed-cloud-is-now-available)


[Product Zed v0.30.2 Release Zed CLI provides seamless interaction with SpiceDB clusters, allowing you to manage schemas, relationships, and permissions checks. Our v0.30.2 release adds composable schema support, automatic retries, backup functionality, and upcoming Windows package integration via Chocolatey. May 1, 2025 · 2 min](https://authzed.com/blog/zed-v0-30-2-release)[Product Zed v0.30.2 Release Zed CLI provides seamless interaction with SpiceDB clusters, allowing you to manage schemas, relationships, and permissions checks. Our v0.30.2 release adds composable schema support, automatic retries, backup functionality, and upcoming Windows package integration via Chocolatey. Maria Inés Parnisari · May 1, 2025 · 2 min](https://authzed.com/blog/zed-v0-30-2-release)


[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Sam Kim · Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)
