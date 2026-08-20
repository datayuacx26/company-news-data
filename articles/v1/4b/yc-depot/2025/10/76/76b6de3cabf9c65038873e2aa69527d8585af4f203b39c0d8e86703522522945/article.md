---
schema_version: "1.0.0"
document_id: "76b6de3cabf9c65038873e2aa69527d8585af4f203b39c0d8e86703522522945"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-organizations-have-slow-actions-checkout"
published_at: "2025-10-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:261ba66a2ff12ddb876a547172efa78afbcd367bdec8b2575c014bf3d0bbde6f"
---

# Why 98.5% of organizations have slow actions/checkout

If you're using GitHub Actions, you've probably used` actions/checkout` thousands of times to clone your repository onto your CI runner. For many repositories, you never need to think about it. But for large monorepos, cloning can take forever.


[Darwin Sanoy's GitLab blog](https://about.gitlab.com/blog/supercharge-your-git-workflows/) on optimizing git clone operations inspired us to see how we could apply similar optimizations to GitHub Actions.


**[We analyzed over 60,000 real-world checkout steps](https://depot.dev/blog/we-analyzed-66821-github-actions-runs)** from all organizations using Depot's fast GitHub Actions runners. *98.5% of organizations use the default checkout settings.* There is potential for massive performance gains.


We benchmarked one of our own repositories and by applying a few simple optimizations, we **reduced clone times by 96.6%: from 60s to 2s** .


Not every repo will see the same gains as our benchmark. But if you're experiencing painful clone times, these optimizations might offer a cure.


## What is actions/checkout?


The` actions/checkout` action is GitHub's official solution for downloading repository code into your workflow runner. Every time you see this step in a workflow...


```text
-   uses  :   actions/checkout@v5
```


...you're telling GitHub Actions to clone your repository so subsequent steps can access your code.


The` actions/checkout` is already reasonably optimized by default, because it performs a shallow clone with` depth=1` .


A` depth=1` means git checks out the current` HEAD` commit and only its contents. This means it cannot go to a different commit in history.


That's fine. CI workflows almost always want the latest commit anyway. But how can we improve clone times even further?


## Optimization strategy #1: Sparse checkout


In our tests and analysis, we found that only **1.47% of organizations use` sparse-checkout`** . In other words, those organizations that did change from the default checkout settings, most commonly used sparse checkout. And of no surprise in our benchmark, this was the single biggest win.


Why? Sparse checkout downloads only the files you actually need, not the entire repository.


You'll want to specify only directory paths because sparse checkout uses “cone-mode.” Cone-mode is much faster than the older "patterns" mode.


When I first encountered cone-mode sparse checkout, I was confused about how it worked. It's probably a good idea to read the[git-sparse-checkout documentation](https://git-scm.com/docs/git-sparse-checkout#_internalscone_mode_handling) . Now, I kinda get it, and I understand why it is called a "cone".


Here is my understanding. You specify directories to include in your sparse checkout. When you list a directory like` src/api/` , it includes:


- All files at any depth under` src/api/` (this is like the downwards cone).
- All files immediately under` src/` (this is part of the upwards cone).
- All files in the top level-directory (this is also part of the upwards cone).


The idea is that sparse checkout only includes subtrees you care about while keeping your checkout minimal.


```text
-   uses  :   actions/checkout@v5
with  :
sparse-checkout  :   |
.github/
src/api/
```


## Optimization strategy #2: Partial clone with filters


Along with` sparse-checkout` you can do partial cloning. It downloads the repo structure and metadata, but skips downloading file contents until git needs them.


```text
-   uses  :   actions/checkout@v5
with  :
sparse-checkout  :   |
.github/
src/api/
filter  :   'blob:none'   # Don't download file contents initially
```


Partial cloning is generally useful during sparse checkouts in CI where workflows often only need a subset of files.


## Optimization strategy #3: Advanced Git configuration


You can also tweak git's internal settings for better performance, especially on large repositories.


Darwin Sanoy noticed that disabling compression and increasing the HTTP post buffer size can help a lot.


So far I've had mixed results with adjusting these settings.


In theory, disabling compression (` core.compression 0` ) should speed up network transfers by trading CPU time for bandwidth.


Depot's runners have fast network connections, so this tradeoff makes sense. For my benchmark repo, I saw a small improvement.


Darwin also noticed that the default HTTP post buffer size can be too small for large repositories. This can lead to more round-trips and slower transfers. Increasing it to 1GB (` http.postBuffer 1073741824` ) can help.


Again, in my benchmark repo, I saw a small improvement. There is[advice in the git-faq](https://git-scm.com/docs/gitfaq#Documentation/gitfaq.txt-WhatdoeshttppostBufferreallydo) about this too. Although it seems to say avoid using it.


Another interesting setting that may be good in CI environments is to turn off fsync-ing altogether. Remember that CI environments are ephemeral so no need to fsync. To disable fsync you can “negate” all with this config option:` core.fsync -all` .


Anyway, here is how to turn these settings on. Before your` actions/checkout` you can configure your Git with another step:


```text
-   name  :   Optimize Git and Checkout
run  :   |
# Disable compression for faster network transfer.
git config --global core.compression 0
# Increase HTTP buffer for large repositories.
git config --global http.postBuffer 1073741824
# Turn off fsync
git config --global core.fsync -all


-   uses  :   actions/checkout@v5
with  :
filter  :   'blob:none'
sparse-checkout  :   |
.github/
src/api/
```


## Our recommendations


Based on the benchmark results and real-world usage analysis it is definitely worth trying out these optimizations.


Here is a copy/pasta you can use as a starting point:


```text
-   name  :   Optimize Git Configuration
run  :   |
git config --global core.compression 0
git config --global http.postBuffer 1073741824
-   uses  :   actions/checkout@v5
with  :
filter  :   'blob:none'
sparse-checkout  :   |
# List only directories your workflow needs
src/api
.github/
```


Consider adding` sparse-checkout` to the directories you access the most. That single change delivered the biggest win in our testing. Then experiment with the other optimizations to see what works for your setup.


## FAQ


Will sparse checkout break my workflow if other steps need files outside the specified directories?


Yes, it will. If you sparse checkout only src/api/ but a later step tries to access src/web/, that step will fail. You'll need to include all directories that any step in your workflow touches. Start conservative by including everything you think you need, and then remove directories once you've confirmed your workflow runs cleanly.


If I'm using self-hosted runners, will sparse checkout settings persist between workflow runs?


Yes, and this can cause problems. While there might be a fix on the way, for now, when you use sparse checkout, it sets core.sparseCheckout=true in the git config. This setting persists on self-hosted runners. The next workflow that runs on the same runner will inherit those sparse settings even if it doesn't specify sparse checkout. Depot GitHub Actions runners (and GitHub-hosted runners) don't have this issue, since they're ephemeral. For self-hosted runners, you may need to explicitly clean up or reset the git config between jobs.


What happens if I use filter: blob:none but my workflow needs to run git diff or git blame?


Git will automatically fetch the missing blobs on-demand when commands like git diff or git blame need them. The first run triggers the download, then you have those blobs locally for future use. If you're running operations that touch many files across the entire repo (like blame on hundreds of files), you might actually be slower with blob:none than without it due to the on-demand fetching overhead.


## Related posts


- [We analyzed 66,821 GitHub Actions runs](https://depot.dev/blog/we-analyzed-66821-github-actions-runs)
- [Faster GitHub Actions](https://depot.dev/blog/faster-github-actions)
- [GitHub Actions Cache](https://depot.dev/blog/github-actions-cache)


Chris Goller


Principal Software Engineer at Depot
