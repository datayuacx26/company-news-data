---
schema_version: "1.0.0"
document_id: "6d7e776def42b9756a3111a1b9c101b65900c332242fb9d51f74cae0406bbe96"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-macos-26-github-actions"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c235d48c2e9c66b5c6f7dfba0875903f3aa32d3d51f679254be1d4b29ee5db00"
---

# Now available: macOS 26 for GitHub Actions runners

macOS 26 (Tahoe) is now available on Depot GitHub Actions runners. Add` depot-macos-26` to a job and it runs on Apple M4 hardware with our in-memory disk accelerator, the macOS 26 SDKs, and Xcode 26 preinstalled. It joins our existing macOS 14 (Sonoma) and macOS 15 (Sequoia) runners, which run on M2.


## What's in the image


### Xcode and Apple SDKs


Xcode` 26.4.1` is the default. Five more releases are installed and can be selected per job with` xcode-select` :


- Xcode` 26.5` (17F42)
- Xcode` 26.4.1` (17E202) — default
- Xcode` 26.3` (17C529)
- Xcode` 26.2` (17C52)
- Xcode` 26.1.1` (17B100)
- Xcode` 26.0.1` (17A400)


The iOS, iPadOS, watchOS, tvOS, and visionOS simulator runtimes are preinstalled, so simulator-based tests don't download them on every run.


### Languages and toolchains


The image also carries the usual GitHub Actions macOS toolchain, including Node.js 24, Ruby 3.4, LLVM 20, .NET 8/9/10, Java 21, PHP 8.5, plus Homebrew, Python, and common CLI tools. For the full list, see the[macOS 26 image README](https://github.com/actions/runner-images/blob/main/images/macos/macos-26-arm64-Readme.md) .


## How to use it


Set the runner label in your workflow file:


```text
jobs  :
build  :
runs-on  :   depot-macos-26
steps  :
-   uses  :   actions/checkout@v4
-   run  :   xcodebuild -version   # uses the default, Xcode 26.4.1
```


To build against a different Xcode, select it with` xcode-select` :


```text
jobs  :
build  :
runs-on  :   depot-macos-26
steps  :
-   uses  :   actions/checkout@v4
-   run  :   sudo xcode-select -s /Applications/Xcode_26.5.app
-   run  :   xcodebuild -version   # now uses Xcode 26.5
```


Or test across several Xcode versions with a matrix:


```text
jobs  :
test  :
runs-on  :   depot-macos-26
strategy  :
matrix  :
xcode  :
-   /Applications/Xcode_26.5.app
-   /Applications/Xcode_26.4.1.app
steps  :
-   uses  :   actions/checkout@v4
-   run  :   sudo xcode-select -s ${{ matrix.xcode }}
-   run  :   xcodebuild test -scheme MyApp -destination 'platform=iOS Simulator,name=iPhone 17'
```


All available macOS labels:


- ` depot-macos-26` for macOS 26 (Tahoe)
- ` depot-macos-15` or` depot-macos-latest` for macOS 15 (Sequoia)
- ` depot-macos-14` for macOS 14 (Sonoma)


Full specs and the complete label list are in the[GitHub Actions runner types](https://depot.dev/docs/github-actions/runner-types) docs.


## Pricing


macOS 26 runners are billed per minute under the same usage-based[pricing](https://depot.dev/pricing) as the rest of our macOS runners. See the[runner types](https://depot.dev/docs/github-actions/runner-types#macos-runners) page for the current per-minute rate.


## Try it


macOS 26 runners are available now. Add` depot-macos-26` to a workflow to give it a try, and[let us know](https://depot.dev/help) if you run into any issues.


## Related posts


- [macOS GitHub Actions Runners are now twice as fast](https://depot.dev/blog/ultra-runners-for-macos)
- [Now available: macOS GitHub Actions runners](https://depot.dev/blog/mac-github-actions-runners)
- [What we learned optimizing EC2 macOS build performance](https://depot.dev/blog/what-we-learned-optimizing-ec2-macos-build-performance)


Héja Péter (Vau)


Staff Infrastructure Engineer at Depot
