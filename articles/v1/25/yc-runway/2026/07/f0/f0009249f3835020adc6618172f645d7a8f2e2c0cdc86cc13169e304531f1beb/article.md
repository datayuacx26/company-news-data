---
schema_version: "1.0.0"
document_id: "f0009249f3835020adc6618172f645d7a8f2e2c0cdc86cc13169e304531f1beb"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/rollbacks-on-mobile-yes-they-are-possible-and-this-is-why-you-need-them"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:5e44bd547473e69dfa786094b234c46aec9847979bf095ed71c03b859d7e2a0f"
---

# Rollbacks on mobile: yes, they’re possible, and this is why you need them

Let's face it — no matter how much testing you do, there is always a chance that a release of your mobile app will go wrong. But reacting as quickly as possible can significantly minimize the impact that a bad release can have on your users.


In many ecosystems, such as web development, it is common to deploy code changes to production multiple times a day. If a deployment suddenly breaks something in production, teams can usually fix things by re-deploying the last stable version of their app, giving them time to diagnose and fix the issue. This process is known as **rolling back a release** .


Rolling back problematic releases is a common practice across many software development ecosystems. With robust observability tools in place, teams can monitor the health of a deployment in real time, quickly identify issues, and even automate the rollback process. This ensures that problematic releases are mitigated swiftly — often within minutes — minimizing downtime and reducing the impact on end-users.


But for mobile teams, it’s an entirely different story. The nature of mobile releases makes implementing a true rollback mechanism inherently impossible. But luckily, with a little bit of effort, a close approximation to rollbacks can be achieved.


In this article, we will discuss what sets mobile releases apart from other software deploys, making “true” rollbacks impossible, and how you can implement a process that gets you pretty close to rollbacks on mobile, serving as a powerful tool to mitigate incidents should they arise.


## Mobile releases are different


On mobile platforms, shipping a fix for a bad release is uniquely difficult. A number of key factors make mobile releases much more challenging when compared to those of other ecosystems:


- **App Store review process** : When you release a new version of your app to the Apple App Store or Google Play Store, you package it up into a binary (with a unique version string and build number) and send that binary to Google or Apple for review. The review process can take anywhere[from a few hours to a few days](https://www.runway.team/appreviewtimes) . This means that if you need to quickly patch a bad release with a new binary, you will need to resubmit and wait for the store review to complete before you can release the patched version. While it is possible to expedite the review in some cases, it is still a time-consuming process that creates a bottleneck for mobile app releases. Even worse, the review team could reject the update altogether, forcing you to fix any issues raised by the review team before your update can go live.
- **Build numbers matter:** Mobile app stores validate build numbers to ensure that devices install only newer versions of the same app. For this reason, when you want to release a new version of your app to the store, you have to generate a new build with a higher build number and, depending on the size of your project and how long it takes to build, this can slow releases down.
- **User adoption** : Unlike web or backend deployments, where users request the latest deployed version of the code each and every time, mobile apps are binaries that get installed on people’s devices. This creates a long-tail of app versions, since there is no guarantee that all users will update to the latest available version. If there’s a problem with a release, it means that even if you are able to release a hotfixed version quickly, some users may still be using the bad version for some time (or even indefinitely!). There are some strategies you can put in place to help with this (think forced updates or remote configuration changes), but they can be complex to implement, are usually considered a bad user experience, and may not be possible in all cases.


If we take into account the above points and combine them with the fierce competition that exists in the mobile app market, it is clear that getting things right each and every release (avoiding the need for hotfixes or rollbacks entirely) should be the goal. But it’s also important to acknowledge that mistakes happen: one day you *will* ship a bad release, and when that day comes, wouldn’t it be great to have a backup plan? Wouldn’t it be great to be able to simply (ideally, automatically), roll back to the last stable binary in the same way as it’s done in other ecosystems?


## Rollbacks on mobile – how it works


Achieving rollback capabilities for mobile apps isn’t trivial, but with a little planning and preparation, it’s possible to get to something that’s pretty close.


In simple terms, performing a mobile rollback involves re-submitting the last stable binary as a “new” version. Unfortunately, due to the build number requirements described in the previous section, the rollback build must have a higher build number (and on the Apple side, a higher version).


Generating a rollback build can be accomplished with the following steps:


1. **Identify the source binary.** Find the build artifact file (.ipa, .apk, or .aab) of the last stable release version. This will be the source binary for your rollback build.
2. **Update the version.** Update the version number on your source binary to be higher than the number of the version you’re replacing.
3. **Update the build number.** Update the build number on your source binary to be higher than the build number of the latest build you uploaded to the relevant app store.
4. **Re-sign & upload your rollback build.** Modifying the build number or version of a binary invalidates its signing on both iOS and Android. This means you must re-sign your updated build in order to upload it successfully to the relevant app store.
5. **Create a new version or release.** Create a new version (or release) in App Store Connect or Play Console using the rollback build’s version number.
6. **Update required metadata.** Attach all the required metadata (screenshots, release notes, app details) from the source version to the rollback version.
7. **Select the rollback build.** Select the rollback build as the build to be released with the rollback version.
8. **Submit rollback for review.** Submit the prepared rollback version for review.


The process above sounds simple in theory but there are a few considerations that make it a bit more complex in practice:


1. **Obtaining a copy of the last stable version binary** : The Google Play Console’s app bundle explorer keeps a record of all uploaded binaries, but unfortunately Apple doesn’t offer the same functionality. In either case you need to obtain a copy of the last stable version of your app, either through a build distribution service that allows you to download build artifacts or through some other means, and these binaries aren’t always easily accessible. If you don’t store a copy of your build artifacts, finding the source binary file might prove to be particularly problematic.
2. **Updating the build and version number** : Once you have retrieved the source binary, you need to unpack it and update the build and version numbers to ensure that the new version is higher than that of the current version. While the process of updating a build’s information is often trivial, its consequences are not. Modifying a build that has been signed with your team's signing credentials will change its signature and, if you don't re-sign it, it will not be accepted for upload to either app store.
3. **Creating a new version in App Store Connect and Play Console** : On the Apple side, due to the way that App Store Connect works, you[can't create a new version of your app until the current version is live](https://developer.apple.com/help/app-store-connect/update-your-app/create-a-new-version/) . This means that if you want to prepare a rollback for your current release, you will need to time things properly, waiting for the current version to be live before you can create the rollback version and submit it for review. This can add a significant delay to the rollback process if it’s happening just-in-time, which is why it can be beneficial to automate the process of preparing rollbacks for all releases – this is the only way to ensure that a rollback version is ready to be released at all times, if needed. On the Google side, the limitations are similar — only one draft release can exist on the Production track at a time, meaning you can’t prepare a rollback release until the current release has been submitted for review.
4. **Rollbacks aren’t always possible:** When considering releasing a prepared rollback to mitigate an issue in production, it’s important to be aware of the fact that a rollback isn’t always possible. For instance, if the faulty release included a database migration or schema change that cannot be reversed, deploying a rollback build might break functionality even further or corrupt user data.


To be most effective, rollbacks on mobile need to be available as soon as possible after the latest release has gone live to any number of users. By automating the rollback build creation and submission steps and timing, you can minimize any lag in having a rollback approved and available to release, ensuring that your mobile team’s reaction time is as quick as possible if you’re faced with a bad release.


## A safety net with caveats


While rollbacks are a great resource to have in your mobile toolbox, rolling back might not always be the best course of action when faced with a buggy release. Not all issues warrant a rollback, and in some cases, rolling back could introduce more problems than it solves. If the issue is minor and a hotfix can be prepared quickly, rolling forward and addressing the issue directly may be more practical than initiating a rollback.


Rollbacks should be considered the last line of defense when it comes to bad releases. Your team should very rarely need rollbacks and should instead try to minimize the risk of incidents in production across all releases by establishing a solid release process, using feature flags to enable and disable features in production without requiring a new release, and by thorough monitoring of both release and pre-release builds.


For teams that consider the stability of their production releases mission-critical, rollbacks may still be a process investment worth making. Although they require effort, the benefits can outweigh the cost by providing a clear fallback mechanism for critical incidents.


Designing a process by which each and every release has an accompanying rollback prepared and ready to go if needed is certainly possible, but it’s clear that the steps involved (along with the need for precise timing for each step) make the effort involved non-trivial.


Platforms like Runway now offer[mobile rollbacks out-of-the-box](https://www.runway.team/rollbacks) , providing that extra sense of security without the upfront time and resource investment involved in getting a reliable rollback process in place on your mobile team.


Think of rollbacks as a tool you reach for when all else fails. Although it’s something you might only need once in a blue moon, establishing a rollback process can lead to higher confidence in your releases and reassurance that your team has the tooling in place to mitigate the impact of the worst possible incidents in production on your users and business.


‍
