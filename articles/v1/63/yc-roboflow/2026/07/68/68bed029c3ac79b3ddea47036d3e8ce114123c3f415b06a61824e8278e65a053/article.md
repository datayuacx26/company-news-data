---
schema_version: "1.0.0"
document_id: "68bed029c3ac79b3ddea47036d3e8ce114123c3f415b06a61824e8278e65a053"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/rf-detr-nvidia-deepstream/"
published_at: "2026-07-22T17:32:53+00:00"
first_seen_at: "2026-07-25T21:41:10.504044+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:e12517f238d117980794f2c4badf53061a08dec94b60db484655c81e8f86c93b"
---

# Run RF-DETR in NVIDIA DeepStream on Jetson

[Erik Kokalj](https://blog.roboflow.com/author/erik/)


Published Jul 22, 2026 • 6 min read


This post shows how to run[RF-DETR](https://rfdetr.roboflow.com/latest/?ref=blog.roboflow.com) inside[NVIDIA DeepStream](https://developer.nvidia.com/deepstream-sdk?ref=blog.roboflow.com) on a Jetson Orin NX. We cover what DeepStream and RF-DETR are, how to build a TensorRT engine and the parser DeepStream needs to read the model's output, how to run it on RTSP stream, and how to give each class its own box color.


We used NVIDIA's[deepstream-import-vision-model](https://github.com/NVIDIA/DeepStream/tree/main/skills/deepstream-import-vision-model?ref=blog.roboflow.com) agent skill to do most of the import for us. The numbers below come from JetPack 7.2 with DeepStream 9.1 and TensorRT 10.16.


## What Is DeepStream?


DeepStream is NVIDIA's video analytics SDK, built on[GStreamer](https://gstreamer.freedesktop.org/?ref=blog.roboflow.com) . It gives you hardware-accelerated pipeline parts (decode, batching, TensorRT inference, on-screen display, encode) that keep frames in GPU memory the whole way through. The ones you will touch in this post:


- ` nvstreammux` : batches N decoded streams into one GPU buffer
- ` nvinfer` : runs a TensorRT engine and attaches detection metadata to each frame
- ` nvdsosd` : draws boxes and labels from that metadata


On a Jetson it installs straight from NVIDIA's apt repo:` sudo apt install deepstream-9.1` on JetPack 7.2 (or` deepstream-7.1` on JetPack 6.2).


Keep in mind the inference engine doesn't know or care what your model's output format is; it hands you raw tensors and leaves the interpretation to you. That interpretation lives in a small parser function you write, and it's where most of these projects succeed or fail.


## What Is RF-DETR?


RF-DETR ([GitHub](https://github.com/roboflow/rf-detr?ref=blog.roboflow.com) ) by[Roboflow](https://roboflow.com/?ref=blog.roboflow.com) is a DETR-family detector, released under Apache 2.0. It uses 300 learned queries that look over the image features, and each query predicts one box on its own. So there is no anchor grid and no NMS step afterward. It ships in a few sizes; here I'll use RF-DETR Nano, which takes a 384x384 RGB image and puts out two tensors: 300 boxes (center x, center y, width, height, all scaled 0 to 1) and 300 x 91 class scores run through a sigmoid.


## Getting the Weights and ONNX


The[rfdetr package](https://pypi.org/project/rfdetr/?ref=blog.roboflow.com) downloads pretrained COCO weights the first time you use it, and exports a ready-to-deploy ONNX file for you:


```text
pip install rfdetr


from rfdetr import RFDETRNano


model = RFDETRNano()          # downloads pretrained weights
model.export(                 # writes an ONNX file
output_dir=".",
batch_size=1,             # static batch, baked into the graph
)
```


The export is already simplified and uses a fixed batch size by default, which matters on Jetson. Building the TensorRT engine is one command (about two minutes on the Orin NX):


```text
/usr/src/tensorrt/bin/trtexec \
--onnx=rfdetr-nano.onnx \
--fp16 \
--saveEngine=rfdetr_nano_b1.engine
```


## Telling DeepStream What the Output Tensors Mean


DeepStream doesn't know the output bytes are boxes, or how they are laid out, so you give it a parser function. DeepStream calls it with the raw output tensors for each frame, and it fills in detection structs (class id, left, top, width, height, confidence).


For RF-DETR, the post-processing steps are: run the class scores through a sigmoid, pick the best class for each query, and turn the center-form box into a corner-form box in pixels. Here is the core of it:


```text
// called by DeepStream with the raw output tensors of one frame
extern "C" bool NvDsInferParseCustomRfdetr(...)
{
// find the two output layers by name
// ("dets"/"labels" from the rfdetr export)
const float *boxes  = layer("dets");    // [300, 4]  cx, cy, w, h in [0,1]
const float *logits = layer("labels");  // [300, 91] raw class logits


for (int q = 0; q < 300; q++) {
// best class for this query (skip index 0, the background slot)
int   best  = argmax(logits + q * 91);
float score = sigmoid(logits[q * 91 + best]);
if (score < threshold) continue;


NvDsInferObjectDetectionInfo obj = {};   // zero-init, always
obj.classId = best;
obj.detectionConfidence = score;
// normalized center-form to pixel corner-form
obj.width  = boxes[q*4 + 2] * netW;
obj.height = boxes[q*4 + 3] * netH;
obj.left   = boxes[q*4 + 0] * netW - obj.width  / 2;
obj.top    = boxes[q*4 + 1] * netH - obj.height / 2;
objectList.push_back(obj);
}
return true;
}
```


The parser gets compiled into a small shared library, and the config file tells the system where it is, plus it holds the preprocessing settings. But if you mess any of it up, you won't get any notification that something's wrong - you'll either see no detections or boxes in totally wrong spots.


```text
[property]
onnx-file=rfdetr-nano.onnx
model-engine-file=rfdetr_nano_b1.engine
labelfile-path=labels.txt          # 91 COCO class names, one per line
# RF-DETR expects plain 0..1 input: rescale only, no ImageNet normalization
net-scale-factor=0.00392156862745098   # = 1/255
model-color-format=0               # RGB
maintain-aspect-ratio=0            # model squash-resizes to 384x384, no letterbox
cluster-mode=4                     # DETR family: no NMS
network-mode=2                     # FP16
infer-dims=3;384;384
num-detected-classes=91
custom-lib-path=libnvdsinfer_rfdetr_parser.so
parse-bbox-func-name=NvDsInferParseCustomRfdetr


[class-attrs-all]
pre-cluster-threshold=0.4          # confidence cutoff
```


Check the model's preprocessing settings instead of assuming ImageNet normalization. This export expects plain 0 to 1 input.


## Running on a Live Camera


Once the engine, parser, and config are in place, running a live camera takes a single` deepstream-app` config file:


```text
[application]
enable-perf-measurement=1     # prints per-stream FPS every few seconds


[source0]
enable=1
type=4                        # 4 = RTSP source
uri=rtsp://user:pass@CAMERA_IP:554/stream1
latency=200                   # jitter buffer, ms


[streammux]
batch-size=1                  # match the engine's batch size
width=1280                    # frames are scaled to this before inference
height=720
live-source=1


[primary-gie]
enable=1
config-file=config_infer_rfdetr.txt   # the nvinfer config above


[sink0]
enable=1
type=1                        # 1 = fakesink (headless); use type=4 to re-stream
sync=0
```


```text
deepstream-app -c ds_app_rtsp.txt
```


A single 720p stream runs in real time with plenty of headroom on the Orin NX. If you want to record or re-stream the annotated output, the Orin NX has a hardware H.264 encoder (NVENC), so encoding stays on the GPU (` nvv4l2h264enc` ) instead of loading the CPU.


Default rendering: correct detections, but every class gets the same red box.


## Per-Class Colors Need Code, Not Config


There is no config key for coloring boxes by class. The color lives in per-object metadata, and you change it in a probe (a callback on the pipeline) right before the on-screen display draws the box. That means writing a small app of your own instead of using the stock binary. The[Python bindings](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps?ref=blog.roboflow.com) install as a wheel, with no root needed. Grab the pyds wheel matching your Python version from the deepstream_python_apps releases (not` pip install pyds` , which is an unrelated PyPI package). The whole thing is about 20 lines:


```text
import colorsys, pyds
from gi.repository import Gst


# 91 visually distinct hues via golden-ratio spacing
PALETTE = [colorsys.hsv_to_rgb((i * 0.618) % 1.0, 0.85, 1.0) for i in range(91)]


def osd_probe(pad, info, _):
batch = pyds.gst_buffer_get_nvds_batch_meta(hash(info.get_buffer()))
l_frame = batch.frame_meta_list
while l_frame:
frame = pyds.NvDsFrameMeta.cast(l_frame.data)
l_obj = frame.obj_meta_list
while l_obj:
obj = pyds.NvDsObjectMeta.cast(l_obj.data)
r, g, b = PALETTE[obj.class_id % 91]
obj.rect_params.border_width = 3
obj.rect_params.border_color.set(r, g, b, 1.0)   # box color
obj.text_params.set_bg_clr = 1
obj.text_params.text_bg_clr.set(r, g, b, 0.55)   # label background
l_obj = l_obj.next
l_frame = l_frame.next
return Gst.PadProbeReturn.OK


# attach to the on-screen-display element's input
osd.get_static_pad("sink").add_probe(Gst.PadProbeType.BUFFER, osd_probe, 0)
```


Attach that to the same pipeline, built in Python, and every class gets its own steady color, with the label background matched to the box.


Four streams batched through one RF-DETR Nano engine on a Jetson Orin NX, tiled 2x2 with the colored-box probe above, hardware-encoded and streamed off the device. Every detection here is the same class (car), so they share one color; the probe assigns a distinct hue per class id when the scene has more than one.


## Measured Throughput


Numbers from this setup: RF-DETR Nano, FP16, 384x384, on the same Orin NX 16GB module under two JetPack stacks. The TensorRT rows are` trtexec` on the engine alone; the DeepStream row is the full four-stream pipeline (decode, inference, on-screen display, and H.264 encode), with frames counted on the inference output. Both runs used the maximum power mode (` nvpmodel -m 0` ), which on JetPack 7.2 is the higher-clocked MAXN SUPER profile.


Metric Orin NX 16GB (Nano, FP16, 384, MAXN SUPER)


TensorRT, batch 1 166 img/s (6.0 ms)


TensorRT, batch 4 205 img/s (19 ms/batch)


DeepStream, 4x 720p RTSP at 30 fps 30 fps/stream (120 aggregate)


GPU utilization, 4-stream pipeline ~66%


Peak RAM, 4-stream pipeline 3.6 GB


A thing worth noting: batching from 1 to 4 lifts inference throughput about 20% on both stacks, less than the gains you see with CNN detectors, because a detection transformer already keeps the GPU busy at batch 1.


## Conclusion


We've done the full path from pretrained weights to live camera inference on a Jetson: export ONNX with the` rfdetr` package, build an FP16 TRT engine, add the parser that tells DeepStream what the tensors mean, point deepstream-app at your camera, and add a Python probe when you want per-class colors. The[DeepStream import skill](https://github.com/NVIDIA/DeepStream/tree/main/skills/deepstream-import-vision-model?ref=blog.roboflow.com) does most of this for you.


RF-DETR is on[GitHub](https://github.com/roboflow/rf-detr?ref=blog.roboflow.com) under Apache 2.0. If you want to train one on your own data, the[Custom Model Training guide](https://docs.roboflow.com/guides/model-training?ref=blog.roboflow.com) covers that.


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Erik Kokalj](https://blog.roboflow.com/author/erik/) . (Jul 22, 2026). Run RF-DETR in NVIDIA DeepStream on Jetson. Roboflow Blog: https://blog.roboflow.com/rf-detr-nvidia-deepstream/*


Stay Connected


Get the Latest in Computer Vision First


### Written by


Erik Kokalj


Developer Experience @ Roboflow


[View more posts](https://blog.roboflow.com/author/erik/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Tutorial](https://blog.roboflow.com/tag/tutorial/)
- [RF-DETR](https://blog.roboflow.com/tag/rf-detr/)
