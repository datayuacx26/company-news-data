---
schema_version: "1.0.0"
document_id: "57d93f9483fddc91e6c758639228cfc63f0850de808a63a7b92b6684ec4fb30b"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/end-to-end-tracing-for-playwright-tests"
published_at: "2024-01-12T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:e2295271863a8ee172ee3d1031bf0f2c16da6c793baa43fb65edab3919e65996"
---

# End-to-end tracing for Playwright tests

January 12th, 2024[Announcement](https://www.artillery.io/blog/tag/announcement)


# End-to-end tracing for Playwright tests


Ines Fazlic


Today we are announcing support for tracing in Playwright tests through the[OpenTelemetry reporter](https://www.artillery.io/docs/observability/opentelemetry) . End-to-end tracing provides a more granular insight into how each scenario is executed, making it easier to pinpoint potential performance bottlenecks or inefficiencies.


Integration with the OpenTelemetry reporter means you can visualize the traces on almost any[platform compatible](https://opentelemetry.io/ecosystem/vendors/) with OpenTelemetry or even process it further using an OTel collector.


## Outlier detection


Identifying outliers or their root cause can be challenging when relying only on aggregated data from Artillery reports. With tracing, every virtual user will result in an individual trace which holds every[test.step](https://www.artillery.io/docs/reference/engines/playwright#teststep-argument) call and page navigated within the scenario being executed. This allows you to zoom into specific virtual user (VU) actions to determine which step or page might be causing delays or inconsistencies.


## Better visualization in Fargate/Lambda


Visualize distributed tests more effectively, particularly when using services like Fargate, allowing for better analysis of individual scenario performance including error tracking.


## Web Vitals monitoring


[Web Vitals](https://web.dev/articles/vitals) are logged both as attributes and events, providing an exact timeline of when these occurred during the page interactions, along with their values and rating.


## Pinpointing errors


Errors can now be easier to detect and analyze as they’ll appear in the spans in two distinct ways:


- The span status will switch to` ERROR` , displaying the error message within the attributes.
- An exception will be recorded on the span, revealing not only the type of error but also when in the test it occurred. Additional details like stack traces are readily available, making troubleshooting more efficient.


## Example


Consider this user processor` flow` function:


```text
async   function   cloudWaitlistSignupFlow  (  page  ,   userContext  ,   events  ,   test  ) {
await   test.  step  (  'Go to Artillery'  ,   async   ()   =>   {
const   requestPromise   =   page.  waitForRequest  (  'https://www.artillery.io/'  );
await   page.  goto  (  'https://www.artillery.io/'  );
const   req   =   await   requestPromise;
});


await   test.  step  (  'Go to cloud'  ,   async   ()   =>   {
const   cloud   =   await   page
.  getByLabel  (  'Main navigation'  )
.  getByRole  (  'link'  , { name:   'Cloud'   });
await   cloud.  click  ();
await   page.  waitForURL  (  'https://www.artillery.io/cloud'  );
});


await   test.  step  (  'Click on Join button'  ,   async   ()   =>   {
await   page
.  getByRole  (  'button'  , {
name:   'Join Artillery Cloud early access waitlist'  ,
})
.  click  ();


await   page.  waitForURL  (  'https://www.artillery.io/cloud?tf=1'  );
});
}


module  .  exports   =   {
cloudWaitlistSignupFlow,
};
```


This would result in a scenario trace on the Honeycomb UI as shown below:


## Learn more


Find out more in the[docs](https://www.artillery.io/docs/observability/opentelemetry) .


OpenTelemetry support is experimental, and we are interested in hearing your feedback, or any issues you may encounter. Don’t hesitate to[reach out or ask a question](https://github.com/artilleryio/artillery/discussions/new/choose) whenever you have one.
