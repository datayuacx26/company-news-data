---
schema_version: "1.0.0"
document_id: "257623eca6b0792de2ebbb4b5a10cea4eaffee0ae20e0328140aac7b99c31c71"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/git-is-really-cool-actually"
published_at: "2025-09-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:fc1371f14ab5fa62084b4c087994ea5487a7e388edad2aadab98fe53295a7393"
---

# Git is really cool, actually

I have a favorite Git protocol operation. It's fetch.


Before you judge me for having strong opinions about wire protocols, hear me out: fetch reveals the best things about Git's design. The conversation between your client and the server is simple on the surface: the client says "I have these commits" and the server responds with exactly what's missing. Nothing wasted or redundant. Just a stateless request-response exchange that manages to sync distributed repositories without either side maintaining complex session state. Let's break down how it works.


## git's smart protocol is really cool, actually


Modern Git clients and servers use a "[smart protocol](https://git-scm.com/docs/http-protocol) ". Essentially, it lets the Git client negotiate with the server on what objects it needs. On the surface, the Git smart protocol is pretty simple:


1. Client -> Server \[what can you do?\]
2. Server -> Client \[can list references, grab commits, and push up commits\]
3. Client -> Server \[please list refs matching x filters, or send me some objects I'm missing, or take these new commits, or …\]
4. Server -> Client \[here's what you need\]


There are a couple things to note here:


- Git's smart protocol is request -> response. The client sends a single request to the server, and the server statelessly gives a response. This design makes it much easier to scale servers and reduces the amount of complexity (and bugs :P) of Git server implementations. And new Git features (like partial clones) can be added without worrying as much about backwards compatibility.
- The protocol works over multiple different transports: HTTPS, SSH, or even Git's own (legacy) protocol.


That's the high level picture. But things get interesting when we dive into an actual operation. Let's take a closer look at my favorite Git protocol operation (if I dare to have a favorite): fetch.


### example: fetch


Fetch, of course, follows the same request/response pattern:


- Client -> Server: "hey, I'd like to fetch \[commit/branch\]. I have these commits, so I don't need them or any commits before them. I'm done specifying what I want."


Here's an example of a real request. You can see something similar by running` GIT_TRACE_PACKET=1 git fetch origin main` on a repo with some data to fetch:


```text
# 1. The server advertises capabilities
# 2. The client asks what commits are available


# 3. The server responds with commits and their corresponding branches
fetch  <   8f2037f2b3df3d34baec05df27f41f9c4fe601cd   refs/heads/main
fetch  <   3a54ae1da97c7c4d404829232bc0a2b824c78d13   refs/tags/v0.0.1
...
fetch  <   0000   # we're done, server


# 4. Client requests objects (wants/haves + pack optimizations)
fetch  >   command=fetch
fetch  >   agent=git/2.51.0-Darwin
fetch  >   object-format=sha1
fetch  >   0001
fetch  >   thin-pack
fetch  >   ofs-delta
# Ask for main's tip; tell the server what we already have
fetch  >   want   8f2037f2b3df3d34baec05df27f41f9c4fe601cd
fetch  >   have   56cf9cd609ce7f39bc9bfeacea14cee05a137164
fetch  >   have   733ea9d857c90e23369942515c341ef56f16c680
fetch  >   have   ed4b55cfbcd73e853c782d48346d929898dcf54a
..   many   many   more   refs
fetch  >   0000   # we're done, please respond


# 5. Server acks the common base(s), signals readiness
fetch  <   acknowledgments
fetch  <   ACK   56cf9cd609ce7f39bc9bfeacea14cee05a137164
fetch  <   ACK   733ea9d857c90e23369942515c341ef56f16c680
...
fetch  <   ready
fetch  <   0001


# Packfile over sideband (progress + data multiplexed)
fetch  <   packfile
sideband  <   \2  Enumerating   objects:   16,   done.
sideband  <   \2  Compressing   objects:   100%   (9/9), done.
sideband  <   PACK   ...                    # (binary packfile)
sideband  <   \2  Total   16   (delta   7  ), reused 14 (  delta   7  ), pack-reused 0
```


This is pretty sick. The server only sends exactly the data needed, and nothing more. Sync is a hard problem, and this solution is elegant from the client's side.


The main downside is that crawling the commit graph to figure out what needs to be sent is **expensive** for the server. Lots of very smart folks have done a lot of work to optimize this ([GitLab has a cool doc on some of the work they did](https://gitlab.com/gitlab-org/gitaly/-/blob/a568252daff606a31d25915ab585e6f49f1238f6/doc/design_pack_objects_cache.md) ), but the first graph crawl of large repos will almost always be painful.


If you were looking closely, you might have noticed this specific line:


```text
sideband  <   PACK   ...                    # (binary packfile)
```


Once the server figures out what objects you're missing, how does it actually send them? That's where packfiles come in.


### packfiles are really cool, actually


Git has four kinds of objects:


- **Blobs** (raw file contents)
- **Trees** (directories pointing to blobs and other trees)
- **Commits** (metadata + pointers to a root tree and parent commits)
- **Tags** (human-readable labels pointing to any object)


Put those together, and you get the full history of a project.


Think of these objects like this:


```text
Tag   (v1.0.0)
└──   Commit   (abc123)
├──   Author:     Billy   <  billy@example.co  m  >
├──   Committer:   Billy   <  billy@example.co  m  >
├──   Message:    "Add src/util.c"
├──   Parent:     (789abc)
└──   Tree   (def456)
├──   Blob   README.md     (sha1:111111)
├──   Blob   main.c        (sha1:222222)
└──   Tree   src/   (ghi789)
└──   Blob   util.c    (sha1:333333)
```


Object files are normally just loose files in` .git/objects/` . Sending all those objects over a network individually would be expensive and inefficient. Packfiles are Git's solution: they're like compressed tarballs for Git objects, but smarter. They compress, deduplicate, and send diffs instead of whole files.


Here's a toy example. Suppose you had a blob with the contents:


```text
Hello world
```


And then you commit a change to make it:


```text
Hello universe
```


Git can send a delta that says something like:


```text
reuse characters 0 through 6 ("Hello ") of the old blob
insert "universe"
```


Deltas also stack on top of each other, by default up to a maximum of 50 before Git just creates a new object. When you have hundreds or thousands of commits, this can save a ton on network bandwidth.


Packfiles also come with pack index files that map object IDs to their byte offsets inside the (sometimes very large) binary pack. Instead of scanning through a multi GB packfile, Git uses the index to find exactly what it needs. For example, when reversing deltas to create the final file, the index points Git to each delta.


Put it all together, and you can clone the Linux kernel, with millions of commits and gigabytes of history, in a single highly compressed packfile.


## Git's protocol sorta sucks


Before I give any critiques, I can't stress enough that I think Git's protocol is genuinely beautiful. Git isn't trying to offer a complete development workflow; it's trying to offer a distributed version control system (VCS). With that goal, it's more than succeeded. It's the most popular VCS by far, and for good reason.


However, we can also be honest and say that software development has changed since[2005](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git) . Think of the rest of this section as a wish list of what I'd want from a future VCS.


### Git & modern development


At its core, Git only has two concepts: objects and refs. That simplicity is excellent for a content addressable storage system, but insufficient for modern software development. Reviews, pull requests, issues, and CI are all a major part of modern development. None of these exist in the protocol itself. We end up naming everything by branch names, then layering meaning. While the client speaks "blobs/trees/commits," the rest of the world speaks "did CI pass and do two reviewers with CODEOWNERS approval sign off?"


Authorization has a smaller but still notable gap. The protocol can say "you can fetch" and "you can push to these refs," and that's about it. Real policy-required reviews, linear history, protected paths, merge queues, and status checks all live outside the protocol. When a push is rejected, the wire just says "no." Was it a missing approval? A failing check? A merge queue gate? Wrong trailer? You have to click around a UI to learn the actual reason. We've built sophisticated policy engines around Git, but the client gets a shrug.


### Out-of-band CI workflow


CI exposes a different hole. There's no first class handshake like "prove this commit is green before I accept it" or "block until my queued build finishes." Instead, hosts stitch it together with webhooks and REST calls: you push, runners start, statuses get attached, dashboards turn red or green. It works, but it's out of band. The transport doesn't know your push is gated on a pipeline, so the workflow devolves into: push -> alt-tab -> browser -> refresh -> repeat.


The protocol stays simple and stable; everything that makes software development feel modern happens off to the side.


## FIN


At the end of the day, Git’s protocol is still **really cool** 😎. The whole design is built on a handful of simple ideas: content addressable objects, stateless request/response exchanges, and packfiles to make distribution efficient. Those ingredients helped to create the most popular VCS on the planet. The protocol is simple, extensible, and elegant.


But it’s also clear that Git, by itself, isn’t the whole story anymore. Almost everything that makes modern development “modern” happens outside the protocol. Git’s wire format just shrugs and says: “objects and refs.” Hosting providers, plugins, and extensions do the rest. That separation is both a blessing (Git stays stable and compatible) and a curse (every workflow feels bolted on).


So if you care about sync or VCS protocols, Git is both an inspiration and a challenge. Maybe the future is Git plus layers. Maybe it’s a brand new VCS that takes CI, reviews, and policy as first-class citizens. Either way, Git is here to stay for a while, and it's for a good reason.


## Related posts


- [GitHub Actions Runner architecture: The listener](https://depot.dev/blog/github-actions-runner-architecture-part-1-the-listener)
- [What is a tar file?](https://depot.dev/blog/what-is-a-tar-file)
- [Pulling containers faster with eStargz](https://depot.dev/blog/booting-containers-faster-with-estargz)
- [BuildKit in depth: Docker's build engine explained](https://depot.dev/blog/buildkit-in-depth)


Billy Batista


Software Engineer at Depot
