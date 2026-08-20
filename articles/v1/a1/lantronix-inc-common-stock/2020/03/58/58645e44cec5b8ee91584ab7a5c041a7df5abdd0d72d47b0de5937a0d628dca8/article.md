---
schema_version: "1.0.0"
document_id: "58645e44cec5b8ee91584ab7a5c041a7df5abdd0d72d47b0de5937a0d628dca8"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/need-know-googles-android-10-release-2/"
published_at: "2020-03-05T20:59:44+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:a2ce0cf96b8914983b4fe1b84bfded781e3a642275b57ab27aabd68d4035a6ab"
---

# What You Need to Know about Google’s Android™ 10 Release

March 5, 2020


- [General](https://www.lantronix.com/blog/category/general/)


### What You Need to Know about Google’s Android™ 10 Release


## – A Technical Perspective of the Android™ 10 Release


As you may have seen from some of the recent press releases,[Lantronix](https://www.lantronix.com/) Inc. (LTRX) acquired Intrinsyc Technologies Corporation (“Intrinsyc”) a leading provider of solutions for the development of embedded and IoT products. This is Intrinsyc’s first blog release under Lantronix and we are happy to continue to bring news and information on continued developments for our Open-Q™ development kits, System on Modules (SOMs) and Mobile Hardware development kit.


### Today’s topic is the latest Android release from Google, Android™ 10.


Google released[this](https://developer.android.com/about/versions/10/highlights) latest version of AndroidTM 1o most recently, bringing added control on Device and App Privacy and Security, along with other features such as extended Neural Network and Thermal APIs, and modularized software update via[Project Mainline](https://android-developers.googleblog.com/2019/05/fresher-os-with-projects-treble-and-mainline.html) . Android 10 BSPs are available already for Intrinsyc’s[Open-Q 626 Development kit](https://www.lantronix.com/products/open-q-626-usom-development-kit/) and Snapdragon™ 855 Mobile Hardware Development KIT ([HDK)](https://www.lantronix.com/products/snapdragon-855-mobile-hdk/) , with software releases for other development kits to follow.


Below are some of the major changes , as result of Android™ 10 that are of interest to application developers, BSP developers, or OEMs who use Intrinsyc’s development kits.


**Privacy**


Android 10 has several changes that provide added privacy. Some of them are:


- Apps will be allowed to access location data (and other data) only when they are running in the foreground.
- The system places restrictions on starting activities from the background.
- By default, apps are given scoped access into external storage, meaning that they have external storage access to only app files and media created by the app.


Wherever such restrictions are introduced, Android provides alternate options (e.g., to receive periodic location updates, to run a timed activity without use input, or to have an app access another app’s folder) without breaking privacy, and with the user’s concurrence. It is necessary to migrate apps to API level 29 for them to take advantage of such alternate options.


**Security**


- TLS 1.3 is enabled by default, leading to enhanced Transport Layer security. TLS 1.3 deprecates less secure or obsolete cryptographic algorithms and adds support for new ones.
- The OS Platform has been hardened in terms of Sandboxing of some components and other changes such as the addition of bounds and integer checking, to reduce vulnerabilities.


**Modularization**


Android 10[modularizes](https://source.android.com/devices/architecture/modular-system) some system components, allowing framework components to be updated with critical bug fixes and other improvements as needed, without affecting lower-level vendor implementations or higher-level apps and services. These updates can now be done at modular granularity instead of as a full system update, enabling upgrades outside of normal release cycles. Lantronix software development services can assist with all your device upgrade needs.


**App usage statistics**


Android™ 10 accurately tracks app usage with UsageStats when apps are used in split-screen or picture-in-picture mode. Additionally, Android 10 correctly tracks instant app usage.


**Restrictions on non-SDK APIs**


Android 10 adds more restrictions on non-SDK APIs to improve stability, and for consistency in app development and behavior. Apps are restricted from using APIs that are not in the[Android Framework Package Index](https://developer.android.com/reference/packages.html) . Android provides several ways to ensure newly developed apps confirm to these restrictions using debuggable apps, and Veridex/Lint tool.


Since this change affects developers, there are options provided to access non-SDK APIs in the development phase[using ADB commands](https://developer.android.com/distribute/best-practices/develop/restrictions-non-sdk-interfaces#how_can_i_enable_access_to_non-sdk_interfaces) depending on API level. For development purposes, these features may be accessible in the Open-Q development kit BSPs, and Lantronix software development services can assist with any security needs in your final production software releases.


To take advantage of these new features, and to ensure apps work seamlessly when moving from previous Android versions to Android™ 10, it is suggested to update them to API level 29.


More details on the changes in Android 10 are explained by Google[here](https://developer.android.com/about/versions/10/features) .


Authors:


Dinesh RV​​, SW Engineering Manager, Lantronix IES, India


Ganesh Biradar, Embedded SW Engineer, Lantronix IES, India
