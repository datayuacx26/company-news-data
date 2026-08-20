---
schema_version: "1.0.0"
document_id: "3167744a8233766285091e8dcbcfd08f5af01ebedc22c492fbf8b7d4d6c35bac"
company_key: "yc-chonkie"
company: "Chonkie"
source_id: "yc-chonkie-atom-7829c83b5d35"
canonical_url: "https://github.com/feyninc/chonkie/releases/tag/v1.6.7"
published_at: "2026-05-19T04:50:43+00:00"
first_seen_at: "2026-07-24T22:18:14.501867+00:00"
fetched_at: "2026-08-20T03:16:58.267707+00:00"
content_hash: "sha256:148719494e7503eff3af400f0f09f7c4949ddcc0bd5b6c4d8b591b79e1e0996b"
---

# v1.6.7

# Chonkie v1.6.7 ✨


## MistralOCR support


Extract text from images with the new MistralOCR integration:


```text
from    chonkie    import    MistralOCR
ocr    =    MistralOCR  ()
out    =    ocr  ( "image.jpg"  )
print  ( out  . content  )
```


## What's Changed


- chore(deps): bump langsmith from 0.7.31 to 0.8.0 by[@dependabot](https://github.com/dependabot) \[bot\] in[#590](https://github.com/feyninc/chonkie/pull/590)
- Fix: initial CodeChunker languages downloaded by[@chonk-lain](https://github.com/chonk-lain) in[#591](https://github.com/feyninc/chonkie/pull/591)
- Add justified method for the overlap option by[@anaslimem](https://github.com/anaslimem) in[#562](https://github.com/feyninc/chonkie/pull/562)
- feat: add MistralOCR by[@chonk-lain](https://github.com/chonk-lain) in[#593](https://github.com/feyninc/chonkie/pull/593)
- chore: bump version by[@chonk-lain](https://github.com/chonk-lain) in[#594](https://github.com/feyninc/chonkie/pull/594)


**Full Changelog** :[v1.6.6...v1.6.7](https://github.com/feyninc/chonkie/compare/v1.6.6...v1.6.7)
