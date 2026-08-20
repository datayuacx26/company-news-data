---
schema_version: "1.0.0"
document_id: "5fb444fe564a4b041f6e90ab262a2b7c8126511defa39184209f4b8e51fb0175"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-imessage-support"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T18:27:33.266522+00:00"
fetched_at: "2026-08-11T18:27:35.070831+00:00"
content_hash: "sha256:d0c3eba49877ff9c8be0b62c410fcc4e6cf4b7295df2b0b2fd58f99dbaa6f6b8"
---

# Introducing iMessage Support for Mastra Agents

Your Mastra agents can now send and receive messages using[iMessage](https://mastra.ai/integrations/channels/imessage) on iPhone, iPad, or Mac. The[Photon](https://app.photon.codes/) chat adapter enables direct and group chat messages and uses your agent's[webhook URL](https://mastra.ai/integrations/channels/imessage#webhook-url) to handle two-way communication.


iMessage conversations have a limited surface area available to help you debug or monitor agent behavior. Mastra's[observability](https://mastra.ai/docs/observability/overview) system provides visibility into each trace, letting you inspect and debug each run.


Your browser does not support the video tag.


Agent interactions differ from app to app. Some conversations are more suited to mobile chat interfaces, others to desktop applications. Choosing the most suitable interface can enhance user experience. iMessage joins Mastra's existing channel adapters:[Slack](https://mastra.ai/integrations/channels/slack) ,[Discord](https://mastra.ai/integrations/channels/discord) ,[WhatsApp](https://mastra.ai/integrations/channels/whatsapp) ,[Microsoft Teams](https://mastra.ai/integrations/channels/teams) , and more, letting you choose the right UI for your users.


Delivery reliability varies by channel. Photon retries failed messages so[duplicate deliveries](https://mastra.ai/integrations/channels/imessage#duplicate-deliveries) can occasionally occur. Mastra automatically dedupes using in-memory keys, which persist in long-running instances but can be lost in serverless environments. To persist dedupe keys across cold starts, restarts, or crashes, add` createRedisState()` to your agent's` channels.state` .


## Get started


Install the iMessage adapter:


Terminal


```text
npm   install   @photon-ai/chat-adapter-imessage
```


note


Requires` @mastra/core@1.56.0` or later, added in[PR #20702](https://github.com/mastra-ai/mastra/pull/20702) .


Add` createiMessageAdapter()` to your agent's` channels.adapters` object:


src/mastra/agents/concierge-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   createiMessageAdapter   }   from   "  @photon-ai/chat-adapter-imessage  "  ;


export   const   conciergeAgent   =   new   Agent  ({
id:   "  concierge-agent  "  ,
name:   "  Concierge Agent  "  ,
description:   "  Handles restaurant bookings over iMessage  "  ,
instructions:   /* ... */  ,
model:   "  anthropic/claude-sonnet-5  "  ,
channels: {
adapters: {
imessage: {
adapter:   createiMessageAdapter  (),
toolDisplay:   "  hidden  "  ,
gateway:   false
}
}
}
});
```


- ` toolDisplay: "hidden"` — hides tool call details from message output
- ` gateway: false` — disables the[gateway listener](https://mastra.ai/integrations/channels/imessage#gateway-listener) when using webhooks


For more information and full configuration options, see:


- [iMessage](https://mastra.ai/integrations/channels/imessage)
- [Channels overview](https://mastra.ai/docs/capabilities/channels)
