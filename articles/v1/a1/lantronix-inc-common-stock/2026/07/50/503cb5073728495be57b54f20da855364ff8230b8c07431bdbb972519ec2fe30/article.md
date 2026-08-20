---
schema_version: "1.0.0"
document_id: "503cb5073728495be57b54f20da855364ff8230b8c07431bdbb972519ec2fe30"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/stop-logging-into-console-servers-automate-with-ansible/"
published_at: "2026-07-28T19:18:04+00:00"
first_seen_at: "2026-07-28T19:45:59.664126+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:b41b1d253a464d8adc7c6a016f7fb962c890dffca748eb5c6b81217fcf8ad44a"
---

# Stop logging into console servers & automate with Ansible

July 28, 2026


- [General](https://www.lantronix.com/blog/category/general/)


### Stop logging into console servers & automate with Ansible


*Meet **lantronix.oob*** *, a Red Hat–certified Ansible collection that puts the SLC 9000 console server and the Percepxion cloud platform inside the playbooks you already run.*


As a network admin, you already live in Ansible. You configure switches, push router baselines, manage firewall rules, and keep dozens of sites consistent and it’s all from version-controlled playbooks. So why is your out-of-band platform the one box you still log into by hand?


That gap has a cost. Every in-band change goes out through automation; your OOB platform never hears about it. Every time you need to verify the state of a console-server fleet, someone checks it by hand. And the moment a playbook needs to reach a device


*through*


the out-of-band path, you hit a wall.


The


[Lantronix SLC 9000 with Percepxion](https://www.lantronix.com/lantronix-products/lantronix-out-of-band-management/autonomous-out-of-band/)


closes that gap — and


[lantronix.oob](https://catalog.redhat.com/en/software/collection/lantronix/oob)


is how. It’s a certified Ansible collection that gives your existing playbooks direct, API-native access to both the SLC 9000 console server and the Percepxion cloud management platform. No web UI detour. No manual steps. Just YAML.


## What the collection is


**lantronix.oob**


is an Ansible collection published on Red Hat Automation Hub and certified for production use (current version


**1.0.21** ). Every module talks straight to the REST API — no CLI scripting, no screen scraping, no SSH piped through


` expect` .


SLC 9000 – 8 Modules Percepxion – 10 Modules


Device-level configuration you’d otherwise do by hand on every box.


Fleet-level reach into the cloud control plane.


` slc_facts` Firmware, model, serial, uptime


` percepxion_facts` Fleet summary & platform facts


` slc_users` Manage the sysadmin account


` percepxion_devices` Query & update device inventory


` slc_system` Hostname, description, reboot


` percepxion_import_devices` Bulk device registration


` slc_network` Ethernet interface configuration


` percepxion_projects` Device project assignments


` slc_config` Backup, compare, batch CLI, save


` percepxion_smart_groups` Dynamic device groups


` slc_firmware` Check version, trigger upgrades


` percepxion_firmware` Fleet compliance & upgrades


` slc_device_ports` Serial/console port configuration


` percepxion_config` Backup, restore, push at scale


` slc_managed_devices` Devices connected via serial ports


` percepxion_jobs` Job group lifecycle


` percepxion_audit_logs` Security & access log queries


` percepxion_telemetry` Device telemetry & history


**Every module is idempotent.** Run the same playbook ten times and the result is identical each time — nothing changes on a device that’s already in the desired state.


## Why this matters at scale


Enterprise networks aren’t getting simpler. Most admins are all-too-familiar with more sites, more devices, more compliance requirements, flatter headcount… Work that was tolerable to do by hand at 20 sites is unacceptable at 80. Yet traditional OOB platforms have historically been the exception to whatever automation discipline governs the rest of your stack. That model breaks. Three failure modes show up most often:


#### Configuration drift


#### The audit scramble


#### A disconnected stack


You set a standard NTP and syslog baseline. Three months later a site doesn’t match because someone changed something manually. Without Ansible, the only way to find out is to log into every device and check.


An auditor asks for proof that every OOB device runs a specific firmware and enforces MFA. If you can’t produce it programmatically, someone burns a week gathering it by hand. That’s not a posture. It’s a fire drill.


NetOps writes runbooks in Ansible. Ops builds workflows in ServiceNow. Monitoring talks to everything by API. But when something has to happen through the OOB path, the automation stops at the door.


The lantronix.oob collection answers all three. The SLC 9000 and Percepxion are


**API-first by design.** It


*works with what you already have* , slotting the OOB platform into the automation you already run instead of asking you to rip and replace.


## How it works


Both the SLC 9000 and Percepxion REST APIs run on OpenAPI (3.1 and 3.01 respectively). The modules handle authentication, request construction, and response parsing. You write YAML; the collection makes the calls.


**Install from Red Hat Automation Hub**


Your inventory defines the devices and connection parameters. Credentials live in **Ansible Vault** , never in plaintext.


Each module follows the same pattern to pass connection parameters and the desired state. The module checks current state, changes only what’s needed, and reports back.


Already at that hostname? The module reports


` ok`


and moves on. Not configured? It makes the change and reports


` changed` . Standard Ansible behavior. The Percepxion modules work identically by pointing at the Percepxion API instead of the device so the same playbooks that touch your SLC 9000s can query fleet state, trigger firmware jobs, and manage smart groups.


## Four playbooks that show the value


### 1 Security Baseline


#### Enforce a baseline across the entire fleet


Your security team publishes a baseline: consistent hostnames, standard NTP servers, syslog forwarding to a central collector. Without Ansible, enforcing that across 200 console servers means 200 manual sessions. With lantronix.oob, it’s one playbook.


**The value |** A two-day manual task becomes a five-minute automated run AND you can prove it was done.


### 2 Compliance check


#### Verify firmware before a maintenance window


Your change process requires every OOB device at the affected sites to be on a specific firmware before tonight’s window opens. You need to know which ones aren’t, and preferably without logging into each one.


**The value |** A day-long check now takes minutes, and the output is machine-readable for your ticketing system.


### 3 Security review


#### Pull an access audit trail before the quarterly review


Security needs a record of who touched what, and when, across the fleet before a periodic review without doing 150 separate login sessions to piece it together by hand.


**The value |** A machine-readable audit trail on demand andno one burnt a day exporting logs device by device.


### 4 Provisioning


#### Pre-populate Percepxion for new sites


You’re opening four data centers. Each SLC 9000 ships with zero-touch provisioning ready to call home to Percepxion. But you want the inventory registered and grouped by site


*before*


the hardware lands in the rack.


**The value |**


New-site provisioning becomes a planned step in your deployment pipeline and not cleanup after the hardware’s already racked.


## Certified, signed, and ready


This isn’t a community side project. The Lantronix collection is published and


**signed**


on Red Hat Automation Hub, with full module documentation, dependencies, and version history. It’s the same place your team already pulls other certified content.


*The lantronix.oob collection on Red Hat Automation Hub is published, signed, and certified.*


## This belongs in your pipeline


Because the collection uses Ansible Vault for credentials, it drops cleanly into GitHub Actions, Jenkins, or GitLab CI. Store the vault password as a pipeline secret and run the playbook as a stage. Baseline enforcement can run on a schedule or as part of a larger infrastructure pipeline so your OOB fleet stays in sync with your documented state without anyone remembering to kick off a check.


**Prefer asking over scripting?**


The[Percepxion MCP server](https://www.lantronix.com/blog/reach-any-device-in-your-network-just-ask/) puts the same API behind a conversational interface. Point an AI agent at a device and ask what’s wrong, instead of writing a playbook. Different entry point, same API-first foundation. Both MCP servers are live on GitHub:[slc-mcp-server](https://github.com/Lantronix/slc-mcp-server) and[percepxion-mcp-server](https://github.com/Lantronix/percepxion-mcp-server) .


---


## Getting started


The collection is available now on Red Hat Automation Hub (certified) and Ansible Galaxy. The repo includes a getting-started guide with complete, runnable playbook examples.


` $ansible-galaxy collection install lantronix.oob`


- [View the repo & docs](https://github.com/Lantronix/ansible-collection-oob)
- [Getting Started Guide](https://github.com/Lantronix/ansible-collection-oob)


Evaluating the SLC 9000 and Percepxion or planning an SLC 8000 migration before the December 2026 end-of-sale? Test the Ansible collection early. It’s where the out-of-band platform meets the automation stack you already run.


**Requirements |** The lantronix.oob collection requires Ansible Core 2.17 or later, Python 3.10 or higher, and network access to your SLC 9000 or Percepxion instance. *Autonomous Out-of-Band (AOOB)* — a dedicated, always-available management plane reaching every device independent of the production network is the foundation the SLC 9000 is built on.
