---
schema_version: "1.0.0"
document_id: "25c394e826fcb75938ed00895523956ab6725b8aed23185cbe108fb31fa60cb7"
company_key: "yc-chonkie"
company: "Chonkie"
source_id: "yc-chonkie-atom-7829c83b5d35"
canonical_url: "https://github.com/feyninc/chonkie/releases/tag/v1.6.4"
published_at: "2026-04-21T20:50:00+00:00"
first_seen_at: "2026-07-24T22:18:14.501867+00:00"
fetched_at: "2026-08-20T03:16:58.267707+00:00"
content_hash: "sha256:6d96a830a308a6e3ff064514d1b3a576a1da84e89435e7621af628548a6fc91c"
---

# v1.6.4

# 🚀 Chonkie v1.6.4


## 🐛 Fixes


- **Pandas Import Fix** : Fixed` import chonkie` failing with` ModuleNotFoundError: No module named 'pandas'` when installed without the` \[table\]` extra. The pandas import in` utils/table_converter.py` is now lazy-loaded, matching the existing pattern for optional dependencies. By[@Pringled](https://github.com/Pringled) in[#566](https://github.com/feyninc/chonkie/pull/566)


Tip


If you encountered` ModuleNotFoundError: No module named 'pandas'` on v1.6.3, upgrading to v1.6.4 resolves the issue:


```text
pip install --upgrade chonkie
```


**Full Changelog** :[v1.6.3...v1.6.4](https://github.com/feyninc/chonkie/compare/v1.6.3...v1.6.4)
