---
schema_version: "1.0.0"
document_id: "f65fc3d1b500be8d8945dbdc67c7fa1ae738e6e26b4cd1ddc6be332dce6a5156"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/react-native-workaround-the-hard-way-to-run-animations-at-60fps-56ba7658fe05"
published_at: "2021-12-21T09:19:12+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:0e2582a7622601775865314d2327e2d3740d6a58b89e8b9e20ecc74f5fbf6c13"
---

# [React Native] Workaround/The hard way to run animations at 60FPS

React Native


React Native Animation


60fps


Performance


# \[React Native\] Workaround/The hard way to run animations at 60FPS


[Mouli Mohan N](https://medium.com/@moulimohann?source=post_page---byline--56ba7658fe05---------------------------------------)


3 min read


·


Dec 10, 2021


--


This post is about how I got the animation work smooth on React Native for a given use case. I’ll first walk you through the possible ways to animate on React Native, then the challenges I faced and why I chose a particular approach.


**Use Case:** I need to show a number roller animation on the Home screen on every App Launch.


I didn’t know how to do this animation on ReactNative, when I googled, I came across[this](https://www.npmjs.com/package/react-native-animate-number#user-content-formatting) library and here is how I tried the sample app.


**Approach 1**


When I ran the above code, below is the animation I saw.


The animation is working as desired in the sample app. It’s time to evaluate if the animation works the same in a real-world production app.


> **Challenges in Real-world Production app:**
>
>
> While the user is landing on the HomeScreen JS thread is too busy executing the app’s business logic, processing network requests, creating UI objects to render.
>
>
> If the Home screen is built with a Nested scroll view i.e multiple Horizontal Scrollviews inside a Vertical Scrollview with more than 2 vertical screen offset pages then imagine how jerky the animation would be like.


Here is what happened when I mocked the above challenges in the sample app by adding the below code.


So, keeping the JS thread busy made animation Jerky. This is not the experience I want to give to the users.


**Is there a way to improve the above code?**


I started exploring library code to see if there is a scope for improving the performance.


After reading the library’s code I knew it is not possible to improve animation performance for the following reason.


> The math behind the animation of every frame is calculated on JS Thread. The calculated values are passed through the props node attached to a View. Then, the JS thread serialises the View object and passes it over the Native bridge to the Native thread. While the JS thread is busy executing business logic/processing network requests, it won’t be able to calculate a new value within[16.67ms](https://www.youtube.com/watch?v=CaMTIgxCSqU) . Hence we see the lag/frame drop.


**Approach 2**


After doing a bit more research I came across[this](https://reactnative.dev/blog/2017/02/14/using-native-driver-for-animated) blog which talks about using Native Driver.


Using Native Driver won’t improve the performance of animation in **my UseCase** for the below reasons


> Native driver config works just, with in-build Animated library of react-native SDK. Also, You can only animate non-layout properties. Ex: transform and opacity will work but flex-box and position properties won’t.


**Approach 3**


**React Native Re-Animated** has Re-Designed and worked on new architecture whose primary goal is to get rid of the native bridge and directly communicate with Native Thread. With the help of Worklets, it executes part of JS code on a separate JS Virtual Machine which can run synchronously with Native UI thread without frame drops.


> **Cons —** It’s still in the experimental phase, doesn’t have the full support and you may also notice intermittent crashes.


So, approaches #1, #2, #3 discussed above are not the options for me to try.


**Approach 4**


I want to trigger animation with required props from JS thread and pass it on to Native thread and let Native thread, maintain the context and do the math behind the animation for every frame.


By going through[this](https://reactnative.dev/docs/native-modules-android) blog I understood how to create Native modules and export them to React Native. Now, let’s create and export one.


```text
packages.add(new NumberRollerPackage());
```


Exporting Native component to ReactNative


Using Native component in React Native


Let’s see JS and Native thread animation at the same time in action by keeping JS thread busy.


It doesn’t matter how busy JS thread is, our NumberRolling animation is going to be performant at any circumstance/device configuration.


**Note:** You can implement the same on IOS too.
