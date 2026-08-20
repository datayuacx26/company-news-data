---
schema_version: "1.0.0"
document_id: "ea3773c1ae1722d5bc32555cff820b602859b8d3a11df4d04407e77de886ecf4"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/additional-authentication-method-types"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:16070d6deb4d874a3a9173d6a21c71d904353e832883c3cf1a7961a1abe5ef62"
---

# Additional Authentication Method types

[Back to All](https://docs.castle.io/changelog)


Added


We’ve expanded` authentication_method.type` with two new options:


- ` $pop` (Proof-of-Personhood): use when verifying a user through PoP providers.


- Example variants:` worldid` ,` brightid` ,` gitcoin_passport` ,` sismo` ,` civic` ,` idena` , etc


- ` $payment` : use when authentication is tied to a payment method or provider.


- Example variants:` stripe` ,` plaid` ,` ach` .


These additions make it easier to describe proof-of-personhood and payment-based flows.


Example:


```text
{
"authentication_method": {
"type": "$pop",
"variant": "worldid"
}
}


```


Be sure to check out the full[Authentication Method documentation](https://docs.castle.io/update/docs/authentication-method#/) for details.
