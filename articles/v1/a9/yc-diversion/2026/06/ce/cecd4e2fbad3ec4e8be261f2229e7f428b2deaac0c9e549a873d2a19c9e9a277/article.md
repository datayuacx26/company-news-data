---
schema_version: "1.0.0"
document_id: "cecd4e2fbad3ec4e8be261f2229e7f428b2deaac0c9e549a873d2a19c9e9a277"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/diversion-product-updates-june-highlights-06-26"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:a03ad624d515dbd90548f19ac0db351296275413da75fe187c6c453f66a36146"
---

# Diversion Product Updates: June Highlights (06/26)

Dear community,
World Cup season is here, and we’re bringing our own lineup: a redesigned app, stronger branch protection, Unity Asset Store launch, and a few updates your repo will cheer for. Let’s kick it off 😉


‍


## **🎨 A redesigned Diversion app**


Diversion got a glow-up. We refreshed the app layout to make everything faster to find, easier to navigate, and smoother to work with day to day.


Here’s what’s new:


- **New top bar** - global search with Cmd/Ctrl+K, so you can jump to almost anything in a second, plus a one-click light/dark mode toggle.


- **New left navigation rail** - quick access to Files, Commits, Graph, Reviews, and Merges.


- **Workspace bar** - your branch picker, sync status, collaborators, and workspace switcher now live together in one clean row.


- **Org breadcrumb and repo picker** - switch between organizations and repositories without losing your place.
‍


## **🧹 Obliterate: permanently remove files and their history**


Some files should never have been there in the first place. Huge accidental commit? Secret pushed by mistake? Files that need to be gone-gone? Repo admins can now use[Obliterate](https://docs.diversion.dev/advanced/obliterate#obliterate) to permanently remove file content and its history from a repository.


You can:


- Target files with glob patterns, including Perforce-style wildcards.


- Preview exactly what will be removed before confirming.


- Run it directly from the CLI.
‍


## **🎮 Diversion is now on the Unity Asset Store!**


Unity users, we made setup even easier. Diversion is[now available on the Unity Asset Store](https://assetstore.unity.com/packages/tools/version-control/diversion-connect-for-unity-353134?srsltid=AfmBOoo-71iPrp3XihyfAiK-Ct_i6S9TbFhd4irDWeN9abl7xyiNhp9P) , so you can install our Unity integration directly from the editor - no manual setup required.


The integration now runs as a standalone connector, without depending on the Desktop app, and includes a clear **Connect to Diversion** flow if Diversion isn’t installed yet.
**Love the plugin? We’d really appreciate a review on the**[Unity Asset Store ‍](https://c.vialoops.com/CL0/https:%2F%2Fassetstore.unity.com%2Fpackages%2Ftools%2Fversion-control%2Fdiversion-connect-for-unity-353134%3Fsrsltid=AfmBOoo-71iPrp3XihyfAiK-Ct_i6S9TbFhd4irDWeN9abl7xyiNhp9P%23reviews/1/0100019eff33fc35-391aa6a7-3347-46e1-b228-90742dab3184-000000/dQ2O8Wl_yaX1IEbZaHv5RosIZYH_NXmIkgXac5CmQF8=452) ‍


## **👀 Whitespace changes are now visible in diffs**


Tiny formatting changes can cause surprisingly big “wait, what changed here?” moments. Reindents, tabs vs. spaces, and trailing spaces are now visible by default in the diff view - with spaces shown as dots and tabs shown as arrows.


Prefer the cleaner view? You can turn it off from the diff dropdown using **Show whitespace changes** , and Diversion will remember your choice.


‍


## **🔒 Stronger branch protection**


Branch protection just got a lot better at actually protecting branches.


Protected branches now block merges too - not just direct commits - including review merges and draft publishes. You can also require a minimum number of approvals before merging, and the Merge button now clearly explains why it’s blocked, like “2 of 3 approvals.” Fewer mystery blockers. Fewer accidental merges. More confidence before changes land.


‍
