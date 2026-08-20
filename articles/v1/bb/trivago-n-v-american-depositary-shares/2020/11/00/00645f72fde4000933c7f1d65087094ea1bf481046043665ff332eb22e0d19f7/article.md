---
schema_version: "1.0.0"
document_id: "00645f72fde4000933c7f1d65087094ea1bf481046043665ff332eb22e0d19f7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-11-17-exploringthepagevisibilityapifordetectin/"
published_at: "2020-11-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:bd8917328be871511581c60eafb9f0df5be86dc8fff4ee7fdb45d980924c4581"
---

# Exploring the Page Visibility API for Detecting Page Background State

At trivago we are working heavily on the web platform and, based on the scale that we need to serve our users, our applications need to cater for many different kinds of environments and conditions.


As one of our use cases demanded we look into how a background (and possibly terminating) state is defined in the modern web, we investigated the options and discovered a few interesting points, specifically about the[Page Visibility API](https://www.w3.org/TR/page-visibility-2/) . We would like to share our experience working on this topic, a few suggestions and hopefully inspire and give you new perspectives on how you can tackle your own similar use cases.


## Scenario from the Good Old Times


Engineers working on the web for quite some time have likely come across the requirement of triggering an event log request or popping an alert prompt to the user before they leave the page. As though reflexively, the first implementation that would come to mind is the beforeunload or unload browser events that have served web developers in these kinds of situations in the past.


Each of those events respectively are fired just before and at the time the document is about to unload its resources. For example, when you navigate from Page A to Page B or when you try to close a visible tab on your session. For most desktop browsers, these scenarios are covered reliably enough, with only a few caveats to be aware of.


However, this appears not to be the case for the mobile platform and the fact could be attributed to a couple of good reasons that render these events mostly unreliable.*


*Apart from the reliability aspect of the unload handlers on modern platforms, there is also the fact that these handlers do not allow browsers to use an important internal implementation, commonly known as back/forward cache, degrading page navigation performance.


# The Difference in the Environment


As the world started moving towards a more modern and mobile-first web, the intricacies and new capabilities of the mobile platform have introduced a new context we need to account for when designing and evaluating web experiences.


## Modern Constraints


As mobile devices are much more constrained in resources than desktop devices, mobile operating systems will go to great lengths in order to save and reclaim resources. When required, they will simply shut down an application without further ceremonies. This could well be the browser displaying your page, and as you can probably imagine , the unload handlers you have registered will not be triggered in such a case.


## Modern States


More importantly, what we can think of as regular mobile interactions result in many new states and transitions that a page might record in its lifecycle. Examples include:


- The user locks the screen so the page is no longer visible.
- The user presses the Home button to be presented with the homescreen.
- The user switches between apps using the App Switcher.


In the above interactions, the page could be deemed as shifting in a background state and has the probability to be terminated without any possible way for us to get notified about it – either from the user swiping the app away or the OS re-allocating resources. In none of these above state changes will the unload handlers be fired.


## Modern Requirements


The bloom in frequency that people are engaged in what we can call the “mobile web” as well as the new possible page states have brought forth the mental shift from web pages to something close to web applications. This shift has surfaced a whole new wave of requirements that we as web engineers need to implement in this new paradigm.


Some of these requirements and best practises clearly connect with detecting a background state.


- A video player should not continue playing when we lock the phone screen.
- Heavy animations, as they tend to be resource intensive, could preemptively stop.
- The valuable offers that we might include in an auto-rotating carousel should not keep shifting if we want to treat all our deals fairly.
- Polling activity to our server should be significantly reduced or even paused completely. (Actually browsers have already implemented a minimum timeout and a max-time budget strategy for us)


## Modern Solutions demand Page Visibility


Since we went through all the constraints, new states and requirements that are closely related to when a page can shift in and out of a background state, we are going to now reveal that there is (and has been for quite a long time) a browser API that is used to capturing shifts between these states.


This is the Page Visibility API that, as officially stated, defines a means to programmatically determine the visibility of a document.


## Page Visibility 101


The addition of this new API gives developers access to a new Document event which is called “visibilitychange”, and a Document attribute called visibilityState.


By registering a listener for this specific event, you are guaranteed to be notified when the browser detects a change in the visibility of the page. This change then can be safely detected by querying the document.visibilityState attribute inside the event handler.


The visible state signifies that a document is in a usable state and partially observable in the viewport.


The hidden state is the one that interests us the most in the context we have been discussing up until now. The hidden state is active when a document can be considered not visible on a screen. That seems like a simple term, but listing some cases that are covered by the hidden state can give us web engineers a lot of inspiration for our modern requirements and implementations.


Examples where we can consider the document as hidden:


- When from the foreground tab in the browser, the document goes to a background tab.
- When the browser is minimized.
- When the user refreshes the page or navigates to another one.
- When the lock-screen of the operating system is invoked.
- When another app is selected from the App Switcher other than the browser with the page previously on the foreground.


Covering all these scenarios, the “visibilitychange” event is a great fit for our own use case.


## Solving our use case


Just as a reminder, our implementation requirement was to run a cleanup process whenever the page could be considered shifting to the background or a user initiating a navigation.


For the specific project we are referring to, we are using React (actually Preact) as our view-layer library, so it made sense for us to create a custom hook that could be re-used for similar requirements.


```text
export   const   useOnPageHidden  :   (  sideEffectCb  )  :  > {
useEffect  (()  :  > {
const onHideHandler: (event):  >   {
if   (document.visibilityState  :  ==   'hidden'  ) {
sideEffectCb  ();
}
};


document.  addEventListener  (
'visibilitychange'  ,
onHideHandler,
);


return   ():  >   {
document.removeEventListener(
'visibilitychange'  ,
onHideHandler,
);
};
}, [sideEffectCb]);
};
```


If you are not React-savvy, you can just ignore the useEffect wrapping, consider the returned function as cleanup for when the caller of the function is removed from the DOM and focus on the onHideHandler.


### An unexpected issue


Initially this seemed like an amazingly simple and solid implementation, but for the Safari browser, it does not cover the basic case of page navigations, which was important for the use-case. You can see the issue referenced in the WebKit issue tracker:[https://bugs.webkit.org/show_bug.cgi?id=151234](https://bugs.webkit.org/show_bug.cgi?id=151234)


(The funny thing is that it was considered fixed as we were developing our solution).


To cover this portion of interactions, we decided to combine the visibilitychange event with another one called pagehide . We will not go into details about this Window event, but only mention that it is fired when the “current page” entry changes in a session, making up for our case in Safari.


Attentive readers can already notice that there is an overlap between these two events. In browsers where Page Visibility works as expected, we would have both visibilitychange and pagehide running for scenarios like page navigation, and we would prefer to avoid that.


The possible solutions here would be to do some Safari detection and adjust events accordingly. That would be the more resource-friendly way. However, when going through the specification repositories and browser issue trackers, we quickly see that there are reported cases where, in other browsers like Chrome, the visibilitychange is still not firing in all the cases that pagehide covers.


Taking the above into consideration, we decided to move our thinking away from the internals of browsers and examine the actual nature of the events we can use.


### Taking a step back


The pagehide event is fired before the current page is no longer the current one for the browser session. That essentially means that it runs before the page is hidden or terminated, meaning there won’t be any chance for the user to keep interacting with the same instance of the page.


Taking into account that the visibilitychange will additionally be triggered when the page could possibly be reused by the user (switching tabs etc.), we can see that the only intersection between the two events is when the page is either going to be reloaded, navigated away from, or ultimately terminated. In those cases, we want our side effect to run only once.


To achieve this goal, we can use a flag between the two events that will let our code know if the page is moving towards a terminating state and prevent either of the listeners from runing the side-effect process a second time.


However, in that logic, there is a little caveat based on the facts we stated about the two events. Since the visibilitychange event can possibly run many times before a termination, we need to understand the order in which visibilitychange and pagehide are fired, and also if the order is consistent among browsers. The place to look for these facts initially is the HTML specification.


### Digging into the Specs


When we went through the HTML spec about a month ago, it stated that the visibility state change events would be run before the pagehide event. Testing this statement in the field, however, we found that it was actually the opposite. The visibilitychange event handlers were run after the pagehide ones, conflicting with what was stated.


Luckily for our case, the specific topic was actively discussed in the Web Performance Working Group, which is responsible for the Page Visibility API, and as it turns out, user agents had in fact implemented the opposite of what the spec has stated. This, in turn, resulted in a recent change in the HTML specification that now correctly states the order of events.


You can find the specific pull request here[https://github.com/whatwg/html/pull/5949](https://github.com/whatwg/html/pull/5949)


### Final Proposal


Having all this information in hand, we now are sure that the pagehide event will signal page termination and is run before the visibilitystatechange for the same scenarios. Flagging the event as terminating after the pagehide event has been executed, is all we need.


Finally we can come up with an implementation that would address the needs of our use case across the majority of our audience.


```text
enum   DismissalEvents   {
VisibilityChange  : 'visibilitychange',
PageHide  : 'pagehide',
}


const   useOnPageHidden  :   (  sideEffectCb  :   ()  :  >   void  )  :  > {
useEffect  (()  :  > {
let terminatingEventSent:   false  ;


const onHideHandler: ({ type }: Event):  >   {
if   (  terminatingEventSent  ) return;
if   (  type  :  ==   'pagehide'  ) {
terminatingEventSent  :   true  ;
sideEffectCb  ();
}


if   (  type  :  ==   'visibilitychange'   &&
document.visibilityState  :  ==   'hidden'
) {
sideEffectCb  ();
}
};


document.  addEventListener  (  'visibilitychange'  , onHideHandler);
window.  addEventListener  (  'pagehide'  , onHideHandler);


return   ():  >   {
document.removeEventListener(  'visibilitychange'  , onHideHandler);
window.removeEventListener(  'pagehide'  , onHideHandler);
};
}, [sideEffectCb]);
};
```


As a trivia, there is an additional case that needs to be addressed that has to do with the scenario of the user switching a tab and then shutting down our page from a background state – let’s see how you can creatively solve this with the information you have!


## A word of caution: Page Visibility is “alive”


If we were developing in a perfect web, where all the implementations were behaving the same across devices, browsers and operating systems, and where there was only one version in effect at a time, we would be able to guarantee 100% reliability for these states.


In reality though, we are in an imperfect web ecosystem that does not provide these guarantees, and this probably won’t change in the foreseeable future. The web is alive and so are its workings. Proposals are written, new specifications are published and current ones are continuously getting refined and enriched. W3C Working Groups and other technical steering committees are working together with browser vendors to do their best to cover all the use cases that move us towards a better web both for users and developers.


Conformance work, browser bug fixes and specification fine tuning are done at a really fast pace. If you want to follow the latest discussion and changes, you can check the open GitHub repositories of these specifications or if you’re really interested, the issue tracking lists of different browsers.


Happy coding!
