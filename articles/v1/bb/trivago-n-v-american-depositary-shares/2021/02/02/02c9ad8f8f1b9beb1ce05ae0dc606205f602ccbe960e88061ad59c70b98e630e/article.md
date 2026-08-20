---
schema_version: "1.0.0"
document_id: "02c9ad8f8f1b9beb1ce05ae0dc606205f602ccbe960e88061ad59c70b98e630e"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2021-02-24-reactiveprogrammingthepriceyouhavetopayf/"
published_at: "2021-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:006c661038e4d4507571d5bac28a6539b167331edf638da576e8936399fee7ad"
---

# Reactive Programming - The Price You Have To Pay For A Responsive Backend

In the trivago backend, we use the[reactive programming pattern](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) for fetching prices from advertisers and updating our caches. This helps us to increase the responsiveness (i.e., scalability and resilience) of our backend. Thus, our backend system can alleviate high response times from internal components and our advertisers while staying responsive, even if downstream components fail entirely. Here is how we use the Java library[Reactor Core](https://projectreactor.io/) to ensure those guarantees:


First, let’s have a look at what our backend architecture for fetching prices looks like. The trivago backend consists of multiple[microservices](https://martinfowler.com/articles/microservices.html) and two caches with different purposes. The mid-term cache stores prices from our advertisers for multiple hours - depending on the advertiser’s update policy - in order to reduce downstream load on our advertisers. The short-term cache stores further refined prices for similar searches to improve response times and reduce load on backend components.


Recently, we moved a component responsible for selecting the best prices for our users, called Auction Service, from post processing into the reactive price fetching process (see blue arrow in the diagram above). The Auction service handles prices in batches and by migrating this component, we can optimize the utilization of the short-term cache to further reduce load on other components and refine our price selection algorithm by covering more prices. One of the most challenging parts of the migration was that given batches need to be processed and returned in the order of arrival to ensure that we store the latest prices for every accommodation in the short-term cache. We solved that problem by fully integrating the Auction Service into the reactive pipeline establishing bidirectional streams/[fluxes](https://projectreactor.io/docs/core/release/reference/#flux) between the Price Processor and Auction Service for every search. A flux guarantees a sequential order of events (if not transformed into a parallel flux). However, by sending a flux of requests and responses, one cannot access the request and the corresponding response within one operation.


The reactive programming pattern requires chaining of[operators](https://projectreactor.io/docs/core/release/reference/#_operators) and every operator should avoid side-effects. This means the operator should not access the data of another operator. Yet, we need to access the prices from the request in order to enrich the response. One solution to that problem would be to provide all request data in the response, but this would increase network bandwidth and we want to avoid that the implementation defines the API. We solved that issue by converting the flux of requests into a[hot publisher](https://projectreactor.io/docs/core/release/reference/#reactor.hotCold) and connecting to it twice. One flux is sent to the Auction service, the other one is zipped together with the responses from the Auction service. This provides a POJO (plain old java object) with request and response data (see diagram above). The replay operator ensures that elements emitted before connecting to the flux are replayed.


```text
private   Flux  <  AuctionResult  >   callAuctionAndZipRequestsWithResponses  (
final   Flux  <  AuctionRequest  >   auctionRequestFlux) {
final   var   hotSource  :   auctionRequestFlux.  replay  ().  autoConnect  (  2  );
return   Flux.  zip  (  fireRequests  (hotSource), hotSource, AuctionResult  ::new  );
}
```


This was just a brief glimpse into the reactive programming pattern and how it can solve many problems regarding reliability and scalability, but also confront you with problems you didn’t even think about before. Integrating reactive programming into your architecture requires a mindset shift and on-boarding time, which are real cost factors for any business. But, used carefully it has the benefit of keeping your components decoupled.
