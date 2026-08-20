---
schema_version: "1.0.0"
document_id: "146c8a9a688657dca6686a791b3a792ba11790e31015688fc79202da3a42080c"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/sam-2-gpt-4o----cascading-foundation-models-via-visual-prompting----part-1"
published_at: "2024-08-02T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:0de24fcc027229b82371aaaba2ae1e0633a80cb6fa27a0c0d54c1e481904f669"
---

# SAM 2 + GPT-4o — Cascading Foundation Models via Visual Prompting — Part 1

In Part 1 of this article we introduce Segment Anything Model 2 (SAM 2). Then, we walk you through how you can set it up and run inference on your own video clips.


‍


🔥 Learn more about visual prompting and RAG:


- CVPR 2024:[Foundation Models + Visual Prompting Are About to Disrupt Computer Vision](http://www.tenyks.ai/blog/cvpr-2024-foundation-models-visual-prompting-are-about-to-disrupt-computer-vision)
- [RAG for Vision: Building Multimodal Computer Vision Systems](http://www.tenyks.ai/blog/rag-for-vision-building-multimodal-computer-vision-systems)


‍


## Table of Contents


1. What is Segment Anything Model 2 (SAM 2)?
2. What is special about SAM 2?
3. How can I run SAM 2?
4. What’s next


‍


## 1. What is Segment Anything Model 2 (SAM 2)?


‍ **TL;DR:**


SAM 2 can segment objects in any image or video without retraining.


‍


Segment Anything Model 2 (SAM 2) \[1\] by Meta is an advanced version of the original Segment Anything Model \[2\] designed for object segmentation in both images and videos (see Figure 1).


‍


Figure 1. A pedestrian (blue mask) and a car (yellow mask) are segmented and tracked using SAM 2


‍


Released under an[open-source Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0) , SAM 2 represents a significant leap forward in[computer vision](http://www.tenyks.ai/blog/the-foundation-models-reshaping-computer-vision) , allowing for real-time, *promptable* segmentation of objects.


‍


SAM 2 is notable for its accuracy in image segmentation and superior performance in video segmentation, requiring significantly less interaction time compared to previous models: **we show how SAM 2 required 3 points to segment objects across an entire video!**


‍


Meta has also introduced the SA-V dataset alongside SAM 2, which features over 51,000 videos and more than 600,000 masklets. This dataset facilitates its application in diverse fields such as medical imaging, satellite imagery, marine science, and content creation.


‍


### 1.1 SAM 2 features summary


The main characteristics of SAM 2 are summarized in Figure 2.


Figure 2. Main features of Segment Anything Model 2 (SAM 2)


‍


## 2. What is special about SAM 2?


What’s novel about SAM 2 is that it addresses the complexities of video data, such as object motion, deformation, occlusion, and lighting changes, which are not present in static images.


‍


This makes SAM 2 a crucial tool for applications in mixed reality, robotics, autonomous vehicles, and video editing.


‍


Figure 3. SAM 2 in action: the ball is removed from the original video (top left), and a new video with no ball is created (bottom right) ([Source](https://x.com/AIWarper/status/1819073143287550022) )


‍


SAM 2’s key innovations are:


1. **Unified Model for Images and Videos** : SAM 2 *treats images as single-frame videos* , allowing it to handle both types of input seamlessly. This unification is achieved by leveraging *memory* to recall previously processed information in videos, enabling accurate segmentation across frames.
2. **Promptable Visual Segmentation Task** : SAM 2 generalizes the image segmentation task to the video domain by taking input prompts (points, boxes, or masks) in any frame of a video to define a spatio-temporal mask (masklet). It can make immediate predictions and propagate them temporally, refining the segmentation iteratively with additional prompts.
3. **Advanced Dataset (SA-V)** : SAM 2 is trained on the SA-V dataset, which is significantly larger than existing video segmentation datasets. *This extensive* ***dataset*** *enables SAM 2 to achieve state-of-the-art performance in video segmentation.*


‍


## 3. How can I run SAM 2?


You can either check SAM 2[repository](https://github.com/facebookresearch/segment-anything-2) or setup your model on your own machine using[this Jupyter Notebook](https://drive.google.com/file/d/1KJZOWyU0W5afGADSsiGwRhWerATd1VNt/view?usp=sharing) . In this section we describe the latter approach.


‍


### 3.1 Pre-requisites


- A machine with a GPU
- A library to extract frames from a video (e.g., ffmpeg)


‍


### 3.2 Setup


```text


import os
HOME = os.getcwd()


# Clone the repository
!git clone https://github.com/facebookresearch/segment-anything-2.git
%cd {HOME}/segment-anything-2


# install the python libraries for "segment-anything-2"
!pip install -e . -q
!pip install -e ".[demo]" -q


```


‍


### 3.3. Download SAM-2 checkpoints


We’ll only download the largest model but there are smaller options available too.


```text


!wget -q https://dl.fbaipublicfiles.com/segment_anything_2/072824/sam2_hiera_large.pt -P {HOME}/checkpoints


```


‍


### 3.4 Create a predictor


```text


from sam2.build_sam import build_sam2_video_predictor


sam2_checkpoint = f"{HOME}/checkpoints/sam2_hiera_large.pt"
model_cfg = "sam2_hiera_l.yaml"


predictor = build_sam2_video_predictor(model_cfg, sam2_checkpoint)


```


‍


### 3.5 Extract the frames from your video and explore the data


```text


# Extract the frames
video_path = f"{HOME}/segment-anything-2/SAM2_gymnastics.mp4"
output_path = f"{HOME}/segment-anything-2/outputs/gymnastics"
!ffmpeg -i {video_path} -q:v 2 -start_number 0 {output_path}/'%05d.jpg'


video_dir = f"{HOME}/segment-anything-2/outputs/gymnastics"


# scan all the JPEG frame names in this directory
frame_names = [
p for p in os.listdir(video_dir)
if os.path.splitext(p)[-1] in [".jpg", ".jpeg", ".JPG", ".JPEG"]
]
frame_names.sort(key=lambda p: int(os.path.splitext(p)[0]))


# take a look the first video frame
frame_idx = 0
plt.figure(figsize=(12, 8))
plt.title(f"frame {frame_idx}")
plt.imshow(Image.open(os.path.join(video_dir, frame_names[frame_idx])))


```


‍


Figure 4. In this stage we simply explore the first frame of our video


‍


### 3.6 Define the objects to segment using coordinates


We define a function to help us provide a list of *x, y* coordinates:
‍


```text


def refine_mask_with_coordinates(coordinates, ann_frame_idx, ann_obj_id, show_result=True):
"""
Refine a mask by adding new points using a SAM predictor.


Args:
coordinates (list): List of [x, y] coordinates,
e.g., [[210, 350], [250, 220]]
ann_frame_idx (int): The index of the frame being processed
ann_obj_id (int): A unique identifier for the object being segmented
show_result (bool): Whether to display the result (default: True)
"""
# Convert the list of coordinates to a numpy array
points = np.array(coordinates, dtype=np.float32)


# Create labels array (assuming all points are positive clicks)
labels = np.ones(len(coordinates), dtype=np.int32)


# Add new points to the predictor
_, out_obj_ids, out_mask_logits = predictor.add_new_points(
inference_state=inference_state,
frame_idx=ann_frame_idx,
obj_id=ann_obj_id,
points=points,
labels=labels,
)


if show_result:
# Display the results
plt.figure(figsize=(12, 8))
plt.title(f"Frame {ann_frame_idx}")
plt.imshow(Image.open(os.path.join(video_dir, frame_names[ann_frame_idx])))
show_points(points, labels, plt.gca())
show_mask((out_mask_logits[0] > 0.0).cpu().numpy(), plt.gca(), obj_id=out_obj_ids[0])
plt.show()


```


‍


We establish the state and provide the coordinates of the objects we aim to segment:


```text


inference_state = predictor.init_state(video_path=video_dir)


refine_mask_with_coordinates([[950, 700], [950, 600], [950, 500]], 0, 1)


```


‍


Figure 5. With three coordinates (green stars) the model automatically recognizes the entire object


As shown in Figure 5, three points were enough for the model to assign a mask to the whole body of the individual.


‍


Now we run the process on all the frames (Figure 6):


```text


# run propagation throughout the video and collect the results in a dict
video_segments = {}  # video_segments contains the per-frame segmentation results
for out_frame_idx, out_obj_ids, out_mask_logits in predictor.propagate_in_video(inference_state):
video_segments[out_frame_idx] = {
out_obj_id: (out_mask_logits[i] > 0.0).cpu().numpy()
for i, out_obj_id in enumerate(out_obj_ids)
}


# render the segmentation results every few frames
vis_frame_stride = 30
plt.close("all")
for out_frame_idx in range(0, len(frame_names), vis_frame_stride):
plt.figure(figsize=(6, 4))
plt.title(f"frame {out_frame_idx}")
plt.imshow(Image.open(os.path.join(video_dir, frame_names[out_frame_idx])))
for out_obj_id, out_mask in video_segments[out_frame_idx].items():
show_mask(out_mask, plt.gca(), obj_id=out_obj_id)


```


‍


‍


Figure 6. Once SAM 2 identifies an object, it can automatically track the same object across the entire video


Finally, we combine the frames to generate a video using ffmpeg. The end result is shown in Figure 7.


‍


Figure 7. Top: original video, Bottom: video after running SAM 2 on it


‍


## 4. What’s next


SAM 2’s ability to segment objects accurately and quickly in both images and videos can revolutionize how computer vision systems are created.


‍


In Part 2 we’ll explore how we can use GPT-4o to provide visual prompts to SAM 2 in what we call a cascade of foundation models, meaning, chaining models together to create the vision systems of the future.


‍


🔥 Learn more about the cutting edge of[multimodality](http://www.tenyks.ai/blog/the-foundation-models-reshaping-computer-vision) and foundation models in our CVPR 2024 series:


- [Image and Video Search & Understanding](http://www.tenyks.ai/blog/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more) (RAG, Multimodal, Embeddings, and more).
- [Top Highlights You Must Know](http://www.tenyks.ai/blog/cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding) — Embodied AI, GenAI, Foundation Models, and Video Understanding.


‍


## References


\[[1\]](https://ai.meta.com/blog/segment-anything-2/) Segment Anything Model 2


‍


\[[2](https://segment-anything.com/) \] Segment Anything Model


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan


‍


*👉 If you would like to know more about* ***Tenyks*** *, try*[sandbox.](https://sandbox.tenyks.ai/?utm_source=blog&utm_medium=referral&utm_campaign=blogpost)
