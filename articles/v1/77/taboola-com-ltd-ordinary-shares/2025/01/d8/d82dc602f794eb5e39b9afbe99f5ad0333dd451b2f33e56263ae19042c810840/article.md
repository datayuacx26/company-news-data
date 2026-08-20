---
schema_version: "1.0.0"
document_id: "d82dc602f794eb5e39b9afbe99f5ad0333dd451b2f33e56263ae19042c810840"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/sdk-testing-with-hot-swapper/"
published_at: "2025-01-14T14:37:11+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:06fa1cf6d113db241a41b51d3e0158879bd6b307b8295f9dfd4b15202c721436"
---

# SDK testing with hot swapper

In the following article, I describe how we came up with a way to improve the chances that our SDK library gets smoothly integrated in our customers’ Applications and reduce issues when going to production. The main idea is to take a number of significant clients’ applications and replace your existing SDK code with a new code, allowing you to see how the apps perform before you release a new SDK version.


### Why releasing a reliable SDK is so important


Developing an SDK for mobile apps is very different from developing a standalone app. You can think of an SDK as a guest in someone else’s house. You need to behave, you can’t put your legs on the table or wipe your hands on the sofa (well, in most countries you can’t). So what I mean is that you can’t interfere with the app’s normal behavior, break some flows or worse than that, cause memory leaks or god forbid crashes.


### The common scenario and why you can do better


Now let’s say you developed a new feature or fixed a bug in your SDK. You’ve checked it on your device, passed the QA process (manual, automation, several devices and OS versions) and everything works fine. Great, you can release a new version… can you?


Well, No. At least not yet. Building an SDK means there are all kinds of apps that use your product with different implementations and numerous customizations. Of course you can’t send your new release to all your customers and tell them to test the new released version. What you might do is choose several of your biggest customers and tell them to do it, but still the tests and fix cycles will take time, not to mention you don’t want them to see the bugs you have. You must be stable from day one, making sure your SDK does not interfere with the app running it. The worst thing that can happen is your customer losing users because your SDK is messing up with the app functionality


#### Another Such Scenario


It’s getting even more tricky when your customer is updating their own app, regardless of your SDK. For example, a mobile app can change the layout of the app, resulting in a new use case that you didn’t check. So how can we make sure that releasing an SDK does not result in any regression? How can we keep track on the app changes regardless of the SDK?


### Enter SDK Hot Swapper


To solve this issue we had to come up with a new idea – SDK hot swapper ! Imagine you hold in your hands your customer’s application and you can replace the current version of your SDK with a new SDK version you want to test. That’s exactly what our system does.


## *So, how does this magic happen?*


In a gist, to replace an SDK in an APK you uncompress the APK, using ApkTool, replace the folder with your SDK files and re-compress & sign. The main tool we are using is[Apktool](https://ibotpeaches.github.io/Apktool/) . This tool can disassemble an application. This action is called baksmali and what it practically does is disassembling the classes.dex file (all the application’s classes transformed into bytecode) to[smali](https://github.com/JesusFreke/smali) files, which, unlike .dex files are readable. The opposite operation of assemble the apk is called “smali”. * Fun fact – “smali and baksmali” is assemble and disassemble in Icelandic.


### Sounds good! Now tell me how to do it…


Here’s a step by step guide:


1. Disassemble the APK you want to test (So that you can find your SDK’s smali files in the APK and replace them). You can do it by running this command:


```text
apktool d [your apk file]
```


2. The files are arranged by folders. Each package has its own folder so you can easily find your code’s package name. You can also find your SDK package within the source code files.
3. Create a new apk with your new sdk. (It’s important to build this apk in release mode,assuming your customer’s app, which is in production, is also built in release mode). Once you have the new apk you need to run the apktool on this apk too, as described above (Now you have two disassembled apks in your hands). Replace the package folder of your new SDK smali files with the folder containing the files of your old SDK.
4. Now it’s time to assemble the apk back. In order to that you need to run this command:


```text
apktool b [test apk folder] -o [new apk file]
```


This will rebuild the application with the new version.


5. Last but not least, you need re-sign the application. There are several ways to do it, the one I use is[jarsigner](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/jarsigner.html) . This is the command you need to run:


```text
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore [my application apk] alias_name
```


* Tip: As of JDK 7, the default signing algorithm has changed, requiring you to specify the signature and digest algorithms (-sigalg and -digestalg) when you sign an APK


### Final Result:


You have the customer’s app APK running with your new SDK version. Now you can run tests on this application when it is not yet in production, and without any work required from your customer.


** Tip: you can use the apktool not only to replace your sdk, but also to edit the AndroidManifest.xml – I will leave it to you to decide what and how to change there.
