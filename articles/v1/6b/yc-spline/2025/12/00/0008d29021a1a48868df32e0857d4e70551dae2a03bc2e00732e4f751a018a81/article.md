---
schema_version: "1.0.0"
document_id: "0008d29021a1a48868df32e0857d4e70551dae2a03bc2e00732e4f751a018a81"
company_key: "yc-spline"
company: "Spline"
source_id: "yc-spline-news-import-c9c95af69f21"
canonical_url: "https://blog.spline.design/introducing-3d-shapes-hana"
published_at: "2025-12-10T00:00:00+00:00"
first_seen_at: "2026-07-24T02:11:24.958525+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:07b5b005e4c6bdf4466e22093368fe547a1a73b4a2e5e087cfd49bcd294997b4"
---

# Introducing 3D in Hana

December 10, 2025


# Introducing 3D in Hana


Bringing 3D and 2D together


[Open Hana](http://app.spline.design/)


For the longest time, most of the design "process" has taken place on a 2D canvas.


The historical divide between 2D and 3D has its roots in technical complexities. 2D design tools evolved to be both real-time and high-quality (what you see is what you get), but 3D requires far more computing power, making real-time work possible only at lower quality.


But technology has advanced significantly (both hardware and software), and our team has made significant progress over the last few years, enabling us to achieve a fundamental milestone: combining 3D and 2D on the same canvas in real time.


Move mouse or touch to interact (Experience made with Hana)


Touch to interact (Experience made with Hana)


## The convergance of 3D and 2D


We believe a design canvas should leverage all available dimensions, and that the user (human or AI agent) should be able to decide what's best for the solution at hand.


Unfortunately, while this works in theory, implementing anything in 3D is a technical nightmare, and combining these two systems makes it even worse.


Most current design tools, or traditional ones, were built on a similar approach: you work on a preview, then press "render" or "hand off" your work, and someone will implement the actual thing using a different stack.


In other words, nearly all current design tools are not real-time, and thus 3D is usually required to use specialized tooling whose output is ultimately handled or embedded as flat images or videos; not editable and non-interactive. It's a slow process that doesn't align with today's design workflow standards.


A new approach is needed to change this.


Experiences made with Hana


## Real-time is the ground base


When we built Hana, we made it to be real-time while also capable of rendering *anything* you can render in 2D. This means Hana's renderer can handle all sorts of pixel-perfect effects (like shadows, blur, etc), vector shapes, booleans, and more, all in real-time.


This means that we can leverage Hana's effects pipeline to render 3D graphics in real-time as well.


Essentially, Hana can combine 3D and 2D altogether.


Vectors turned into real-time 3D with Hana


## The journey to combine two worlds


Adding 3D to a 2D canvas seems like an easy task, but in reality, there are myriad challenges. Most of the time, it requires running two renderers simultaneously or combining multiple runtimes, resulting in long download times that are not ideal.


Hana uses the same runtime for 3d and 2d, which is more efficient and substantially more flexible.


Another central issue is quality, or rather, the lack of it.


Combining the two systems into one while maintaining the same level of quality is challenging because 3D quality encompasses several factors: anti-aliasing, resolution, polycount (or high voxel density), number of objects, and more.


In Hana, you can create any 2D shape and convert it into a 3D object while maintaining a real-time connection to the original shape.


This means that at any time, you can modify the original shape, and the 3D object will be regenerated in real-time in connection to the original shape.


This allows for a substantial productivity improvement in 3D creation within a 2D composition. For example, you can easily create 3D text and change its color, font, or other properties while preserving all the flexibility of the text in a non-destructive workflow.


Layouts can update dynamically, and the 3D objects will follow the rules of the 2D composition.


Experiences made with Hana 3D Shapes


## 3D follows 2D


In Hana, every 3D object has its own coordinate system. However, it continues to follow the rules and behavior of the 2D canvas, which means you can apply to 3D objects anything you would normally use on 2D objects.


This opens the door to a wide range of design possibilities.


Experiences made with Hana 3D Shapes


## Limitations


Currently, only a limited number of features are available within Hana’s 3D/2D system.


You can extrude any 2D shape into 3D and have a fair amount of control over materials, but there is still plenty of room for new possibilities.


Ideally, the 3D system should be able to render any type of 3D object or scene and be far less limited.


-


-


-


-


-


-


-


-


-


-


-


-


-


-


-


-


Experiences made with Hana 3D Shapes


## This is the beginning


We have been working hard over the past few years at Spline to create a coherent design system that integrates 2D, 3D, and eventually 4D.


We can’t wait to continue making progress toward this exciting new future.


— The Spline Team 🌸


[Open Hana](https://app.spline.design/)


©2026 - Spline, Inc.


–


[spline.design](https://spline.design/)
