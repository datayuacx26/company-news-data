---
schema_version: "1.0.0"
document_id: "60f03b4e5d6f8b50f7e50197b51965f610908c71d60ab4d36b00021cec46df47"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/launching-2-new-developer-tools-lsp-and-vs-code-extension"
published_at: "2024-06-26T11:53:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:7c65a91b6860e30dcdb878c1afde0e3307d2c03b0b84f248d571b8adba9fb11b"
---

# Launching 2 New Developer Tools: LSP and VS Code Extension

Announcing 2 new developer tools for SpiceDB: a[Language Server Protocol](https://microsoft.github.io/language-server-protocol/) implementation within SpiceDB and a[new extension for Visual Studio Code](https://github.com/authzed/spicedb-vscode) that provides the capabilities of Playground!


Earlier this year, we[open sourced](https://authzed.com/blog/spicedb-playground-is-open-source) the[SpiceDB Playground](https://play.authzed.com/) , the tool AuthZed built for easy development and testing of[SpiceDB Schema](https://authzed.com/docs/spicedb/concepts/schema) . The Playground provides not only syntax highlighting and formatting, but also validation and testing tooling, to streamline development of ReBAC-style authorization systems.


Since launching Playground a few years ago, we’ve had a number of users ask for capabilities similar to the Playground, but natively in their own IDEs (integrated development environments) or other development environments. Recognizing that developers often have their own preferred environment, today we are quite happy to announce two new capabilities for developing schema for SpiceDB.


## Language Server Protocol


The first new capability we’re announcing is support for the Language Server Protocol in SpiceDB. The[Language Server Protocol](https://microsoft.github.io/language-server-protocol/) is a shared protocol for IDEs and other developer tooling that allows tools to request semantic information about a piece of code.


In the case of SpiceDB’s new LSP implementation, the standard language server protocol API can be used to request information about the SpiceDB schema provided.


The language server protocol in SpiceDB can be run locally via the` lsp` command:


1


2


3


4


```text
$ spicedb lsp
INF configured logging async=false format=auto log_level=info provider=zerolog
INF starting LSP server addr=-
INF listening for LSP connections on stdin


```


1


2


3


4


```text
$ spicedb lsp
INF configured logging async=false format=auto log_level=info provider=zerolog
INF starting LSP server addr=-
INF listening for LSP connections on stdin


```


## Visual Studio Code (VSCode) Extension


[Visual Studio Code](https://code.visualstudio.com/) is a full-featured[open source IDE](https://github.com/microsoft/vscode) originally created by Microsoft. One of its most powerful and important features is the ability to extend the functionality of the core IDE by writing and publishing[VS Code Extension](https://marketplace.visualstudio.com/vscode) to the extension marketplace.


The[SpiceDB VSCode Extension](https://marketplace.visualstudio.com/items?itemName=authzed.spicedb-vscode) provides most of the features found currently in the SpiceDB Playground, but within VS Code itself. The extension can be installed from the marketplace, the GitHub repository or from within VS Code itself by going to the extension manager **⇧⌘X** and searching for “SpiceDB”.


Here are some of the features currently available in the VS Code extension:


### Syntax and Semantic Highlighting for SpiceDB Schema


The SpiceDB VSCode extension provides both syntax and **semantic** highlighting of SpiceDB schemas.


The semantic highlighting includes marking the use of` relation` s and` permission` s in different colors:


### Real time validation and diagnostics


The VS Code extension provides real time feedback about both errors and warnings that are found in the schema, as it is being edited:


### Automatic formatting for SpiceDB Schema


The VS Code extension provides automatic formatting-on-save:


### Built-in check watch


One of the most important features of the[SpiceDB Playground](https://play.authzed.com/) is the ability to watch permissions and see how their computation changes as schema and test relationships are modified.


To support this feature in the VS Code extension, the AuthZed team constructed a[custom panel](https://code.visualstudio.com/api/ux-guidelines/panel) to host a copy of SpiceDB within VSCode and provide check watches via actual SpiceDB API calls:


In a future blog post, we’ll discuss in technical detail how we built the check watch panel.


To use the check watch panel, a file containing relationships yaml named` {name}.zed.yaml` is placed in the same directory as the` {name}.zed` schema file being checked. The relationships+schema are then evaluated by SpiceDB and used to display the results of the check permissions calls executed against the schema+relationships.


## Future


With the LSP and extension joining the[playground](https://authzed.com/blog/spicedb-playground-is-open-source) and our CLI tool[zed](https://github.com/authzed/zed) , SpiceDB now has a full set of developer tooling for all major environments!


Have a question about SpiceDB, schemas or the new tooling? Don’t hesitate to[join our Discord](https://authzed.com/discord) to talk to both AuthZed employees and community experts now!


On this page


- Language Server Protocol
- Visual Studio Code (VSCode) Extension
- Syntax and Semantic Highlighting for SpiceDB Schema
- Real time validation and diagnostics
- Automatic formatting for SpiceDB Schema
- Built-in check watch
- Future


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
