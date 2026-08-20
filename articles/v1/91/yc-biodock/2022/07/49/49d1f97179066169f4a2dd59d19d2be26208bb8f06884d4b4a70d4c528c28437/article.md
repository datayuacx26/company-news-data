---
schema_version: "1.0.0"
document_id: "49d1f97179066169f4a2dd59d19d2be26208bb8f06884d4b4a70d4c528c28437"
company_key: "yc-biodock"
company: "Biodock"
source_id: "yc-biodock-rss-567b1dbd4991"
canonical_url: "https://blog.biodock.ai/biodock-coming-out-of-beta/"
published_at: "2022-07-05T14:00:00+00:00"
first_seen_at: "2026-07-24T19:19:16.186046+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:8a41ee07662852d89166ebdbd51680701c065ad5d03327d67adfcfcf7a8c8d23"
---

# Biodock's end-to-end deep AI platform is coming out of beta

The Biodock team is incredibly excited to announce that we are coming out of beta today, unveiling our end-to-end deep AI platform for biological images. Biodock makes it an afternoon's work to train powerful custom AI models without code, and run them easily on a platform that brings data and results together.


0:00


/


Deep AI brings undeniable advantages over traditional image analysis for R&D teams - we've seen it first hand all across our early academic and enterprise users. Radically improved accuracy and consistency in analyzing image-based data accelerates new discovery.


However, getting practical wins from deep AI presents difficult **software** challenges. To name just a few:


- Labeling and managing data
- Training and scaling optimized AI models
- Interpreting results in the context of experiments
- Intuitive graphical interfaces for scientists.


These challenges often force otherwise stellar teams to use old techniques like thresholding, off-the-shelf software, or even drawing regions of interest by hand.


A palette that shows a small fraction of applications that Biodock can work for. Each image is split into segmented and unsegmented halves


## Making deep AI work for the scientist


Biodock is looking to change that - we think that if you can see something in your images, you should be able to quantify it across groups with a high degree of accuracy. Biodock makes it easy for any scientist to train a custom deep AI pipeline powered by the latest vision transformer models, without any installation or code.


We've built our platform from the ground up to work with the workflows, data formats, and visualizations that scientists need, with best-in-class compute infrastructure that can accelerate AI analysis by up to 3000x.


0:00


/


So what can you use it for? To give just a few examples, you could build a one-click, sharable, state-of-the-art pipeline to segment and quantify metrics for:


- Phase-contrast image of dendritic cells in mice
- Out-of-focus or artifact regions for quality control
- Collagen and blood vessels in H&E stained images
- Fluorescent epithelial layers and their intra-epidermal neurons


and anything else that you can see in your images!


# Train custom AI models in two steps


### 1. Create a training set by labeling in our collaborative platform.


Easily add your team or collaborators in a project in order to label in our seamless labeling tool. Create region-level or object-level annotations by using our click and drag pen on a zoomable image view.


0:00


/


### 2. Fill out a few fields and click train


Fill out a form that includes class selection, and questions that help us choose the right settings (like augmentations and test sets) for your images. We give you errors and warnings that guide you to the best performing model.


Your model trains across 4 GPUs, finishing in a few hours, and gets dropped ready for analysis into Biodock.


# More than just an AI model


Once an AI pipeline is trained using Biodock, it is supercharged in our web platform. Pipelines come with a results dashboard, best-in-class parallelized scalability, and a full-featured filesystem that connects with your data, wherever it lives. Collaborate easily, by sharing your pipelines and files with your colleagues in-platform.


## Run in one click - scale to any number or size of image


Choose files or folders from the Datastore, select your trained AI pipeline, and just click submit. **Zero configuration or parameters are needed.** Easily share your pipeline with anyone within your organization or beyond.


0:00


/


Your analysis job is distributed across Biodock’s autoscaling cluster, **executing AI analysis up to 3000x faster than if you ran locally** . We spin up a machine per image, and then further subdivide each image for inference, splitting each image and then stitching it back together.


## Interpretability built in


For any AI pipeline, once an analysis finishes on your images, you can open the run up into the Results Dashboard.


In this case, a powerful cell segmentation pipeline was created. For each cell, you can interrogate morphological characteristics such as: X/Y position, area, minor/major axis, orientation, roundness, solidity, channel expression (for multichannel images), and more. **Every single detected object is mapped both to a dynamic scatter plot and to an image viewer** , so you can see exactly how the AI analysis performed and what was considered an object.


If there are any objects that were incorrectly labeled or missing, our QC tool allows you to quickly edit any objects to pixel-perfect accuracy, recomputing results and storing a history of all edits.


0:00


/


We’ve also taken inspiration from flow cytometry, with a full-featured gating tool that allows you to gate by rectangles, thresholds, freeform, image name, and statistics to create child populations. **You’re never locked in** : at any time, download a raw CSV of the data output, sub-populations, graph output, or image viewer output.


When you create hierarchies of experimental in Biodock and run that folder, we save those hierarchies and make them comparison groups in your results, allowing you to easily compare experiments against each other with a single click


This user is only looking at their negative control, and comparing their replicates.


## Connected to your data


Biodock features a full filesystem that will be familiar to anyone who has used Google Drive before. Create folders, share images between scientists, and organize your imaging data easily. Unlike Google Drive however, the Biodock filesystem **can open specialized formats** like Zeiss or Leica natively, even scaling to multi GB slide images for pathology. Behind the scenes, we generate tiled visualizations to power an **efficient, streaming image viewer in the browser** that works smoothly for even the largest images.


0:00


/


And with native integrations for Amazon S3, Google Drive, Dropbox, Box, and OneDrive, Biodock hooks into your data wherever it lives. All of your data is encrypted in-flight and at rest, in a platform backed by the gold standard for cloud security, **SOC2 Type II** certification.


# Getting started


If you don’t have time to label, we have labelers who can help you. I **f you’re an academic, everything is totally free to use** . And if you are from a commercial organization, you’ll see exactly how accurate your AI pipeline is on your images before you pay a dime.


## Get started free with training


Sign up directly from our website -[www.biodock.ai](https://www.biodock.ai/?ref=blog.biodock.ai)


## Get a demo or introductory call


Sign up for a demo or intro call[here](https://www.biodock.ai/contact?ref=blog.biodock.ai) .
