---
schema_version: "1.0.0"
document_id: "26a1abec30699b148b846d3c92eff1f4ea1ca1e2e95db81a2cf46f1143e38438"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/redesigning-android-ui-used-millions"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:8968fb8067b6ed0df38909b7402090c3e1940a98c0a56a2fa2835f53a322504c"
---

# Redesigning an Android UI used by millions

As the first Android developer of Photoroom, I knew I would have a lot of work to catch up with all the iOS app features as fast as possible. We did a lot in few months, and the first feedback and reviews on the Play Store were excellent! After six months, it was the perfect time to rethink the central part of the app: **the editing view** .


## Create a better editing user experience 🧑‍🎨


Our main goal at Photoroom is to let our users create images for selling or having fun in the easiest possible way. Considering this, it’s safe to say that the user experience is our top priority. We decided to revamp the editing UI to give the user quick access to the main object features, like lighten a dress, changing the background color depending on the product colors, or quickly finding the perfect font for your title.


The thing is that when you have an app already downloaded and used by millions of users, you don’t want to mess up 🙃


## Design & talks 🎨


The first step was to define how we could improve the editing process. To do that, we tested many different apps, asked for users’ feedback, selected the UI patterns that made sense for us, and created a Figma design with all our ideas. After some back and forth between the design & tech teams, we finally had our first design version ready!


With the help of Figma, our designer created a prototype to give us an idea of how the UI should work and how all components will interact together. Everything was ready to start prototyping the new UI.


## MotionLayout to the rescue 🪄


Time to get your hands dirty! 🛠 What do we want here? A layout with a rendering view on the top (more precisely as a list view header), followed by a list of objects and actions. Ho, and yes, it should be animated too!


This is a perfect use case for a` MotionLayout` with two states:


1.


Extended: the rendering view will take around 50% of the screen height to allow the user to focus on his work.


2.


Collapsed: the rendering view will take around 30% of the screen height to let more space for the available editing actions.


How does this work? We want to start transitioning from the extended to the collapsed mode when the user scrolls. It becomes a bit more complicated when we add that the user should be able to reset the UI to the extended state when he wants (for example, by clicking on the rendering view or selecting an action).


Well, thanks to the` MotionLayout` , half of the work will be done quickly:


-


We create a` MotionLayout` scene with two` ConstraintSet`


```text
<ConstraintSet android:id="@+id/scene_extended>
<Constraint android:id="@+id/constraint_1" ... />
<Constraint android:id="@+id/constraint_2" ... />
<!-- Other constraints -->
</ConstraintSet>
<ConstraintSet android:id="@+id/scene_collapsed>
<Constraint android:id="@+id/constraint_1" ... />
<Constraint android:id="@+id/constraint_2" ... />
<!-- Other constraints -->
</ConstraintSet>
```


-


Then, we add a` Transition` to animate the components between our two` ConstraintSet`


```text
<Transition
android:id="@+id/transition_from_extended_to_collapsed"
app:constraintSetEnd="@id/scene_collapsed"
app:constraintSetStart="@id/scene_extended"
app:duration="200"
app:layoutDuringTransition="honorRequest">


<OnSwipe
app:dragDirection="dragUp"
app:touchAnchorId="@id/recycler_view"
app:touchAnchorSide="top"
app:onTouchUp="stop" />


</Transition>
```


Here’s the result:


One more thing to add: we want to let the user move from collapsed to extended states even if the` RecyclerView` isn’t scrolled at the top. That’s where things get a bit more complicated. To do that, we have to say goodbye to the transition element` OnSwipe` and start managing the transition progress ourselves. After some tests, the easiest and cleanest way we found was to add an` OnDrawListener` to the` viewTreeObserver` of the` RecyclerView` and do the math.


The first step is to manually compute and update the transition` progress` value by catching the scroll direction and` scrollY` distance from the top (precisely what OnSwipe was doing before). Then, we add two things:


-


A function to reset the transition to the expanded mode


-


A variable to keep in memory the starting` scrollY` value


With that, we will be able to re-start the expanded-to-compact transition whatever the` scrollY` value of our` RecyclerView` .


Let’s take a look at the result:


After one month of work and a lot of iterations, we were ready for the next step: user testing!


## Time for tests 🧪


To have quick feedback and understand how users will behave with the new UI, we decided to launch few tests on[usertesting.com](https://www.usertesting.com/) . Even if the first batch of tests (3 users) was a bit disappointing (because they weren’t really involved), some key functionalities weren’t easy enough to find, like changing the background color.


Example of fast iteration changes we made after the first batch of users’ tests (from top to bottom):


-


Change the opacity and behavior of the back button (deselect the current object instead of closing the edit view)


-


Update the size of the buttons to fill the width, and increase background contrast


-


Add a quick color picker (which was accessible by the button` Replace` before)


In less than a day, we were able to make these changes and launch a new batch of tests. The feedback was way more positive! 🥳


---


Once we decided the new UI was ready to be deployed, we had to be sure it will perform at least as the previous UI. To monitor that, we used[Firebase Remote Config](https://firebase.google.com/docs/remote-config) to activate the new UI only for a small percent of our users and[Amplitude](https://amplitude.com/) to follow our users' behaviors:


The new UI performed around 5% better than the previous one


## Conclusion & what’s next? 👀


With the appropriate tools and fast iterations cycles, we were able to get great feedback quickly and put in production a new UI which was a few weeks ago only a Figma prototype.


The fast iterations process allows us to test and publish new features every week and take decisions depending on how our users receive them.


We’ll continue to learn from our users.
