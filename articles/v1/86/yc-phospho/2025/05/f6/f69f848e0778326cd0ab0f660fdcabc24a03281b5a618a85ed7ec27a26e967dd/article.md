---
schema_version: "1.0.0"
document_id: "f69f848e0778326cd0ab0f660fdcabc24a03281b5a618a85ed7ec27a26e967dd"
company_key: "yc-phospho"
company: "phospho"
source_id: "yc-phospho-rss-5d0953ae6f5a"
canonical_url: "https://blog.phospho.ai/repair-merge-and-split-lerobot-datasets/"
published_at: "2025-05-23T16:46:59+00:00"
first_seen_at: "2026-07-25T18:58:01.685792+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:7cab34434c95a3abc2bdfabbf41a8f3fe511dd9956c25caf76fb30a3eea79ac9"
---

# Repair, merge and split LeRobot datasets

**TL;DR** : You can now repair, merge and split LeRobot datasets with phospho. Works in the dashboard or in Python.


### The Challenges of LeRobot Datasets


While LeRobot simplifies dataset creation, it has some notable limitations:


- **No way to delete faulty episodes:** If a demonstration or experiment fails, you’re stuck with the bad data. There’s no built-in mechanism to remove it, which can degrade the quality of your dataset.
- **No merging or splitting:** Combining multiple datasets or breaking a large dataset into smaller subsets isn’t supported, making it hard to scale or experiment efficiently.
- **Corrupted datasets:** Issues like camera disconnection can leave datasets unusable, with no easy fix.


These pain points can turn dataset management into a nightmare, leaving users with no choice but to record a new dataset from scratch.


### Phospho v0.3: Fixing the LeRobot dataset


Enter Phospho, the open-source platform finally fixing this issue. Phospho extends the LeRobot format with powerful tools to streamline dataset management, all while staying 100% compatible with LeRobot. With Phospho, you can:


- Repair corrupted datasets
- Delete unwanted episodes
- Merge multiple datasets
- Split datasets, for instance to do a train/test split


Available today in its open-source version, Phospho empowers you to take control of your robotics data—whether through a Python interface or a user-friendly dashboard.


## Try it today


Download phospho and try to repair, merge or split your first LeRobot dataset today. Learn more in the[docs](https://docs.phospho.ai/welcome?ref=blog.phospho.ai) or the[open-source repo](https://github.com/phospho-app/phosphobot?ref=blog.phospho.ai) .


Don't have a dataset? Just grab one of ours to test the pipeline[here](https://huggingface.co/phospho-ai?ref=blog.phospho.ai) .
