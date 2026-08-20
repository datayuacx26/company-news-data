---
schema_version: "1.0.0"
document_id: "6d9c365c20763aee650441d1c8e5f70e533bde319438f422da2139a90bbf4093"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/latest-linux-updates-for-june-2026"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:dd6c97f90b6843db81b4e3e7dd6cb6d8d948997b6a84a8a8ab71d52d8ef63dc8"
---

# Latest Linux updates for June 2026

‍ **An outdated build environment can slow down your team, introduce security risks, and cause hard-to-debug issues. With our upgraded Linux stacks, you get a faster, more secure, and fully maintained build environment: so your team can focus on shipping great apps, rather than managing infrastructure.**


` Ubuntu Noble 24.04 - Bitrise 2025 Edition` is now available as a stable stack, bringing Noble Numbat as the default Ubuntu version to Bitrise.


## Why this matters?


A modern, up-to-date build environment means faster, more secure, and more reliable builds. With the upcoming changes, you get the latest Ubuntu LTS, updated tooling, and a stable foundation that keeps your CI/CD pipeline running smoothly and your mobile engineers happy.


Nobody wants to spend time debugging their build environment. By keeping stacks up to date, Bitrise takes that complexity off your plate, so you can focus on what matters: building and shipping great apps.


## What is changing?


**June, 2026**` ‍`


- ` Ubuntu Noble 24.04 - Bitrise 2025 Edition` is now the default stable stack.
- ` Ubuntu 22.04 for Android & Docker` is now a frozen stack
- ` Ubuntu Resolute 26.04 - Bitrise 2026 Edition` is now available as a new edge stack.


**April, 2027**


` Ubuntu 22.04 for Android & Docker` will be removed from Bitrise. After that, you won’t be able to run builds on this stack.


## What do I need to do?


**Your builds on Ubuntu Noble 24.04 - Bitrise 2025 Edition**


- All set, no action required.


**Your builds on Ubuntu 22.04 for Android & Docker**


- From June 2026, this stack is frozen and won’t receive any updates.
- This stack will be removed completely in April, 2027.
- Test your builds on ****` Ubuntu Noble 24.04 - Bitrise 2025 Edition` and switch over before April, 2027.
- [Find our handy migration guide here.](https://bitrise.io/stacks/announcements/deprecating-ubuntu-22.04)


**Switching between stacks**


When we release a new stable stack, we update the tooling as well as the Ubuntu version. These updates can bring breaking changes for some builds, so we advise that you test and adapt early for a smooth transition.


## How Bitrise stacks target Ubuntu versions


By default, Android builds run on Linux-based stacks. We create a new stack annually, and that new stack is based on the latest LTS version at the time.


With this change, the previously stable Linux stack (` Ubuntu 22.04 for Android & Docker` ) is now frozen, and will be removed in April 2027. This stack is based on Ubuntu 22.04 Jammy Jellyfish, which was released in April 2022 and will be supported until April 2027.


Last year, we introduced a[Stack Update Policy](https://docs.bitrise.io/en/bitrise-platform/infrastructure/build-stacks/linux-stack-update-policy.html) for our Linux stacks as well, so the Linux stack updates, deprecations and removals are more regular and predictable.


🔗[Learn more about Ubuntu versions here](https://wiki.ubuntu.com/Releases) .


` Ubuntu Resolute 26.04 - Bitrise 2026 Edition` is already available as an edge stack. We suggest you test your builds on this stack as soon as possible. In April 2027, this stack will become the default stable stack.


Along with the new Ubuntu version, some preinstalled tools on the stacks will be updated accordingly.


## Key changes


Compared to the current stable Linux stack (` Ubuntu 22.04 for Android & Docker` ), you can expect the following differences in the new stable stack (` Ubuntu Noble 24.04 - Bitrise 2025 Edition` ).


For more information, check out our documentation on[configuring tool versions.](https://docs.bitrise.io/en/bitrise-ci/configure-builds/configuring-build-settings/configuring-tool-versions)


### Ruby


Ruby 3.4 is now the default version, while Ruby 3.3 and 4.0 are also installed. Ruby 3.1 and 3.2 are no longer installed.


### Node.js


The previous default Node version, Node 20 is no longer installed. The new default is Node 24, the current LTS version. Node 22 is also installed.


### Emulator system images


There are some emulator system images preinstalled to reduce your build times. As a new Android OS version is released, the matching system image and platform package is added to the stack.


Compared to the previous stable stack:


- ` system-images` and` platform` version` 33` are no longer installed
- ` system-images` and` platform` version` 36` are now preinstalled
- ` system-images` and` platform` version` 37` are now preinstalled


### Flutter


The preinstalled Flutter SDK has been upgraded from` 3.16.8` to` 3.38.7` .


### Go


Go 1.25 is now the default version, while Go 1.24 is also installed. Go 1.21 and 1.22 are no longer installed.


### Bazel


[Bazelisk](https://github.com/bazelbuild/bazelisk) is now installed. It also overrides` bazel` in` $PATH` , so any` bazel` execution will either respect your` .bazeliskrc` /` .bazelversion` file or download and execute the latest stable release of Bazel.


### Java


JDK 25 is now installed next to the existing versions.


You can learn more about selecting tool versions and installing additional ones[here](https://docs.bitrise.io/en/bitrise-ci/configure-builds/configuring-build-settings/configuring-tool-versions.html) .


### Python


Python 3.12 has been removed, and Python 3.13 and 3.14 are both pre-installed. Python 3.14 is the default version.


## We’ll keep you updated


[Find out how to get notified here](https://stacks.bitrise.io/tips/get-notified/) .


If you have any questions or feedback on the new stack, don’t hesitate reaching out to us on the usual channels. Looking forward to building amazing mobile apps together on our new Linux stacks.
