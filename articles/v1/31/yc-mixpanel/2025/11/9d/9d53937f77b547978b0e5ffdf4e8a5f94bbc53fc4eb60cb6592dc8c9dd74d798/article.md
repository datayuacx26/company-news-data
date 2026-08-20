---
schema_version: "1.0.0"
document_id: "9d53937f77b547978b0e5ffdf4e8a5f94bbc53fc4eb60cb6592dc8c9dd74d798"
company_key: "yc-mixpanel"
company: "Mixpanel"
source_id: "yc-mixpanel-rss-ee9e8862ebc7"
canonical_url: "https://community.mixpanel.com/x/announcements/msg_MsOagJa5bZ4k/mixpanel-mcp-server-beta-now-supports-chatgpt-and"
published_at: "2025-11-13T16:43:39+00:00"
first_seen_at: "2026-08-05T06:08:58.596386+00:00"
fetched_at: "2026-08-05T06:09:01.015413+00:00"
content_hash: "sha256:259abcee8d2437751f050443c51ae62dfcbbf11a841c0ecddb3544ef750d3389"
---

# Mixpanel MCP Server Beta Now Supports ChatGPT and Gemini!

-


[Niek S.](https://community.mixpanel.com/members/fe6ebed4-2da5-47a3-b69e-22151d6af878) ·


·


Does this work in Europe as well?


-


[Sharan M.](https://community.mixpanel.com/members/d85b130e-44d2-4661-9f53-1339f99db4cb) ·


·


Not yet, coming this month


🙏


🫠 12🙌 3


-


·


Thought so, still super excited for it coming up!


-


[Tamas](https://community.mixpanel.com/members/b6ff149b-f4e2-40a0-97a3-0866a61eb68e) ·


·


Hi


[Sharan M.](https://community.mixpanel.com/members/d85b130e-44d2-4661-9f53-1339f99db4cb) ,


This beta is available to all projects based in US data center


--> Does that mean that if my project's data residency is US then this feature is available for me, right? And if the data residency of the project is EU then it's not yet available. Am I correct?


🙂


Thanks in advance!


-


·


Hey


[Tamas](https://community.mixpanel.com/members/b6ff149b-f4e2-40a0-97a3-0866a61eb68e) that is right


👍 1


-


[Hunter H.](https://community.mixpanel.com/members/bca3fdea-f5f3-4ad9-b46d-b67ce08fee39) ·


·


[Marisa B.](https://community.mixpanel.com/members/ef7cbacf-9c7b-4fcb-890b-4e3fb2c272c4) This has been working for me in Cursor in general but as of today, Mixpanel returns 'invalid scope' during the oAuth process. The scopes it is asking for are openid, email and profile.


➕ 1


-


·


Very simple for me to reproduce using the npx command from your docs:


npx -y mcp-remote


[https://mcp.mixpanel.com/sse](https://mcp.mixpanel.com/sse) --allow-http


. Using the generated URL then shows the error. MCP is enabled for the org (and was working previously).


-


[Blake K.](https://community.mixpanel.com/members/7c751345-6986-4f06-ac11-04ca006a9ee8) ·


·


Similar to


[Hunter H.](https://community.mixpanel.com/members/bca3fdea-f5f3-4ad9-b46d-b67ce08fee39) , we’ve been using the new Mixpanel MCP functionality over the last week but are now seeing the error above. Is this issue related to the Cloudflare outage today or something different? Any resolution timeline?


👍 1


-


·


[Hunter H.](https://community.mixpanel.com/members/bca3fdea-f5f3-4ad9-b46d-b67ce08fee39)


[Blake K.](https://community.mixpanel.com/members/7c751345-6986-4f06-ac11-04ca006a9ee8) taking a look


🙏 1🙌 3


-


·


We are working on a fix. To temporarily unblock your work flow, please complete the authorization flow with the following command


`


```text
npx mcp-remote   https://mcp.mixpanel.com/sse   --static-oauth-client-metadata '{ "scope": "projects analysis events insights segmentation retention data:read funnels flows data_definitions" }' --allow-http
```


`


-


·


Thanks


[Sharan M.](https://community.mixpanel.com/members/d85b130e-44d2-4661-9f53-1339f99db4cb) - appreciate you keeping us informed and providing a workaround


🙌 4


-


·


Thanks from me also! Some on my team have gotten so used to having this, they were starting to give me some side-eye when they couldn't get on.


🙂


🙌 1


-


·


Well since I have your attention, please share any feedback for MCP.


-


Top use cases


-


How is it better than the past


-


What more would you like from MCP?


DM me, email me


sharan.multani@mixpanel.com , let's chat!


-


·


[Sharan M.](https://community.mixpanel.com/members/d85b130e-44d2-4661-9f53-1339f99db4cb) Will send you an email for sure!


💜 1


-


·


[Sharan M.](https://community.mixpanel.com/members/d85b130e-44d2-4661-9f53-1339f99db4cb) - Submitted the feedback form earlier this week. Happy to discuss feedback in more detail. Really grateful to have MCP as another tool for further integration!


🙌 2
