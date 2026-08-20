---
schema_version: "1.0.0"
document_id: "d2c35617912757fc538cbab9923542c0dfd2d44a7741718ee3710b4895c198b4"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/stateless-mcp/"
published_at: "2026-07-30T13:00:00+00:00"
first_seen_at: "2026-07-31T23:27:38.805062+00:00"
fetched_at: "2026-07-31T23:27:39.625511+00:00"
content_hash: "sha256:369d7d5901127cc593b38cb20620cbf6f6a3d430f8e901fd2977c4fb12bdb97c"
---

# Fully Event-Driven MCP is Coming

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


As of the latest[MCP release (2026-07-28)](https://blog.modelcontextprotocol.io/posts/2026-07-28/) , MCP is now stateless!


It's great to finally have it out. The stateful parts of MCP always felt the most complex and least scalable part of the protocol. It was also different to the rest of the internet which is almost entirely stateless.


MCP is widely used and is becoming more popular by the day, so having it align to the rest of the web is a big deal. Though I also personally care about this because this aligns well with how we think about APIs and webhooks.


APIs and webhooks are inherently stateless at the protocol level, and are only stateful at the application level. That is, an API call is one request, and that's where it ends. If it triggers some asynchronous flow, the response will come later via webhook, which will also just be one request. The API request can be handled by one server, and the webhook response can be handled by a whole different server on the other side of the globe which makes things more reliable and easier to scale.


With that being said, knowing when an asynchronous task finishes and its results is immensely useful with for APIs and MCP, which is where the application level state comes into play. This version of MCP defines the` tasks` extension as the means to achieve that. When you create an asynchronous task you will get an associated task ID back, which you can then use to poll or subscribe for updates about.


The reason why all of this matters, is because being stateless is a precursor to being fully event-driven. Now that all of this is in place, MCP can finally introduce real event-driven behavior (rather than a live connection). An MCP client will be able to make a request, get back a response hours or days later, and continue right back from where it's started.


The MCP working group is working on exactly this, and the next version will include webhooks (MCP webhooks) to facilitate that. This is going to be a huge unlock for asynchronous agents, and I'm looking forward to the next MCP release.


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
