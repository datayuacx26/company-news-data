---
schema_version: "1.0.0"
document_id: "ead3bcd13f44218c364e88b884819e001774da7961a27cf4b23fa7abbd07393b"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/go-generics-writing-an-immutable-slice-in-go/"
published_at: "2023-12-05T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:241becc678e54ba9a463699474c16ed00c2b1d4be756b5189a34349c081aa2fa"
---

# Go Generics: Writing an Immutable Slice in Go

## Why would we want an Immutable Slice in the first place?


When getting started with Prequel, customers define schemas for data models they want to share. These model configurations contain a list of data columns, and the company guarantees preserving the order those columns are provided.


After fundamental changes to how model configs are tracked, the team needed to continue preserving column order. While using database query ordering combined with Go slices and unit tests could technically work, the engineering team wanted to “codify rules about the system” to protect against future mistakes.


The problem with Go slices is that they are mutable. They can be sorted, rearranged, appended to, and modified in various ways. The team asked: how can we prevent someone in the future from changing column order in the codebase? This prompted the creation of an immutable slice data structure.


## What are the benefits of an Immutable Slice?


This data structure provides all the read benefits of a slice, but removes any of the mutable functions so that we are unable to change anything about the slice once it is created.


## How do we write one?


Rather than writing separate versions for each type, we leveraged Go 1.18’s generics feature. A basic implementation includes:


The example makes liberal use of the` deepcopy` library to ensure that we sever any references to objects passed into or out of the data structure.


For iterating over items, an` Items` function can be added for use with the` range` keyword:


Additional functions like` MarshalJSON` ,` UnmarshalJSON` ,` Scan` , and` Hash` handle serialization and embedding use cases.


By embedding` ImmutableSlice\[Column\]` into structs, the team protects all developers from accidentally breaking column order preservation, a core feature customers depend on.


A full implementation with example usage is available in a linked Gist.
