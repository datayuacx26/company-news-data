---
schema_version: "1.0.0"
document_id: "f249a8b4f257081cb85cbf9455d7d91160bfc761ee5f2f5510579e32e5255d7d"
company_key: "unity-software-inc-common-stock"
company: "Unity Software Inc."
source_id: "unity-software-inc-common-stock-rss-726793b11211"
canonical_url: "https://unity.com/blog/47-tab-problem"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-27T23:28:44.568027+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:d5c020a56ec256465891e1f912d8230b3ad121e2fd0991930a41b4f6594ee072"
---

# The 47-tab problem: How Unity devs find answers mid-build

## **The 47-tab moment**


***We press Play. A NavMesh agent in our Unity Render Pipeline (URP) project walks into a dynamic obstacle, spins in place, and fails to pathfind.***


***We open the first tab: Unity documentation. The sample code is helpful, but it needs a few minor adjustments for fit with the Unity version and render pipeline we’re using.***


***Next tab: a Unity Discussions thread from 2019. The accepted answer reflects the tooling available at the time, while a comment clarifies its scope within the Built-in Render Pipeline.***


***Another tab: a Stack Overflow post with a similar error message, but the proposed fix assumes a different scene structure.***


***We open a YouTube tutorial. It runs 18 minutes, was recorded in Unity 5, and halfway through it becomes clear that everything is baked into a static level with no dynamic obstacles.***


***More tabs follow: Reddit, Discord archives, blog posts, AI chat logs. Each one is “almost right,” but each assumes a slightly different version, pipeline, or project setup.***


***This is what we refer to in this article as the 47-tab problem: The challenge is not a lack of information, but the difficulty of finding an answer that matches our Unity version, render pipeline, and scene by searching on the web.***
