---
schema_version: "1.0.0"
document_id: "274c51b4d508e322c357a300cdf372e3e8ade10bdd552c779be13c9e17597ec9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/enhanced-checks-and-observability-in-graphos"
published_at: "2024-07-16T09:19:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:264e3ae0663248e7bad261c2e324d70cfd380668179944f5e4bd74d7a9b89bd8"
---

# Enhanced Checks and Observability in GraphOS

Good API programs promote agility and can evolve APIs according to the needs of the business. While it’s important to roll out changes quickly, it cannot come at the expense of reliability. Apollo GraphOS provides workflows to evolve a supergraph safely, including schema checks and insights. These popular features enable API teams to test for breaking changes before rolling out schema updates, allowing them to deliver changes quickly without impacting reliability downstream. We’ve recently shipped significant reliability and usability improvements to schema checks and insights that benefit GraphOS customers. Users now benefit from improved reliability and usability related to input objects, enum types, union types, and interface types.


## Improved operation checks for interface and union types


Previously, any time a user removed an interface from a type, or a type was removed for a union, a schema check would fail for any operation that used that interface or union type, even if the specific type that was removed was not used in the operation. We now do more detailed checks to ensure that we only consider the change as breaking if the operation uses the specific type that was removed.


## Ignore potentially breaking changes and default value changes


In the past, when you ran an operation check against a variant with no traffic, we would fail the check if we saw any potentially breaking changes. Similarly, a change to a default value of an argument was always considered a breaking change for any operation that used the default. API platform teams now have some additional configuration available to override this default behavior, so that checks run against can be run against variants with no traffic without errors, and default value changes are not considered breaking. Like the other checks configuration, these new options can be enabled per-variant or per-graph.


## Improved usability of the operation checks page


The operation check page has been a bit difficult to use in the past, especially when a check is run against a lot of operations. To fix this, we’ve given the page a fresh coat of paint, and the checked operations are now shown in a paginated table that can be filtered by status or by operation name or ID. The details of the operation are still just a click away, and they have been given some more real estate to allow you to more easily see the operation details. We have also made some other usability improvements, such as always showing operations that have been checked even if they were previously marked as safe or ignored, as well as providing more details about what was changed in the schema.


## Enhanced signature reporting in Apollo Router


In older versions of Apollo Router, operation signatures that appeared in GraphOS had aliases removed and inline input objects replaced with a placeholder empty object. This meant that some operations could not be executed when they were opened in Explorer, and it also hid some useful information. Starting from v1.49.0, you can enable a new experimental config in your router:` telemetry.apollo.experimental_apollo_signature_normalization_algorithm: enhanced` which configures the router to send aliases and the shape of inline input objects to GraphOS. We still redact all values from the inline input objects, but now you should be able to run operations from the insights page more reliably.


## Enhanced checks and better insights on input object fields and enum values


Until now, GraphOS has not shown insights on any input object fields or enum values used in operations, and there have been some limitations on operation checks related to enum values and input object types. In Apollo Router v1.50.0 we have introduced a new configuration option for GraphOS Enterprise customers:` telemetry.apollo.experimental_apollo_metrics_reference_mode: enhanced` . With this option enabled, you will now see insights on all input object fields and enum values that are used in your operations. And with this new data, we are able to mark schema checks as passing if an unused enum value or input object field is removed, or the nullability or default value of an input object field is changed in a safe way.


We hope you can give all these new features a try soon, and if you have any feedback you’d like to provide on them, please let us know!


Written by


Nick Marsh


[Read more by Nick Marsh](https://www.apollographql.com/blog/author/nick-marsh)
