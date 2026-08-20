---
schema_version: "1.0.0"
document_id: "c80a709e5169eed9cad8185d242707444116342a26e80d749e1efbb317087303"
company_key: "yc-spline"
company: "Spline"
source_id: "yc-spline-news-import-c9c95af69f21"
canonical_url: "https://blog.spline.design/hana-3d-glass-autolayout-video"
published_at: "2025-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T02:11:24.958525+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:b3d7402e9513ce5a70dade57f6b3c49bbf7392d824217be6800435f2d27d1f6a"
---

# Spline Blog - Glass, 3D, Autolayout, and Video layers

Jul 9, 2025


# Liquid Glass and 3D in Hana


New in Hana: 3D Transform, Liquid Glass and Autolayout


[Open Hana](http://app.spline.design/)


Today, we are announcing a huge update for Hana that introduces groundbreaking features like real-time Liquid Glass, 3D Transform, Autolayout, and Video Layers.


## Liquid Glass in Hana


With the introduction of iOS 26, the Liquid Glass effect has captured the attention of the design community. Now, you can apply this effect to any object layer in Hana.


You can edit the glass parameters, such as Distortion, to control the amount of refraction.


However, you can go beyond the iOS style and create custom glass effects. We've ensured the parameters are general enough to achieve various creative outcomes by allowing you to exaggerate some of the effects' behaviors and achieve more striking visual aesthetics.


Hover or touch to interact (Experience made with Hana)


Touch to interact (Experience made with Hana)


## Stackable Glass


**** In most design tools, the implementation of blur or glass effects is usually constrained to a single layer, meaning that if you stack multiple layers, the distortion of the glass will not accumulate and remains flat.


In Hana, you can easily stack multiple layers of glass and see how each refracts the elements behind or below, just like on real life, enabling a whole new set of creative opportunities. This is possible for the glass effects, but also for background blur effects.


Manipulate multiple layers of Liquid Glass in Hana


## 3D Transform


Going from 2D to 3D has always been a challenge. When we started working on this feature, we knew we wanted to allow a seamless way to easily switch between 2D and 3D without losing anything in between.


In Hana, any object can now be a 3D object, with parameters to control its position (Depth), rotation, and perspective in 3D space.


All of these parameters are animatable and compatible with the states/event system.


Control the perspective, rotation, and z-position of the object in Hana


## Frames and 3D Portals


When a frame becomes 3D, it also becomes a window into its own 3D space. This is especially useful for creating portals, a visual element where you can see depth within itself.


Its easy: just put items inside a frame, make the frame 3D, and change the 3D position or rotation of the items within. Now, if you change the rotation or position of the frame, you will see how the items are rendered inside, with multiple levels of depth.


3D portals within Frames created in Hana


## Autolayout


Hana now supports layouts with fully animatable parameters. You can create both vertical and horizontal layouts with customizable padding and gaps.


Layouts work in conjunction with events and states, and updating in real-time to enhance interactivity.


We've also added support for masonry layouts, which are similar to Pinterest's column layouts.


Experiences made with Hana using Layout


## Video Layers


Now you can easily embed a video on any layer within Spline Hana. This comes in handy when building an interactive presentation or designing for motion. Just switch to the Video Layer in the Fill panel and upload or drag and drop your video.


Similar to Spline's 3D editor, videos in Hana can be triggered under custom events also. By default, a video will play on the Start event, but you can change this and trigger it with mouse events or any other event.


Control the perspective, rotation, and z-position of the object in Hana


## New creative possibilities


That was a lot!
We hope you enjoy all the new features we shipped today and start creating exciting new designs.


— The Spline Team


[Open Hana](http://app.spline.design/)


©2026 - Spline, Inc.


–


[spline.design](https://spline.design/)
