---
schema_version: "1.0.0"
document_id: "76fb2a0ca84ef0d0944f14eb1eb6a6d01d61598255bfc1fca485c78d07626628"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/changelog-image-generator"
published_at: "2024-08-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:ba096df16b44cb1ac23d541e2533fcbbec040dba0958b7a14a3513d13e6c9e30"
---

# We built an internal tool to generate changelog images for social media

# We built an internal tool to generate changelog images for social media


- [Cory Watilo](https://posthog.com/community/profiles/30200)


Aug 28, 2024


#### Contents


-
-
-
-
-
-
-
-
-


PostHog's marketing team recently[created a plan](https://github.com/PostHog/meta/issues/218)


to improve our social media presence. One of the[ideas](https://github.com/PostHog/meta/issues/232)


was to share our[changelog updates](https://posthog.com/changelog)


in a more visual way – using a graphic instead of just a text-based post.


##


Why image-based updates?


Image-based posts perform better on X than linking out to another site.[Ian Vanagas Ian Vanagas](https://posthog.com/community/profiles/29296)


originally trialed this with "visual essays". After posting a few of these, we quickly updated the design to match our brand.


So that works great for the occasional visual essay, but for changelog posts, we had a problem... we didn't have the bandwidth to create images for every new changelog post – nor should design be a blocker for the marketing team. And I didn't want to force anyone to use Figma to create these images.


So we started to think... could we productize this so our team could generate high-quality images automatically? After all, our[changelog updates](https://posthog.com/changelog)


already include most of the information we'd need.


A couple years ago, we had done something similar for auto-generating[custom open graph images](https://posthog.com/blog/dynamic-open-graph-images)


for most pages on the website, so we thought... why not do something similar here?


##


Figma mockup


So I got to work, mocking up a template that would pull information from our self-rolled CMS.


##


How we did it


I wrote up[an issue](https://github.com/PostHog/posthog.com/issues/9149)


outlining how I thought we'd be able to combine data from changelog updates, small teams, and team members for use in the rendered images.


###


Changelog data


Our changelog entries already included` title` ,` description` ,` screenshot` ,` update_type` , and associated` product` and` team` .


###


Product branding


Each PostHog product has an icon and color associated with it. These are defined in a JSON file that builds our navigation submenus.


```text
{      name: 'Data warehouse',      url: '/docs/data-warehouse',      color: 'lilac',      icon: 'IconDatabase',    }
```


(The` color` maps to a HEX value defined in Tailwind.)


###


Customization


Then it just comes down to visual polish, so we added a few options.


###


Hedgehogs


And what PostHog graphic wouldn't be complete without a hedgehog?


Recently,[Lottie Coxon Lottie Coxon](https://posthog.com/community/profiles/27881)


organized our[hedgehog library](https://www.figma.com/design/I0VKEEjbkKUDSVzFus2Lpu/Hoggies?node-id=2226-55&t=UfQboIrCRcRuvSSK-1)


and named each hedgehog.


This made it easy for our front end developer[Eli Kinsey Eli Kinsey](https://posthog.com/community/profiles/28804)


to export them and create a searchable library (based off the filenames).


Now we can choose any hedgehog image from our library – as long as it's uploaded to a folder in Cloudinary.


###


Generating the image


We used[html-to-image](https://github.com/bubkoo/html-to-image)


to render the final product. When a team member clicks the *download* button, the image is generated[in a very clever way](https://github.com/bubkoo/html-to-image#how-it-works)


, then downloaded to the client.


##


The final product


And voila, here's an example of our first image produced with this new tool. You should[check it out on X](https://twitter.com/posthog/status/1828458717810983330)


- and give us a follow while you're there!


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
