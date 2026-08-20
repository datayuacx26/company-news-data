---
schema_version: "1.0.0"
document_id: "ae116d960b820d18376070edce6766baabee593c10180782550a2f9506b13e7f"
company_key: "yc-chonkie"
company: "Chonkie"
source_id: "yc-chonkie-atom-7829c83b5d35"
canonical_url: "https://github.com/feyninc/chonkie/releases/tag/v1.6.2"
published_at: "2026-04-07T01:21:39+00:00"
first_seen_at: "2026-07-24T22:18:14.501867+00:00"
fetched_at: "2026-08-20T03:16:58.267707+00:00"
content_hash: "sha256:0d4f41f64955e7e9d6eff9f982c4abfcfdcbcea09282587d75de4e261294da20"
---

# v1.6.2

## TeraflopAI Chunker


A new chunker has been added to your toolkit 🎉
you can now use the newly added[TeraflopAI](https://www.teraflopai.com/) chunker freely using the code below


```text
from    chonkie    import    TeraflopAIChunker


chunker    =    TeraflopAIChunker  ( api_key  =  "your_api_key_here"  )


text    =    "Your text here"
chunker  . chunk  ( text  )
```


## What's Changed


- Fixed per-chunk overlap calculation for float context_size by[@anaslimem](https://github.com/anaslimem) in[#512](https://github.com/feyninc/chonkie/pull/512)
- add teraflopai chunker by[@chonk-lain](https://github.com/chonk-lain) in[#539](https://github.com/feyninc/chonkie/pull/539)
- Validate tree-sitter language support in CodeChunker by[@chimchim89](https://github.com/chimchim89) in[#469](https://github.com/feyninc/chonkie/pull/469)
- chore: bump version by[@chonk-lain](https://github.com/chonk-lain) in[#541](https://github.com/feyninc/chonkie/pull/541)


## Dependencies


- chore(deps): bump requests from 2.32.5 to 2.33.0 by[@dependabot](https://github.com/dependabot) \[bot\] in[#532](https://github.com/feyninc/chonkie/pull/532)
- chore(deps): bump pygments from 2.19.2 to 2.20.0 by[@dependabot](https://github.com/dependabot) \[bot\] in[#535](https://github.com/feyninc/chonkie/pull/535)
- chore(deps): bump cryptography from 46.0.5 to 46.0.6 by[@dependabot](https://github.com/dependabot) \[bot\] in[#534](https://github.com/feyninc/chonkie/pull/534)
- chore(deps): bump langchain-core from 1.2.19 to 1.2.22 by[@dependabot](https://github.com/dependabot) \[bot\] in[#533](https://github.com/feyninc/chonkie/pull/533)
- chore(deps): bump aiohttp from 3.13.3 to 3.13.4 by[@dependabot](https://github.com/dependabot) \[bot\] in[#538](https://github.com/feyninc/chonkie/pull/538)
- chore(deps): bump litellm from 1.82.3 to 1.83.0 by[@dependabot](https://github.com/dependabot) \[bot\] in[#540](https://github.com/feyninc/chonkie/pull/540)


**Full Changelog** :[v1.6.1...v1.6.2](https://github.com/feyninc/chonkie/compare/v1.6.1...v1.6.2)
