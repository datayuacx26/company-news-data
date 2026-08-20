---
schema_version: "1.0.0"
document_id: "737ddeb2e9e1f75cdf996b15b21feb91d11c6c21f335239a83053a292af22483"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-client-integration-nextjs-officially-released"
published_at: "2025-04-07T11:26:16+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:770d2d115634ea956b0766563325f48a53aed72f235ee5cb7b0607bfc2a0ef6f"
---

# @apollo/client-integration-nextjs Officially Released

## **Exciting News:**` @apollo/client-integration-nextjs` **Officially Released!**


After almost two years of testing, gathering user feedback, and refining our approach, we’re thrilled to announce that` @apollo/experimental-nextjs-app-support` has been released as a stable package with the new name` @apollo/client-integration-nextjs` . This milestone marks a significant achievement for the Apollo Client community 🎉


## **A Brief History of Apollo Client + Next.js**


When we first started working on this integration, we were concerned about potential race conditions during hydration with how Next.js works. Next.js became a very popular framework for React and it was important to the Apollo Client community to provide a happy path forward.


After millions of downloads and many successful production use cases, we’re happy to report that these issues haven’t materialized in the wild. In parallel to this success, React also made significant strides in handling hydration errors.


## **So What’s New?**


One of the biggest changes is the package being renamed from` @apollo/experimental-nextjs-app-support` to` @apollo/client-integration-nextjs` , removing the “experimental”. This requires adjusting your imports if already using the experimental package and also provides confidence to new users in the projects they are building.


The latest release also includes a new implementation for` PreloadQuery` that now supports multipart responses. This feature allows you to preload queries with` @defer` in RSC and rehydrate your cache seamlessly 🤯


## **The Perfect No-Waterfall Experience**


Combining` PreloadQuery` with fragment colocation and` @defer` enables a truly no-waterfall experience, outperforming plain REST APIs by a significant margin. Where RSC just moves the waterfall from the client to the server (where it is admittedly faster), fragment colocation allows you to avoid the waterfall altogether – and thanks to` @defer` , you won’t have to wait for the full result either: your app can make the most of React 19’s streaming SSR, rendering things as soon as they are available.


After your application hydrates on the client, you can make all future requests in the client, where they will be merged into your normalized cache and allow for component updates without the additional server-side workload introduced by sticking to a RSC-only solution.


This hybrid approach gives you the best of both worlds.


## **Setup Instructions**


For those eager to get started, please refer to our comprehensive setup guide:[https://github.com/apollographql/apollo-client-integrations/tree/main/packages/nextjs](https://github.com/apollographql/apollo-client-integrations/tree/main/packages/nextjs) . There are detailed instructions for both RSC and SSR use cases.


## **More Apollo Client Improvements in Alpha**


Along with our Next.js release, we’ve also started work on React-Router 7 (Remix) and TanStack Start. We’ve received a lot of community requests on these two and we’re excited to announce that integration packages for these frameworks are now out in alpha. Your feedback counts here – please try them out and join the discussion. You can find them at[https://github.com/apollographql/apollo-client-integrations](https://github.com/apollographql/apollo-client-integrations) .


Apollo Client 4.0 has officially entered the alpha stage. This alpha release contains some great improvements that we think you’re gonna want to try out:


- switch to ESM, modern bundling an d a modern build target
- ~25% reduction in bundle size
- more coherent error handling
- ` useLazyQuery` rewrite
- move to` rxjs` (from` zen-observable` )


Please take a look at the CHANGELOG that contains a list of all changes you might need to take into account when testing the new alpha:[apollo-client/CHANGELOG.md at release-4.0 · apollographql/apollo-client](https://github.com/apollographql/apollo-client/blob/release-4.0/CHANGELOG.md)


The Apollo Client community continues to thrive, and we’re grateful for your support. Stay tuned for more updates, and get ready to take your applications to the next level with` @apollo/client` !


Written by


Lenz Weber-Tronic


[Read more by Lenz Weber-Tronic](https://www.apollographql.com/blog/author/lenz)
