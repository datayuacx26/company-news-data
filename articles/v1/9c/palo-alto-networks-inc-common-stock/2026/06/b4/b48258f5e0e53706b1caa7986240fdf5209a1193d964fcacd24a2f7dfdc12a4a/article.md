---
schema_version: "1.0.0"
document_id: "b48258f5e0e53706b1caa7986240fdf5209a1193d964fcacd24a2f7dfdc12a4a"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/new-macos-artifact-discovered/"
published_at: "2026-06-12T22:00:14+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:f43bc3f55c81815d29a1b1ac796a8c0fd68fcc8a89935df2fdb3701f6cb264b5"
---

# Tracing Digital Intent: New MacOS Tahoe 26 Artifact Discovered

## Surfacing a New Artifact


Forensic examiners are constantly hunting for data that reveals not just what happened on a system, but the user's intent behind it. With the release of macOS Tahoe 26, a new artifact has surfaced that provides exactly this level of granularity. We have identified a new Biome stream, App.MenuItem


, which logs specific menu selections made by users across the operating system.


This artifact offers a step-by-step record of user actions — from compressing files to emptying the trash — providing critical context for user activity across the operating system. This blog outlines where to find this artifact, how to process it and what stories the data can tell.


## Apple Biome – A Gold Mine for Forensic Investigators


The Apple Biome system has long been a gold mine for forensic investigators, tracking everything from app usage to media consumption. In macOS Tahoe 26.x, Apple appears to have introduced a new stream specifically designed to track menu selections, likely to facilitate user suggestions or learning behavior.


### Location and Structure


The artifact is located at ~/Library/Biome/streams/restricted/App.MenuItem/local


. Unlike simple logs, this file contains SEGB-encapsulated protobuf entries. SEGB is the file format used by the Biome. While this format requires specific tooling to parse, the payoff is significant. The stream captures the exact text of menu items selected by the user, along with the timestamp of the activity, providing a narrative of their interaction with the interface.


### Parsing the Artifact


Because standard forensic tools may not yet parse this specific stream, examiners can utilize open-source tools like ccl-segb


to extract the raw data. In our testing, this artifact is not parsed by the most common commercially available digital forensic tools available.


To process the file:


1. Export the file(s) from the directory ~/Library/Biome/streams/restricted/App.MenuItem/local


.
2. Run the[ccl-segb Python script](https://github.com/cclgroupltd/ccl-segb) : python ccl_segb_cli.py <exportedfilename> > outputfilename.txt


.
3. Convert the resulting text output into a CSV format for easier filtering and analysis using a Python script.


### Analyzing User Intent


The true value of App.MenuItem


lies in its ability to reconstruct a user's workflow. Where a file system event might simply show a file was deleted, this artifact can show the deliberate action of selecting "Move to Trash" followed by "Empty Trash.”


Consider the following sequence of events observed in our sample analysis:


- **18:32:37** : The user navigates using Go > Go to Folder


… in Finder.
- **18:36:59** : In TextEdit, the user selects File > Save…, followed by typing "u42validation".
- **18:37:54** : The user highlights a folder named "stolendata" and selects Compress “stolendata”


.
- **18:38:19** : The user selects Move to Trash


.
- **18:38:41** : The user interacts with the Dock to select Empty Trash


.


In this scenario, we see a clear pattern: data creation, compression (likely for exfiltration) and subsequent cleanup. We even see interaction with specific UI elements, such as Copy


and Paste Item


later in the timeline.


### Limitations


While powerful, this artifact is not without limitations. It relies on the menu item text itself. If a menu option does not explicitly contain the file or folder name (e.g., a generic "Open" command vs. "Compress 'Report'"), the specific target of the action might not be visible in this stream alone. However, when correlated with file system logs, App.MenuItem


provides the "human" context that technical logs often miss.


## Final Thoughts


The discovery of the App.MenuItem


artifact in MacOS Tahoe 26 adds a powerful new layer to forensic investigations. By capturing the specific menu choices a user makes, examiners can reconstruct digital intent with greater precision than before. Whether you are investigating data exfiltration or trying to understand a sequence of events, this Biome stream provides a narrative view of user behavior.


As macOS continues to evolve, so must our forensic methodologies. We encourage all examiners working with Tahoe images to verify if this artifact is present and incorporate it into their standard analysis workflows.
