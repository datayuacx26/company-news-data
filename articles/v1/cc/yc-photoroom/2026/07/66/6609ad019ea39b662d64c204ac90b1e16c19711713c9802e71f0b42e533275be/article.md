---
schema_version: "1.0.0"
document_id: "6609ad019ea39b662d64c204ac90b1e16c19711713c9802e71f0b42e533275be"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-d1044ff9c1aa"
canonical_url: "https://www.photoroom.com/blog/why-social-apps-should-enable-users-to-create-personalized-stickers"
published_at: null
first_seen_at: "2026-07-23T22:00:08.835718+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:1748d2f970933cf44791cdd3699453e7cfd805bbf986a8771f59be6ef14302fa"
---

# Why social apps should enable users to create personalized stickers

If you work on a social app, you know how important it is to deliver an experience that keeps your users engaged and excited over time.


A great way to deliver such an experience is to allow your users to personalize the way they interact with your app.


And that’s when stickers come into play!


Stickers are small background-less images that users can use, for example, to react in a conversation or to decorate their profile page:


But what’s really great with stickers is that you can enable your users to create their own personalized ones, which will allow them to bring a lot of their personality into your app!


Popular social apps such as[Mojo](https://mojo-app.com/) and[Id by amo](https://apps.apple.com/us/app/id-by-amo/id6470037899) actually use the Photoroom API in order to offer this personalized stickers feature to their users.


So let’s see how the Photoroom API will help you add this feature to your app.


## How to create stickers with the Photoroom API


From a technical point of view, turning a regular image into a sticker is a two-step process:


1.


first, cutting out the main subject of the original image


2.


then, cropping the result image to the bounds of that main subject


Fortunately, the Photoroom API takes care of both steps in a single call!


To implement this, we’re going to use Photoroom’s[Remove Background API](https://docs.photoroom.com/remove-background-api) .


Here’s how a call to this API looks like:


```text
curl --request POST \
--url https://sdk.photoroom.com/v1/segment \
--header 'x-api-key: YOUR_API_KEY' \
--form 'image_file=@/absolute/path/to/image.jpg' \
--form 'crop=true'
--output sticker.png
```


*To make the call, you will need an API key. Here are the steps to[get your API key](https://docs.photoroom.com/getting-started/how-can-i-get-my-api-key) .*


And here's a before/after example of a call to this API:


The API call is made through a POST HTTP request, which takes three arguments:


-


**x-api-key** is your Photoroom API key


-


**image_file** is the file containing the image to use to create the sticker


-


**crop=true** tells the API to crop the result image to the bounds of the cutout subject


As you can see, it’s just a standard HTTP call that uses multipart formatting to send its data.


This means that integrating the API call into your codebase should be fairly straightforward.


We’ve even written wrappers to help you seamlessly integrate the API into[Web](https://docs.photoroom.com/remove-background-api/web-integration) ,[Node.js](https://docs.photoroom.com/remove-background-api/node.js-integration) ,[Python,](https://docs.photoroom.com/remove-background-api/python-integration) and[iOS](https://docs.photoroom.com/remove-background-api/ios-swift-integration) projects.


## Conclusion


That’s it, thanks to the Photoroom API we’ve seen that you can easily enable your users to create high-quality stickers that they’ll be able to use inside your social app.


For more details about all that can be achieved using Photoroom’s API, check out **[this page on our website](https://www.photoroom.com/api)** !
