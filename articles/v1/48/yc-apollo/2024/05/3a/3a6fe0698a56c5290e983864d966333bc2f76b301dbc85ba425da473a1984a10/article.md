---
schema_version: "1.0.0"
document_id: "3a6fe0698a56c5290e983864d966333bc2f76b301dbc85ba425da473a1984a10"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/optimizing-data-fetching-with-apollo-client-leveraging-usefragment-and-colocated-fragments"
published_at: "2024-05-10T10:50:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:e049634ff2dce495cba555b8113057989ef3a003780fa90e2390953de7ae7ab9"
---

# Optimizing Data Fetching with Apollo Client: Leveraging useFragment and Colocated Fragments

When building complex applications today, Apollo Client users typically reach for` useQuery` to get data to their components. This is no surprise as this is considered a[best practice](https://www.apollographql.com/docs/react/data/operation-best-practices#query-only-the-data-you-need-where-you-need-it) by the Apollo Client documentation. However, this can cause challenges while trying to minimize over-fetching while maintaining a clear separation of concern.


During my time at[RedShelf](https://redshelf.com/) , a leader in the Higher EdTech space, I’ve been an advocate for Apollo’s best practices including “query only the data you need, where you need it.” However, this led to over-fetching because we naturally pushed our queries (a.k.a. network requests) as far down the component tree as possible, in most cases usually to the individual components.


Enter the power of the[useFragment](https://www.apollographql.com/docs/react/data/fragments/#usefragment) hook and[colocated fragments](https://www.apollographql.com/docs/react/data/fragments#colocating-fragments) . Combining these two techniques have enabled us to separate accountability from responsibility, remaining aligned with Apollo’s best practice while optimizing our client application’s fetching strategies along the way.


## Exploring Colocated Fragments


Colocated fragments are not a new concept and, in fact, they[are one of the distinguishing features of Relay](https://relay.dev/docs/tutorial/fragments-1/) . Essentially, colocated fragments move the responsibility of declaring data needs from parents to children further decoupling the two. Although a step in the right direction, they still require parent components to provide fragmented data through React props, or components that have defined their own data needs to manually access the Apollo Client cache.


However, with the introduction of the` useFragment` hook, we can now leverage the benefits of colocated fragments with the simplicity of accessing the fragmented data via a streamlined hook.


## Understanding Apollo’s` useFragment` Hook


A clear separation between accountability and responsibility in data fetching is important. While components should be responsible for declaring the data they need, they shouldn’t necessarily be accountable for fetching it. This is where the` useFragment` hook shines.


This article mainly focuses on mitigating over-fetching and improving code maintainability, but the` useFragment` hook provides other performance gains, too, like handling re-renders more efficiently. Read more about it in[this Apollo blog post](https://www.apollographql.com/blog/introducing-apollo-clients-nonreactive-directive-and-usefragment-hook) .


Introduced in[Apollo Client 3.7](https://github.com/apollographql/apollo-client/releases/tag/v3.7.0) and stabilized in[3.8](https://github.com/apollographql/apollo-client/releases/tag/v3.8.0) , the` useFragment` hook enables components to directly access fragment data from the normalized cache.


This not only helped our team stay true to Apollo’s best practice of querying only necessary data, but also helped solve some of the over-fetching we were doing by following the best practice in the first place.


## Integration into the Team’s Applications


The first place we leveraged colocated fragments and the` useFragment` hook was to refactor a “Product Details” drawer within one of our applications where over-fetching had become a major problem. When the drawer mounted, it sent 9 network requests(!) – one for each component – because they were responsible for declaring AND accountable for fetching their data.


By associating colocated fragments and replacing` useQuery` with` useFragment` in every component, we were able to reduce the network requests count from 9 to 2 within the drawer. The only thing that kept us from only needing a single network request was some of the data required a network-only fetch policy.


Since then, we’ve propagated this pattern throughout our client applications, reducing over-fetching without compromising data requirements. This technique has been truly transformative for our team, improving the performance of our applications and enhancing the maintainability of our codebases.


## Lessons Learned


While challenges are inevitable when adopting new patterns, we couldn’t have made the progress we did in the timeframe we did without the help and support from the[Apollo Supergraph Champions community](https://events097286.typeform.com/joincommunity) .


For example, while replacing the` useQuery` hooks in each component with the` useFragment` hook, we weren’t receiving the fragmented data we were expecting. After trying a few approaches, we reached out to the Apollo community channel and had a great conversation with[Adam Luikart](https://www.linkedin.com/in/adamluikart/) , a Principal Software Engineer at Indeed, who helped us realize we needed to pass the same exact variables we passed to` useQuery` to the` useFragment` hook or it won’t have a valid way to locate the data in the cache.


Thanks to that conversation, our team gained a deeper understanding of the inner workings of the Apollo Client cache and a working example that led to the eventual buy-in and integration of colocated fragments and the` useFragment` hook into our pattern library.


## Takeaways


Apollo’s maxim of “query only the data you need, where you need it” is a great starting point. But certain component structures can result in more network calls than is needed. When we used the new` useFragment` hook in Apollo Client, though, we were able to stay true to the principle behind that best practice while reducing the number of fetches in our app.


By embracing this pattern, our team effectively separated the responsibility and accountability for data requirements. This helped us control the total demand on our network and enhance the end user experience without compromising our architecture.


Written by


Tyler Goelz


[Read more by Tyler Goelz](https://www.apollographql.com/blog/author/tyler)
