---
schema_version: "1.0.0"
document_id: "76cffa639b76bbe2303ad6edd3cb6799e87735913a076ced3901e54776a15875"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-03-09"
published_at: "2026-03-09T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:f2918d8931436b137d47a502bac673b4d9415add1aa1027843c40e9a48c9630e"
---

# Convert more file formats to Deepnote projects and vice versa

## [March 9, 2026](https://deepnote.com/changelog/2026-03-09)


###


[Interoperability with more notebook formats](https://deepnote.com/changelog/2026-03-09#interoperability-with-more-notebook-formats)


###


[Convert more file formats to Deepnote projects and vice versa](https://deepnote.com/changelog/2026-03-09#convert-more-file-formats-to-deepnote-projects-and-vice-versa)


The Deepnote convert CLI previously supported **bi-directional conversion** between **Deepnote projects and Jupyter notebooks** . Based on popular demand, we shipped[bidirectional,](https://github.com/deepnote/deepnote/tree/main/packages/convert)[lossless notebook conversions](https://github.com/deepnote/deepnote/tree/main/packages/convert) between Deepnote, Jupyter, Quarto, Percent, and Marimo.


We’ve also included a smarter CLI with automatic format detection and a new` -- format` flag to make the work more seamless. This helps increase interoperability across projects and reduce lock-in, in the true spirit of open-source!


That means you can move work both ways:


- Bring your previous projects back into Deepnote without losing momentum, and enjoy the extended functionality Deepnote offers!
- Convert a Deepnote project into any of the above notebook formats when you need to share, archive, or run elsewhere


You can now also **export projects and notebooks directly from the VS Code extension** .


We will continue working on .deepnote format portability to better integrate it with the ecosystem - let us know if you have any suggestions!
