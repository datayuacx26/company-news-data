---
schema_version: "1.0.0"
document_id: "a518f1e10974565b94a6efe89a3344c366a3eb80407ea5b3a171f5da1941ba18"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-apollo-cache-persist-cb05aec16325"
published_at: "2017-12-12T22:48:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:9d78b76e8efa0436d2cf9d152635525663de06179c8d46953679318d06f66c24"
---

# Cache persistence for Apollo Client 2.0

*This is a guest post from Apollo contributor*[James Reggio](https://medium.com/u/373d9846244b?source=post_page-----cb05aec16325----------------------) *, co-founder of*[Banter](http://banter.fm/) *, a podcasting client built with Apollo and React Native.*


First impressions are important, but with mobile apps the second impression is sometimes what counts the most. From a user’s perspective, there’s nothing more frustrating than launching an app only to stare at a blank screen, waiting for data to load that’s already been fetched before. That’s why we’re introducing` <a href="https://github.com/apollographql/apollo-cache-persist" target="_blank" rel="noreferrer noopener">apollo-cache-persist</a>` , a library to seamlessly save and restore your Apollo cache from persistent storage.


:’(


Whether you’re persisting data with` AsyncStorage` or` localForage` ,` apollo-cache-persist` works with a[variety of storage providers](https://github.com/apollographql/apollo-cache-persist#storage-providers) for the web and React Native. It also supports any cache that works with Apollo Client 2.0, including the default` <a href="https://www.npmjs.com/package/apollo-cache-inmemory" target="_blank" rel="noreferrer noopener">InMemoryCache</a>` and` <a href="https://www.npmjs.com/package/apollo-cache-hermes" target="_blank" rel="noreferrer noopener">Hermes</a>` , a community-built alternative.


Best of all, you can get started with just a couple lines of code:


```text
import { InMemoryCache } from ‘apollo-cache-inmemory’;
import { persistCache } from ‘apollo-cache-persist’;


// Set up your cache.
const cache = new InMemoryCache({...});


// Set up cache persistence.
persistCache({
cache,
storage: window.localStorage,
});


// Finish configuring Apollo.
```


` persistCache` will immediately restore your cache from storage, and automatically persist it after every update (with a short, configurable, debounce interval).


If you need more control over the process, we’ve got you covered. You can instantiate a` <a href="https://github.com/apollographql/apollo-cache-persist#advanced-usage" target="_blank" rel="noreferrer noopener">CachePersistor</a>` , which offers methods to explicitly extract, restore, purge, and measure the cache, as well as methods to pause and resume the automatic persistence.


Have specific storage needs?` apollo-cache-persist` is compatible with any storage provider that works with` <a href="https://www.npmjs.com/package/redux-persist" target="_blank" rel="noreferrer noopener">redux-persist</a>` , so there’s already a[suite of well-tested packages](https://github.com/apollographql/apollo-cache-persist#storage-providers) at your disposal, including providers for compression and encryption.


## Automatic persistence


The default persistence timing — after every write to the cache — rarely loses data, and performs well in modern browsers. However, in resource-constrained environments like React Native, it’s less than ideal.


Your app’s UI typically needs to update in response to a write to the cache, so it’s not the best time to send potentially hundreds of kilobytes over the native bridge. (It takes around 600ms to send 100kb over the bridge to AsyncStorage on an iPhone 6, during which your app will not be responsive to touch.)


That’s why we’ve included a` background` persistence trigger for React Native, which will persist the cache as soon as your app leaves the foreground. This happens whenever the phone is locked, or whenever the home screen, multitasking view, or another app is activated — all good times for an expensive operation.


```text
import { InMemoryCache } from ‘apollo-cache-inmemory’;
import { persistCache } from ‘apollo-cache-persist’;
import { AsyncStorage } from ‘react-native’;


// Set up your cache.
const cache = new InMemoryCache({...});


// Set up cache persistence.
persistCache({
cache,
storage: AsyncStorage,
trigger: 'background',
});


// Finish configuring Apollo.
```


I’ve been using the` background` persistence trigger in my app,[Banter](https://banter.fm/) , for months without any issues. However, if the non-deterministic nature of these triggers worries you, you can always[write your own](https://github.com/apollographql/apollo-cache-persist#advanced-usage) by passing a function as the` trigger` option.


## Size management


The Apollo community is in the[early stages](https://github.com/apollographql/apollo-cache-persist/issues/2) of designing fine-grained cache controls, including the ability to utilize directives and metadata to control cache policy on a per-key basis. In the meantime, we’re limited to persisting and restoring the entire cache, which will cause it to grow over time.


In my own usage, I haven’t seen this become a performance problem, even with hundreds of kilobytes in the cache. However, it’s good practice to set a high upper bound on its growth, which you can do with the` maxSize` option. When your cache exceeds the size limit, persistence will pause and the cache will be reset upon the next launch of the app. I recommend starting with 500kb (` 512000` bytes) and tuning from there.


For further control, you can setup a background task to periodically reset the cache to contain only your app’s most important data. (On the web, you can use a service worker; on React Native, there’s[react-native-background-task](https://www.npmjs.com/package/react-native-background-task) .) The background task would start with an empty cache, query the most important data from your GraphQL API, and then persist. This strategy has the added benefit of ensuring the cache is loaded with fresh data when your app launches.


---


This is just the first piece of the puzzle to providing a robust offline experience. Stay tuned for future releases, and if you want to help shape the future of partially-connected GraphQL, join us on[Apollo Slack](https://www.apollographql.com/client/#slack) or tweet at me,[@jamesreggio](https://twitter.com/jamesreggio) .


Written by


James Reggio


[Read more by James Reggio](https://www.apollographql.com/blog/author/jamesreggio)
