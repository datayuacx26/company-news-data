---
schema_version: "1.0.0"
document_id: "0f68d004820cb4d3d1e50de758df2d59ba821afbf80e8987ab7870b43b2af4c6"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-sensitive-data-redaction"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T16:05:29.649529+00:00"
fetched_at: "2026-08-10T16:05:31.214856+00:00"
content_hash: "sha256:ea6832e46ac45d08a7f1dc83bc74c50b5c88a93234cacbd386c3a12187b3ca4a"
---

# Introducing Sensitive Data Redaction for Mastra Observability

With Mastra's new[SensitiveDataFilter](https://mastra.ai/reference/observability/tracing/processors/sensitive-data-filter) , you can automatically redact customer information from agent observability traces, or configure rules per-environment for application-specific fields.


The` SensitiveDataFilter` is enabled by default and ships with 15[default value fields](https://mastra.ai/reference/observability/tracing/processors/sensitive-data-filter#default-sensitive-fields) — including` password` ,` ssn` , and` auth` .


You can customize which fields to redact with the` sensitiveFields` array.[Field matching](https://mastra.ai/reference/observability/tracing/processors/sensitive-data-filter#field-matching) normalizes case and separators, for example;` api-key` ,` api_key` , and` ApiKey` would all match. You can also configure redaction styles, change the replacement token, and set different rules per-environment. E.g.` partial` for development, and` full` for production.


Your browser does not support the video tag.


Before the` SensitiveDataFilter` , keeping secrets and PII out of your agent traces meant writing a custom span processor to sanitize tool inputs. Now the` SensitiveDataFilter` is enabled by default, catching and redacting sensitive field names automatically.


` SensitiveDataFilter` is a[SpanOutputProcessor](https://mastra.ai/reference/observability/tracing/interfaces#spanoutputprocessor) that recursively walks each span's` attributes` ,` metadata` ,` input` ,` output` , and` errorInfo` — including nested objects, arrays, and JSON-encoded strings. To disable the functionality, set` sensitiveDataFilter: false` .


## Get started


Install` @mastra/observability` :


Terminal


```text
npm   install   @mastra/observability
```


note


Requires` @mastra/observability@1.12.0` or later, added in[PR #16234](https://github.com/mastra-ai/mastra/pull/16234) .


## Default config


A default` Observability` config automatically redacts the[default values](https://mastra.ai/reference/observability/tracing/processors/sensitive-data-filter#default-sensitive-fields) and replaces sensitive data with` \[REDACTED\]` . Additional configuration may be required to ensure all customer-specific sensitive data is redacted.


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   MastraStorageExporter,   Observability   }   from   "  @mastra/observability  "  ;


export   const   mastra   =   new   Mastra  ({
// ...
observability:   new   Observability  ({
configs: {
default: {
serviceName:   "  mastra-dev  "  ,
exporters: [  new   MastraStorageExporter  ()]
}
}
})
});
```


Example output from default config:


```text
{
"  email  "  :   "  paul@example.com  "  ,
"  phone  "  :   "  +44-20-7946-0142  "  ,
"  creditCard  "  :   "  4111 1111 1111 1111  "  ,
"  ssn  "  :   "  [REDACTED]  "  ,
"  apiKey  "  :   "  [REDACTED]  "  ,
"  notes  "  :   "  Priority customer since 2024. Prefers email contact.  "
}
```


## Extended config


Define your own fields using the` sensitiveFields` array — this overrides the defaults, set the` redactionStyle` to` full` , and add a` redactionToken` . Use the` configSelector` to configure different rules per environment:` NODE_ENV=development` is the default,` NODE_ENV=production` selects production.


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   MastraStorageExporter,   Observability,   SensitiveDataFilter   }   from   "  @mastra/observability  "  ;


export   const   mastra   =   new   Mastra  ({
// ...
observability:   new   Observability  ({
configSelector  : ()   =>   process.env.NODE_ENV,
configs: {
default: {
serviceName:   "  mastra-dev  "  ,
exporters: [  new   MastraStorageExporter  ()]
},
production: {
serviceName:   "  mastra-prod  "  ,
exporters: [  new   MastraStorageExporter  ()],
spanOutputProcessors: [
new   SensitiveDataFilter  ({
sensitiveFields: [  "  email  "  ,   "  phone  "  ,   "  creditCard  "  ,   "  ssn  "  ,   "  apikey  "  ],
redactionStyle:   "  full  "  ,
redactionToken:   "  *  "
})
]
}
}
})
});
```


Example output from extended config:


```text
{
"  email  "  :   "  *  "  ,
"  phone  "  :   "  *  "  ,
"  creditCard  "  :   "  *  "  ,
"  ssn  "  :   "  *  "  ,
"  apiKey  "  :   "  *  "  ,
"  notes  "  :   "  Priority customer since 2024. Prefers email contact.  "
}
```


For more information and full configuration options, see:


- [Observability overview](https://mastra.ai/docs/observability/overview)
- [SensitiveDataFilter reference](https://mastra.ai/reference/observability/tracing/processors/sensitive-data-filter)
- [SpanOutputProcessor](https://mastra.ai/reference/observability/tracing/interfaces#spanoutputprocessor)
- [Sensitive data filter guide](https://mastra.ai/docs/observability/integrations/processors/sensitive-data-filter)
