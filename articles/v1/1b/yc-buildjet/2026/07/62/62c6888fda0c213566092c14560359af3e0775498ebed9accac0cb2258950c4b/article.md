---
schema_version: "1.0.0"
document_id: "62c6888fda0c213566092c14560359af3e0775498ebed9accac0cb2258950c4b"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/why-we-want-to-fix-github-actions"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:a76ae4cedd3bac9d937f2ded53780d6647bd31ba19ba13764960dbb82f273218"
---

# Why we want to fix GitHub Actions

In many previous jobs, we found ourselves hitting refresh on GitHub for 10 minutes just to wait for a test to finish, to be able to merge some simple PR.


As most developers know, regardless of how much you try to multitask, waiting for the CI to finish always feels like an unnecessary bottleneck. As a consequence of project complexity and developers waiting for simple PR's, we always ended up migrating to a self-hosted CI despite it always being extra effort to set it up and maintain.


In the process of repeatedly doing so throughout our careers, we realized that existing CIs run their jobs on a Cloud VM, which hardly goes beyond 3 GHz, while our dev laptop comfortably boosts to 5GHz, and finishes the same build in half of the time.


Realizing this discrepancy, we came up with the idea of building a new CI that instead of paying cloud providers for their overpriced VMs, we build jobs on the latest Zen2, Zen3, 10th gen Intel desktop CPUs, as well as EPYC server CPUs with up to 64 vCPU and 128G of RAM. As a result, we have a much lower cost per job and a significantly faster CI.


To orchestrate all the VM's, we developed our own VM hypervisor management solution based on libvirt, which allows us to easily and efficiently set up a secure VM as soon as someone pushed to their code.


The first iteration of the new CI was a completely independent CI under a different name. We dog fed the CI and as users, we loved the 4x speed and how effortless it was to debug build issues in the CI when something went wrong – a welcomed change in a landscape where existing CI's have bad hardware and sluggish UI's.


Despite how much we enjoyed the CI, we quickly realized that the hurdle of migrating from an already established and working CI to a different platform was quite a big ask, even for people who cared about helping us out.


We still believed speed was a strong USP for a CI product, but had to figure out how to reduce the hurdle for the user to migrate to our new, fast infrastructure. After some back and forth, we created a BuildJet for GitHub Actions. On average it reduces the CI run time by 50% (we forked and tested on ~100 public repos), and is extremely easy to install. It hooks into GitHub Actions as a *self-hosted* runner and leverages our existing dedicated bare-metal infrastructure, without any maintenance work for the user. Simply change 1 line in your GitHub Actions config file and you're set. Easy to install and equally easy to uninstall.


Hopefully, this will be a realization for people that waiting for a CI should not be a necessity.


Enjoy the speed!
