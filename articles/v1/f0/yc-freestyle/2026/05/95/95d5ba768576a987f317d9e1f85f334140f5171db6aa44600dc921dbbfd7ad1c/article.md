---
schema_version: "1.0.0"
document_id: "95d5ba768576a987f317d9e1f85f334140f5171db6aa44600dc921dbbfd7ad1c"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/agent-filesystems-git"
published_at: "2026-05-02T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:b5e9662d9935f48e7bde308850a6fc0cb093d9f48182d7a61096e112c7cf7cff"
---

# The best filesystem for AI agents is Git

Agents are suddenly making everyone care about filesystems again.


That is not nostalgia. It is an interface problem. Agents are trained on commands like` ls` ,` cat` ,` grep` ,` find` ,` git diff` , and` npm test` . They can inspect a tree, edit a file, run a command, and explain what changed. When we give an agent a pile of API endpoints, a vector database, or a bespoke memory protocol, we are often asking it to learn a new operating model. When we give it a filesystem, we are giving it one of the few abstractions it already understands.


You can see the convergence everywhere.[Archil](https://docs.archil.com/) is building elastic file systems for agents that can synchronize to S3-compatible storage and run commands directly on the disk.[Supermemory's smfs](https://smfs.ai/) mounts agent memory as a real directory and turns familiar operations like` grep` into semantic search.[Mintlify's ChromaFs](https://www.mintlify.com/blog/how-we-built-a-virtual-filesystem-for-our-assistant) replaced a docs-assistant RAG path with a virtual filesystem over Chroma and reported session creation dropping from roughly 46 seconds to roughly 100 milliseconds.[Wordware's Sauna](https://www.freestyle.sh/products/git) is a good example of a knowledge-work product that needs versioned files, and its team uses Freestyle Git with LFS support.


The pattern is real: agents want a filesystem. The best way to handle an agent's filesystem is to make Git the source of truth underneath it.


The question is what should be underneath it.


## What an agent filesystem actually needs


For a local script, "filesystem" means files and directories. For a remote agent product, it means more than that.


The filesystem has to survive the agent run. It has to be inspectable by humans. It has to support multiple attempts, rollbacks, promotion, and sometimes multiple agents working on the same project. It has to be cheap enough to create per user or per task. It has to be debuggable when the model does something strange. And if the product touches code, docs, workflows, reports, or generated app state, the filesystem needs a review model.


That gives us a practical checklist:


- Can an agent use normal tools against it?
- Can we preserve state after the VM or sandbox goes away?
- Can we fork work cheaply for parallel exploration?
- Can a human review exactly what changed?
- Can we merge, rollback, or promote state without copying an entire machine?
- Can we attach permissions, tokens, and automation to it?
- Can it coexist with large binary data and generated artifacts?


Most storage primitives answer a few of these well. Git answers the important ones natively.


## Option 1: attach a persistent disk


The simplest answer is to give every agent a real disk and keep it around.


That is useful, and Freestyle supports it. Our[VM lifecycle docs](https://www.freestyle.sh/docs/vms/lifecycle) cover how VMs can suspend, resume, stop, and fork because different jobs need different lifetimes. A persistent VM makes sense when the VM itself is the source of truth. A cached VM makes sense when you want a warm workbench but the canonical state lives somewhere else.


For agents, persistent disks are excellent scratchpads. They preserve installed dependencies, generated build output, browser caches, and other expensive-to-rebuild state. They also keep POSIX semantics simple. If the agent can run on Linux, it can write to the disk.


But a disk is a poor review artifact.


The interesting unit in an agent product is rarely "this block device changed." It is "the agent edited these files, for this reason, on this branch, and these changes passed these checks." Persistent disks do not naturally give you commits, diffs, authorship, branch isolation, pull requests, or merge semantics. You can build those on top, but once you do, you are reimplementing a lot of Git badly.


Use persistent disks for workspaces and caches. Do not make them the canonical filesystem unless the thing you are building really is a long-lived machine.


## Option 2: use an S3-backed filesystem


The next obvious move is object storage. S3-compatible systems are cheap, durable, elastic, and familiar to infrastructure teams. If agents need to read large document sets, generated assets, logs, datasets, or media files, object storage is hard to beat.


The filesystem layer on top is where the tradeoffs show up. AWS's[Mountpoint for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mountpoint.html) is a good reference point because the docs are clear about the boundary: Mountpoint maps S3 objects into file operations, but it is not a full POSIX filesystem. AWS says it supports basic operations and large read-heavy workloads, but not features like modifying existing files, deleting directories, symbolic links, or file locking.


That boundary is not a flaw. It is the nature of object storage. An object store is not a tree of mutable inodes. It is a key-value store with object APIs. You can expose it as files, but the mount has to decide what rename means, what overwrite means, what directory consistency means, and what happens when two agents edit the same path.


This is still a useful fit when the data is mostly large, durable, and append-oriented:


- raw uploads
- screenshots and videos
- scraped documents
- build artifacts
- logs
- model outputs
- data lakes


But if the agent is doing iterative knowledge work, code work, or document work, object storage alone leaves you without the key product primitives: branch, diff, review, merge, revert. If the reason you reached for S3 was large files, Git LFS is usually the better answer because the workspace still behaves like Git.


## Option 3: build a custom filesystem with FUSE


This is the most fun option.


[FUSE](https://www.kernel.org/doc/html/latest/filesystems/fuse/) lets a userspace program present a filesystem to the kernel. The agent and its tools see files. Your implementation can back those files with almost anything: S3, Postgres, Chroma, Redis, a vector index, a memory graph, a Git remote, or some combination of them.


This is why FUSE and virtual filesystems are having a moment. They let infrastructure teams give agents the interface they already know while keeping the backing system optimized for the actual workload.


Mintlify's ChromaFs is a good example. Their docs assistant did not need a real sandbox for read-heavy documentation lookup. It needed something that behaved enough like a repository tree for commands such as` grep` ,` cat` ,` ls` ,` find` , and` cd` . By translating those filesystem-like operations into queries against the existing Chroma index, they kept the agent workflow while removing most of the boot cost.


Supermemory's smfs is another version of the same idea. It exposes memory as a mounted directory, with semantic search hidden behind normal command-line behavior. The agent does not need to learn a "memory API" to start exploring context.


Custom filesystems are also where product taste matters. You can expose generated files that do not physically exist until read. You can make` profile.md` a live synthesis. You can make` grep` semantic. You can lazily hydrate file contents. You can enforce permissions before the agent ever sees a path.


The trap is making the FUSE layer both the interface and the source of truth.


If your custom filesystem is the source of truth, you now own all the hard questions:


- What is a version?
- How do two agents branch?
- How does a human review a proposed change?
- How do you merge?
- How do you prove what the agent saw when it acted?
- How do you promote state from draft to production?
- How do you sync to existing developer workflows?


FUSE is an excellent presentation layer. It is an excellent optimization layer. The clean version is often a Git-backed FUSE mount: the agent sees files, while Git still owns branches, commits, diffs, and LFS pointers.


## Option 4: make Git the filesystem database


Git already is what many agent filesystem systems are trying to become.


The[Git internals book](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) describes Git as a content-addressable filesystem: at its core, a key-value store where content is stored and retrieved by hash. On top of that object database, Git gives you trees, commits, refs, branches, tags, remotes, diffs, merges, authorship, and signatures.


That is the part agent platforms need.


A commit is not just "some files changed." It is a durable snapshot with identity, time, message, parentage, and a tree. A branch is not just a folder copy. It is a cheap movable pointer to a line of work. A diff is not just a comparison. It is the artifact humans use to decide whether an agent's work should ship. A remote is not just storage. It is the replication protocol every development tool already speaks.


That makes Git the right default source of truth for agent workspaces:


- Agents can work in branches instead of mutating production state.
- Humans can review diffs instead of inspecting disks.
- Failed attempts can be abandoned instead of cleaned up.
- Successful attempts can be merged, reverted, tagged, or deployed.
- Every action can be traced to a commit.
- Existing tools already know how to clone, fetch, push, diff, blame, and merge.


This is why Freestyle built[Git repos for AI agents](https://www.freestyle.sh/products/git) as a product primitive. You can create repositories programmatically, grant identity-scoped access, issue tokens, inspect contents without a checkout, compare branches, read raw Git objects, and attach automation to pushes. The agent can use the normal Git CLI, your backend can use the API, and your product can keep one canonical version graph.


Git also handles the "what about big files?" objection better than people give it credit for.[Git LFS](https://git-lfs.com/) keeps large file contents in LFS storage while Git tracks small pointer files in the repository. The agent still gets one workspace. The product still gets branches, commits, diffs, review, rollback, and permissions. Large binaries stop being a reason to split the filesystem model.


## Git does not force one interface


The best part is that choosing Git as the source of truth does not mean forcing every agent through` git` commands.


You can mount Git as a filesystem. Cloudflare's[ArtifactFS](https://developers.cloudflare.com/artifacts/guides/artifact-fs/) mounts a Git repository as a local filesystem without waiting for a full clone, hydrating contents on demand. That is a good fit when startup time matters and tools expect a working tree.


You can use the Git CLI. This is still the most universal interface for coding agents. It gives agents the exact operations they have seen in training data:` git status` ,` git diff` ,` git add` ,` git commit` ,` git push` .


You can use Git from JavaScript.[isomorphic-git](https://isomorphic-git.org/) is a pure JavaScript implementation that runs in Node and browser-like environments, including workers, and can clone, checkout, commit, push, merge, and read raw Git objects.


You can use a hosted API. Freestyle Git lets your application read contents at a branch or commit, compare two revisions, create commits, grant permissions, and download archives without making your own Git infrastructure part of the product surface.


This is the key architectural distinction: Git should own the version graph. The agent interface can be a FUSE mount, a shell, a browser API, or a backend API.


## Why Git LFS matters


Git LFS changes the storage argument because it lets Git remain the product abstraction even when the workspace contains files that are too large or too binary for normal Git storage.


That matters for agents. The agent does not need a separate "artifact store" concept for every image, PDF, or generated asset. It can write files, commit pointers, push a branch, and let the platform move the bytes. The reviewer sees the same branch. Automation sees the same commit. Deployment can still key off Git.


This keeps the mental model small:


- files are in the repo
- large files are still referenced by the repo
- changes are reviewed as Git changes
- promotion is a merge
- rollback is a revert or reset
- automation starts from commits


That is the product advantage. Git is not just storing files. Git is storing the decision history around the files.


## The conclusion


Agents need filesystems because filesystems are the interface they can operate. But remote agent products need more than files. They need durable, branchable, reviewable, auditable state.


Persistent disks give agents continuity. S3-backed filesystems give them elastic object access. FUSE gives them a great interface for custom semantics. Git gives them the filesystem database, and Git LFS keeps that database practical when the workspace includes large files.


The best architecture is not to choose between Git and filesystems. It is to make Git the database for the filesystem, then expose it through whatever interface your agent and product need: a mounted tree, the Git CLI, isomorphic-git, or a hosted API.


That is the role of[Freestyle Git](https://www.freestyle.sh/products/git) : it is the best way to handle an agent filesystem when you need real Git repos, programmatic creation, scoped permissions, serverless file inspection, branch comparison, webhooks, GitHub sync, and LFS support as infrastructure your product can depend on.


That is the default we think agent platforms should choose.
