---
schema_version: "1.0.0"
document_id: "a866b2d669da90a4b5de0853b8d8525cefceb58455a0bbd2b2afb2f31865cd0b"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/presenting-paywalls-from-survey-responses"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:3eadd0b72baa7c06c2a986388c50050a8a9d3b02831227521706c837384dc0e3"
---

# Presenting Paywalls from Survey Responses

People may initially skip converting on your paywall for all sorts of reasons. Some of the more common ones we see at Superwall include:


1. Sometimes, the price point is either too high, or they want to reinforce that the price is worth it by using the app first.
2. They simply want to use the app a little, and make sure it meets their expectations.
3. Or, maybe they aren't initially willing to pay, but they may change their mind later.


Of course, that list is far from exhaustive, but if you can start to pin down *why* a user didn't convert, you can start to tailor your paywalls more effectively. For example, if you know that the user found the price too high, then you can turn around and offer a discount of some kind.


We built[surveys](https://superwall.com/docs/dashboard/surveys) into paywalls for this exact reason. When you associate a survey to a paywall, you can ask a few questions when the user closes the paywall (i.e. they don't convert) or when they did convert.


Your browser does not support the video tag. A survey presenting when a paywall is dismissed.


This opens up several interesting flows, such as being able to show a paywall based on a response. That's our focus here. I'll show you a quick example of how you could offer a discount to someone who indicated the price was too high on a survey response.


## Take out the guesswork using surveys


First, you'll need to attach a survey to a paywall: **Open the paywall editor → Click "Surveys" in the left sidebar → Under "Close Survey" → Select a survey** .


Attaching a survey when the paywall is closed.


In the image above, I attached a survey I made ahead of time to show once the user dismisses a paywall. If you want, you can create and customize as many surveys as you need. Simply visit your Superwall dashboard, and choose "Surveys" from the left sidebar.


Manage your surveys in the Superwall dashboard.


With your surveys set and associated to a paywall, we'll make sure they show up next using a[campaign](https://superwall.com/docs/dashboard/dashboard-campaigns/campaigns) .


## Presenting paywalls based off of survey responses


Superwall has a special placement for when a survey response occurs, it's called` survey_response` . It works like any other[placement](https://superwall.com/docs/dashboard/dashboard-campaigns/campaigns-placements) , which means creating a campaign just for survey responses is trivial. Simply add the` survey_response` placement, pick a paywall to show, and you're done! I should note that you can add this placement to an existing campaign, but personally, I like it separated out into its own campaign.


The survey_response placement triggers when a survey completes.


In this campaign, I've added a paywall that shows up with a discounted offer when a survey response is triggered. Now, that's fairly broad, so we could add an audience filter so that this paywall *only* shows up when the response to the survey was "Too expensive".


The paywall attached to this audience only shows when the response was "Too expensive."


## Seeing it in action


Now, anytime a user sees a paywall and declines it, our survey appears. Then, if the response was "Too expensive", we'll show a paywall that offers a discount.


Your browser does not support the video tag. A full, working example.


At Superwall, we've seen this exact flow boost conversions significantly. And remember, you can tailor a specific paywall to *any* survey response. Or, simply show the same one for any. It's all up to you, and Superwall gives you the flexibility to create the exact setup and targeting you want based off of surveys.


## Wrapping up


At Superwall, we've always said that showing the right paywall, to the right people, and at the right time is critical to converting. That's what the Superwall platform is built to do. Setting up surveys is quick, easy, and — best of all — requires no app updates if you've already integrated our SDK. Give it a try today, and even if you don't show a paywall from a survey response, you can start to learn why people are dropping off from viewing the survey results.


Happy converting, and get started with a free[Superwall account](https://superwall.com/sign-up) today if you haven't already!
