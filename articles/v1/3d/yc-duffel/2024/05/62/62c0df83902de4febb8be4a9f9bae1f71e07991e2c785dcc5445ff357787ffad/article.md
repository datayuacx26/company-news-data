---
schema_version: "1.0.0"
document_id: "62c0df83902de4febb8be4a9f9bae1f71e07991e2c785dcc5445ff357787ffad"
company_key: "yc-duffel"
company: "Duffel"
source_id: "yc-duffel-news-import-589396e7daca"
canonical_url: "https://duffel.com/blog/what-is-developer-experience-and-why-does-it-matter-2"
published_at: "2024-05-02T09:00:35+00:00"
first_seen_at: "2026-07-24T05:21:37.080376+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:96f540583cbeaa354bfc9382d936e5a3a1e49344ffe3afb88848c1887f52311f"
---

# Tired of wrestling with frustrating APIs? Duffel was built with developer experience in mind. Think clear docs, intuitive design, and features that make integrating flights a breeze. #API #developer #DX #traveltech

In this post, we'll cover:


- What "developer experience" is and why it matters - even if you're not a developer
- What we've worked on recently to make Duffel even better for developers


Not a developer? Don't worry! This post is still for you.


## What is developer experience and why does it matter?


To build modern websites and apps, developers have to integrate with lots of APIs.


Let's take building an online travel agency (OTA) website, for example. You'll have to work with many different APIs for different tasks - for example sending booking confirmation emails with SendGrid, collecting payments through Stripe, tracking users' activity using Mixpanel and, of course, actually booking flights, hotels and cars.


If you speak to any developer, they'll be able to share their API horror stories: broken test environments, unclear error messages, unhelpful customer support, painful version upgrades...


They'll also be able to tell you about their favourite API: intuitive design, clear and up-to-date documentation, fast response times, code snippets they can just copy and paste to build more quickly...


We're doing all of the right things to make sure that Duffel is *one of the APIs that developers love and tell their friends about* - not one of the APIs they hate and want to forget.


If you're not a developer, why does any of this matter to you? Great developer experience means:


- you can effortlessly build a state-of-the-art user experience and delight your customers
- your team will be able to integrate faster, which saves you money and accelerates time to market
- what you do build will be more reliable, with fewer unexpected errors or unwanted surprises once you're live
- once your integration is up and running, it's quick and easy to integrate new features (for example[Pay Later](https://duffel.ghost.io/duffel-launches-pay-later/) , which we've just launched)


## **Ways we've invested in developer experience**


###
**We've made our API docs easier to navigate**


Our API reference is the "index page" of our API. It lists all of the functions available, telling developers what they can send to us, and what we'll send back.


We are continuously launching new features which make the API reference easier to navigate.


You can now search within the reference - so if, for example, you want to learn how to select seats when making a booking, you can search for "seat" and jump to exactly the right place.


You can also expand and collapse API attributes, and link to specific ones (like[this](https://duffel.com/docs/api/offers/schema?ref=duffel.ghost.io#payment_requirements-requires_instant_payment) !) if you want to share with a colleague or add them to your own internal documentation.


### **Leveraging our getting started guides**


It's great when a book has an index, but that isn't where you go when you first start reading! In the same way, an API reference isn't the best way to get to grips with an API. It's perfect if you know exactly what you're looking for, but it doesn't show you where to start.


We think a developer's first time experience with our API is incredibly important. To make that as smooth as possible, we've published a series of "getting started" guides.


Our[Quick Start](https://duffel.com/docs?ref=duffel.ghost.io) guides takes you through the first steps with Duffel, right through to booking your first flight and/or stay, adding ancillaries or book a hotel. A developer can follow it through, step by step, in less than 10 minutes - or even less with the handy[Postman collection](https://app.getpostman.com/run-collection/15595862-cce09558-a1c8-44f9-bde8-8a18715936c1?action=collection%2Ffork&collection-url=entityId%3D15595862-cce09558-a1c8-44f9-bde8-8a18715936c1%26entityType%3Dcollection%26workspaceId%3D03431b8a-0e9e-49c3-afe5-e2ba9ca63fbc&ref=duffel.ghost.io) .


We've also published guides to some of the more advanced features in our product, for example[adding extra bags](https://duffel.com/docs/guides/adding-extra-bags?ref=duffel.ghost.io) and[selecting seats](https://duffel.com/docs/guides/adding-seats?ref=duffel.ghost.io) .


### **A client library**


When integrating with a new API, there is so much boilerplate code to write before you can actually start building.


A client library - sometimes known as a "software development kit" or SDK - takes care of the boring and repetitive bits for you. It's a pre-made bundle of code that developers can use to get building faster.


We also released a[Duffel client library, written in JavaScript for developers using Node.js](https://www.npmjs.com/package/@duffel/api?ref=duffel.ghost.io) . It allows developers to make their first flight search with Duffel in just three lines of code.


We've already updated our[Quick Start guide](https://duffel.com/docs/guides/quick-start?ref=duffel.ghost.io) with copy and paste Node.js code samples, making it super easy to get started.


### **Our own test airline, Duffel Airways**


In test mode, you can try the Duffel APIs and build your integration without spending actual money.


Because airlines' test environments aren't always reliable, we've added our own fake airline, Duffel Airways, which you can search and make bookings with.


To learn more, check out the "[Interacting with Duffel Airways](https://docs.duffel.com/api/test-mode/duffel-airways?ref=duffel.ghost.io) " section in the docs.


### **List place suggestions**


To provide the most comprehensive travel booking experience, you need a location search tool that's as flexible as your users' itineraries. Our solution allows you to retrieve a list of locations based on a wide range of search criteria:


- **Direct Matches:** Find places by their exact airport names, city names, country names, or IATA codes.
- **Proximity Searches:** Discover locations within a specified radius of a given point (useful for 'near me' functionality).
- **Geocoding:** Search directly by latitude and longitude coordinates to pinpoint precise locations."


### **Get high-quality, brand-compliant airline logos in our Flights API**


With airline logos in our Flights API, you can quickly access 600+ high-quality, brand-compliant logos to use in your booking tool. There’s no need to spend weeks collecting and manipulating image files – we’ve done the work for you. You only need an airline's IATA code to get their logo.


To give you flexibility with how you use airline logos and to maintain the accuracy and consistency of their brands, we’ve included two variations in our API:[logo lockup](https://duffel.com/docs/api/airlines/schema?ref=duffel.ghost.io#airlines-schema-logo-lockup-url) and[logomark](https://duffel.com/docs/api/airlines/schema?ref=duffel.ghost.io#airlines-schema-logo-symbol-url) . The logo lockup is the full version that includes the symbol and the logotype (airline name). The logomark is just the symbol. In practice, you might choose to show the logomark in search results and the full logo lockup on the booking confirmation page.


### **Launch seat selection on your own website in minutes**


Our focus on developer experience is all about making it faster and easier for anyone to integrate flights from Duffel into their website or app.


Earlier this year, we[launched seat selection](https://duffel.com/blog/american-airlines-new-ndc-exclusive-main-plus-product-is-available-through-duffel?ref=duffel.ghost.io) in the Duffel API. We have a great[guide](https://duffel.com/docs/guides/adding-seats?ref=duffel.ghost.io) which explains how to build your own seat map - but it's still quite complex and fiddly.


What if you didn't need to build anything at all? What if you could just "copy and paste" seat maps onto your website and be up and running in minutes?


***That's why we launched Duffel Components, a JavaScript library with a suite of drop-in components that make it quick and easy to build a fantastic travel experience.***


The Duffel component ***Seat Selection*** is a cutting-edge solution that enables seamless integration of seat selection functionality into your website. With minimal effort, you can incorporate a visually appealing and highly responsive seat map that caters to various devices, including computers, smartphones, and tablets. By leveraging this innovative technology, you can enhance user experience and streamline the seat selection process for your customers.


### **We want to hear from you**


Our API is never done. We're always looking for ideas and feedback for how we can make it even easier and faster to build with Duffel.


Have you seen an API with outstanding developer experience? Or have you noticed something not quite right with our API or documentation? Reach out to us at[\[email protected\]](https://duffel.com/cdn-cgi/l/email-protection#ed8588819dad89988b8b8881c38e8280) - we want to hear your thoughts.
