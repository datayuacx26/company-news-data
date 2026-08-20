---
schema_version: "1.0.0"
document_id: "ed254bee5bb1d66cb965788c125be08559efc37a08643a305b9e03c1c818925e"
company_key: "yc-quivr"
company: "Quivr"
source_id: "yc-quivr-news-import-c3f3e0926723"
canonical_url: "https://www.quivr.com/blog/quivr-s-new-knowledge-management-system"
published_at: null
first_seen_at: "2026-07-27T04:35:10.435547+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:596aa4a9729ee5b157a1f478c3d81c465db3750b625930545454ef5a38d7585a"
---

# Advantages

Feature


### Quivr's new Knowledge Management System


We just released our latest and greatest feature: KMS or Knowledge management system. It was hard work but we think it is the way forward. Think of it as your Google Drive but connected to Dropbox, Sharepoint, Notion and other Google Drives. All in one place and where you can chat with your documents. ‍


# **Advantages**


Unified view of all your knowledge! Here are the main points:


- You can have a better view of **ALL your knowledge** in Quivr from a **single place**
- Asynchronous processing of your files
- Flexibility and control over what goes into your Quivr brains :


# **Here is why we love it:**


Yes, it’s going to be a list of a lots of thing. But this is why we love it.


# **Local files**


- Folders:
- Files:
- You can see the hierarchy of your knowledge in the left panel
- You can move files and folders into folders by drag and dropping them into folders or subfolders
- You can drag and drop files and folders to left panel
- You can navigate through your knowledges by left panel or using the to bar ( to back one parent up)


# **Sync Files : Gdrive, dropbox, Sharepoint …**


- Browse through your remote files from **Quivr**
- Connect/disconnect files and folders to your brains
- Single view over local and remote files
- Immediate hint to see which knowledge are in you brains
- Syncs are read-only from our side :
- Click on sync file should open up the knowledge in new tab of browser
- You can’t move files from your Syncs to Quivr.


# **Link to your brains:**


- You can add files/folders to **one or more brains**
- You can add brains from KMS
- You can connect knowledge to KMS:
- Knowledges are **asynchronously processed** when first added to a brain
- Clicking on the brain emoji takes you the **brain page**
- You can drag and drop local knowledges/folders to folders on the left menu or on the current folder explorer


# **KMS View from Brains**


- Brain` Knowledges` tab now has a` My Knowledge` toggle. Toggle it to get a view into the KMS
- You can add the knowledges from the KMS to this brain by clicking the (+) button
- You can remove the knowledges from the KMS to this brain by clicking the (-) button


# **Technical details (for techies) on why it’s 🔥**


- linking folders to a brain should process all files and folder in it recursively
- Adding knowledge (either file or folder) to a folder that **is already linked to some brains** will process the knowledge and **link the knowledge and its children to the parent brains**
- Moving knowledge cross folders retains the knowledge brains AND adds the target folder brains to the knowledges.
- Unlinking processed knowledge doesn’t *unprocess* them → Relinking them to brain is instantaneous
- Error in processing one file in folder doesnt block all files in folder
- Processing files is atomic: either we succeed : PROCESSED or we error ERROR
- File processing is parallelized over Workers
- Sync files are reprocessed on update every 8 hours
- Redis for caching list of files from Syncs with a TTL of 1min. Browsing your sync should be faster when served from cache


# **Coming Soon Features (KMS V1)**


- Keyword search in KMS (per knowledge source at first, then a generic search)
