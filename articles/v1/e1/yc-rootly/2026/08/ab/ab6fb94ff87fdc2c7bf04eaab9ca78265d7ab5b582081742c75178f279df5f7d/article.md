---
schema_version: "1.0.0"
document_id: "ab6fb94ff87fdc2c7bf04eaab9ca78265d7ab5b582081742c75178f279df5f7d"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/investigating-cve-2026-66066-at-rootly-with-rails-forensics-agent-skills"
published_at: "2026-08-04T12:00:00+00:00"
first_seen_at: "2026-08-06T00:18:12.995751+00:00"
fetched_at: "2026-08-06T00:18:14.293904+00:00"
content_hash: "sha256:5968eab6f8e58e60fa066b248272d00eba25bea411aa62a89b71a08015e8f0e5"
---

# Investigating CVE-2026-66066 with the Rails Forensics Agent Skills

On July 29, the Rails team[released security patches](https://rubyonrails.org/2026/7/29/Rails-Versions-7-2-3-2-8-0-5-1-and-8-1-3-1-have-been-released) for[CVE-2026-66066](https://discuss.rubyonrails.org/t/cve-2026-66066-possible-arbitrary-file-read-and-remote-code-execution-in-active-storage-variant-processing/91432) , a vulnerability in Active Storage’s libvips processing path. It had been discovered independently by researchers at[Ethiack](https://ethiack.com/) and[GMO Flatt Security](https://flatt.tech/) , who called it KindaRails2Shell.


At[Rootly](https://rootly.com/) , my team and I patched the affected Rails version that day. The upgrade closed the vulnerable path, but it did not settle the issue.


What if somebody had already used the vulnerability to read application secrets? I'd need to rotate credentials, invalidate sessions and investigate access to connected systems. But doing all that without some evidence would just be a big disruption.


So I had to dig deeper. And that meant examining tens of thousands of candidate blobs, then correlating the results with database records and telemetry looking for evidence. Thankfully, Claude handled much of the heavy lifting. (Spoiler: Rootly was never at risk, phew.)


## What is KindaRails2Shell


KindaRails2Shell works because Rails, libvips, libmatio and HDF5 interpret different parts of the same file. During a direct upload, Rails can trust the client-supplied content type, so an attacker can declare that a blob is an image even when its bytes describe something else. Active Storage later uses that value to allow image-variant processing.


Libvips makes its own decision from the bytes. The crafted file identifies itself as MATLAB Level 5, selecting the MATLAB loader. Libmatio then finds MAT 7.3 in another header field and hands the file to HDF5. An external HDF5 dataset can point to a local path such as` /proc/self/environ` and use those bytes as its contents.


Libvips still believes it is processing an image. It renders the dataset, turning the contents of the local file into pixel values. The attacker receives an image and extracts the secret from its pixels.


Anything readable by the Rails process is potentially useful:` config/master.key` , database credentials, storage keys or the process environment. The right secrets could allow an attacker to move beyond file disclosure into session forgery, access to external systems or remote code execution.


The mechanism also leaves forensic evidence. The first 128 bytes must claim MATLAB Level 5 in one field and MAT 7.3 in another, a combination the toolkit says legitimate MATLAB writers do not produce.


Successful processing can leave three related artifacts: the crafted source blob, an Active Storage variant record showing that it was processed, and the rendered image containing the returned bytes.


That third artifact is unusual. The material sent to the attacker may still be present in the application’s object store as pixels. An investigation may be able to establish not only that the exploit ran, but which local path it targeted and which bytes came back.


## An agentic forensics toolkit


Shortly after the release, DHH[posted a link](https://x.com/dhh/status/2083193882347487242) to[rails-forensics-CVE-2026-66066](https://github.com/rails/rails-forensics-CVE-2026-66066) , a toolkit created from a production investigation at 37signals.


I expected a scanner and a runbook. Instead, the repository was organized around two agent skills:


The first instruction in` AGENTS.md` is: “This repository is not the application under investigation.” Without that warning, an agent could inspect the toolkit itself and return a clean but irrelevant result.


The remaining instructions showed the same attention to investigative mistakes. They separated the toolkit, target application and evidence directory; required deployment dates instead of commit dates; prohibited the agent from executing production commands; and documented what a clean result could and could not establish.


The repository explained why each step existed, not simply which commands to run. I pointed Claude Code at the toolkit, gave it the path to Rootly’s application and started the investigation.


## Modeling Rootly’s potential exposure


Before scanning anything, Claude reconstructed the conditions under which the known exploit could have worked against Rootly.


It traced when Rootly began using libvips, whether the required upload path was reachable, whether Active Storage variant tracking was enabled and when the fixed Rails version reached production. It also prepared a production check for the` matload` operation because the available libvips loaders depend on how the production library was compiled.


The exposure window had to come from deployments, not commits. Libvips created no exposure before serving traffic, and the Rails upgrade closed nothing before reaching production. Commit dates could have excluded real activity while still producing a successful scan.


The upstream scanner narrows its search through` active_storage_variant_records` . It starts with transformed blobs inside the exposure window, removes those attached to ordinary application records and reads the first 128 bytes of the remaining candidates from object storage. It then checks for the two incompatible header fields required by the known attack.


Rootly’s application differed from the scanner’s assumptions. It soft-deletes Active Storage records, so ordinary queries could hide forensic material. Claude adapted the relevant relations with` with_deleted` while preserving the different roles of source blobs, attachments and variant records.


Rootly also uses PostgreSQL, while one timeout-related query reflected a MySQL environment. Finally, the expected database replica role was not configured the same way at Rootly.


Claude modified a separate copy of the scanner and documented each change. More importantly, it found two additional code paths where Rootly passed files to libvips outside the normal Active Storage variant pipeline.


Those paths would not necessarily create the variant records used by the scanner. Claude traced their database records and prepared separate queries for every file they processed during the exposure window.


The scanner had not failed to cover its stated scope. Rootly’s application had additional scope, and the skill made that boundary explicit.


## Investigation with a human in the loop


Claude prepared a bounded, read-only ECS task that downloaded the adapted scanner from S3. Before I launched it, I verified the Rails environment, database connection, storage service, date boundaries and candidate range. A wrong environment or window could produce a plausible clean result. The task then worked through tens of thousands of blobs; most of the four-hour investigation was waiting for that sweep. Once it was running, I went to the gym.


While the sweep ran, I used Datadog to examine evidence the database could not preserve on its own.


A rejected upload creates no blob and is invisible to the scanner. An unattached blob may not identify the user behind it. An attacker could also perform the read, attach the crafted blob by signed ID and then delete or replace the attachment, causing Active Storage to purge the source, variant record and rendered output.


Claude had already identified the Rootly endpoints that accepted existing blobs by signed ID. I searched retained logs for the corresponding update and destroy requests, along with rejected uploads, unusual volumes and scripted user agents.


I found no use of the relevant cleanup path during the period covered by Rootly’s logs. That conclusion could not extend beyond the retention window, but it ruled out one way evidence might have disappeared during the observable period.


## Interpreting 1000s of blobs


The scanner examined tens of thousands of candidate blobs and found zero files carrying the incompatible header pair required by the known KindaRails2Shell exploit.


It also reported a few hundred objects whose database records remained but whose S3 objects no longer existed. I counted them as neither malicious nor clean. Claude analyzed their metadata, while my team and I reviewed every file from the two non-standard libvips paths. Neither population contained evidence of the known attack.


The final determination was specific:


- No retrievable object selected by the Active Storage sweep carried the known crafted header pair.


- The additional libvips processing paths contained no evidence of the known attack.


- Rootly’s retained logs showed no use of the identified cleanup path during the period they covered.


- Some missing storage objects could not be classified.


That is strong evidence that the known exploit was not used against Rootly. It is not proof that exploitation was impossible.


The method could not detect an unknown variation, classify objects missing from storage or extend log conclusions beyond retention. A clean result is strong evidence, not proof.


I found that qualification useful. It connected each conclusion to a specific artifact or coverage limit. After roughly four hours, I had an answer I could defend without making it stronger than the investigation allowed.


## A security investigation as agent skills


The scanner detects the known exploit structure. The agent skills carry the investigative method around it.


They define which application version to inspect, how to derive the exposure window and which facts require production confirmation. They explain why variant records matter, how they could disappear and which paths fall outside the scanner. They also require the final report to name missing objects and unobservable periods.


The determination rubric is particularly useful. A small read from` /etc/hostname` by an attributable researcher points in one direction. Contiguous reads from` config/master.key` ,` .env` or` /proc/self/environ` point in another.


Timing near disclosure and low volume do not establish benign research. Missing logs make attribution unavailable, not harmless.


Those are rules for reasoning about evidence rather than parsing files.


Claude could adapt the scanner because the skill explained what its queries were intended to establish. It could identify the custom libvips paths because the skill defined the scanner’s boundary. It could qualify the final result because the documentation distinguished negative evidence from missing evidence.


Claude still needed Rootly’s codebase and a human operator. The production decisions and final determination remained mine; the skill supplied the context an experienced investigator would normally bring.


## The new way of handling CEVs


The Rails response included patched releases, a detector, two agent-guided investigations and technical references explaining the attack and its limits.


The[repository](https://github.com/rails/rails-forensics-CVE-2026-66066) deliberately omits a working payload generator. Its tests synthesize only the header bytes needed to validate the detector, which was also checked against researcher-provided files from the 37signals investigation.


Both[Ethiack’s assessment](https://ethiack.com/info-hub/research/kindarails2shell-rails-rce-cve) and[GMO Flatt Security’s write-up](https://blog.flatt.tech/entry/kindarails2shell_rails) warned that attackers could likely reconstruct a proof of concept quickly with AI once the patches became public. Full technical disclosure was planned for[August 28, 2026](https://discuss.rubyonrails.org/t/cve-2026-66066-possible-arbitrary-file-read-and-remote-code-execution-in-active-storage-variant-processing/91432) , but operators needed a forensic method immediately.


Not every CVE supports this kind of investigation. KindaRails2Shell leaves an identifiable source file, a processing record and potentially the returned bytes.


For vulnerabilities with similar consequences, maintainers should consider the forensic question alongside the patch: how can operators determine their real exposure window, distinguish an attempt from successful exploitation and understand the limits of a clean result?


Those answers often exist in maintainer notes, research methodology or the first production investigation. The Rails community packaged them in a form an agent could carry into Rootly’s application.


That is the part of this response I hope other framework communities copy.
