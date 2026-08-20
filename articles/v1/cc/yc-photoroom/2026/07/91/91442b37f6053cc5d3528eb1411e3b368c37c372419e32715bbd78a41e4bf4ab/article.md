---
schema_version: "1.0.0"
document_id: "91442b37f6053cc5d3528eb1411e3b368c37c372419e32715bbd78a41e4bf4ab"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/mutagen-tutorial"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:4af07e649e56a0e604edb2b7068663d49ac454af503b02d868360ff0f04e7602"
---

# Mutagen tutorial: syncing files easily with a remote server

What if you could enjoy the computing power of a remote server in the comfort of your laptop? In this article, I'll show you how to use[Mutagen](https://mutagen.io/documentation/synchronization) to enable **bidirectional sync between your local computer and a remote server** . Every time you edit a file on either computer, it'll be synced to the other.


To get this running, **we only need to install Mutagen on our local machine** . Installations instructions are[available here](https://mutagen.io/documentation/introduction/installation) , for Mac OS for instance:


```text
brew install mutagen-io/mutagen/mutagen
```


## Starting a sync


To start a sync, simply run:


```text
mutagen sync create --name=backend ~/Documents/backend  [email protected]  :/home/user/backend
```


To monitor sync constantly you can use:


```text
mutagen sync monitor
```


Sync conflicts can occur from time to time. To resolve them simply delete the file from the host or the target. You can list conflicts by running


```text
mutagen sync list
```


## Creating a config file


By default, Mutagen will sync everything. While you can run it with arguments, I'd suggest using a config file. Create a file in` ~/.mutagen.yml` and add the following content:


```text
sync:
defaults:
ignore:
vcs: true
paths:
- "node_modules"
- "*.ckpt"
- ".DS_Store"
- "__pycache__"
- ".idea"
- ".ipynb_checkpoints"
```


This way, you'll be able to handle version control on your local machine and you'll avoid sync conflicts with git files.
