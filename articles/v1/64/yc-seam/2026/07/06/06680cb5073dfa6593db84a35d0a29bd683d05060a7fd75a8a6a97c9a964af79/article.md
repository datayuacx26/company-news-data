---
schema_version: "1.0.0"
document_id: "06680cb5073dfa6593db84a35d0a29bd683d05060a7fd75a8a6a97c9a964af79"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/introducing-seam-components"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:f630167981c016cd56e20646b09c7ea1337383bbe60f7b562d5e097c347e2581"
---

# Introducing Seam Components

Today we’re announcing Seam Components! Components are a suite of pre-built UI elements to rapidly and effortlessly add advanced device management UI into your products.


**Getting started:**


- Check out our guide for[Getting Started with Seam Components](https://docs.seam.co/latest/seam-components/get-started-with-react-components-and-client-session-tokens)
- View and play with components in the[Interactive Storybook Component Library](https://react.seam.co/)
- Check out some[Full Example Apps](https://github.com/seamapi/react/tree/main/examples)


## What are Seam Components


Seam Components are a set of white-labeled UI elements that can be added to your applications in seconds. With these components, you can offer advanced device management features to your users without needing to develop complex logic for managing device state, refreshing data, and performing actions. These Components only require making a single API call from your server to the Seam API to obtain a Client Session Token (see below for details). After that, all of the data fetching logic and state management is fully encapsulated on the client side.


Let’s take a look at three examples of Components we are releasing today.


### Device Table


To start, the` <DeviceTable />` Component expands into a full, paginated table for a user to view all these devices. Each row includes the device name, its image, and device properties. This is very handy for presenting to a user a list of their devices connected to your application.


### Device Details


The` <DeviceDetails />` provides individual information about a device and lets you perform basic actions on the device such as unlock or locking a door. Depending on the supported capability of the device, different actions will be available. If the device supports Access Codes, this can link to a dedicated table showing those codes, which we are also releasing today (see` <AccessCodeTable />` table)


### Supported Device Table


Last but not least, we’re releasing a` <SupportedDeviceTable />` component that makes our entire device db publicly available! We’ve heard from partners that they are constantly being asked which devices their users can connect to their app.


With this component, **you can now publicly display a fully searchable list of devices** that your application supports through Seam. This is very handy for users to understand whether their devices are compatible with your application, so we recommend that you put this component on a publicly accessible part of your site. Note that this component is fully white-labeled specifically for this purpose so as not to interfere with your branding.


## Client Session Token


Seam Components let you control a secure device resource from within a browser without having to go through your server. Stated another way, these components communicate directly with the Seam API without requiring youto create additional endpoints and server-side logic on your end.


Understandably, you may thus ask, “ok but how does this work and is this secure?”


First, these components do NOT use your regular workspace API key. This would be insecure and would enable an attacker to start controlling other devices in your workspace.


Instead, these components make use of Client Session Tokens.


A Client Session Token is a new form of credential we are introducing as part of this release. It’s a token that can be scoped only to devices and Connected Accounts you assign it for.


This token is initially requested by your application from the Seam API. When your server initially requests this token from the Seam API, you specify the Connected Accounts which you want this token to control.


Once returned, your server passes this token to the SeamProvider in your frontend. The SeamProvider wraps all subsequent components and ensures that your user can only access the devices and accounts for which the Client Session Token was created.


## Why We Built Seam Components


Over the past few months, as more and more applications began integrating Seam, we had the opportunity to see many developers building UI on top of the Seam API. This was to satisfy their users’ demand to be able to manage large fleets of devices from within their applications.


When we interviewed these developers, we often saw that they were all building more or less the same UI elements to manage devices. In addition, we saw that this process was time consuming and complicated as numerous requests had to be proxied through their servers in order to fetch data from the Seam API.


Seeing this, we began brainstorming ideas to see if there could be a way to quickly enable any developer to offer world-class device management UI with the least amount of effort. After a few days of prototyping, we gave a handful of partners a quick demo. The response was enthusiastic and we knew we were on the right track. It took a few more weeks to get here, but the past couple of weeks we finally got a set of Components that were fully functional, well-tested, and ready to be integrated.


## Getting Started


To begin using components, make sure to go check out our[API guides](https://docs.seam.co/latest/seam-components/get-started-with-react-components-and-client-session-tokens) . You can embed components directly inside your application or inside of modals. Components are fully responsive and work on desktop and mobile.


## What’s Next


We’re going to continue adding components as well as improve the existing ones. We are also going to work on letting you further customize the look and feel of those components by being able to override CSS styles. In addition, components are currently only supported in React, but we will be looking to add support for other frontend frameworks such as Angular.


Since this is a new product, there may also be bugs and issues to improve. Please reach out to us if you find any bugs! Hope you enjoy it and we can’t wait to see Seam Components in your apps.


If you have any questions, ping us at


support@getseam.com
