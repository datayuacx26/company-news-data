---
schema_version: "1.0.0"
document_id: "5166a60c4191508201823ddf923f012f01d72ef4888f20848a552c54254df28d"
company_key: "yc-mason"
company: "Mason"
source_id: "yc-mason-news-import-529b235964fd"
canonical_url: "https://www.bymason.com/blog/11-most-popular-android-os-customizations"
published_at: null
first_seen_at: "2026-07-22T03:32:07.336551+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:76f5449f1dae4817081aaec0849169926955e974895c27253ba75c5416a5ecaf"
---

# 11 Most Popular Android OS Customizations

One of the biggest benefits of Android being open source is that it gives anyone the ability to build their own custom ROM to make the OS look, feel, and behave exactly as they want. This customization ability has been a big pull for hobbyists in the consumer world who want to personalize their smartphone experience, and has also propelled[Android to new heights in the enterprise world](https://adtmag.com/articles/2019/05/29/idc-android-enterprise.aspx) — specifically for companies building a dedicated device or smart product made for a single application (often referred to as a[corporate-owned-single-use device](https://deviq.io/resources/articles/what-is-cosu/) ).


The ability to build on Android’s mature infrastructure *and* customize the OS is what has drawn big players like Square and Peloton to base their products on Android. Although Square originally went to market on iPads,[when making the decision to build a custom POS](https://www.wired.com/story/square-register-tablet/) , they went with Android.


While many developers love digging into the weeds of[AOSP](https://source.android.com/) , there’s also a huge population out there wanting the benefits of a custom Android OS without having to spend weeks or months building and testing it (when they could be spending that time building their killer app!).


This idea is a big part of what inspired us to create Mason—we wanted to provide an easy way for any developer to build a best-in-class hardware product, without needing a hardware team or in-house Android system experts.


Since launching, Mason users have built over **five thousand customized Android operating systems for their dedicated device use cases** .


To celebrate this milestone, let’s take a look back at the 11 most popular customizations implemented by Mason users. We’ll describe what the configuration does, why it’s useful in a dedicated device, and provide some related configs for you to explore.


***NOTE: Some of the customizations included below are Mason specific, but others can be found in*[AOSP](https://source.android.com/) *if you want to build an OS from the ground up.*


**Top Android OS Customizations**


- [disable_keyguard](https://bymason.com/blog/11-most-popular-android-os-customizations/#disable_keyguard)
- [disable_status_bar](https://bymason.com/blog/11-most-popular-android-os-customizations/#disable_status_bar)
- [direct_boot_package](https://bymason.com/blog/11-most-popular-android-os-customizations/#direct_boot_package)
- [config_allow_download_over_metered (app updater)](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_allow_download_over_metered)
- [config_allow_download_over_metered (fota)](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_allow_download_over_metered_fota)
- [config_default_tile_list](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_default_tile_list)
- [disable_screen_capture](https://bymason.com/blog/11-most-popular-android-os-customizations/#disable_screen_capture)
- [disable_camera](https://bymason.com/blog/11-most-popular-android-os-customizations/#disable_camera)
- [config_cameradoubletappowergestureenabled](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_cameradoubletappowergestureenabled)
- [config_update_prompt_prior_to_install](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_update_prompt_prior_to_install)
- [config_sms_capable](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_sms_capable)
- [config_lowbatterywarninglevel](https://bymason.com/blog/11-most-popular-android-os-customizations/#config_lowbatterywarninglevel)


### [disable_keyguard](https://docs.bymason.com/c/25/D450B/index.html#disable_keyguard)


**What disable_keyguard does** This feature disables the lock screen on the device, along with any password or security protocols meant to stop end-users from accessing the device. When the keyguard is disabled, “locking” the device (e.g. short-press on the power button), will simply put the device to sleep. When the device is woken up again, it will skip showing the lock screen and instead display whatever was in the foreground when it went to sleep.


**Why it’s used** For dedicated device use cases, disabling the lock screen is primarily about user experience. A “key guard” is a protective screen implemented in software that guards against accidental touches on a device’s touch screen — mostly an issue for devices used in pockets, handbags, or other such scenarios. Lock screens will often also have different backgrounds, which can sometimes be tricky for certain classes of users and can also create paths other applications you might not want them accessing (e.g. the dialer or camera from the lock screen).


**Related Android customizations**


- [config_shortpressonpowerbehavior](https://docs.bymason.com/c/25/D450B/index.html#config_shortpressonpowerbehavior)
- [config_longpressonpowerbehavior](https://docs.bymason.com/c/25/D450B/index.html#config_longpressonpowerbehavior)
- [config_keyguardshowphoneaffordance](https://docs.bymason.com/c/25/D450B/index.html#config_keyguardshowphoneaffordance)
- [config_keyguardshowcameraaffordance](https://docs.bymason.com/c/25/D450B/index.html#config_keyguardshowcameraaffordance)


### [disable_status_bar](https://docs.bymason.com/c/25/D450B/index.html#disable_status_bar)


**What disable_status_bar does** This feature disables the status bar that can be accessed when you swipe down from the top of the screen. The status bar allows you to access quick tiles (things like toggling wifi on/off, turning the flashlight on/off, etc.), view notifications, or get to the settings menu.


**Why it’s used** Disabling the status bar has a number of advantages. The primary benefit is that it minimizes the number of ways end-users could access parts of the OS you may not want them to (e.g. the settings menu or other background apps), and it also minimizes the chances that a user may accidentally toggle a setting (like turning wifi off or putting the device in airplane mode). Additionally, this helps create a very locked down experience, with the look and feel of a truly dedicated device. However, if you are using direct_boot_package (see below), and you do ***not*** have a way to access the Android settings menu directly from your application, you may want to consider leaving the status bar enabled as a way to set things like wifi, bluetooth, cellular, etc. (you also can set these from your app – but it’s a little more involved).


**Related Android customizations**


- [quick_settings_tiles_default](https://docs.bymason.com/c/25/D450B/index.html#quick_settings_tiles_default)
- [config_default_tile_list](https://docs.bymason.com/c/25/D450B/index.html#config_default_tile_list)


### [direct_boot_package](https://docs.bymason.com/c/25/D450B/index.html#direct_boot_package)


**What direct_boot_package does** direct_boot_package puts an app into a “kiosk” mode and defines an app that you want to effectively act as your launcher. The package you define will launch when the device boots up and the user will not be able to exit that app, although they can open other apps on top of it. Essentially it is the default app that shows when a device is turned on or when a user tries to access the home screen.


**Why it’s used** This customization is the foundation to building a dedicated, single-use Android device. It replaces the standard Android launcher, which is what makes a normal device look and feel like a consumer OS. With your app defined in the direct_boot_package, it limits the number of ways end-users can access other apps – creating a dedicated experience. Your own application can also be configured to feel like a launcher, with icons launching a set of applications you control.


**Related Android customizations**


- [autogrant_runtime_permissions_packages](https://docs.bymason.com/c/25/D450B/index.html#autogrant_runtime_permissions_packages)


### [config_allow_download_over_metered (app updater)](https://docs.bymason.com/c/25/D450B/index.html#config_allow_download_over_metered)


**What config_allow_download_over_metered does** This configuration defines whether or not you want *app* updates to be allowed to be downloaded over a cellular connection. If set to false, app updates can only be downloaded over wifi.


**Why it’s used** This is primarily used to control costs. Depending on the size of your app, an update could be large (i.e. expensive). That being said, for app updates, we typically recommend leaving this enabled so that you always have the ability to push an update if needed. If you have a critical bug that needs to be fixed, you may not want to wait until the user has a wifi connection.


**Related Android customizations**


- [config_show_download_progress_as_notification](https://docs.bymason.com/c/25/D450B/index.html#config_show_download_progress_as_notification)
- [config_update_install_time](https://docs.bymason.com/c/25/D450B/index.html#config_update_install_time)
- [config_update_check_when_idle](https://docs.bymason.com/c/25/D450B/index.html#config_update_check_when_idle)


### [config_allow_download_over_metered (fota)](https://docs.bymason.com/c/25/D450B/index.html#config_allow_download_over_metered-2)


**What config_allow_download_over_metered (fota) does** This configuration defines whether or not you want *OS* updates to be allowed to be downloaded over a cellular connection. If set to false, OS updates can only be downloaded over wifi.


**Why it’s used** Again, this is primarily used to control costs. Updating the OS is less common than app updates, but can also be expensive as it’s typically a larger package. Whether you allow this is up to you and your use case.


**Related Android customizations**


- [config_update_install_time-2](https://docs.bymason.com/c/25/D450B/index.html#config_update_install_time-2)
- [config_install_prompt_package](https://docs.bymason.com/c/25/D450B/index.html#config_install_prompt_package)


### [config_default_tile_list](https://docs.bymason.com/c/25/D450B/index.html#config_default_tile_list)


**What config_default_tile_list does** Configuring the default tile list allows you to define what options are available in the native Android Settings app. For example, instead of giving the user access to all the settings, you could only allow them to access critical options like connectivity.


**Why it’s used** Giving end-users access to the full suite of Settings options is usually not a good idea – especially in use cases calling for dedicated devices. From the Settings menu, users can intentionally or unintentionally create issues like stopping applications, disabling connectivity, deleting data, etc. Although it’s possible to build much of the functionality you need into your app (e.g. setting the wifi network, turning Bluetooth on, etc.), customizing the Settings menu can be a quicker and easier alternative to let the user control what they need without getting into too much trouble. If you do decide to build settings functionality into your app, make sure to explore the[Mason Framework](https://docs.bymason.com/mason-framework/) , which gives your application additional privileges to configure system settings that you wouldn’t get access to with the Android SDK (like changing the system keyboard, programmatically enabling/disabling the status bar, etc.).


### [disable_screen_capture](https://docs.bymason.com/c/25/D450B/index.html#disable_screen_capture)


**What disable_screen_capture does** You could probably guess by the name, but this makes it so that end users cannot take screenshots on the device.


**Why it’s used** This is typically used for two reasons. First, for security and data privacy, you may not want users being able to take screenshots which could then be shared or sent to someone else. Second, screenshots can easily be taken accidentally by pressing two buttons simultaneously (usually power and volume down), so from a user experience perspective, you might not want people accidentally taking them and then wondering what they did, or what they should do next.


**Related Android customizations**


- [disable_camera](https://docs.bymason.com/c/25/D450B/index.html#disable_camera)
- [config_cameradoubletappowergestureenabled](https://docs.bymason.com/c/25/D450B/index.html#config_cameradoubletappowergestureenabled)


### [disable_camera](https://docs.bymason.com/c/25/D450B/index.html#disable_camera)


**What disable_camera does** This customization makes it so no application (even the native camera app), can access the device’s camera.


**Why it’s used** Similar to the screen capture, disabling the camera is done usually for privacy and/or user experience. There are a number of ways the camera can accidentally be launched, and in privacy-conscious industries like healthcare, disabling it at the OS level can be an added security/privacy layer.


### [config_cameraDoubleTapPowerGestureEnabled](https://docs.bymason.com/c/25/D450B/index.html#config_cameradoubletappowergestureenabled)


**What config_cameraDoubleTapPowerGestureEnabled does** This config disables the functionality of accessing the camera app by double-tapping the power button.


**Why it’s used** This is primarily disabled for user experience, and to create a more locked-down environment. This can be overlooked but is also something that the end-user can access unintentionally.


**What config_update_prompt_prior_to_install does** This lets you define if the user is prompted with a dialogue box prior to an OTA update.


**Why it’s used** We typically recommend setting this to false, because the prompt can allow users to cancel the OTA. If canceled, then you will have a portion of your fleet that did not receive the update, leading to inconsistencies, potential security vulnerabilities, or out of date products. However, in some use cases, it may be preferable to have the user delay updates if the device is in use.


### [config_update_prompt_prior_to_install](https://docs.bymason.com/c/25/D450B/index.html#config_update_prompt_prior_to_install)


### [config_sms_capable](https://docs.bymason.com/c/25/D450B/index.html#config_sms_capable)


**What config_sms_capable does** This lets you disable the ability for the device to send *or* receive SMS messages (even with an SMS capable SIM card inserted in the device).


**Why it’s used** For most dedicated device use-cases, the device is *rarely* being used as a traditional smartphone (e.g. no need for calling or text messages). However, they *do* often need cellular connectivity, which means having a SIM card. While cellular data is a good thing, allowing SMS can open up the device to spam messages, which can create a poor user experience, get the user out of your main application, or[create serious security vulnerabilities](https://www.engadget.com/2019/09/06/sms-phishing-attack-android/) .


**Related Android customizations**


- [config_voice_capable](https://docs.bymason.com/c/25/D450B/index.html#config_voice_capable)


### [config_lowBatteryWarningLevel](https://docs.bymason.com/c/25/D450B/index.html#config_lowbatterywarninglevel)


**What config_lowBatteryWarningLevel does** This lets you define when the user will be prompted with a low battery warning. Setting it to 0 will never notify the user of a low battery.


**Why it’s used** This config is often used to either ensure the user isn’t warned of a low battery (to not interrupt the user experience), or to set the warning at a level that’s safe enough to give the user ample time to plug the device in (this is most common for use cases where the device dying could have drastic consequences for the end-user – often related to medical devices).


**Related Android customizations**


- [config_lowbatteryclosewarningbump](https://docs.bymason.com/c/25/D450B/index.html#config_lowbatteryclosewarningbump)


## **Ready to Build Your Own Android OS?**


While it’s pretty indisputable that Android is the go-to OS for building a smart hardware product, there are a few routes you could take to get there. If you like diving into the weeds, exploring AOSP and building your own ROM is a fun (albeit tedious) way to go.


However, if you’re interested in something a bit quicker and easier, feel free to[sign up for a free account and start exploring how you can use Mason for your hardware and OS infrastructure](https://platform.bymason.com/controller/signup) .
