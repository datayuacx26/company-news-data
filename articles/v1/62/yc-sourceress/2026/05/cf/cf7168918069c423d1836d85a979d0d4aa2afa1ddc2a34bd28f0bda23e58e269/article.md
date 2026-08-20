---
schema_version: "1.0.0"
document_id: "cf7168918069c423d1836d85a979d0d4aa2afa1ddc2a34bd28f0bda23e58e269"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/fine-grained-http-permissions-for-ai-agents"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:cf141744edc86a62d5bc4b1cfc9324a4d5e74bb284a7c8a137be51883d9d523b"
---

# Fine-grained HTTP permissions for AI agents

In a previous post ([Latchkey: Use plain old REST instead of MCP](https://imbue.com/product/latchkey/) ), we looked at letting agents use arbitrary HTTP services with[Latchkey](https://github.com/imbue-ai/latchkey) . Between the lines, one question remained unanswered: when we set up agents to work with third-party services on our behalf, how can we control what they actually do with them?


Imagine that you want to use an AI agent to triage the unread emails in your inbox. You want the agent to inspect the messages and bring the urgent ones to your attention, but you certainly don't want the AI to delete them or reply to them. According to the[Least Privilege Principle](https://owasp.org/www-community/controls/Least_Privilege_Principle) , a well-known security best practice, the agent in our example should only be able to list and read emails. No other actions should be permitted.


Some services offer suitable authorization mechanisms. Typically, they would let users generate API tokens constrained to specific scopes. What if that is not an option, though? For example, when the service doesn't support scopes or when they aren't granular enough?


## Custom permissions in Latchkey


As of recently, users can define fine-grained permissions for agents directly in Latchkey. Internally, it uses the[Detent library](https://github.com/imbue-ai/detent) which validates requests against user-provided[JSON schemas](https://json-schema.org/) . To understand how it works in practice, let's take a look at a different example: imagine that you're a user of[Memos](https://usememos.com/) , a lightweight, self-hosted note-taking tool that has a pretty typical REST API. You want an agent to research a topic and save the results in Memos for later inspection by you without worrying about your existing personal notes being disrupted. Following the Least Privilege Principle, it should be enough for agents to be able to create new memos. Accessing the existing ones is not necessary.


Let's try to model this in Latchkey. Assuming that the app is running at` memos.example.com` , this short JSON schema will identify all related requests:


```text
json
```


After checking the[API reference](https://usememos.com/docs/api/latest) , we see that only POST requests to` /api/v1/memos` are necessary:


```text
json
```


Putting these two pieces together, we can create the full configuration:


```text
json
```


Here's what the config says:


- When accessing the API, the only allowed action is the creation of new memos.
- No other requests are allowed (not even outside the API).


Saving it as` permissions.json` in your[Latchkey directory](https://github.com/imbue-ai/latchkey?tab=readme-ov-file#other-configuration) (e.g.` ~/.latchkey/permissions.json` ) and making it read-only (` chmod -w ./permissions.json` ) will ensure that subsequent` latchkey curl` calls made by agents will only go through if they fall within these constraints.


## Predefined examples


Chances are you won't actually need to define all these schemas in practice. The underlying library ships with predefined schemas for some of the popular services. To revisit our initial example with email triaging, depending on your email provider, you may be able to simply do this:


```text
json
```


See the[docs](https://github.com/imbue-ai/detent/blob/main/docs/builtin-schemas.md) for the full list of preconfigured definitions.


## Concluding thoughts


If you’d like to keep your inner peace while an AI agent works with the data in your various personal accounts, give Latchkey a try. It’s as easy as` npm install -g latchkey` followed by` npx skills add latchkey` . Here are some final notes to help you decide:


### Defense in Depth


Even just pointing an AI at the docs (or this article) and letting it generate the` permissions.json` for you is better than nothing. Individual security measures don't have to be perfect to have a positive effect, especially when combining multiple different ones.


### Lethal trifecta


You may wonder: why would you ever want to prevent the agent from read access? What harm can arise from that? It turns out that the ability to read private or untrusted data is a major part of the attack surface in modern LLM setups. The topic is beyond the scope of this blog post - if you're interested, read Simon Willison's[Lethal Trifecta article](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) .


### Privacy


Aside from the above, the less of your personal data the agent reads, the less of it gets sent to AI companies who then train their models on it.


### Isolated instances


Different agents may naturally need to be constrained differently. With Latchkey, you can achieve that by using the` LATCHKEY_PERMISSIONS_CONFIG` environment variable override or completely isolated Latchkey instances (determined by the` LATCHKEY_DIRECTORY` environment variable).


### Limitations


The permission system in Latchkey has been designed to help users and developers control their AI agents and reduce the odds of them accidentally performing harmful actions or getting exposed to untrusted content. If you want protection against actively malicious agents, consider using the permission system in conjunction with Latchkey's[gateway mode](https://docs.imbue.com/latchkey/basics/gateway) while running agents in isolated sandboxes.


If you have thoughts about any of the above,[leave us a comment on GitHub](https://github.com/imbue-ai/latchkey/issues/56) . Also, contributions are always welcome!
