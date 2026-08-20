---
schema_version: "1.0.0"
document_id: "b525da2eaa2c0e270f32b2e7fee6a1124b54603430b8ef9708c6074f76ea5661"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/whats-new-in-git-2-55-0/"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:0ae62a96e9c212746fedf367ee0fb566215935bf3a4e19968fe8645db34f2d17"
---

# What's new in Git 2.55.0?

The Git project recently released[Git 2.55.0](https://lore.kernel.org/git/xmqqv7b1w9vr.fsf@gitster.g/T/#u) . Let's look at a few notable highlights from this release, which includes contributions from the Git team at GitLab.


What's covered:


- git-history(1) learns fixup
- fsmonitor daemon for Linux
- git push to a remote group
- Limiting git log --graph lane width
- Evolution of Rust in the Git codebase
- Faster git-grep(1) and git-cherry(1) in partial clones


## git-history(1) learns` fixup`


In[our highlights of Git 2.54.0](https://about.gitlab.com/blog/whats-new-in-git-2-54-0/#easier-editing-of-your-commit-history) , we covered the introduction of[git-history(1)](https://git-scm.com/docs/git-history) . In 2.55.0, a new subcommand for this tool was added:` fixup` .


Imagine you've made some changes and you want to amend those changes into an existing commit. The most common approach to this is to create a fixup commit and autosquash it with[git-rebase(1)](https://git-scm.com/docs/git-rebase) :


```text
git   commit   --fixup=  <  commit-id  >
git   rebase   -i   --autosquash   <  commit-i  d  >  ^


```


Doing this in two steps is clumsy, especially because it requires an interactive rebase. Instead you can use the git-history(1)` fixup` command:


```text
git   history   fixup   <  commit-i  d  >


```


This takes the staged changes and amends them into the given commit. As an added bonus, because you're using git-history(1), all other local branches that contain the fixed-up commit are updated as well. So when working with stacked branches, fixup-ing a commit in the stack will automatically rebase all related branches.


This[feature](https://gitlab.com/git-scm/git/-/commit/ca7d7d642406e608214ee4529a75a6ad2310de35) was[implemented](https://lore.kernel.org/git/20260427-b4-pks-history-fixup-v3-0-cb908f06264b@pks.im/) by[Patrick Steinhardt](https://gitlab.com/pks-gitlab) .


## fsmonitor daemon for Linux


When working with large monorepos,[git-status(1)](https://git-scm.com/docs/git-status) can be slow to determine what changed in the local worktree because Git would need to traverse the whole working tree to see which files are modified. To speed up this process, in January 2018 a setting` core.fsmonitor` was[added](https://gitlab.com/git-scm/git/-/commit/e05336bddacb90cf243aacc0f7b7f34f900453d7) in Git 2.16. Back then, you had to provide your own tool (like[Watchman](https://facebook.github.io/watchman/) ). When this was configured, this tool runs in the background and monitors changes on the file system. This informs Git that a file was touched and Git then verifies whether the file was modified and updates the cached status. Then whenever the user calls git-status(1), it can simply return the cached status.


In April 2022, the setting` core.fsmonitor` was[changed](https://gitlab.com/git-scm/git/-/commit/1e0ea5c4316d2241dd76ef430a2779db9a097dfb) to accept a boolean value. When this setting is set to` true` , a daemon built-in into Git is used and no more third-party tool is needed. But this filesystem monitor was only implemented for Windows and macOS, support for GNU/Linux did not yet exist.


This changes in Git 2.55, where support for Linux has been added, too. To achieve this,[inotify(7)](https://man7.org/linux/man-pages/man7/inotify.7.html) is used. inotify(7) was chosen over[fanotify(7)](https://www.man7.org/linux/man-pages/man7/fanotify.7.html) because fanotify(7) requires elevated privileges. This comes with a small[caveat](https://git-scm.com/docs/git-fsmonitor--daemon#_linux_caveats) though, the fsmonitor needs to put a watcher on each and every directory in the repository. In a large repo you might hit the limit of inotify watches (` fs.inotify.max_user_watches` ), which you may need to raise.


These[changes](https://gitlab.com/git-scm/git/-/commit/4d11b9c21863c1a860fdf61a9066f0d94c0d692a) were[submitted](https://lore.kernel.org/git/pull.2147.v15.git.git.1776259657.gitgitgadget@gmail.com/) by[Paul Tarjan](https://paultarjan.com/) based on work by Eric DeCosta and[Marziyeh Esipreh](https://github.com/maryis) .


## ` git push` to a remote group


Quite some time ago[git-fetch(1)](https://git-scm.com/docs/git-fetch) learned to fetch from a[group](https://git-scm.com/docs/git-fetch#Documentation/git-fetch.txt-group) of remotes.


The following command configures a group of remotes:


```text
git   config   set   remotes.forks   "origin upstream"


```


When this is set up, you can git-fetch(1) from this "forks" group, and then all the remotes in that list are fetched from. This can be useful when you want to get the updates from a set of remotes in one go.


[git-push(1)](https://git-scm.com/docs/git-push) , however, was not able to use remote groups.


In Git 2.55, this gap is closed and git-push(1) now accepts a remote group too. For example if you want to push the` main` branch to the group mentioned above:


```text
git   push   forks   main


```


Similar as with git-fetch(1), this command pushes the specified refs to each of the remotes in the group. Each remote is pushed independently and honors its own` remote.<name>.push` mapping and mirror settings.


This[feature](https://gitlab.com/git-scm/git/-/commit/2c677d20b684cdcae669fe508d4cd60fea8cb320) was[submitted](https://lore.kernel.org/git/20260503153402.1333220-1-usmanakinyemi202@gmail.com/) by[Usman Akinyemi](https://github.com/Unique-Usman) , suggested by[Junio C Hamano](https://lore.kernel.org/git/xmqqiki0ivgy.fsf@gitster.g/) .


## Limiting` git log --graph` lane width


The[--graph](https://git-scm.com/docs/git-log#Documentation/git-log.txt---graph) option of[git-log(1)](https://git-scm.com/docs/git-log) draws an ASCII representation of the commit history. In a repository with many active contributors this graph can grow very wide. For example, on the[git.git](https://gitlab.com/git-scm/git/) repository this graph grows nine lanes wide after only 30 commits:


```text
* 26d8d94e94 A few more topics before -rc2
*   02bb39c5cb Merge branch 'js/objects-larger-than-4gb-on-windows-more'
|\
| * c6a4629e32 odb: use size_t for object_info.sizep and the size APIs
| * 7a3a78cc76 packfile,delta: drop the `cast_size_t_to_ulong()` wrappers
| * 188bac14f7 pack-objects: use size_t for in-core object sizes
| * 2d83cc3f84 packfile: widen unpack_entry()'s size out-parameter to size_t
| * 1d43315b31 pack-objects(check_pack_inflate()): use size_t instead of unsigned long
| * 33afe87338 patch-delta: use size_t for sizes
| * 8ea69373a4 compat/msvc: use _chsize_s for ftruncate
* |   8cf57cbec4 Merge branch 'kw/gitattributes-typofix'
|\ \
| * | 0bf506efd4 gitattributes: fix eol attribute for Perl scripts
* | |   8d96f09e92 Merge branch 'js/objects-larger-than-4gb-on-windows'
|\ \ \
| * | | ab3810eb6f zlib: properly clamp to uLong
* | | | 95e20213fa Hopefully final batch before -rc2
* | | |   8632b5c49d Merge branch 'en/commit-graph-timestamp-fix'
|\ \ \ \
| * | | | fbcc5408fc commit-graph: use timestamp_t for max parent generation accumulator
* | | | |   619931f561 Merge branch 'dl/posix-unused-warning-clang'
|\ \ \ \ \
| * | | | | cf48887610 compat/posix.h: simplify GIT_GNUC_PREREQ() comparison
| * | | | | ffd45926dc compat/posix.h: clean up GIT_GNUC_PREREQ() and UNUSED
| * | | | | 689dc92e50 compat/posix.h: enable UNUSED warning messages for Clang
* | | | | |   621962aa7a Merge branch 'td/ls-files-pathspec-prefilter'
|\ \ \ \ \ \
| * | | | | | 3f5203eeb4 ls-files: filter pathspec before lstat
| | |_|_|_|/
| |/| | | |
* | | | | |   0c706d5092 Merge branch 'ta/doc-config-adoc-fixes'
|\ \ \ \ \ \
| * | | | | | 4fa2c6e045 doc: git-config: escape erroneous highlight markup
| * | | | | | 042221cccb doc: config/sideband: fix description list delimiter
| * | | | | | 3eb61fda62 doc: config: terminate runaway lists
* | | | | | |   49cb068fb2 Merge branch 'jc/t1400-fifo-cleanup'
|\ \ \ \ \ \ \
| * | | | | | | e8f12e0e95 t1400: have fifo test clean after itself
* | | | | | | |   b4970f8448 Merge branch 'td/describe-tag-iteration'
|\ \ \ \ \ \ \ \
| * | | | | | | | 55088ac8a4 describe: limit default ref iteration to tags


```


This happens because every lane continues downward to the commit from where the branch was created. This pushes the commit messages off to the right, making it harder to read. Especially when the terminal screen width is reached, this becomes unusable.


Git 2.55 adds a new` --graph-lane-limit=<n>` option to limit the number of lanes that are drawn. Any lanes beyond the limit are replaced with a` ~` truncation mark, so it stays obvious that the graph was trimmed:


```text
git   log   --graph   --graph-lane-limit=5


```


Using this option for the same 30 commits as above, we'll get:


```text
* 26d8d94e94 A few more topics before -rc2
*   02bb39c5cb Merge branch 'js/objects-larger-than-4gb-on-windows-more'
|\
| * c6a4629e32 odb: use size_t for object_info.sizep and the size APIs
| * 7a3a78cc76 packfile,delta: drop the `cast_size_t_to_ulong()` wrappers
| * 188bac14f7 pack-objects: use size_t for in-core object sizes
| * 2d83cc3f84 packfile: widen unpack_entry()'s size out-parameter to size_t
| * 33afe87338 patch-delta: use size_t for sizes
| * 8ea69373a4 compat/msvc: use _chsize_s for ftruncate
* |   8cf57cbec4 Merge branch 'kw/gitattributes-typofix'
|\ \
| * | 0bf506efd4 gitattributes: fix eol attribute for Perl scripts
* | |   8d96f09e92 Merge branch 'js/objects-larger-than-4gb-on-windows'
|\ \ \
| * | | ab3810eb6f zlib: properly clamp to uLong
* | | | 95e20213fa Hopefully final batch before -rc2
* | | |   8632b5c49d Merge branch 'en/commit-graph-timestamp-fix'
|\ \ \ \
* | | | |   619931f561 Merge branch 'dl/posix-unused-warning-clang'
|\ \ \ \ \
| * | | | ~ cf48887610 compat/posix.h: simplify GIT_GNUC_PREREQ() comparison
| * | | | ~ ffd45926dc compat/posix.h: clean up GIT_GNUC_PREREQ() and UNUSED
| * | | | ~ 689dc92e50 compat/posix.h: enable UNUSED warning messages for Clang
* | | | | ~ 621962aa7a Merge branch 'td/ls-files-pathspec-prefilter'
|\ \ \ \ \~
| * | | | ~ 3f5203eeb4 ls-files: filter pathspec before lstat
| | |_|_|_~
| |/| | | ~
* | | | | ~ 0c706d5092 Merge branch 'ta/doc-config-adoc-fixes'
|\ \ \ \ \~
| * | | | ~ 4fa2c6e045 doc: git-config: escape erroneous highlight markup
| * | | | ~ 042221cccb doc: config/sideband: fix description list delimiter
| * | | | ~ 3eb61fda62 doc: config: terminate runaway lists
* | | | | ~ 49cb068fb2 Merge branch 'jc/t1400-fifo-cleanup'
|\ \ \ \ \~
| * | | | ~ e8f12e0e95 t1400: have fifo test clean after itself
* | | | | ~ b4970f8448 Merge branch 'td/describe-tag-iteration'
|\ \ \ \ \~
| * | | | ~ 55088ac8a4 describe: limit default ref iteration to tags


```


The option only makes sense together with` --graph` . The default is` 0` , which means no limit, and zero or negative values are treated the same way, just like` --max-parents` does.


This[feature](https://gitlab.com/git-scm/git/-/commit/7af2503365b4bc683c16e667b88c0796d6d9d2ee) was[submitted](https://lore.kernel.org/git/20260328001113.1275291-1-pabloosabaterr@gmail.com/) by[Pablo Sabater](https://pablosabater.dev/) .


## Evolution of Rust in the Git codebase


In March 2025, with the release of Git 2.49, the first[Rust](https://www.rust-lang.org/) code was added to the Git codebase. Rust bindings were added to allow Rust code to call into libgit. But none of that Rust code was used by the Git binaries.


In November 2025, in Git 2.52, the first Rust production code was introduced into Git. Then a Rust implementation for the` varint` subsystem was added. This code is optionally compiled if the Rust compiler is available, and when it's not, the C implementation is used. This was added as a test balloon for distributors to start preparing their tooling for a Git release that requires Rust at some point.


Earlier this year, in Version 2.54, more Rust code was added to the codebase with the introduction of the` ObjectID` type. This was added as part of the efforts to implement interoperability between SHA-1 and SHA-256.


Until this release, both build systems Make and Meson would gracefully fall back to the C implementation if the Rust compiler is not found. With this v2.55 release the Rust compiler is required unless you explicitly disable it in the build system.


Please note that this doesn't affect users of Git. It only affects those who *build* Git from source. If you compile Git and don't want to use Rust, disable it with one of these commands:


```text
# Meson
meson   configure   -Drust=disabled


# Makefile
make   NO_RUST=YesPlease


```


Bringing Rust into Git has been an ongoing (and unfinished), multi-release, community effort. It's impossible to attribute this to a single person, but some of the most prominent contributors include[brian m. carlson](https://mastodon.social/@bk2204) ,[Patrick Steinhardt](https://gitlab.com/pks-gitlab) ,[Ezekiel Newren](https://github.com/ezekielnewren) , and Calvin Wan.


## Faster git-grep(1) and git-cherry(1) in partial clones


[git-clone(1)](https://git-scm.com/docs/git-clone) has this feature called[partial clone](https://git-scm.com/docs/partial-clone) . This allows the user to apply a filter to what is sent over from the server. In practice, this is done with the[--filter](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---filterfilter-spec) option. For example:


```text
git   clone   --filter=blob:none   <  remot  e  >


```


This will clone the repository, but that clone excludes all blobs (i.e. the contents of the files in the tree). This can speed up the clone tremendously, but it comes at the cost that Git needs to download blobs later when other commands are used that read file contents. And some commands might need a lot of missing blobs.


[git-grep(1)](https://git-scm.com/docs/git-grep) is one of those commands, as it searches the content of the files. To do so, it obviously needs to have those files. Imagine you want to search the word "TODO" 100 commits back in history:


```text
git   grep   TODO   HEAD~100


```


This command resolves the` HEAD~100` commit and the trees associated with that. But those trees might point to blobs that aren't downloaded yet. Previously, each blob was downloaded separately. But that is improved in Git 2.55. In this version of Git, the blob downloads are batched together into a single negotiating round-trip with the server.


This batching is now implemented for both git-grep(1) and[git-cherry(1)](https://git-scm.com/docs/git-cherry) .


This[change](https://gitlab.com/git-scm/git/-/commit/6d2ba7ead7ab074bfd88e194bb75d6b384c44d4e) was[submitted](https://lore.kernel.org/git/pull.2089.v3.git.1778775928.gitgitgadget@gmail.com/) by[Elijah Newren](https://github.com/newren) .


## Read more


This article highlighted just a few of the contributions made by GitLab and the wider Git community for this latest release. You can learn about these from the[official release announcement](https://lore.kernel.org/git/xmqqv7b1w9vr.fsf@gitster.g/T/#u) of the Git project. Also, check out our[previous Git release blog posts](https://about.gitlab.com/blog/tags/git/) to see other past highlights of contributions from GitLab team members.
