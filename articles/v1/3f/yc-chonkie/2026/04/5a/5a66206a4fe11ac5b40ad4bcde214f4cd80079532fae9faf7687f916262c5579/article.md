---
schema_version: "1.0.0"
document_id: "5a66206a4fe11ac5b40ad4bcde214f4cd80079532fae9faf7687f916262c5579"
company_key: "yc-chonkie"
company: "Chonkie"
source_id: "yc-chonkie-atom-7829c83b5d35"
canonical_url: "https://github.com/feyninc/chonkie/releases/tag/v1.6.3"
published_at: "2026-04-21T20:48:33+00:00"
first_seen_at: "2026-07-24T22:18:14.501867+00:00"
fetched_at: "2026-08-20T03:16:58.267707+00:00"
content_hash: "sha256:02b40240ede5ec50fddeb1dd0c5873f956355de563caf78bfc0047dfbf5caf12"
---

# v1.6.3

# 🚀 Chonkie v1.6.3


Caution


**Known Bug:**` import chonkie` fails with` ModuleNotFoundError: No module named 'pandas'` when installed without the` \[table\]` extra. This is caused by an unconditional top-level pandas import in` utils/table_converter.py` . **Please upgrade to v1.6.4** which fixes this issue.


## ✨ Features


- **LanceDB Handshake** : Introduced a new handshake mechanism for LanceDB integration by[@chonk-lain](https://github.com/chonk-lain) in[#546](https://github.com/feyninc/chonkie/pull/546)
- **Metadata Enhancements** : Added` filename` to metadata for better traceability by[@chonk-lain](https://github.com/chonk-lain) in[#554](https://github.com/feyninc/chonkie/pull/554)
- **Markdown Support Improvements** : Added` MarkdownDocument` support for` CodeChunker` and fixed no-op behavior in` TableChunker` by[@chonknick](https://github.com/chonknick) in[#563](https://github.com/feyninc/chonkie/pull/563)
- **Table Utilities** : Added a table-to-JSON converter by[@anaslimem](https://github.com/anaslimem) in[#531](https://github.com/feyninc/chonkie/pull/531)


## 🧠 Improvements


- **Chunking Consistency** : Deduplicated delimiter-based text splitting across chunkers by[@anaslimem](https://github.com/anaslimem) in[#510](https://github.com/feyninc/chonkie/pull/510)
- **Model Loading Robustness** : Improved error handling for neural model and tokenizer loading by[@chimchim89](https://github.com/chimchim89) in[#472](https://github.com/feyninc/chonkie/pull/472)
- **Refactor Handshake IDs** : Moved` _generate_default_id` into` BaseHandshake` by[@chimchim89](https://github.com/chimchim89) in[#455](https://github.com/feyninc/chonkie/pull/455)


## 🐛 Fixes


- **CJK Delimiter Handling** : Fixed handling of single-character delimiters in` RecursiveChunker._split_text` by[@nightcityblade](https://github.com/nightcityblade) in[#537](https://github.com/feyninc/chonkie/pull/537)


## 📚 Documentation


- **JavaScript Docs** : Added JavaScript documentation by[@chonk-lain](https://github.com/chonk-lain) in[#545](https://github.com/feyninc/chonkie/pull/545)
- **Semantic Chunker Examples** : Fixed embedding examples by[@narumiruna](https://github.com/narumiruna) in[#544](https://github.com/feyninc/chonkie/pull/544)
- **README Cleanup** : Removed outdated full API documentation link by[@narumiruna](https://github.com/narumiruna) in[#543](https://github.com/feyninc/chonkie/pull/543)
- **General Docs Updates** : Refactored and improved documentation by[@chonk-lain](https://github.com/chonk-lain) in[#542](https://github.com/feyninc/chonkie/pull/542) and[#557](https://github.com/feyninc/chonkie/pull/557)
- **Contribution Guidelines** : Added PR checklist to` CONTRIBUTING.md` by[@swamy18](https://github.com/swamy18) in[#465](https://github.com/feyninc/chonkie/pull/465)


## 🔧 Maintenance & Dependencies


- **Test Coverage** : Improved test coverage by[@chonk-lain](https://github.com/chonk-lain) in[#555](https://github.com/feyninc/chonkie/pull/555)
- **Version Bump** : Bumped library version by[@chonk-lain](https://github.com/chonk-lain) in[#564](https://github.com/feyninc/chonkie/pull/564)


## 🙌 New Contributors


- [@narumiruna](https://github.com/narumiruna) made their first contribution in[#544](https://github.com/feyninc/chonkie/pull/544)
- [@nightcityblade](https://github.com/nightcityblade) made their first contribution in[#537](https://github.com/feyninc/chonkie/pull/537)
- [@swamy18](https://github.com/swamy18) made their first contribution in[#465](https://github.com/feyninc/chonkie/pull/465)


**Full Changelog** :[v1.6.2...v1.6.3](https://github.com/feyninc/chonkie/compare/v1.6.2...v1.6.3)
