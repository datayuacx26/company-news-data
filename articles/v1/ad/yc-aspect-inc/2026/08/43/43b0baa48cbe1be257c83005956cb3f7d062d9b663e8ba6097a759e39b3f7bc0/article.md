---
schema_version: "1.0.0"
document_id: "43b0baa48cbe1be257c83005956cb3f7d062d9b663e8ba6097a759e39b3f7bc0"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/aspect-workflows/aspect-for-streaming-previews-to-clients-without-downloads"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T05:57:31.682951+00:00"
fetched_at: "2026-08-13T05:57:32.908442+00:00"
content_hash: "sha256:ae1eb944265d6f578e253de7ce307df8e56444c53092c6f7c1723c89501d4993"
---

# Aspect for Streaming Previews to Clients Without Downloads

A client doesn't need a[90 GB ProRes master](https://optiview.dolby.com/docs/ad-engine/how-to/cloud-preview/) to tell you the logo is too small, the legal line is missing, or the second act drags. They need something that opens quickly, plays cleanly, and lets them leave useful notes without turning review into a file-transfer project.


A streaming preview gives reviewers playback and notes without handing over the full master file. That's the job of streaming previews in Aspect: keep the heavy media in your controlled workspace, generate a web-playable version, then share access at the right permission level. The reviewer gets browser access while the team keeps the source of truth, without waiting for a full-resolution master to download before the conversation can start.


This matters most when the file is large, the client is non-technical, or the review round is time-sensitive. A download link sounds simple until the client is on hotel Wi-Fi, their laptop sleeps halfway through the transfer, or someone forwards the file to a person who was never supposed to have it. Streaming doesn't solve every delivery problem, but for review and approval it removes a lot of needless friction.


## The job is review, not delivery


The first decision is whether you're sending a file for review or delivering a file for use.


For review, the client needs playback, comments, annotations, version context, and maybe approval. They usually don't need the original master. In fact, giving them the original can create new problems: local copies, wrong-version feedback, uncontrolled forwarding, and confusion about which export is approved.


For delivery, the client may need the actual file. That could mean the final master, a broadcast spec deliverable, mezzanine files for localization, audio stems, captions, graphics, or a package that another vendor will ingest. In that case, a file transfer workflow may still be the right tool. Transfer platforms, cloud storage links, or managed delivery systems make sense when the recipient’s job is to download and use the asset.


Aspect is strongest when the review object should be playable before it's downloadable. The rough cut, VFX temp, color pass, director’s cut, trailer revision, or social version can stay attached to the project, and your team can control access per recipient. The master stays in the workspace, and the client sees what they need to see.


[Avid’s review and approval guidance](https://www.avid.com/resource-center/streamlining-the-review-and-approval-workflow-for-video-post-production) makes the same workflow point in a broader way: teams should decide who reviews, when they review, and how many review cycles they expect before the work begins. The technology only helps if the process is clear, and a streaming preview link makes the review round more controlled.


## Use a proxy for viewing


Optimize the preview for playback, not for finishing. Aspect[automatically generates previews and proxies](https://github.com/aspect-hq/aspect-node-sdk) for uploaded media, including support for video and image formats such as RED and BRAW. That gives you a web-playable representation of media that would otherwise be painful or impossible for a client to open directly.


Uploaded source media can be transformed into a lightweight web-playable proxy for review. That proxy has a specific role. It should be good enough for feedback, but it isn't necessarily the legal, color, audio, or broadcast deliverable. Treat it as the version reviewers see.


For most client review rounds, your team should build the useful setup around a few decisions:


- Which version is the current review version
- Whether the reviewer can only view, or can also comment
- Whether your team should disable downloading
- Whether the link needs a password
- Whether your team should set the link to expire after the review window
- Whether the share should expose one file, a folder, or a curated collection


Those choices are more important than the exact encode settings in most review workflows. If the client can instantly play the edit and leave a clear note on the right frame, the round moves forward. If they have to download a huge file, find a player that supports the codec, then write feedback in an email, the round slows down.


Review situation Useful Aspect setup Download setting Access limit When another workflow may fit better


Rough cut creative review Collection with the current cut, comment access, and version context Usually off Expiry after the review window, password if sensitive If the client is approving fine color, HDR, mix, or VFX detail


Legal or compliance review Single file or curated collection, view or comment access, and a status field such as Needs legal Off unless the reviewer needs a local record Password and shorter expiry If legal requires an auditable export package or mandated file format


Agency or brand producer review Collection with the current version, optional prior version, comments, replies, and notifications Usually off, unless a proxy download has a real use Per-recipient or group access If the producer must hand files into another platform or storage system


Finishing vendor handoff Approved elements shared with download permission On for the required elements only Named users or a controlled group Use managed transfer or delivery for masters, stems, captions, or formal packages


Final client delivery Not primarily a preview workflow On for deliverables Follow the client’s delivery rules Use a delivery or file transfer workflow when the recipient needs to ingest or reuse the file


There are exceptions. If the client is approving fine color, HDR behavior, surround mix, audio sync at a forensic level, or compression artifacts,[the proxy may not be enough](https://workflow.frame.io/guide/) . In those cases, use the streaming preview for broad creative review and separately define how technical approval happens. That may be a calibrated room, a higher-quality review encode, a live supervised session, or delivery of the final master.


The mistake is pretending one link can serve every kind of approval. It usually can't, so make the preview link do the fast feedback job, and keep technical signoff as its own workflow when needed.


## Share the smallest useful surface


Aspect lets teams share files and folders with permission levels such as view, download, comment, edit, and full access. It also supports collections, which let your team share groups of assets without changing the underlying folder structure.


For client previews, collections are often cleaner than folder shares. A production folder may include camera originals, exports, audio, graphics, WIPs, references, old versions, and internal notes. The client rarely needs that whole context. They need the three cuts under review, the latest trailer, or the approved selects.


A good review collection might include:


- The current edit
- A previous edit for context, if needed
- A reference file the client already approved
- Captions or a transcript, if relevant
- Any alternates that need a decision


The collection becomes the client-facing set of assets, while your internal folder structure can stay organized for editors and producers, and the client sees only the assets that matter for that round.


Match permissions to the person’s role. A brand manager may need comment access. A legal reviewer may need view-only access plus the ability to send notes through a producer. An agency producer may need download access for a low-bandwidth proxy, but not the master. A finishing vendor may need download access to approved elements, which is a different workflow from client review.


The key is to avoid “one link for everyone.” Per-recipient or per-group access keeps the review surface clean. It also makes revocation easier when a freelancer rolls off, a client stakeholder changes, or a review window closes.


## Add password and expiry when the link can travel


Any shared link can move beyond the person you sent it to. It can be forwarded, pasted into a Slack channel, copied into a client ticket, or resurfaced months later. That doesn't mean every link needs maximum friction, but it does mean your access settings should match the sensitivity of the material.


Aspect supports password protection and link expiry for shared links and collections. Use those controls when the content is unreleased, confidential, client-sensitive, talent-sensitive, or simply not meant to live forever.


Shared preview links can be protected with access and timing controls. There are a few common access patterns that work well:


- Short expiry for rough cuts that should only be reviewed this week
- Password protection for unreleased campaigns, episodic content, or celebrity/talent footage
- Comment-only access for reviewers who shouldn't download files
- View-only access for stakeholders who need visibility but shouldn't participate in the note thread
- Download permission only when the recipient has a real downstream use for the file


Use these controls to prevent a rough cut link from becoming permanent, forwardable access to your work. If a client needs ongoing access after approval, create that intentionally instead of leaving the review link open by accident.


This is also where streaming beats a plain download link. With a download link, the control often ends once the file is downloaded to someone’s machine. With a streaming review link, you can keep the review experience tied to permissions, versions, comments, and expiry.


## The laptop reviewer experience


On a laptop, the best client review experience is boring in the best way. The client opens the link in a browser, presses play, watches the edit, and leaves notes where they belong.


They shouldn't need to install an NLE. They shouldn't need to understand codecs. They shouldn't need to know whether the source was ProRes, DNxHR, RED, BRAW, or something else. They shouldn't have to download 90 GB just to scrub to minute 14.


For the post supervisor setting this up, the laptop experience is where you should think about review behavior:


- Can the reviewer play without waiting for a full download?
- Can they pause and comment at the exact moment they mean?
- Can they draw or annotate when a visual note is easier than a written one?
- Can they compare versions without asking which file is current?
- Can they reply in the thread instead of starting a separate email chain?


Aspect supports frame-accurate comments, annotations, replies, notifications, and version stacking. That matters because “client feedback” isn't just the words someone types. It's the relationship between the note, the frame, the version, and the person who left it.


If the client sends back “the graphic still feels late” in an email, someone has to translate that into a timecode, find the right version, and decide whether the note is new or already handled. If Aspect keeps the note attached to the frame on the current version, the editor has less interpretation work to do.


For Premiere Pro teams, Aspect’s Premiere panel can bring comments directly into the timeline. That closes the loop between client playback and editorial action. The reviewer stays in the browser, while the editor sees the feedback where they work.


## The phone reviewer experience


Phone review isn't ideal for every decision, but it's real. Executives, clients, producers, and brand stakeholders often review from a phone because that's where they're when the edit arrives.


A phone-friendly review link is valuable because it removes the biggest blocker: “I’ll look when I’m back at my laptop.” If the decision is broad creative feedback, approval to proceed, copy review, or a quick comparison between versions, mobile playback can keep the project moving.


The limits are also real. A phone isn't the right place to approve fine color, small legal text, audio mix details, or subtle VFX edges. It's also easier for reviewers to leave vague comments because typing is slower and screen space is limited.


Set expectations in the message that accompanies the link. For example, ask for mobile approval only on story, timing, selects, copy, or general direction. If you need a laptop or calibrated review, say that directly.


For phone reviewers, the shared link should do a few things well:


- Open without forcing a download
- Play a web-optimized preview
- Keep the current version obvious
- Allow lightweight comments or replies
- Respect the same password, expiry, and permission rules as desktop review


The benefit is continuity. The client can watch on a phone during travel, then reopen the same review on a laptop later. Their access, comments, and version context stay attached to the same asset instead of fragmenting across text messages, email, and downloaded files.


The same review can continue from a phone to a laptop without a separate download.


## Keep metadata attached to the review round


Review links also create metadata.


Who reviewed? Which version did they see? Which edit did the client approve? Which notes did your team address? Which asset should your team move to finishing, archive, localization, or delivery?


Aspect supports custom metadata, version stacking, revision history,[automatic transcription](https://aspect.inc/features/asset-intelligence) , and searchable asset records. That gives your team a better way to connect review state to the asset instead of burying decisions in email.


Avid’s metadata guidance warns that[metadata only stays useful](https://www.avid.com/resource-center/map-it-out-how-to-manage-metadata-when-importing-and-exporting-media) when teams design the pipeline to preserve it. Teams lose metadata when they move between tools, platforms, and inconsistent operating methods. That warning applies to review too because if the approval happens in one place, the final file lives in another, and your team tracks the status in a spreadsheet, someone eventually has to reconcile them.


In Aspect, you can keep review activity closer to the media. A custom status field might track “Internal review,” “Client review,” “Approved,” or “Needs legal.” A version stack can keep the current edit and previous rounds together. Transcripts can help people find spoken moments later. Revision history can show how the asset changed over time.


Don't overcomplicate this. Pick metadata that answers your team's real questions:


- Is this the version the client saw?
- Has the client approved it?
- Who still needs to review?
- Did your team allow downloads?
- Is this safe to archive?
- Does this asset have rights, legal, or campaign constraints?


The payoff comes later, when someone asks for “the approved launch edit” or “the version legal reviewed last Thursday.” If your team updates the asset record during review, the answer will be easier to find.


## Where partner tools still fit


A streaming preview workflow doesn't replace every tool in the post stack.


Your NLE is still where editorial work happens. Your finishing tools are still where color, sound, VFX, and mastering happen. A managed file transfer tool may still be best when the client actually needs to receive a master file. A dedicated MAM may be appropriate for organizations with complex broadcast taxonomies, rights workflows, or legacy integrations that go far beyond review.


The boundary is simple: use Aspect when the problem is shared media, controlled review, permissions, comments, and version context. Use a delivery or transfer tool when you need to get a finished file into someone else’s storage or ingest system.


LucidLink, for example, describes a[shared filespace model](https://www.lucidlink.com/blog/video-production-workflow) where large files are streamed[from cloud storage](https://www.youtube.com/watch?v=OYyrXWENEp0) so production teams can access them without waiting for full downloads. That can be a good fit for remote editing and shared storage workflows. Dalet describes FlexMAM as a system for organizing, reviewing, searching metadata, exporting EDLs, and triggering workflows in larger media asset environments. Those are different centers of gravity.


Aspect’s advantage for this specific workflow is that the same platform can hold the media, generate previews and proxies, share controlled links, collect review feedback, and keep version metadata attached. You don't have to turn a rough-cut review into a download workflow just because the master exists.


Still, if the client’s requirement is “send us the mezzanine master so our system can ingest it,” give them the file through the right delivery path. Don't force streaming preview into a job it doesn't fit.


## Signals that the workflow is working


You know the streaming preview setup is doing its job when review gets quieter. Fewer “I can’t open this” messages. Fewer duplicate downloads. Fewer notes on the wrong version. Fewer mystery approvals buried in email.


The useful signals are concrete:


- Reviewers can open the link on[laptop and phone](https://workflow.frame.io/guide/) without downloading the master
- The current version is obvious to the client
- Comments land on the right frame or moment
- Your team uses passwords and expiry for sensitive or time-boxed review rounds
- Your team reflects approval status in the asset metadata or version history


If those are true, the workflow is probably healthy. When the workflow breaks, the issue is usually one of three things: the wrong people have the wrong permissions, your team shared the wrong version, or the team hasn't defined what kind of approval the preview is meant to support.


Streaming previews separate review from delivery without lowering quality standards. Let the client watch and respond quickly. Keep the heavy master where it belongs. Then deliver the real file only when the job moves from “tell us what you think” to “this is approved for use.”


## FAQ


For review, the client usually needs fast playback, comments, version context, and approval, not a 90 GB master file. A streaming preview lets the media stay in the controlled workspace while the reviewer opens a browser link, watches a web-playable version, and leaves notes. This reduces codec problems, failed downloads, feedback on the wrong version, and uncontrolled local copies.


No. A streaming preview is a review surface. It's useful for creative feedback, timing notes, copy review, annotations, and approval rounds. Final delivery is different: the recipient may need a master, mezzanine file, captions, stems, graphics, or a package for ingest. If the client’s actual requirement is to receive and use the file, a managed transfer or delivery workflow may be the better fit.


Permissions should match the reviewer’s role. A client stakeholder may need comment access. A legal reviewer may only need view access. An agency producer may need download access for a proxy, but not the master. In Aspect, teams can use permissions such as view, download, comment, edit, and full access, along with password protection and link expiry where appropriate. The safest pattern isn't one link for everyone, but access scoped by recipient, group, review window, and asset sensitivity.


Yes, if the decision is appropriate for mobile review. A phone can be useful for broad creative feedback, approval to proceed, quick copy review, or checking general direction while a stakeholder is traveling. It isn't a good place to approve fine color, small legal text, audio mix details, subtle VFX, or technical delivery quality. The review message should make that distinction clear so mobile feedback doesn't get mistaken for technical signoff.


A proxy may not be enough when the approval depends on color fidelity, HDR behavior, surround mix, forensic audio sync, compression artifacts, small on-screen text, or final broadcast specifications. In those cases, use the streaming preview for fast creative review, then define a separate technical approval path. That might be a higher-quality review encode, a supervised session, a calibrated room, or delivery of the actual master through a file transfer workflow.


For review, send a streaming preview instead of the full-resolution master. Aspect keeps the heavy media in the workspace and generates web-playable versions so the client can open the asset in a browser, while your team controls whether they can view, comment, or download the file through generated proxies and previews.
