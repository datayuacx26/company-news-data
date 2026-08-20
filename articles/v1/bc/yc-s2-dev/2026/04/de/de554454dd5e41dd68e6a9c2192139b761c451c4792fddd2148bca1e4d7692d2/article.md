---
schema_version: "1.0.0"
document_id: "de554454dd5e41dd68e6a9c2192139b761c451c4792fddd2148bca1e4d7692d2"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/encryption"
published_at: "2026-04-25T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:8a83ca3d28ade4b6d81b96c6e3347bfd68fb79609b10b1f4b48a50975febf02b"
---

# Your data, your keys

S2 now supports **client-supplied encryption keys** : you hand S2 a key per request, S2 uses it to encrypt or decrypt records in memory, and then forgets it. The keys are never persisted, never logged, and zeroed when the request completes. Without the key, the stored records are unrecoverable — even by us.


This is a good fit for streams carrying sensitive tenant data, audit logs, agent transcripts, or regulated records such as protected health information1 . Your application supplies keys from your own infrastructure, while S2 handles request-time encryption behind the durable stream API.


## Where this fits


There are three places encryption operations can happen — in your application, in S2 with keys you supply per request, or in S2 with keys governed by a cloud key management system (KMS).


**Client-side record encryption** is the right model when your hard requirement is that S2 never sees plaintext. You encrypt each record before append and decrypt after read. This gives you end-to-end control, but every producer and consumer must handle encryption, decryption, and encoding correctly. You also lose compression on the wire, since encrypted bytes don't compress.


**KMS-integrated encryption (CMEK)** gives you key governance through your cloud provider's KMS, IAM policies, and audit logs you already manage. However, it is less portable, since you are limited to that provider's KMS and the services that integrate with it.


**Client-supplied encryption keys (CSEK)** are the middle ground, in the same family as[Google Cloud CSEK](https://cloud.google.com/storage/docs/encryption/customer-supplied-keys) and[Amazon S3 SSE-C](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerSideEncryptionCustomerKeys.html) . Encryption and decryption happen inside S2 at request time, so producers and consumers stay simple. Client↔S2 compression still works since it runs before encryption on writes and after decryption on reads. You decide where keys live and which workloads can use them, independent of any specific KMS.


The tradeoff is S2 sees plaintext and key material in memory while serving. If your compliance posture requires that the service operator never have access to plaintext, use client-side encryption instead.


## How it works


Your browser does not support the video tag.


[Basins](https://s2.dev/docs/concepts/basins) can now be configured with an encryption algorithm2 . Every new stream created in that basin then requires an encryption key to append or read[records](https://s2.dev/docs/concepts/records) .


Keys are sent as base64-encoded key material in the` s2-encryption-key` request header over TLS. The[CLI](https://s2.dev/docs/cli) and all[SDKs](https://s2.dev/docs/sdk) support specifying it easily at the stream level.


In the[s2.dev data plane](https://s2.dev/docs/platform/architecture) , writes are encrypted at the edge service before being forwarded downstream. On reads, encrypted records are decrypted only after they return to the edge service, after any caching and coalescing.


The core encryption path lives in the[open source s2 repo](https://github.com/s2-streamstore/s2) , so you can verify how keys are handled.[s2-lite](https://s2.dev/docs/s2-lite) , the self-hostable form factor of S2, supports the same mechanism.


There is no meaningful impact to performance, as the supported ciphers are designed for high throughput and are hardware-accelerated in practice.


*For operational guidance on key management, including envelope encryption and rotation, see the[encryption docs](https://s2.dev/docs/concepts/encryption#key-management) .*


## Try it


Let's configure a basin to encrypt new streams:


```text
$   s2   create-basin   logs-prod   \
--create-stream-on-append   \
--stream-cipher   aegis-256
```


You can also reconfigure an existing basin – only new streams are affected:


```text
$   s2   reconfigure-basin   logs-prod   --stream-cipher   aegis-256
```


Now, when a new stream is created in this basin, it will use the configured cipher. An encryption key must be provided with all data plane requests that read or write records.


For the CLI, generate a key and pass it explicitly, or use the` S2_ENCRYPTION_KEY` environment variable.


```text
$   export   S2_ENCRYPTION_KEY="$(  openssl   rand   -base64   32  )"


$   printf   'hello from an encrypted stream\n'   |   \
s2   append   s2://logs-prod/app/node-foo


$   s2   read   s2://logs-prod/app/node-foo   --seq-num   0   --count   1
hello   from   an   encrypted   stream
```


With the TypeScript SDK, the key is scoped to the stream handle:


```text
import   { AppendInput, AppendRecord, S2 }   from   '@s2-dev/streamstore'  ;


const   s2   =   new   S2  ({ accessToken: process.env.  S2_ACCESS_TOKEN  !   });
const   stream   =   s2.  basin  (  'logs-prod'  ).  stream  (  'app/node-foo'  , {
encryptionKey: process.env.  S2_ENCRYPTION_KEY  !  ,
});


await   stream.  append  (
AppendInput.  create  ([AppendRecord.  string  ({ body:   'hello'   })])
);
```


Client-supplied encryption keys are available today on the s2.dev[cloud service](https://s2.dev/dashboard) as well as[s2-lite](https://s2.dev/docs/s2-lite) , at no additional cost.


## Footnotes


1.


Contact us for a HIPAA Business Associate Agreement (BAA).↩


2.


We recommend` aegis-256` in general;` aes-256-gcm` is available for organizations that standardize on AES-GCM.↩
