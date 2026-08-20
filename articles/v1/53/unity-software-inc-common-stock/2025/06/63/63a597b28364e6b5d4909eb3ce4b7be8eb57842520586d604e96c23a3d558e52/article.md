---
schema_version: "1.0.0"
document_id: "63a597b28364e6b5d4909eb3ce4b7be8eb57842520586d604e96c23a3d558e52"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/ena-dream-bbq-video-layering"
published_at: "2025-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:48:01.724874+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:3f9c713e9a1d7325cbc84e8df0907d656d5ec031e5671a2db5677030358b993c"
---

# How dialog-driven video layering shapes the surreal world of ENA: Dream BBQ

The third I would call “timeline sequences,” or cut scenes, which we created with custom video tracks and custom dialog tracks. While the timeline controls the animations and camera motions of the sequence, under the hood the video track is driving the timeline. It’s not in the update cycle – it’s driven by the video player. That’s for a specific reason: If the player system is stuttering the video, we don’t want it to be constantly playing catchup. So it’s really running the timeline in the timing of the video itself, which enabled us to do more sophisticated things in the timeline.


**What were the challenges of working with so many different systems?**


**Evan:** From a tech director perspective, it was very interesting because Joel would come to us with an idea saying, “I have this vision where this happens in the foreground, this happens in the background. The camera then pans, and then another 2D sequence happens on the screen.” So we’d need to plan around the in-game world by matching a pre-rendered or pre-animated video that has transparency. And that’s why directing the timeline is important, because if the camera pans and that pan is matched in the 2D animation, we need the animation to direct the Unity timeline.


A lot of it came down to Joel saying, “Hey, this is my idea. Here’s a storyboard or animatic of it. Is it possible?” And then I would go through it, talk to Luke and our artists, and detail out how we could do it in Unity.


**Luke:** I think one of my favorite things about working on this project is that Joel doesn’t have much technical experience with Unity or programming. And that actually was really exciting because he would propose things that are absurd.


The best example that’s visible to players is the inventory. Having it be a newspaper that you look through and see your current jobs as if they were advertisements is counterintuitive to how you would typically approach a UI – usually you have a menu with a scroll box, just something functional.


When Joel showed me the concept for swapping through the papers, I was so excited. But if you come from a typical background of what you would expect a user experience in a menu to be, you’d probably play it straight and narrow.
