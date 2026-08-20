---
schema_version: "1.0.0"
document_id: "410f1261e8aa6d4c78ecf0a5f35aceb3be07eb264df449042afb41c980e2824c"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/how-we-reduced-our-ios-appstore-binary-size-54a870b7a4ed"
published_at: "2021-11-09T19:50:29+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:dabc80b683b5d190e19d62a0f9c79dd42dbdf0c4f43384f509d67861e1c60551"
---

# How we reduced our iOS AppStore binary size

# How we reduced our iOS AppStore binary size


[Victor Sigler](https://medium.com/@Vkt0r?source=post_page---byline--54a870b7a4ed---------------------------------------)


5 min read


·


Nov 9, 2021


--


Press enter or click to view image in full size


A few months ago, our Forensics team was inspecting the crashes in the iOS app in Firebase Crashlytics. A beautiful red banner that I’m sure you have seen before was there alerting us that we were missing **dSYMs** and some crashes were not being shown.


Press enter or click to view image in full size


We decided to start an investigation 🧐 right away to identify the causes of the missing **dSYMs** in Firebase Crashlytics. By now, you might be wondering what has **dSYMs** have to do with reducing app size? Read on 😉.


## Problem


What are these **dSYMs** 🤔 files that Crashlytics is complaining about? According to[Apple](https://developer.apple.com/documentation/xcode/building-your-app-to-include-debugging-information) :


> When Xcode compiles your source code into machine code, it generates a list of symbols in your app — class names, global variables, and method and function names. These symbols correspond to the file and line numbers where they’re defined; this association creates a debug symbol, so you can use the debugger in Xcode, or refer to line numbers reported by a crash report. Debug builds of an app place the debug symbols inside the compiled binary file by default, while release builds of an app place the debug symbols in a companion Debug Symbol file (dSYM) to reduce the size of the distributed app.


We have several environments in our app (QA, Alpha, Beta, Production), and for each environment, we upload the generated **dSYMs** to Crashlytics using **fastlane** and the[upload_symbols_to_crashlytics](https://docs.fastlane.tools/actions/upload_symbols_to_crashlytics/) **** action. We started checking if our build settings in the app were generating the necessary **dSYM** files for our production builds, and everything was good ✔️.


We didn’t have Bitcode enabled in our iOS app. We were aware of the[benefits of Bitcode](https://developer.apple.com/documentation/xcode/doing-basic-optimization-to-reduce-your-app-s-size) and the optimization it allows to reduce the app’s size, but for unknown reasons, we had issues enabling it in the past, and we decided to turn it off. We noticed that some of our third-party and internal libraries have Bitcode enabled by default in their build settings, which could create issues in our binary as we didn’t have it enabled.


## Solution


**We decided to enable Bitcode for our app!** Enabling Bitcode for our entire app and all the project’s in the workspace was not a difficult task but a little tricky as we would need to be sure that Bitcode was enabled for all the projects and libraries consumed in the iOS app and we had to modify our lanes in our **Fastfile** to start adding the symbols when the app is archived.


Once we had everything compiling and running successfully in a private branch we decide to open a draft PR to test the CI and be able to get insights in how much our IPA size was growing now with Bitcode and the symbols included. We created a[Danger](https://danger.systems/ruby/) plugin to control in each pull request how much our IPA is changing compared with the latest successful pull requests merged and we generate a report for developers to keep track of it. The change of our IPA binary size in the pull request was very interesting.


Press enter or click to view image in full size


Our IPA size grew by **124.34MB** ⬆️! We want to ensure that our problem with the **dSYMs** was solved and check the App Store estimated sizes for the generated IPA as the IPA size is not a reflection of the real size in the App Store.


We have the tooling to upload apps to Testflight from a private branch to test critical cases in Production mode and distribute to the stakeholders before it’s released. Once we uploaded the IPA to TestFlight, these were the results compared with our latest released version, 20.5.0:


Press enter or click to view image in full size


The compressed file size was reduced by **4.7 MB** , but let’s see how the comparison between those two builds in App Store File Sizes was for each device.


Press enter or click to view image in full size


**Install Size** is the amount of disk space the app will take up on the customer’s device.


Press enter or click to view image in full size


**Download Size** is the compressed size of the app downloaded over the air.


We reduced our download size by **~5%,** and our most significant improvement was in the install size reducing it by **~22%** ⬇️.


## Sub-problems


Enabling Bitcode for our app means that Apple recompiles our app to reduce its size. The recompilation happens when we upload the app to App Store Connect, so at this point, we had to introduce a job in our CI to download the **dSYMs** from Apple servers and upload it to Crashlytics **** every day. The other option was to wait until a build submitted to Testflight finished the processing, but this could take a long time, making our CI hang out unnecessarily.


Another problem we had to face enabling Bitcode in our app was that every vendor of a third-party library that distribute binaries needs to distribute the **dSYMs** and the *bitcode symbol map* ( **bcsymbolmap** ) included with the framework. Crash report tools like Crashlytics need the *bitcode symbol map* to symbolicate the crashes correctly. Otherwise, you will start seeing the *hidden#109* lines in Crashlytics. Xcode replaces the symbols in your app’s dSYM files with obfuscated symbols, and the *bitcode symbol map* is necessary to deobfuscate it correctly.


*We couldn’t be happier with the results of enabling Bitcode in our app, and the numbers speak for themselves!*


## Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
