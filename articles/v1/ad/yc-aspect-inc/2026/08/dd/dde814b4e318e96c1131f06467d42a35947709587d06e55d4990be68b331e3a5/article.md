---
schema_version: "1.0.0"
document_id: "dde814b4e318e96c1131f06467d42a35947709587d06e55d4990be68b331e3a5"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/post-production/how-to-configure-media-composer-shared-projects-over-vpn"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T21:45:18.324213+00:00"
fetched_at: "2026-08-06T21:45:19.509231+00:00"
content_hash: "sha256:ed9eb71d54829cfa2f886e3ff643ffcbf036097397376ce6ac37740a1741feaf"
---

# How to Configure Media Composer Shared Projects Over VPN

When configuring a shared Media Composer project over VPN, decide first whether editors are really editing across the VPN or only using the VPN to reach machines, projects, and services that stay inside the facility. Those are very different workflows.


For most productions, the stable answer is: keep Media Composer, NEXIS, shared projects, and full-res media on the facility network, then give remote users secure access through a remote workstation or a planned proxy workflow. Directly mounting shared storage over a home VPN can work for light project and bin access, but it's the easiest way to create slow bin opens, media offline surprises, database rebuild pain, and angry editors.


Design the VPN around the actual job it's doing: remote control, proxy sync, Interplay or MediaCentral access, NEXIS access, SMB project sharing, or a mix of those. Media Composer shared projects are sensitive to latency, permissions, naming, version mismatches, and file locking. Treat the VPN as part of the edit system, not as a generic office network pipe.


## Pick the remote editing model before tuning the VPN


There are three common ways teams try to do this.


Remote model Best fit Main VPN dependency Strength Weak point


Remote desktop into an on-prem workstation Full-res editorial, shared NEXIS projects, finishing-adjacent work Stable interactive traffic for display, audio, keyboard, mouse, and tablet input Keeps media, bin locking, databases, and playback on the facility LAN Requires available facility workstations and a well-tuned remote display path


Proxy-first remote editing Creative editing away from the facility, lower bandwidth users, planned relink or conform workflows Reliable transfer, sync, or managed access to proxy media and project data Avoids pulling high-res media through the VPN Requires strict proxy creation, source identity, relink, and turnover discipline


Direct shared storage over VPN Light bin work, metadata prep, small teams, short-form projects, assistant tasks Low latency, near-zero packet loss, correct SMB or storage access, reliable locking Lets remote users touch shared projects and media folders directly Fragile for playback, multicam, simultaneous writes, large bins, and unstable home networks


Remote desktop into an on-prem workstation is usually the safest for full shared-project behavior. The editor connects through VPN, then controls a Media Composer workstation that's already on the facility LAN with normal access to NEXIS or shared storage. Bin locking, media databases, project settings, and high-res playback stay local to the facility network. The internet connection only carries keyboard, mouse, display, audio, and tablet input.


Proxy-first remote editing is the next practical option. Editors work with local or cloud-accessible proxy media, then relink or conform back at the facility. Avid’s newer NEXIS EDGE-style proxy workflows are built around this idea: editors can work remotely with proxy media and switch between proxy and high-res representations without the manual relink mess that older workflows often required. If you aren't using that stack, you can still build a disciplined proxy workflow, but you need tighter rules around source identity, bin movement, and relink.


Direct shared storage over VPN is the most fragile option. Your team can use it for small teams, short-form projects, or assistant workflows where remote users mostly open bins, make metadata changes, prep selects, or move project files. It's a poor fit for high-res playback, multicam, lots of simultaneous editors, or anything where the VPN path has high latency or packet loss.


Direct VPN access stretches storage over a fragile path, while keeping editing near shared storage is more stable. A good rule: if the editor expects real-time playback from shared storage, don't make the media stream across a consumer VPN unless you have tested that exact format, stream count, audio routing, and timeline complexity under load. DNxHD proxy is one thing. Multicam UHD source media is another.


## The VPN settings that matter for Media Composer


Media Composer itself isn't “VPN optimized.” The services around it are what need predictable network behavior: storage, authentication, DNS, remote desktop, Interplay, MediaCentral, NEXIS client connections, and any file sharing used for projects and media.


The useful VPN settings are the boring ones:


- Low and consistent latency is more important than peak speed for project and bin operations.
- Packet loss should be effectively zero. Even small loss can make file sharing and remote display protocols feel broken.
- Set MTU and MSS clamping so packets aren't fragmenting across the tunnel.
- [DNS should resolve](https://resources.avid.com/SupportFiles/attach/Media_Composer_Cloud_Setup_V3_5.pdf) facility hostnames exactly as they do on site.
- Remote users should authenticate against the same identity source used for storage and project permissions.
- VPN sessions shouldn't sleep aggressively during long renders, exports, transcodes, or background tasks.
- The VPN should route only the required production systems unless your security model requires full tunnel.
- QoS should prioritize remote desktop or interactive editing traffic if the VPN is carrying display streams.
- Treat Wi-Fi as a risk factor, not a production network.


The biggest mistake is optimizing only for download speed. A home connection that tests at 800 Mbps down can still be terrible for Media Composer if upload is weak, latency spikes every few minutes, or the VPN encapsulation causes fragmentation. For[remote desktop workflows](https://www.youtube.com/watch?v=Wke0vrmgQzo) , bandwidth needs depend on monitor count, resolution, frame rate, color quality, full-screen playback expectations, and whether the workflow streams audio. For direct shared storage, bandwidth depends on codec, stream count, database traffic, and how much Media Composer is scanning or refreshing.


If editors report “Avid is slow,” separate the symptoms. Slow UI over remote desktop is different from slow bin saves. Slow media playback is different from slow project open. Slow project open is often DNS, permissions, storage latency, or huge project structure, not codec bandwidth.


## Port configuration isn't just “allow VPN”


Avid’s own cloud and remote workflow documentation makes an important point: being connected to[VPN doesn't guarantee](https://resources.avid.com/SupportFiles/attach/Media_Composer_Cloud_Port_Usage.pdf) that Media Composer can reach the required services. Firewalls, IPS devices, endpoint security tools, and VPN ACLs can still block specific hosts, TCP ports, UDP ports, or service discovery traffic.


Start by mapping the workflow, then allow only what that workflow needs. A remote desktop workflow has different network requirements than a direct SMB mount. An Interplay or MediaCentral workflow has different requirements again.


VPN rules should allow only the specific paths required by the editing workflow. For a practical port plan, group traffic by function:


- VPN tunnel access for approved users and devices
- DNS, directory, time sync, and certificate services
- Remote desktop or PCoIP-style display protocol traffic
- NEXIS client and storage management traffic, if remote clients mount NEXIS
- SMB file sharing traffic, if projects or media folders are shared over SMB
- Interplay, MediaCentral, or Cloud UX services, if used
- License services and background services required by the facility build
- Monitoring, logging, and management access for support staff


Don't rely on “any-any” rules just because the VPN is private, and don't let IT guess ports from an unrelated Avid version. Use the port guide for the specific Avid products and versions in your environment, then document the allowed hosts and ports in a way editorial support can read later.


For SMB-based sharing, TCP 445 is the obvious one, but that isn't the whole configuration. Name resolution, authentication, permissions, signing or encryption policy, endpoint firewall rules, and storage ACLs all affect whether the share behaves correctly. If users can browse a share but Media Composer can't reliably lock bins or write settings, you still don't have a working shared project setup.


For cloud or Interplay-style environments, expect both TCP and UDP rules, and expect host-based allowlisting. Avid’s Media Composer Cloud documentation specifically warns that you may need to configure security devices by IP address as well as port. In other words, “the VPN connects” is only the first layer.


## Shared projects need shared settings, not copied chaos


In a shared storage environment, a Media Composer project isn't just a folder full of bins. Project settings matter, and your team needs to keep them consistent across systems.


Avid stores a[Settings.xml file](https://kb.avid.com/articles/en_US/Knowledge/How-do-you-apply-the-same-project-settings-across-all-systems-within-a-shared-storage-environment) at the root of the project folder. When a new computer opens that project on shared storage for the first time, Media Composer creates a folder for that computer under AvidSharedData and copies the project Settings.xml there. That means the first open on each workstation can create workstation-specific settings behavior if you aren't deliberate.


The practical approach is to set the project correctly before everyone piles in:


- Open the project on a known-good system.
- Configure project settings that should apply across the team.
- Save those settings.
- Make sure the root Settings.xml reflects the approved setup.
- Have new workstations open the project only after the shared settings are in place.


This matters more over VPN because remote troubleshooting is slower. If one editor has different render settings, bin behavior, audio settings, or project configuration, the problem may look like a VPN issue when it's really inconsistent project state.


Also keep Media Composer versions aligned. Avid warns teams to be careful when sharing projects and bins between different Media Composer versions. In real production terms: don't casually mix a brand-new workstation build with older edit rooms in the middle of a job. If you need to upgrade, test with copied projects and representative bins first, especially if remote users will be opening the same shared project.


## Media folder sharing has to follow Avid’s rules


Avid managed media is picky in ways that are helpful once you respect them. The[Avid MediaFiles folder](https://kb.avid.com/pkb/articles/en_US/How_To/Moving-Projects-from-one-computer-to-another) belongs at the root of the media volume or workspace. Inside it, MXF media is organized under the MXF folder, usually in numbered subfolders. Media Composer scans those folders and builds database files so clips can relink to media.


Avid managed media belongs at the root of the mounted media volume or workspace. When teams copy media to shared storage over VPN, the common failure is that the team copied the files to the wrong place, with the wrong permissions, with stale database files, or in a folder Media Composer isn't expecting.


A clean shared media layout usually looks like this conceptually:


- Use one or more shared media workspaces for Avid managed media
- Put Avid MediaFiles at the root of each media workspace
- Put MXF media inside Avid MediaFiles/MXF/numbered folders
- Store project folders separately from media where possible
- Assign ingest, transcode, and render locations intentionally
- Set permissions that allow Media Composer to create, modify, and rebuild database files


Avoid letting every remote editor write renders, transcodes, and imports into the same numbered MXF folder at the same time across a weak VPN. On NEXIS, the system is designed for collaborative media access. On generic shared storage over VPN, simultaneous writes are where you find the edge of the setup quickly.


If media shows offline after a copy, look at the basics before relinking everything manually. Confirm the Avid MediaFiles folder is at the root of the mounted workspace. Confirm the folder isn't accidentally nested one level too deep. Confirm users have write permission where Media Composer needs to create database files. Confirm the media database has rebuilt. Confirm the editor has mounted the same workspace that contains the media, not a similarly named local or cached copy.


Path consistency helps too, and on Windows, consistent drive letters or workspace mounting practices reduce confusion. On macOS, make sure volumes mount with predictable names. Avid’s media database model is more forgiving than apps that rely entirely on absolute paths, but humans troubleshooting under pressure aren't.


## Don't put one giant project on the VPN and hope


Project structure is workflow infrastructure, and over VPN, a bloated project becomes a network problem because every open, save, lock, and bin refresh has to cross a less predictable path.


For episodic, unscripted, documentary, and multi-editor jobs, split the work in a way that matches how editorial actually moves material. Avid has described large unscripted workflows where a[single repository-style project](https://www.avid.com/resource-center/how-the-voices-editors-and-aes-manage-assets-in-an-unscripted-post-workflow) or library holds media, while editors work in more focused creative projects. That pattern exists for a reason: it separates asset organization from day-to-day cutting.


Useful split patterns include:


- A media repository project for ingest, grouping, sync, stringouts, and archival bins
- Separate editor projects for episodes, reels, acts, scenes, or story teams
- Assistant projects for prep tasks that don't require editors to open every bin
- Turnover or finishing projects that contain only the sequences and reference bins needed downstream
- Read-only archive areas for locked cuts, exports, and old versions


The goal is to reduce contention. Editors shouldn't need to open a 600-bin master project just to cut one scene. Assistants shouldn't need write access to a producer’s active cut bin to prep new media. Remote users should open fewer bins, save smaller bins, and touch less of the shared structure.


Splitting large projects into focused working projects reduces bin load and contention for remote editors. Bin locking only helps if the team respects bin ownership. If remote users keep duplicating bins to get around locks, you'll eventually spend more time reconciling versions than editing. Use naming conventions that make ownership obvious: editor initials, date, episode, act, scene, or story area. Keep active cut bins small enough that saving them over VPN doesn't feel like a coffee break.


## Workgroup configuration should match editorial roles


Configure workgroups around what people need to do.


Editors usually need active project access, their assigned media workspaces, render locations, and review/export destinations. Assistant editors may need broader ingest and media management permissions. Producers may need screening exports or review systems, not write access to live Avid bins. Finishing may need locked sequences, source media, AAFs, and turnover folders, but not the messy working project.


For Avid environments with Interplay, MediaCentral, NEXIS EDGE, or Cloud Remote-style features, workgroup settings become even more important. Some Avid remote workflows include administrator-controlled settings for whether[remote download is allowed](https://resources.avid.com/SupportFiles/attach/MediaComposerCloudRemoteReadMe_4_0.pdf) for a workgroup. That's a security and workflow decision, not just a convenience toggle, which means sensitive media leaving the facility may be unacceptable on some shows.


At minimum, align these across the workgroup:


- Media Composer version and patch level
- NEXIS client version, if used
- Project type, raster, frame rate, and color settings
- User permissions for projects, media, renders, and exports
- Workspace names and mount behavior
- Remote download policy, if applicable
- Two-factor authentication and device requirements
- Naming conventions for users, systems, bins, and workspaces
- Support escalation path when a remote editor is blocked


The last item sounds administrative, but it affects uptime. If a remote editor can't open a bin at 10 p.m., they need to know whether to call editorial support, IT, the post supervisor, or the assistant editor. VPN workflows fail slowly when nobody owns the whole path.


## Third-party storage and bin sharing need extra caution


Media Composer can work with shared storage beyond Avid NEXIS, but you need to be honest about support boundaries. Recent Media Composer builds include a project setting for enabling bin sharing on[third-party storage emulating](https://resources.avid.com/SupportFiles/attach/Media_Composer/Media_Composer_v2025.6_ReadMe.pdf) Avid NEXIS or ISIS. That setting exists because bin sharing depends on storage behavior, not just folder access.


If you're using third-party shared storage over VPN, test bin locking deliberately. Open the same shared project from two systems. Open a bin on one system and confirm the other sees the expected locked state. Save changes. Close and reopen. Test what happens when a VPN session drops while a bin is open. Test whether lock files clear properly. Test with the actual endpoint security tools enabled.


The dangerous failure mode is “it seems to work until two people edit the same bin and one version wins.”


## Proxies are usually the difference between usable and miserable


If remote users need creative playback, don't try to drag full-res media through the tunnel. Build a proxy plan instead because Media Composer supports proxy workflows, and Avid’s high-resolution workflow guidance calls out important details around project raster, transcode choices, and relink behavior. For example, transcode options can be limited by the current project format, so high-res sources in an HD project may not expose every proxy dimension or original-resolution transcode option you expect.


For VPN shared projects, the proxy decision should include:


- Proxy codec and raster that plays reliably on remote systems
- Where your team creates proxies: facility, assistant workstation, distributed processing, or automated workflow
- Whether your team copies proxies remote, streams them, or accesses them through managed remote tools
- How your team preserves source identity for relink
- Who can relink, consolidate, transcode, or delete media
- How finishing gets back to full-res media


Do a full round trip before production starts. Ingest a real camera card, create proxies, cut a short sequence remotely, relink to high-res, render, export, and turn over. If that test fails, the VPN isn't ready for the show.


## How to know the setup is actually production-ready


Fold testing into each part of the workflow, not just the day before editors start. When your team changes the VPN, open a shared project and save bins. When your team changes storage permissions, create Avid media and confirm databases update. When your team updates Media Composer, open copied bins from the real show and check compatibility. When your team adds proxies, relink a real sequence.


The most useful pilot is one assistant editor and one editor working for a day on representative material. Include multicam if the show uses multicam. Include turnovers if the show turns over weekly. Include producer changes if story bins move between episodes or acts. Include a simulated VPN drop, because someone’s home internet will absolutely drop during the season.


A working Media Composer shared project over VPN is a chain: remote access, ports, DNS, storage, permissions, project settings, media layout, version control, proxy strategy, and human rules around bins. If you keep full-res editing close to the storage, keep remote traffic predictable, and split projects so people only touch what they need, the VPN becomes boring. That's exactly what you want.


## FAQ


Yes, but the most reliable setup is usually not direct full-res editing across the VPN. A safer model is to keep Media Composer workstations, NEXIS or shared storage, shared projects, and high-resolution media on the facility network, then let remote users connect to those systems through remote desktop or a managed proxy workflow. Directly mounting shared storage over VPN can work for light bin access or assistant tasks, but it's more sensitive to latency, packet loss, permissions, DNS, and file locking.


The most important VPN qualities are low consistent latency, near-zero packet loss, correct MTU and MSS clamping, reliable DNS, proper authentication, and stable sessions that don't sleep during long operations. If remote desktop is used, prioritize interactive display traffic where possible. If shared storage is mounted, make sure the VPN and firewall rules allow the storage, authentication, name resolution, and locking behavior required by the workflow.


There's no single universal port list because the required ports depend on the workflow and Avid products in use. A remote desktop workflow, SMB project sharing, NEXIS client access, Interplay, MediaCentral, Cloud UX, licensing, DNS, directory services, and time sync can all require different rules. For SMB shares, TCP 445 is commonly required, but name resolution, authentication, endpoint firewall policy, signing or encryption settings, and permissions also matter. Use the port guide for the exact Avid versions and services in the environment rather than copying rules from another facility.


Common causes include the Avid MediaFiles folder being in the wrong location, media being nested one folder too deep, missing write permissions, stale media database files, or the editor mounting the wrong workspace. For Avid managed media, the Avid MediaFiles folder should be at the root of the media volume or workspace, with MXF media inside Avid MediaFiles/MXF/numbered folders. Media Composer also needs permission to create and rebuild database files in those folders.


Avoid making every remote user open one huge master project. Large projects increase bin refresh, save, lock, and open times over VPN. A better structure is to separate media organization from day-to-day cutting, such as using a repository project for ingest and sync, separate editor projects for episodes or scenes, assistant projects for prep work, and turnover projects for finishing. This reduces contention, keeps active bins smaller, and makes bin ownership clearer.


Producers usually don't need write access to the shared Media Composer project. Aspect can keep review outside the active edit project with timestamped comments, annotations, replies, and version context, so editorial receives usable notes without exposing live bins through a frame-accurate review workflow.
