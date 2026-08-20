---
schema_version: "1.0.0"
document_id: "bece38ea1f261a8163db1d5a75247703ad004d00727bf6b62b54001eff21ac38"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/how-to-create-adaptable-paywalls-for-iphone-and-ipad-using-superwall"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:20fa60e16a1e176f1e9025930d11e8a1c68440767a4e91b045267118d33f41e4"
---

# How to Create Adaptable Paywalls for iPhone and iPad Using Superwall

Designing for iPad can be challenging, though it's a worthwhile investment. With iPad holding over 32% of the tablet market, you can bet you'll have users on the platform. However, simply expanding your existing iPhone design to an iPad often yields an *okay* experience — to truly shine, you need to intentionally[design for it](https://developer.apple.com/design/human-interface-guidelines/designing-for-ipados) .


Your paywalls are no different. Today, I'll show you some tips and best practices to help you bring your paywall designs to iPad. Here's the paywall we'll adapt (shown above). And, here's how it currently appears on iPad:


Current paywall design on the iPad


While it's serviceable, we can do better. An iPad has more display real estate (in *most* cases —[multitasking](https://support.apple.com/en-us/102576) and[Stage Manager](https://support.apple.com/en-sg/guide/ipad/ipad1240f36f/ipados) can yield all sorts of window sizes) than iPhone. I like to start designing for iPad by asking myself two questions:


1. Is there anything I can meaningfully add to the interface that helps make the current task at hand better (in this case — our paywall)?
2. Can I adapt parts of the current interface to look more at home on iPad?


To work through those, I use[Freeform](https://www.apple.com/newsroom/2022/12/apple-launches-freeform-a-powerful-new-app-designed-for-creative-collaboration/) , which is a free app from Apple, to jot down ideas and approaches I might try out:


Sketching out ideas using Freeform


With those design decisions in mind, here's what we will do in the Superwall editor:


1. We'll tweak the width of the bottom footer.
2. We'll change the bullet list to a grid that's 3x2 since we can allow for more information density.
3. Finally, we'll add a new component — some review testimonials to tastefully take advantage of extra space.


Let's dive in!


## Changing the footer width


When you open the paywall editor, there is a[floating toolbar](https://superwall.com/docs/paywall-editor-floating-toolbar) at the bottom that lets you preview different device sizes. It helps to change this to "iPad" for design purposes:


Use the device toggle to view paywalls on an iPad


To change the width of the footer, we'll use[dynamic values](https://superwall.com/docs/paywall-editor-dynamic-values) in the editor. If you're new to these, don't worry — they are incredibly simple. Dynamic values are properties you can set that can change or adapt based on some conditions.


So, for our footer's width — think of it like this: If we are on an iPad, it should be X, otherwise it should be Y. And that's exactly what we'll do. This footer already takes up the width of the container, and its horizontal padding is set to 16 pixels. We'll make the horizontal padding be dynamic:


1. If it's an iPad, the padding will be set to 20% of the container's width.
2. Otherwise, we'll stick with the 16 points.


I'll skip the "how-to" going forward for dynamic values, but in case you've not made one yourself — simply click the component's property you're after and choose "Dynamic":


The horizontal padding is using a dynamic value here.


You can tell a dynamic value is being used when the property has a purple background, just like in the image above. Here's how I set it:


Using dynamic values to adjust padding for iPad devices You can set this to 'ipad' to see your changes live.


## Adding review testimonials


Next, I want to do something additive to take advantage of extra space. Here, we'll add a "feature carousel" to show off our happy users' reviews. We have a[snippet](https://superwall.com/docs/paywall-editor-layout#snippets) for this in our component library, and along with dynamic values — this will be trivial to do.


Again, since we covered *how* to set dynamic values above, I'll move a little faster here. The idea is to add the review carousel component, but hide it on iPhone:


Hiding the review carousel on iPhone


And here's our conditions:


Adding the reviews for iPad devices


## Using a feature grid


While the three listed features showing on iPhone are nice to have, I think we can show off more about what our app's pro plan offers by adding three more. The end goal? On iPhone, the list of three features stays as it is now, but on iPad we'll move it over and add another three features next to it.


Here's what we'll do:


1. Duplicate the existing bullet list component.
2. Embed the two lists into a horizontal stack.
3. Using a dynamic value, hide the list meant for iPad when we are on an iPhone.


Here's an example of step #3:


New grid of bullet lists that are hidden on iPhone


And with that, once we view the paywall on an iPad — we get this nice, 3x2 feature grid:


Expanded feature list on iPad


## End result


With those three tweaks, here is where our iPad paywall ends up:


Our paywall tailored to iPad


I think it turned out quite nice! And, going a step further and using our[theme colors](https://superwall.com/docs/paywall-editor-theme) , we can support dark mode as well:


Paywalls adapting to dark mode and the device size


With Superwall's paywall editor, you can build best-in-class designs, interactions, and paywalls. There is no limit. With the tools Superwall gives you to make some small changes here and there, every paywall can look and feel fantastic on any device medium.


Before we go, here is some lasting advice when designing for iPad:


1. **Responsive Design** : Just like with a website, try to think in terms of responsive design. While I've shown a condition that checks for an iPad, in reality a check that looks at the size of the device is a bit more flexible. Remember, Stage Manager and multitasking can mean that your paywall on an iPad could be sized in a window as small as an iPhone.
2. **Think in percents** : Instead of sizing things at X amount of pixels, it helps to think in percents. For example, you might set a container's width to be 60% of the viewport's width instead of setting it to something like 300 pixels. This helps your designs adapt naturally to several sizes.
3. **Use Dynamic Values** : As we've seen here, dynamic values are critical when designing for many different screen sizes. Sometimes, there isn't a percent of value that works across everything. So, adapt it to a value that *does* work for a given scenario.


## Wrapping up


Designing for iPad can help take your user experience to the next level. When opening a paywall that adapts to the device, your app feels a little more cared for than your competition. Our editor was built to help you make paywalls across any device, and hopefully the tips here can help you get started tailoring your paywalls for iPad.


If you have any other advice or need assistance, we're always around to help you get started with Superwall anytime. Why not build some iPad paywalls[today by signing up with a free account](https://superwall.com/sign-up) ?
