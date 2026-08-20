---
schema_version: "1.0.0"
document_id: "779ebe032710d052fccacadfa1847ad7485b42c644f126193a6522beed60b612"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/software-as-a-biotechnology-how-benchling-extensions-and-integrations-drive-life-sciences-rd"
published_at: "2020-01-28T16:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:94700c21258f945047e0ee68512bf71ee3b38ccef1d8dc2af9b238388bd83361"
---

# Software as a Biotechnology: How Benchling Extensions and Integrations Drive Life Sciences R&D

Modern life science R&D is producing data of a much greater quantity and complexity than ever before. R&D teams at small biotechs and large pharmas alike need a tech stack that allows them to flexibly extend software tools and and integrate them with one another and with lab hardware. Otherwise, scientists are stuck performing manual tasks, and data is fragmented and disconnected.


Benchling's Developer Platform makes it possible for companies to set up smart integrations with lab instruments and internal systems to automate and centralize data capture in Benchling. Here are five powerful ways our clients have developed custom software using our APIs to dramatically accelerate their research workflows.


## 1. Building Robot Integrations


Robotics has become a critical part of life sciences R&D. The emergence of new, complex drug modalities, desire to reduce costs associated with R&D, and highly iterative processes have contributed to the increased investments in robotics.


While lab instruments automate much of the menial work done by scientists, the end-to-end scientific workflow isn’t quite fully automated. There are many intermediate steps — from uploading instructions to the instruments, to downloading their results and feeding intermediate outputs between instruments. To truly increase workflow efficiency, lab hardware needs to be connected to lab informatics. With our Developer Platform, Benchling fully bridges the gap between hardware and software and alleviates the need for manual intervention. Two of the most powerful integrations we've helped build are with liquid handlers and automated freezers.


#### Liquid Handler Integration:[Benchling for Lab Automation](https://www.benchling.com/lab-automation/)


Liquid handlers are some of the most commonly used lab instruments, and they typically provide an interface for communicating with them automatically. Benchling provides in-product support for generating the input instructions. With Benchling for Lab Automation, our clients can ensure that the output is automatically ingested into Benchling to update existing samples and create new samples.


Typically, even with an automated liquid handler, scientists have to manually feed the processing instructions to the instrument and manually upload its results into their LIMS system. With an integration built with Benchling for Lab Automation, Benchling kicks off the whole flow in just one click, and results are automatically uploaded back into Benchling upon completion.


(Benchling for Lab Automation also supports integrations with analytical instruments. You can read more about it[here](https://www.benchling.com/lab-automation/) .)


#### Automated Freezer Integration


In addition to automatically processing samples, frozen storage of biological materials has become an essential activity in many laboratories. Another class of lab hardware is automated cryogenic storage systems: liquid nitrogen-based freezers that also expose a software interface through which actions can be triggered automatically.


While these machines are highly sophisticated, standard operation still requires human intervention to keep the real-world state of the samples in sync with informatics systems. In that case, a scientist's workflow for checking out samples from the freezer would involve a number of manual steps. They would request samples from the freezer, spend time waiting for the samples to be thawed and ready for retrieval, and then manually update their informatics systems.


With a Benchling integration, this process is made much more simple. Now, a scientist can just file a request for the samples in Benchling. Upon receiving the request, the freezer will automatically be notified to prepare the samples, and the Benchling records will be updated automatically upon retrieval. Not only does this reduce the number of steps required, it also ensures that all data is seamlessly kept up to date.


## 2. Uploading Results Automatically


Automating the setup of an experiment — getting the right samples, preparing the plates, and recording the input — is only half of the process. Once an experiment runs, the results of that experiment need to be analyzed and used to inform what should happen next. Benchling makes this easier by providing an API to automatically upload the results from an experiment to our platform, and it’s the second-most commonly used API that we offer.


Traditionally, this has been a pain. Take an ELISA, for example. One of our clients had a process that looked like this:


In this process, it's easy for scientists to accidentally paste in the wrong results or type in the wrong ID. Since the ELN and LIMS systems aren't connected, scientists could occasionally forget to upload the data in one of the two places.


With Benchling's Result Upload APIs, that client wrote a software program that automatically uploads their instrument data into Benchling, associating it with the input samples and allowing a user to insert a table into their Notebook with the data.


A scientist still has to interact with an instrument to run it, but after that, the integration handles most of the workflow. The integration detects when data has been produced by the instrument, looks up samples in Benchling by either ID or plate position, and uploads it into Benchling. From there, a scientist can open the Notebook entry with the relevant experiment and insert all of the uploaded data at once. This workflow leaves no room for forgetting or for manual error.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## 3. Supporting NGS Pipelines


Not all results, like NGS reads, make sense to directly upload to your LIMS or Notebook. The output of NGS machines is complex and virtually unusable until the data has been processed and analyzed. So much is involved — quality assessment, preprocessing, aligning, processing the aligned reads, etc. — that we almost always see a whole pipeline of software used to produce a useful result.


Benchling can’t do all of that analysis for you, but Benchling can conveniently put those results into context, tying them directly to samples and experiments. A number of our clients whose workflows heavily involve NGS work hook Benchling up to the end of their NGS pipeline.


The integrations they use aren’t so different from the integrations used to upload results automatically. Instead of uploading data after the instrument runs, it’s uploaded after the NGS processing and analysis pipeline finishes. Through Benchling’s APIs, the upload of the processed data becomes the final step in the NGS pipeline, after which a scientist can reference the data without opening up any other software.


The results that end up in Benchling are usually just the final results for the analysis — variant data, for instance. But the raw data is still accessible. We recommend that clients store the raw data in an archival system (offered by many existing NGS software tools) and that they upload a link to those data files in Benchling alongside the cleaned-up results. That way, all data is accessible through Benchling.


## 4. Syncing Data In and Out of Benchling


Benchling is a core system of record and the primary source of truth for scientific data for many scientists. It's not just results from experiments and data from lab instruments that need to be in Benchling. For our clients, keeping other software systems in sync with Benchling, and keeping Benchling in sync with those systems, is a common need. These systems may include public scientific data repositories or proprietary internal software.


All of these systems can easily get out of sync. Keeping them up-to-date manually almost always results in inaccurate data. Consequently, data becomes more fragmented, and even more time is required for manual data curation. It's an endless cycle that we've seen among our clients over and over again.


Several of our clients take advantage of the Benchling APIs to build integrations that pull data into or push it out of Benchling, ensuring that their systems are automatically kept up-to-date. Syncs can be triggered on a regularly scheduled cadence, in response to something in Benchling, or even as a one-off run — and in all cases, clients sync Benchling data in a format and structure catered to their custom needs.


## 5. Customizing Processes within Benchling


Some labs use Benchling’s APIs to automate and customize how Benchling itself works. With our Events System, it’s easier than ever to write an integration that triggers certain actions in Benchling based on other ones.


For example, your lab may have a rule: all tissue samples should be submitted for sequencing when they’re uploaded into Benchling. These rules are so lab-specific that we can’t build them into our core product, but they’re also critical to the labs that enforce them. If a sample isn’t sequenced, it delays everything downstream.


This is where our Events System comes in. With it, clients can write software that will be triggered whenever a tissue sample has been created. Once the integration detects this, it creates the appropriate request in Benchling. This simple integration is enough to ensure that a tissue sample will never be created without a corresponding request for sequencing.


Given the broad coverage of both our Events System and our APIs, integrations like this have allowed clients to customize almost every part of Benchling, kicking off chains of processes that are tailored to fit all the unique quirks and constraints of the lab that built it.


*Our Events System is currently in an early access stage. If you're a current customer who is interested in using it, please reach out to your Customer Success Representative.*


*— Neena Parikh leads Benchling's Customer Engineering team, and Reed Molbak is a Product Manager at Benchling.*


---


#### Bring together all of your R&D data with Benchling:


[Learn More](https://www.benchling.com/integration/)


\[rd_form\]
