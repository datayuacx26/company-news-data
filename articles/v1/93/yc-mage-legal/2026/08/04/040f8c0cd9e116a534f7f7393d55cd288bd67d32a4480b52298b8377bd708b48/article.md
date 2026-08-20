---
schema_version: "1.0.0"
document_id: "040f8c0cd9e116a534f7f7393d55cd288bd67d32a4480b52298b8377bd708b48"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/data-room-cli"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-03T21:15:15.140171+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:ac1d5180a6e9e76064a832a0a5800163f84262d5ff2a04d763d30809cf3abe0d"
---

# The Data Room CLI: A Live, Permissioned Data Room from Your Terminal

Two commands take a local folder tree to a live, permissioned data room:


```text
npx @magelegal/cli login
npx @magelegal/cli upload ./diligence


```


The first opens a browser, asks you to confirm a short code, and binds this machine to a room. The second mirrors the folder into the room, in parallel, with processing starting as each document arrives. Node.js 20 or newer is the only prerequisite, and` npx` means nothing gets installed.


That is the whole pitch. The rest of this page is the reference: what each command does, how the credential works, what breaks, and what the tool deliberately will not do.


## Why run a data room from a terminal at all?


Because the documents are already in a folder tree, and the browser is the worst possible way to move 4,000 of them.


Three situations make this concrete. A founder assembling a fundraising room has everything in one directory that mirrors how the company actually files things. An associate prepping a sell-side room has an export from the client's file server. A platform team wants the room populated by the same CI job that assembles the closing binder, with no human in the loop.


All three want the same thing: local structure in, room structure out, no drag-and-drop marathon, and no shared login pasted into a Slack thread.


## Install and first run


```text
node --version          # must be 20 or newer
npx @magelegal/cli login


```


Or install it once and drop the` npx` :


```text
npm install -g @magelegal/cli
mage --help


```


The package is` @magelegal/cli` , the executable is` mage` , it is MIT licensed, and the source is public at github.com/magelegal/mage-dataroom-cli. Runtime dependencies are two small libraries. Cite the install command rather than a version number; the published version moves.


` login` does four things: opens a browser page for your approval, resolves which room this machine should bind to (asking if your organization has several, and offering to create one if the account is brand new), mints a room-scoped API key labelled with this machine's hostname, and stores it locally. On a machine with no browser, or over SSH, pass` --no-browser` and it prints the approval URL instead.


Two constraints before you try. Minting a key requires owner or admin rights on the room. And the login does not expire: it works until the key is revoked.


## How does the credential actually work?


Security is the first objection anyone sensible raises about a CLI that touches deal documents, so here is the model in full.


The key the CLI mints for itself is **room-scoped** . It acts in exactly one room and carries the permission set chosen at mint time, visible under Settings and API keys alongside who minted it and when. A login-minted key can read the room's document list, upload, manage folders, delete documents, download document files, and read and fill the readiness checklist.


What it cannot do is the more useful half:


- It cannot reach another room.
- It cannot delete the room itself.
- It cannot change room settings, people, or sharing. That needs the Manage room permission, which is not granted by default.
- It cannot fetch document contents without the Download permission. Reading the document list and reading the bytes are separate grants.


Permissions are immutable after mint. Widening a key means minting a new one, which is the right trade: a key's authority never quietly grows.


Revoking a key under Settings and API keys kills it everywhere, instantly. The secret itself is returned exactly once, at creation, and is never recoverable afterward; the room's settings list shows only the room binding, who minted it, its permission set, and its audit trail. Locally, credentials live in` ~/.config/mage/config.json` at mode 0600, readable only by you.


Only two commands act as *you* rather than as the key:` mage rooms` and` mage use` . Everything else runs with the key's authority, which is why a stolen laptop is a revocation problem rather than an account problem.


## The command reference


Command What it does


` mage login \[key\] \[--room <room>\] \[--with-key\] \[--no-browser\]` Sign in via browser, or store an API key, and bind this machine to a room


` mage logout` Revoke the CLI's key where possible and clear this machine


` mage rooms` List your organization's data rooms (browser login only)


` mage use <room>` Switch which room this machine is bound to (browser login only)


` mage upload <paths...> \[--to <folder>\] \[--for-item <itemId>\]` Upload files or whole folders, mirroring their structure


` mage readiness` Show the room's readiness checklist: present, partial, and missing


` mage readiness attach <itemId> <documents...>` Attach already-uploaded documents to a checklist item


` mage ls \[folder\]` List the room's documents, grouped by folder


` mage download \[target\] \[dest\]` Download the room, a folder, or one document, mirroring structure


` mage mkdir <folder>` Create an empty folder, for example` 01-Corporate/Charters`


` mage rm <target> \[-r|--folder\] \[-y|--yes\]` Delete a document, or a folder with` --folder`


` mage version` Print the CLI version


` mage help \[command\]` Display help for a command


Global flags:` --json` for machine-readable output on stdout,` --api-url <url>` to point at a different API base, and` -v, --version` .


Environment variables:` MAGE_API_KEY` (a room-scoped key that overrides any stored login),` MAGE_API_URL` (override the API base),` MAGE_ROOM_ID` (pin the room id rather than resolving it from the key), and` MAGE_OAUTH_CLIENT_ID` (rarely needed; normally discovered from the API).


## How folder structure survives the trip


Point the CLI at a directory and the directory mirrors its contents beneath itself. Point it at a file and the file lands in the folder named by` --to` , which is created if it does not exist. Dotfiles like` .DS_Store` and` .git` are skipped. Uploads run in parallel.


```text
mage upload ./diligence
mage upload ./charter.pdf --to "01-Corporate"
mage mkdir "01-Corporate/Charters"


```


Structure matters more here than in ordinary file storage, because the tree becomes the index. Every folder and filed document receives a stable dotted hierarchical number, the classic VDR convention: 1, 1.2, 1.2.3. Within a level, folders and documents share one number sequence, ordered case-insensitively by display name. Numbers are stored rather than recomputed on every read, so a citation to 4.2.1 stays valid until the room is deliberately re-indexed.


One deliberate exception: a document with no folder path sits in Unsorted and is left un-numbered until it is organized into a folder. That is by design, not a gap, and it is the reason to get the tree right before you push.


While the files land, the room does its own work. The server classifies each arrival by type, summarizes it, organizes the room in a single pass that chooses the folder structure, and links amendments, exhibits, and side letters to the agreement they belong to. The[full walkthrough of a first room setup](https://magelegal.com/blog/how-to-set-up-a-data-room) covers that side.


## Reading the checklist and filling it


The room carries a readiness checklist: the documents an investor's counsel or an acquirer's counsel expects to find, scored per item as present, partial, or missing, with items the user has marked not applicable excluded from the score entirely.


```text
mage readiness
mage readiness --json
mage upload ./tax/2025-return.pdf --for-item <itemId>
mage readiness attach <itemId> "Corporate/Bylaws.pdf"


```


` --json` returns each item with a stable` itemId` , a label, and a hint describing what the item should contain.` --for-item` uploads the files and attaches every one that landed to that item, in one step.` readiness attach` accepts a document id, a name, or` folder/name` , and is additive: documents already attached stay attached. Per-item curation lives on its own and survives every recompute, so marking something not applicable is not undone by the next pass.


That loop is what makes this scriptable. Read what is missing, produce it, attach it, re-read. Handing the same loop to an AI agent is a natural extension, which we cover separately in[data rooms in the agentic era](https://magelegal.com/blog/data-rooms-for-ai-agents) .


## Machine-readable output and headless runs


Every command accepts` --json` and emits machine-readable output on stdout. Combined with a key in the environment, that is the whole headless story:


```text
export MAGE_API_KEY="..."
mage readiness --json
mage upload ./collected --for-item cap-table


```


Setting` MAGE_API_KEY` skips` login` entirely. The CLI discovers which room the key belongs to on first use, so the key is all a CI job or an agent needs. On the readiness API specifically, reads and per-item curation accept a room-scoped key, while notify and recompute stay human-gated behind a session. That split is deliberate: a script can fill the room, but it cannot email your investors.


## Which providers expose a terminal client or an API?


Precision matters here, because the marketing in this category is loose.


Access path Where it stands as of August 1, 2026


Command-line client on npm None of Datasite, Intralinks, Ideals, Firmex, Ansarada, DealRoom, SecureDocs, Digify, or DocSend publishes one


MCP connectors Datasite states an MCP connector for Claude, ChatGPT, or Microsoft Copilot; Ideals promotes an MCP connector on its homepage; DealRoom ships an MCP integration


Vendor-published npm executable Papermark publishes` @papermark/mcp-server` on npm from its own scope and repo


API tier gating Ideals gates its MCP connector and API integration to the Enterprise plan


Two honest qualifications. Box publishes an official command-line client for the Box API on npm and also sells a virtual data room, so "no data room vendor ships a CLI" is only true of the deal-VDR incumbents, not of the category read broadly. And Mage is not uniquely agent-reachable: Datasite ships an MCP server plus published agent skills targeting sell-side workflows including VDR index setup and information-request-list tracking, and names Claude Code as a supported host. This space is moving fast, and three of those MCP products shipped within the last four months.


What remains distinctive is the shape rather than the existence of access. An MCP connector or an enterprise REST integration is something an IT team wires up. A published npm executable is something a founder or an associate runs in ninety seconds, with a credential they can revoke themselves from a settings page.


## What this CLI will not do


Four limits, stated plainly, because a reference that only lists capabilities is a brochure.


**It drives the data room, and nothing else.**` mage --help` groups every current command under "Data room". Mage's diligence platform has no command-line surface. If you are looking to script disclosure schedules or memos, that is not this tool.


**There is no unauthenticated path.** The shortest honest flow from zero is two commands, not one. Anything shorter is a tutorial for a different product.


**Deletes are gentler than they look, which cuts both ways.**` rm --folder` moves that folder's documents to Unsorted rather than deleting them, so a scripted cleanup can leave you with a pile of un-numbered documents instead of the clean room you expected. Run` ls` after.


**The room has no redaction and no counterparty Q&A module.** Ask Mage answers questions from the room's documents for members, and guests do not get it. Neither of those is a CLI limitation; they are product limitations that the CLI cannot route around.


For continuous sync from a shared drive, the connector path is the documented one: connect Google Drive, OneDrive, Dropbox, or Box, choose a folder, and Mage keeps the room in sync as new files land. The CLI is for pushes you control, not a background sync daemon.


## The web path, for everyone else


Most of the people you work with will never open a terminal, and they should not have to. Everything above has a browser equivalent: drag in files and folders, drop a ZIP, or connect a cloud drive from the source picker. The room organizes itself the same way, numbers the same way, and shares the same way, through per-recipient invite links carrying view or download permission, an optional expiry, instant revocation, and an NDA gate that is on by default.


Pick the interface that matches the person. The terminal for the founder with a folder tree and the platform team with a pipeline; the browser for the deal team. Start either one at[Mage Data Room](https://magelegal.com/dataroom) , which is free for a limited time, or read the rest of our writing on rooms in the[data rooms resource hub](https://magelegal.com/blog/topics/data-rooms) .
