---
schema_version: "1.0.0"
document_id: "f3f8ce8e684b2da6c3d3a62b66d366c6a74d6e4f75667cbd26b31be8705ced4d"
company_key: "yc-fampay"
company: "FamPay"
source_id: "yc-fampay-rss-25fb68ce5f72"
canonical_url: "https://blog.famapp.in/blog/change-app-icon-dynamically-in-android/"
published_at: "2024-07-10T02:36:09+00:00"
first_seen_at: "2026-07-20T23:23:51.041833+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:78b2b112fb66e4818df01c4f384cc9dede70d21418dd588d440a7afdb6e34504"
---

# Change App Icon Dynamically in Android (without closing app)

You've probably seen apps like Twitter, Snapchat, Zomato etc., which allow users to choose app icon of their choice, or they change it in background on special occasions. It adds a personalised touch to the customer experience and acts as a delight feature for users. At Fam, we were also eager to build this for our users and here’s how we went about it.


I started searching on internet about the same and went through many articles & stackoverflow buzz but the provided implementation everywhere causes the app to close on each app icon change. While the apps of Twitter, Snapchat, etc., only close once when changing the icon for the first time.


I eventually discovered the technique to accomplish the desired functionality. In this article, we’ll first see the basic implementation which talks about the engineering behind it that will close the app every time. Then, we’ll see how to achieve the desired behaviour, which will only close the app once, the very first time.


# Overview


Unlike iOS, changing the app icon programmatically in Android is not very straightforward. Android doesn’t provide any official way of doing this. However, with the help of[activity-alias](https://developer.android.com/guide/topics/manifest/activity-alias-element?ref=blog.famapp.in) we can achieve this.


> *Activity’s alias is like having a different identity of target activity. By creating an alias for our desired activity, we can assign it a different icon and even a different app name.*


**TL;DR -** So basically, to change the icon at runtime, we’ll need to enable the alias representing an icon that we want to switch to and we’ll need to disable the currently enabled alias (i.e. SplashActivity in above example). Sounds easy-peasy, right?


Let’s have a look at Manifest file:


```text
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="<http://schemas.android.com/apk/res/android>">


<application
android:allowBackup="true"
android:icon="@mipmap/ic_launcher"
android:roundIcon="@mipmap/ic_launcher_round"
android:label="@string/app_name"
android:theme="@style/Theme.Example">


<!-- SplashActivity (Your Launcher activity) -->
<activity
android:name=".SplashActivity"
android:exported="true"
android:theme="@style/Theme.Example">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity>


<!-- Alias for SplashActivity with different icon -->
<activity-alias
android:name=".NewYearIconAlias"
android:targetActivity=".SplashActivity"
android:icon="@mipmap/ic_new_year"
android:exported="true"
android:enabled="false">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity-alias>


</application>


</manifest>


```


Here,` SplashActivity` is our launcher activity and we’ve declared an *activity-alias* for it with a different icon which is disabled **(**` enabled="false"` **)** by default. When the app is installed, the icon associated with the SplashActivity (i.e. application’s default icon) will be set automatically. And upon enabling the activity alias, the icon associated with it will be set.


Now let’s see the code to enable/disable the activity alias in runtime. We’ll use *PackageManager* class & it’s[setComponentEnabledSetting(...](https://developer.android.com/reference/android/content/pm/PackageManager?ref=blog.famapp.in#setComponentEnabledSetting(android.content.ComponentName,%20int,%20int)) ) method


```text
fun changeIcon() {
// Enabling NewYearIconAlias, that represents new year icon - for example
packageManager.setComponentEnabledSetting(
ComponentName(
this,
"$packageName.NewYearIconAlias"
),
PackageManager.COMPONENT_ENABLED_**STATE_ENABLED**,
PackageManager.DONT_KILL_APP
)


// Disabling the SplashActivity (we can also have alias to disable if we're playing with multiple aliases)
packageManager.setComponentEnabledSetting(
ComponentName(
this,
"$packageName.SplashActivity"
),
PackageManager.COMPONENT_ENABLED_**STATE_DISABLED**,
PackageManager.DONT_KILL_APP
)


// Note: DONT_KILL_APP flag doesn't work if we disable the
// currently enabled alias at runtime.
}


```


Here in` setComponentEnabledSetting(…)` method, we are passing the component name of activity-alias and splash activity. And we’re changing activity-alias state to enabled i.e.[COMPONENT_ENABLED_STATE_ENABLED](https://developer.android.com/reference/android/content/pm/PackageManager?ref=blog.famapp.in#COMPONENT_ENABLED_STATE_ENABLED) and splash activity state to disabled i.e.[COMPONENT_ENABLED_STATE_DISABLED](https://developer.android.com/reference/android/content/pm/PackageManager?ref=blog.famapp.in#COMPONENT_ENABLED_STATE_DISABLED) .


This will enable our activity-alias representing a different icon and disable the SplashActivity which represents default app icon in our case. Yes! That’s pretty much it, our app icon has been changed.


**BUT as mentioned before, this straightforward approach comes with a pain—it’ll close the app every time we change the icon :/** In contrast, apps like Twitter & Snapchat only close once, the very first time the icon gets changed.


# **Solution to prevent the app from closing every time**


Activity/alias component have multiple states like:


- *PackageManager. **COMPONENT_ENABLED_STATE_ENABLED** → This enables a component*
- *PackageManager. **COMPONENT_ENABLED_STATE_DISABLED** → This disables a component*
- *PackageManager. **COMPONENT_ENABLED_STATE_DEFAULT** → This takes component to it’s default declared state in Manifest (for ex: Enabled or Disabled)*


💡


Android system only kills the app when we change the currently enabled alias/component’s state to its ****disabled**** state in runtime. ****However, Android doesn’t kill the app if we change the state to either Enabled or Default.**** **\[In the manifest, we can set both Enabled (** *` *enabled="true"*`* **) and Disabled (** *` *enabled="false"*`* **) as the default state\]** Therefore, if we set the currently enabled alias to its default state, whose default declared state in the manifest is ****Disabled**** then Android system won’t terminate the app, and the component/alias will also be disabled.


By leveraging this behaviour of the Android system, we can simply enable the new alias and set the currently enabled alias to its Default state (which was declared as Disabled in the manifest). This won't result in the app being terminated. 😎


## Implementation


We’ll create 3 types of aliases:


- **DefaultIconAlias: I** t’ll represent the default icon of our app. It’ll be enabled (` enabled="true"` ) by default in manifest
- **CloneDefaultIconAlias:** This will ****act like a clone of **DefaultIconAlias** (you will see its uses later). It’ll be disabled (` enabled="false"` ) by default


*Both **DefaultIconAlias** & **CloneDefaultIconAlias** will represent the app’s default icon*


- **Rest of the other aliases:** These will represent different icons and these must be disabled by default, you can declare these as many as you want!


💡


So, only ****DefaultIconAlias**** will be enabled by default & the rest will be disabled. All the aliases should target your launcher activity (SplashActivity in our case)


Let’s have a glimpse of Manifest


```text
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="<http://schemas.android.com/apk/res/android>">


<application
android:allowBackup="true"
android:icon="@mipmap/ic_launcher"
android:roundIcon="@mipmap/ic_launcher_round"
android:label="@string/app_name"
android:theme="@style/Theme.Example">


<!-- Your Launcher Activity (SplashActivity in our example) -->
<activity
android:name=".SplashActivity"
android:screenOrientation="portrait"
android:theme="@style/OnboardingTheme"
android:exported="true">
<intent-filter>
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity>


<!-- This is enabled by default -->
<activity-alias
android:name=".app_icon_alias.AppIconsAlias.DefaultIconAlias"
android:enabled="true"
android:icon="@mipmap/ic_launcher"
android:targetActivity=".SplashActivity"
android:exported="true>
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity-alias>


<!-- This is disabled by default -->
<activity-alias
android:name=".app_icon_alias.AppIconsAlias.CloneDefaultIconAlias"
android:enabled="false"
android:icon="@mipmap/ic_launcher"
android:targetActivity=".SplashActivity"
android:exported="true">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity-alias>


<!-- This is disabled by default -->
<activity-alias
android:name=".app_icon_alias.AppIconsAlias.NewYearIconAlias"
android:enabled="false"
android:icon="@mipmap/app_icon_beach"
android:targetActivity=".SplashActivity"
android:exported="true">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity-alias>


<!-- This is disabled by default -->
<activity-alias
android:name=".app_icon_alias.AppIconsAlias.MembershipIconAlias"
android:enabled="false"
android:icon="@mipmap/app_icon_basket_ball"
android:targetActivity=".SplashActivity"
android:exported="true">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity-alias>


<!-- You can keep declaring as many alias as requried... -->


</application>


</manifest>


```


**Now to change the icon for the first time** we’ll disable the **DefaultIconAlias** (which will terminate the app) and enable a new alias that represents a different icon. This will result in an app icon change but as mentioned app will be closed for the first time since we’re changing the alias state from enabled to disabled in runtime.


**Now to change the icon next time** we’ll set the currently enabled alias to its default state (i.e. disabled (` enabled="false"` ) and as discussed earlier, this transition won’t terminate the app). And now we’ll simply enable the new alias. This will result in an app icon change without the app getting terminated. We’ll keep doing the same for all the consequent icon changes—your app won’t get closed! 🎉


**Now, to reset to the default icon** , we will set the currently enabled alias state to default (i.e. disabled) and we’ll enable **CloneDefaultIconAlias** which represents the default app icon too. We’ll always enable CloneDefaultIconAlias in such cases (not DefaultIconAlias) as it’s default state in manifest was disabled. Hence we can always set it to it’s default state in order to change the icon again without terminating the app. We can’t do this with DefaultIconAlias as it’s default state was set as enabled in manifest.


Yay! We’re done, this was it. Now you know the trick behind the apps of Zomato, Twitter, Snapchat and so on. Happy Coding!!


## Please note 🚨


- You need to have all the different icons that you want to switch between included in your app's resources beforehand. Each icon needs to be specified in the manifest file and associated with an activity alias. Therefore, this method isn’t practical if you want to change the app icon to something that wasn't included in the app at the time of installation.
- You can also change the icon in the background. This way, even on the first change, app will be killed when user puts it in the background and if he reopens it from the background, then it will be started over from the beginning. Depending on your use case, this might still look better (Zomato also does similarly). On the other hand, we show an app termination warning when user changes the icon for the first time and then we change it right away.


### Tags


- [Tech](https://blog.famapp.in/blog/tag/tech/)
