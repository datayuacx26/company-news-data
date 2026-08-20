---
schema_version: "1.0.0"
document_id: "2f54f442dac1652c9b960aac857feb40d23db060ddf9471762e2e9d8a7cdbebf"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/xcode-26-edge-stack-removal"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-30T00:21:31.709919+00:00"
fetched_at: "2026-07-31T06:29:34.714502+00:00"
content_hash: "sha256:b5b8045d2bb26f8d9a505bf778e3412d72b2686d43ebef298afff2369cc4bd22"
---

# Xcode 26 edge stacks are being retired: what you need to do

If you're building iOS apps on Bitrise and using one of the Xcode 26 edge stacks, you must switch to the Xcode 26 stable stack before **July 31, 2026** .


## What's changing


On **July 31, 2026** , Bitrise will permanently remove the following edge stacks:


- ` osx-xcode-26.0.x-edge`
- ` osx-xcode-26.1.x-edge`
- ` osx-xcode-26.2.x-edge`
- ` osx-xcode-26.3.x-edge`
- ` osx-xcode-26.4.x-edge`
- ` osx-xcode-26.5.x-edge`


The Xcode 26 **stable** stacks are not affected. They're here to stay.


## Why are we removing these stacks?


Edge stacks are not intended to offer long-term stability. This is the purpose of the stable stack. Edge stacks are updated frequently with the latest tool versions (including beta versions of Xcode). They're designed for teams who want to test against upcoming Xcode versions before they reach stable.


We have already published the Xcode 27 edge stack for teams who want to test against the next major Xcode version.


This follows our[stack update policy](https://devcenter.bitrise.io/en/infrastructure/build-stacks/stack-update-policy.html) , which governs our Xcode stack lifecycle.


## What you need to do


**Check if you're affected.** In your` bitrise.yml` , look for any stack ID containing` xcode-26` and` edge` . You can also check in the Bitrise UI under your workflow settings, in the Stack selector. Be sure to check the stack for all workflows, not just the default stack.


Migrate all workflows targeting an Xcode 26.x edge stack to the stable stack or to the Xcode 27.0 edge stack, whichever suits your use-case.


```text
# If you see something like this...
meta:
bitrise.io:
stack:   osx-xcode-26.4.x-edge


# Change it to this:
meta:
bitrise.io:
stack:   osx-xcode-26.4.x


# Or this:
meta:
bitrise.io:
stack:   osx-xcode-27.0.x-edge
```


‍ **Run a test build.** Once you've switched, trigger a build to verify the build still completes successfully. Users storing their` bitrise.yml` in repo can run their test build on a topic branch before merging to the default branch. Be mindful that long-lived topic branches may have an out of date stack defined and need to rebase on the default branch.


## What happens if I don't migrate?


If your` bitrise.yml` is stored on bitrise.io, Bitrise will avoid disruption by automatically running your builds on the equivalent stable stack. For example,` osx-xcode-26.3.x-edge` would be migrated to` osx-xcode-26.3.x` . We still recommend updating your configuration yourself so the change is intentional, clear, and tested.


If your` bitrise.yml` is committed to your git repository, we won't be able to automatically migrate the stack target. If you haven't updated the` stack:` value on all branches, your builds will fail starting July 31. You'll need to update the file yourself before then.


## FAQ


### **What's the difference between an edge stack and a stable stack?**


Edge stacks are not intended to offer long-term stability. They're updated frequently with the latest tool versions, including beta versions of Xcode, and are designed for teams who want to test against upcoming tool versions before they reach stable. Stable stacks are pinned at release and only receive critical security updates, which provides reliability for production CI workflows where consistency matters.


### **What stack should I use instead?**


For most teams: the matching Xcode 26 stable stack (e.g.` osx-xcode-26.4.x` ). If you were using the edge stack specifically to stay on the latest tooling, consider` osx-xcode-27.0.x-edge` .


### **Do I need to check all my workflows?**


Yes. Be sure to check every workflow individually — any workflow targeting an Xcode 26.x edge stack needs to be updated, not just the default stack.


### **My**` bitrise.yml` **is in my git repo. Will Bitrise auto-migrate for me?**


No. If your` bitrise.yml` is committed to your repository, we can't automatically migrate the stack target. You need to update the` stack:` value on all branches yourself, including any long-lived topic branches, which may have an outdated value and need to be rebased.


**I migrated. How do I know it worked?**


Trigger a build. If it completes successfully and the build log shows the correct stack, you're good.


## More questions?


Our[stack update policy](https://docs.bitrise.io/en/bitrise-build-hub/infrastructure/build-stacks/macos-stack-update-policy) provides more background information on how we approach stack support.


[Setting the stack for your builds](https://docs.bitrise.io/en/bitrise-ci/configure-builds/configuring-build-settings/setting-the-stack-for-your-builds) provides more information on how to set the default and workflow override stacks.


The[stacks](https://bitrise.io/stacks) page lists all stacks.


Post in the[community forum](https://discuss.bitrise.io/) or reach out to[support](https://support.bitrise.io/) .


You can also track all upcoming stack changes at[stacks.bitrise.io/announcements/upcoming-stack-deprecations](https://stacks.bitrise.io/announcements/upcoming-stack-deprecations) .
