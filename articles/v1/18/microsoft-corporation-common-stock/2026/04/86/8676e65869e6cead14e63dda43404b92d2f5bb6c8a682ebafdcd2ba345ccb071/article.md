---
schema_version: "1.0.0"
document_id: "8676e65869e6cead14e63dda43404b92d2f5bb6c8a682ebafdcd2ba345ccb071"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/04/28/continuing-the-story-of-early-dos-development/"
published_at: "2026-04-28T16:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:516cfbfcfd250ee5ff0584a5958a0ec0af16db0a285554c5e7209a6668e03009"
---

# Continuing the story of early DOS development

Over the last few years, we’ve been working to open some of the earliest chapters of Microsoft’s operating system history. In 2018 we (re)-open-sourced MS‑DOS 1.25 and 2.11, and more recently in 2024 we were able to make the[source for MS‑DOS 4.0](https://opensource.microsoft.com/blog/2024/04/25/open-sourcing-ms-dos-4-0) available to the public as well. Today, on 86-DOS 1.00’s 45th anniversary, we’re continuing that tradition by preserving[the earliest DOS source code](https://github.com/DOS-History/Paterson-Listings) discovered to date. These releases are about making historically important systems software available for study, preservation, and plain ol’ curiosity.


[Explore listings and code at DOS-History/Paterson-Listings](https://github.com/DOS-History/Paterson-Listings)


But that work doesn’t end with a GitHub repo. Software history lives in code, yes, but also in scanned listings, internal documents, assembler printouts, and the sometimes wonderfully analog artifacts of how operating systems came together in the late 1970s and early 1980s. If you read the original announcement around[re‑open sourcing MS‑DOS 1.25 and 2.0 on the Windows Command Line blog](https://devblogs.microsoft.com/commandline/re-open-sourcing-ms-dos-1-25-and-2-0/) you’ll know how much context matters when trying to understand where today’s platforms came from.


We’re stoked today to showcase some newly available source code materials that provide an even earlier look into the development of PC-DOS 1.00, the first release of DOS for the IBM PC. A dedicated team of historians and preservationists led by[Yufeng Gao](https://thebrokenpipe.com/about/) and[Rich Cini](http://cini.classiccmp.org/) has worked to locate, scan, and transcribe the stack of[DOS-era source listings](https://github.com/microsoft/MS-DOS/blob/main/v1.25/Tim_Paterson_16Dec2013_email.txt) from Tim Paterson, the author of DOS.


The listings include sources to the 86-DOS 1.00 kernel, several development snapshots of the PC-DOS 1.00 kernel, and some well-known utilities such as CHKDSK. Not only were these assembler listings, but there were also listings of the assembler itself! This work offers rare insight into how MS-DOS/PC-DOS came to be, and how operating system development was done at the time, not as it was later reconstructed.


*What DOS development on the IBM PC looked like in the early 80s. Credits: Rich Cini.*


It’s also worth noting that these materials aren’t just operating system releases in the traditional sense. In several cases, the listings represent point‑in‑time working states and hand-written notes, preserved by Tim Paterson himself. Think of them as a printed commit history of a Git repository. They create a timeline of changes, showing which features were implemented when, what errors were made, and how they were fixed. Soon you’ll be able to visit these living artifacts at the[Interim Computer Museum](https://icm.museum/) as they’ve been generously donated by Tim Paterson.


We want to thank everyone involved in curating and bringing these materials forward in a responsible and accessible way. This kind of software archaeology takes real effort across legal review, archival work, and technical validation, and it’s an important part of preserving the shared history of our industry.


If you’re interested in digging into these early listings yourself, we encourage you to check out the full posts from the team on[Yufeng’s website](https://thebrokenpipe.com/dos/paterson_listings) and[Rich’s website](http://cini.classiccmp.org/recoveryblog.htm) , as well as[Joshua’s research](https://jscarsbrook.me/doshistory/) on printer listing OCR. For an insiders’ look at the process, explore the scanned listings and OCR’ed code at[DOS-History/Paterson-Listings](https://github.com/DOS-History/Paterson-Listings) , which we’ve worked with the maintainers to license under MIT via pull request for researchers, hobbyists, and enthusiasts like us!


## Explore listings and code


Dig into the DOS-History/Paterson-Listings today.


[See more](https://github.com/DOS-History/Paterson-Listings)
