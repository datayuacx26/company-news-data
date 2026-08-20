---
schema_version: "1.0.0"
document_id: "1b4a130fd2eb7edca61a3a0f3b21aa2a853fd6a37de9e712b400936ee2f7ce86"
company_key: "yc-dialect"
company: "Dialect"
source_id: "yc-dialect-news-import-4cb56bc42b2e"
canonical_url: "https://www.dialect.to/blog/presenting-the-blinks-public-registry"
published_at: "2025-07-10T12:59:00+00:00"
first_seen_at: "2026-07-21T16:13:21.108809+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:053c85a4c0963c287441acd8e4f594265e44da1b9cb758a8b62f74dbd50152df"
---

# Presenting the Blinks Public Registry

Since the launch of[Actions and blinks](https://x.com/saydialect/status/1805588148212424901) 3 weeks ago, hundreds of developers have been inspired to build experiences with this new tech.


We’ve been blown away by the quality of submissions to the registry, a roster which we’ve been managing, together with Solana Foundation and participating wallets, to determine which blinks will render on X. Over 100 actions have been approved, with hundreds more submitted.


We’ve realized the whole ecosystem should have access to this roster to see what everyone is building, and be inspired to create more.


That is why, today, we are releasing a major new upgrade to our product offering:


Introducing the Blinks Public Registry, a full catalog of all blinks that have been created and submitted to the registry, regardless of approval status, both as a public good via API, and browsable in a beautiful new interface right on dial.to, your favorite blinks popup site. Learn more about it in our[docs](https://docs.dialect.to/documentation/actions/the-blinks-public-registry) .


Actions and blinks are a new way to create & distribute transactions across the ecosystem, and we believe a registry should serve purposes beyond security: this registry will help developers share their actions with the community, and other developers to compose and build off of them in unexpected new ways, such as training LLMs to create a language interface for blinks, or even create new browsable interfaces like dial.to.


Discoverability also improves security. We have already integrated security providers like Blowfish directly into the registry for greater automation in assessing the existing actions today. But as we decentralize this process, we believe the whole community can get involved.


Browse the new Blinks Public Registry by visiting dial.to, or continue reading to learn more about how you can use this public API to build new experiences for your users.


#### The Blinks Public Registry API


We have made our Blinks Public Registry easily accessible through an API endpoint, as described in our[docs](https://docs.dialect.to/documentation/actions/the-blinks-public-registry) . Running a *GET* request on the endpoint will return *JSON* data of all the action APIs that have been *submitted* , *registered* and *blocked* from the registry The Blinks Public Registry API can be called from https://registry.dial.to/v1/list.


> *curl -X GEThttps://registry.dial.to/v1/list*


Sending the *GET* request to this endpoint returns a *200* response with the data in the following format:


> *{ results: Array<{ actionUrl: string; blinkUrl?: string; websiteUrl?: string; createdAt: string; tags: string\[\]; }> }*


The fields within each element of the *results* array are:


*actionUrl* - The URL of the Action.


*blinkUrl* - (optional) The Webpage or Interstitial URL that points to the Action URL


*websiteUrl* - (optional) URL of the Action owner.


*createdAt* - When the Action was added to the registry.


*tags* - The tags depicting the Action's status on the registry.


The Public Registry API will be updated daily with new Actions and blinks. Learn more about Actions and blinks in our[docs](https://docs.dialect.to/documentation/actions/the-blinks-public-registry) .
