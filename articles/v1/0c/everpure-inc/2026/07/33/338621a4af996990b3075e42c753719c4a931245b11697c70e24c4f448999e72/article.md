---
schema_version: "1.0.0"
document_id: "338621a4af996990b3075e42c753719c4a931245b11697c70e24c4f448999e72"
company_key: "everpure-inc"
company: "Everpure Inc."
source_id: "everpure-inc-rss-a7fca946ec64"
canonical_url: "https://blog.everpuredata.com/perspectives/flasharray-backup-best-practices/"
published_at: "2026-07-08T13:00:00+00:00"
first_seen_at: "2026-07-25T03:30:11.662383+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:d796943d4340632ae291f7120f34da4c973bae4bdac7b002230a75a51093e87c"
---

# FlashArray Backup Best Practices for Ransomware Recovery

### Summary


FlashArray backup best practices leverage immutable snapshots, SafeMode, and a 3-2-1 backup strategy to enable fast, secure recovery from ransomware and data corruption.


Backups are your last line of defense. When ransomware hits, when an admin makes a mistake, or when a volume gets corrupted, the question is never whether your backups exist—it’s whether they’re *recoverable, fast, and beyond the reach* of whoever (or whatever) caused the damage in the first place.


[FlashArray](https://www.everpuredata.com/products/block-file-object-storage.html) ™ is built to make all three of those true. This article walks through best practices to put in place to help ensure your organization can recover faster in the event the worst happens.


## Start with snapshots—they’re the foundation


A snapshot is an immutable, read-only, point-in-time copy of a volume. Once taken, it can’t be changed. That immutability is exactly what makes snapshots the backbone of a good data protection strategy.


FlashArray snapshots have five powerful properties:


- **Immutable:** Once created, a snapshot can never be altered.
- **Portable:** They carry their own metadata, so they can be moved almost anywhere: another FlashArray,[FlashBlade](https://www.everpuredata.com/products/unstructured-data-storage.html) ®, or the cloud.
- **Space-efficient:** Snapshots are deduplicated against the source volume. Data that hasn’t changed costs effectively zero additional flash, and the remainder is compressed.
- **Fast to take:** Taking a snapshot is near-instant.
- **Fastest to recover:** When stored locally on the originating array, recovery is as fast as it gets.


The first snapshot of a volume captures the whole volume. Every snapshot after that captures only what changed since the last one—much like a traditional incremental backup, but without the management overhead.


### Do snapshots use a lot of space?


Generally, no. Because they’re deduplicated and compressed, snapshots of typical data consume very little additional capacity.


The exception: Data that’s already encrypted by the host or already compressed (video, for example) doesn’t reduce well. Snapshots of that kind of data will consume more space, because there’s nothing left to dedupe or compress.


### Do snapshots affect performance?


No, taking a snapshot has no impact on array or volume performance. The only operation that consumes meaningful resources is restoring a large volume—and even then, a restore is no more demanding than a normal write.


## Organize protection with protection groups


A protection group (pgroup) is where you define your protection policies. It’s the central construct for backups on FlashArray, and it handles two independent things:


- **Snapshot schedule:** How often snapshots are taken and how long they’re retained
- **Replication schedule:** How often snapshots are replicated and to which target


These are configured separately, so you can take frequent local snapshots while replicating on a different cadence. Members of a group can be volumes, hosts, or host groups.


**Turn on default protection.** Default protection automatically adds every newly created volume to a default pgroup, so nothing ever slips through unprotected because someone forgot to assign a policy.


### Recommended baselines


These are the minimums we recommend for every environment:


- Attach at least one enabled snapshot policy to every volume or directory.
- Take **at least one snapshot per day** .
- Retain snapshots for **at least seven days** .
- Configure pgroups to replicate **at least one snapshot per day** to a defined target.


Retention is unlimited in Purity//FA 6.6 and later. (Earlier versions cap retention at one year.) There’s rarely a good reason to retain less—the space cost of keeping more is low, and the recovery value is high.


## Lock it down with SafeMode


Snapshots are immutable, but by default, an administrator can still delete them. If that administrator’s credentials are compromised, your backups can be wiped right before or during an attack.[SafeMode](https://www.everpuredata.com/solutions/cyber-resilience/ransomware/safemode.html) ™ closes that gap.


Think of SafeMode like the airbags in your car. You hope you never need it, but when something goes wrong, it’s the thing standing between you and disaster.


SafeMode does two things:


- **Locks snapshots** so they can’t be deleted outside the policy’s automatic roll-off. No one—not even an admin—can delete a protected snapshot early.
- **Prevents policies from being weakened.** Snapshot frequency and retention can’t be lowered.


With ratcheted retention locks, protection can only be *increased* , never reduced. Combine that with disabling manual eradication, and even a compromised admin account can’t dismantle your backups. Unlocking SafeMode requires Everpure Support—it’s intentionally not something a single insider can undo.


This is what we mean when we call snapshots the last line of defense against ransomware: SafeMode guarantees the recovery point is still there when you reach for it. For a deeper walkthrough of setup and a real-world recovery, see “[How to Secure Your Data from Ransomware with SafeMode Snapshots](https://blog.everpuredata.com/products/how-to-protect-your-data-from-ransomware-with-safemode-snapshots/) .”


## Don’t keep all your eggs in one basket: 3-2-1


A snapshot sitting only on the array it protects is vulnerable to anything that takes out the array. A sound strategy spreads copies across locations—the classic[3-2-1 approach](https://www.everpuredata.com/knowledge/3-2-1-backup-strategy.html) .


On FlashArray, that looks like:


- **Keep some snapshots local.** Local snapshots offer the fastest recovery and the lowest RTO.
- **Replicate to another FlashArray** —for example, a high-capacity[FlashArray//C](https://www.everpuredata.com/content/dam/pdf/en/datasheets/ds-flasharray-c.pdf) ™ array.
- **Replicate to FlashBlade** for a different media tier.
- **Offload to the cloud with Purity**[CloudSnap](https://support.purestorage.com/bundle/m_pscd_for_aws/page/pure_storage_cloud/pscd_for_aws/topics/c_cloudsnap_02.html) ™—to AWS, Azure, or Wasabi—for an off-site, long-term copy.


For ransomware recovery specifically, it’s best to recover from a snapshot as close to the array as possible. The closer the copy, the faster you’re back online. For a step-by-step recovery playbook, see “[Recovering from a Ransomware Attack on Everpure FlashArray](https://blog.everpuredata.com/perspectives/ransomware-pure-storage-flasharray/) .”


## Harden eradication settings


Eradication controls are a core part of data protection, not an afterthought. Two settings matter most:


- **Disable manual eradication** so deleted volumes and snapshots can’t be permanently destroyed on demand.
- **Set an eradication delay** so anything deleted sits in a recoverable state for a window before it’s truly gone.


Together with SafeMode, these settings ensure that a mistake—or a malicious action—can be undone.


## Fit it into your existing backup ecosystem


FlashArray doesn’t replace your backup applications; it strengthens them. Snapshots and replication integrate with the major third-party backup platforms, including[Commvault](https://www.everpuredata.com/partners/technology-alliance-partners/commvault.html) ,[Veeam](https://www.everpuredata.com/partners/technology-alliance-partners/veeam.html) ,[Rubrik](https://www.everpuredata.com/partners/technology-alliance-partners/rubrik.html) , and Veritas, so you can layer FlashArray immutable, space-efficient snapshots underneath the tools your team already runs.


## The short version


If you do nothing else, do this:


1. Put every volume in a protection group, and turn on default protection so new ones are covered automatically.
2. Snapshot at least daily and retain at least seven days.
3. Enable SafeMode with ratcheted retention and disable manual eradication.
4. Keep a local copy for fast recovery, and replicate or offload at least one more copy off the array.


Snapshots are immutable, space-efficient, and instant to take. SafeMode keeps them safe from anyone who shouldn’t be touching them. Replication and CloudSnap keep a copy out of harm’s way. Put those together and you have backups you can actually count on when it matters most.


## Further reading


To dive deeper, check out the following blog posts:


- “[How to Secure Your Data from Ransomware with SafeMode Snapshots](https://blog.everpuredata.com/products/how-to-protect-your-data-from-ransomware-with-safemode-snapshots/) ”—SafeMode setup, operation, and a real-world recovery
- “[Recovering from a Ransomware Attack on Everpure FlashArray](https://blog.everpuredata.com/perspectives/ransomware-pure-storage-flasharray/) ”—containment, forensics, and restoring from SafeMode Snapshots
- “[Using Protection Groups with VSS Snapshots](https://blog.everpuredata.com/purely-technical/using-protection-groups-with-vss-snapshots/) ”—application-consistent snapshots with protection groups
- “[Restoring Protection Group Volume Snapshots](https://blog.everpuredata.com/purely-technical/updated-restoring-protection-group-volume-snapshots/) ”—recovering an entire protection group’s volumes


## Protect Backups from Ransomware with SafeMode


Learn how immutable snapshots, SafeMode, and a 3-2-1 backup strategy help strengthen ransomware recovery on FlashArray.


[Learn more](https://www.everpuredata.com/solutions/cyber-resilience/ransomware/safemode.html)
