---
schema_version: "1.0.0"
document_id: "95a1c7b0d5b45b880969e322291849ed8aa76a0bbfbdb864cd61debffbe88b9d"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/the-superwall-newsletter-volume-2"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:460fac4ba2a74568deaa43d6c354df3e7f61294942754398738bf7f6ac45a598"
---

# The Superwall Newsletter: Volume 2

## Welcome to the Superwall Newsletter, Volume 2!


In this issue, we cover:


- 🧪 Experimentation best practices.
- 💰 How to show different paywalls based on placements.
- 🏆 And, this week's paywall is from an Apple Design Award winner!


Let's get started!


## 3 Tips to Grow


### 1. Track every experiment, even small ones


No matter if you're a startup, a smaller scale indie or a large operation — tracking experiments is critical.


The rub? Tracking experiments can be a cumbersome process. It's easy to get lost in the weeds with different tools, burn hours setting up the ideal dashboard, or spend too much effort concocting the perfect solution.


Our advice at Superwall? Just do something, anything, to keep track of results. Our developer advocate, Jordan, simply uses Numbers on macOS for his own apps.


Numbers on macOS to track experiments


Of course, we have several ways to help with this using Superwall. But, even if you aren't using us — the best advice we could leave you with is to use "whatever works for you, right now" to track results.


So today - create an experiment tracker! Set up a kanban board, Google sheet, anything. As long as you're capturing a backlog of experiments, prioritizing them, and tracking results, it doesn't matter what tool you use. What matters is that you know what actions to try next as a result of those experiments.


### 2. The traffic paradox


Speaking of experiments, one major misconception about running them is the amount of traffic you need to get good, actionable data.


That number doesn't have to be 100%. Risk should be the primary decision maker.


Specifically, risk in terms of the experiment you're running — because not all of them carry the same amount of it. Testing whether app icon A leads to more downloads than app icon B is going to be less risky than experimenting with a major change in your entire onboarding flow.


The greater the risk of conversion loss, revenue loss, or impact on the user experience, the more conservative you'll want to go.


This is where it can get tricky, though. You need data to make decisions, but the more risky things are — the lower the number you likely will tolerate to enroll in the experiment. There's a line you need to find here, and it's a mix of using higher numbers to get faster results weighed against the potential loss associated with the changes.


### 3. Experiment rules to live by


Since we're all about experiments this issue, here are three rules our growth advocate Nick encourages orgs to consider when it comes to experimentation:


1. **Organizations that learn the fastest win:** Your product needs to be valuable enough for users to pay for it, and keep paying. Experiments validate how valuable it is, how to communicate that value, and when to capture that value.
2. **Tests fail, frequently:** A failed hypothesis doesn't equal a failed experiment. Finding what doesn't work (and why it doesn't work) still unlocks learnings about your users and product.
3. **Incremental wins, win:** The majority of tests won't lead to massive leaps in APRU or LTV. Incremental wins are like compounding interest. 3% growth per month leads to a 42% increase over a year.


## 2 Tech Tips


### 1. Efficient image decoding in iOS Apps


Do you know how tricky images are to get correct in iOS apps? Or, any idea why a 100 kilobyte image that's 1000 x 1000 pixels eats up much more memory than a 5 megabyte image that's 200 x 200 pixels?


It's fine if you don't know (our developer advocate Jordan has an in-depth post on it[here](https://www.swiftjectivec.com/optimizing-images/) ) all about the image decoding pipeline. Just know that UIKit has a handy function off of UIImage that makes image decoding a breeze:


```text
let   size   =   CGSize  (  width  :   200  ,   height  :   400  )
var   thumbnailImg   =   UIImage  (  named  :   "  Baylor  "  )  !
thumbnailImg   =   thumbnailImg.  preparingThumbnail  (  of  : size  )
```


When should you do this? The documentation has the answer:


*...when the native image size is much larger than the bounds of the view, decoding the full size image creates unnecessary memory overhead.*


### 2. Cleaning up Tailwind CSS


At Superwall, we primarily use Tailwind CSS for front-end work. And, if you do too, it's easy to end up with code like this:


```text
<  div   className  =  "   mx-auto  bg-sky-500 my-12 bg-sky-500   "  >
{foo}
</  div  >
```


Whitespace and duplicate class names are tiny little paper cuts that can take over a codebase. Now, with prettier-plugin-tailwindcss, you can automatically fix those right up. In our example above, it would clean it up like this:


```text
<  div   className  =  "   mx-auto bg-sky-500 my-12  "  >
{foo}
</  div  >
```


Check out their[blog post](https://tailwindcss.com/blog/2024-05-30-prettier-plugin-collapse-whitespace#cleaning-up-unnecessary-whitespace) to learn more.


## 1 Super Superwall Tip


**Show a certain paywall based off a particular placement:**


[Placements](https://superwall.com/docs/campaigns-placements) are the fundamental building blocks of a Superwall campaign (if you're new to campaigns, check out our[revamped docs](https://superwall.com/docs/campaigns) explaining everything you need to know). A common use-case that many Superwall customers have is wanting to display a custom paywall based on whether or not a certain placement was evaluated.


Consider this soccer app. There's a custom paywall which only shows when someone taps on the "Try Free" button (all other placements show a different paywall).


**This is a great way to tailor your message for certain pro features that are paywalled** . Did they just tap on a button to use "Feature A" and they aren't pro? Show a particular paywall showcasing why they *have* to have "Feature A" since they've already shown intent in terms of wanting to use it.


Here's how you can do it:


1. Create a new audience within a campaign (or, you can also create a new campaign just for this purpose).
2. Create a filter that evaluates if` params.event_name` is equal to the text of your chosen placement.
3. Attach a paywall to the audience, and be sure to drag it above any other audiences you want it evaluated before first.


And you're done! Here's a picture of how the setup would look like (in this case, the "Try Free" button fires the` settingsJoinAction` placement).


Audience filter to show a paywall based on a placement


---


## …One More Thing


### From the Blog:


[Learn some new user segmentation recipes to jump start growth](https://superwall.com/blog/user-segmentation-recipes-to-jump-start-app-growth)


### Pretty Design Tweet


Widgets are a design playground, and[we found these](https://x.com/sneqqy/status/1765351567459332533) from[Yevhen](https://x.com/sneqqy) inspiring.


### One Nice Paywall


Congratulations to all of the recently announced Apple Design Award winners! Here's one of our favorites,[Crouton](https://apps.apple.com/us/app/crouton-recipe-manager/id1461650987) , using the paging feature design on their paywall.


Thanks for reading, and we'll see you next time!
