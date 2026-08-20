---
schema_version: "1.0.0"
document_id: "06c356cab1a79625d0a242d45cfb2dcfc4ac824a129a978586008525911f0b1e"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/artillery-vs-code-extension"
published_at: "2023-10-13T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:450c2a448ada5ff9b0ab3ba953ecd2b845153f1569f1cecdb254d72f92195419"
---

# Introducing the Artillery VS Code Extension

October 13th, 2023[Announcement](https://www.artillery.io/blog/tag/announcement)


# Introducing the Artillery VS Code Extension


Bernardo Guerreiro


Today, we’re announcing the launch of a VS Code extension for Artillery designed to make it easier to write test scripts with features like context-aware autocomplete and inline documentation.


You can install the extension from the[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=Artilleryio.vscode-artillery) , or directly from VS Code by searching for “Artillery” in the Extensions tab.


## Autocomplete


Press` Control+Space` to see context-aware autocomplete suggestions, providing you with all available options at any point in your test script. This speeds up the process of writing new tests and reduces the chances of errors.


## Inline documentation


Hover over any part or property of an Artillery test script to get short descriptions, examples, and links to the documentation to learn more.


## Run tests


The extension also supports executing your tests locally with` artillery run` . This feature is designed for debugging simple test runs, removing the need to toggle between your terminal and editor.


## Not a VS Code user?


Artillery for VS Code is powered by a[JSON schema](https://json-schema.org/) definition under the hood. Many editors and IDEs support using a JSON schema definition to provide autocomplete for YAML files, for example:


- [Webstorm](https://www.jetbrains.com/help/webstorm/json.html#ws_json_schema_add_custom)
- [Emacs](https://emacs-lsp.github.io/lsp-mode/page/lsp-yaml/)
- [Sublime](https://github.com/sublimelsp/LSP-yaml)


You can find the JSON schema at[https://www.artillery.io/schema.json](https://www.artillery.io/schema.json) .


## Feedback


We hope this extension makes your life easier. We’d love to hear your feedback on how we can improve it. Please open an issue on GitHub at[https://github.com/artilleryio/vscode-artillery/issues](https://github.com/artilleryio/vscode-artillery/issues)
