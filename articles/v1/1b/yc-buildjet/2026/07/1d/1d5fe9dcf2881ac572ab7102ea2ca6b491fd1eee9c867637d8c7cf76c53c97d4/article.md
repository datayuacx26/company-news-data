---
schema_version: "1.0.0"
document_id: "1d5fe9dcf2881ac572ab7102ea2ca6b491fd1eee9c867637d8c7cf76c53c97d4"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/hardware-accelerated-android-emulator-on-buildjet-for-github-actions"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:51cd0ca34ede405ca4b00b4e89db43e2f0f1efb576c6ebe62535987ecbd73a00"
---

# Hardware accelerated Android Emulator on BuildJet for GitHub Actions

Today, we’re delighted to introduce support for hardware accelerated Android emulators on all BuildJet runners, by adding VM in VM support. This allows Android developers to create fast and affordable runners for their CI workloads. **BuildJet for GitHub Actions runs Android Emulator tests 20x cheaper and 2.5x faster** than what is currently possible on GitHub Actions.


Many Android developers enjoy GitHub Actions, its great integration with GitHub and ease of use. However, running tests that need to emulate Android, are extremely slow on GitHub Action’s Ubuntu runners. This is due to GitHub Actions and most other CIs don't allow nested VMs(a VM in a VM), which prevent us from running hardware accelerated emulators.


To work around this on GitHub Actions, Android developers have resorted to using the macOS runners that have HAXM enabled, which enables VM in VM therefore allowing Android Emulators with hardware acceleration. The issue with running tests with GitHub Action's macOS runner is that it’s extremely slow and very expensive.


# 20x cheaper and 2.5x faster


In our testing, we found that BuildJet for GitHub Actions finished the tests 2.5x faster than GitHub Actions running their` macos-latest` runner. Additionally, GitHub Actions is 20 times as expensive as our base runner, which is the one we used to run the benchmark with.


Our 2vCPU runner costs $0.004/min, compared to GitHub Actions $0.08/min for their macOS runner.


For the test, we used one of the most popular Android Libraries, Retrofit. They have a test suite that they run on GitHub Actions` macos-latest` runner using a commonly used Action,` ReactiveCircus/android-emulator-runner` to set up and run their Android Emulator test.


Check out the benchmark workflow run[here](https://github.com/thinkafterbefore/retrofit/actions/runs/2701207328) for more details.


If this sounds interesting and you'd like to use BuildJet for GitHub Actions, simply authorize BuildJet on the repositories you want to use and then update the` runs-on` property in your` workflow.yml` .


```text
yaml  1   runs-on  :   buildjet  -  2vcpu  -  ubuntu  -  2204


```


For more information, please check out[our documentation](https://buildjet.com/for-github-actions/docs) .


Existing customers that would like to take advantage of the faster Android Emulator tests, don’t have to do anything, as we submitted and merged fixes to[ReactiveCircus/android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner) to properly work with BuildJet.


Lastly, we'd like to thank[Yang C](https://github.com/ychescale9) for this development on the[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner) action, and his quick merges. Thanks for your support!


Happy building!
