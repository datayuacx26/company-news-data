---
schema_version: "1.0.0"
document_id: "697eb297d630f32347a015d0c90fb83aea6b458428bbac0ce70db52c400035a9"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/daft-videofile-seek-lazily-get-frames"
published_at: "2026-05-08T08:00:00+00:00"
first_seen_at: "2026-07-28T08:49:54.145619+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:ba79eca871bff032d3e929c6f73593e3350ec30c8fbf5655b0e133f5bfd2eb8b"
---

# daft.VideoFile: Seek Lazily, Get Frames

A vision model rarely needs every frame. Keyframes, one-frame-per-second, or a specific 10-second window cover most use cases — and decoding the whole file to get there is wasted work.


[daft.VideoFile](https://docs.daft.ai/en/stable/modalities/videos/) and[daft.read_video_frames](https://docs.daft.ai/en/stable/api/io/#daft.read_video_frames) decode only what you ask for. The slice you want is the slice that gets read.


## What this is built for


The clearest case is robotics. Open X-Embodiment aggregates over a million episodes. DROID alone runs 350+ hours of multi-camera 60fps footage. That's hundreds of millions of frames across a single dataset, and most action-model training doesn't need them all: keyframes for retrieval, one frame per second for VLM annotation, a five-second window around each labeled grasp.


The shape repeats. Fleet dashcams record at 30fps for hours, but the events worth scoring (a near-miss, a lane departure, a pedestrian crossing) are seconds long. Security feeds run 24/7; the clip you need is six. Content moderation queues run on user uploads where most of the timeline is empty space between the moments that matter.


Daft's video stack is built for the throw-away ratio. Four column expressions cover the slice patterns:


1. **Keyframes only** —` is_key_frame=True` on` read_video_frames` , or` video_keyframes` on a VideoFile column.
2. **Time-sampled** —` sample_interval_seconds=1.0` picks one frame at or after each second.
3. **Header-only filtering** —` video_metadata` reads resolution, fps, duration, and frame count without decoding a frame.
4. **Time-windowed decode** —` video_frames(start_time, end_time)` decodes just the seek range from a VideoFile.


Same DataFrame, same query plan, same scaling story as the rest of your pipeline.


## The shortcut


The fastest way to get usable frames out of a folder of videos is one call:


```text
import   daft


df   =   daft.read_video_frames(
path  =  "  s3://bucket/videos/*.mp4  "  ,
image_height  =  480  ,
image_width  =  640  ,
is_key_frame  =  True  ,
)
df.show()
```


Each row is one frame. Columns include` frame_index` ,` frame_time` ,` is_key_frame` , and` data` as` Image\[RGB; 480 x 640\]` ready to feed to a model. Globs work; lists of paths work; YouTube URLs work:


```text
df   =   daft.read_video_frames(
path  =  [
"  https://www.youtube.com/watch?v=jNQXAC9IVRw  "  ,
"  https://www.youtube.com/watch?v=N2rZxCrb7iU  "  ,
],
image_height  =  480  ,
image_width  =  640  ,
is_key_frame  =  True  ,
)
```


` is_key_frame=True` filters at the decoder. For a 1-hour H.264 video that's typically 200–500 frames instead of 108,000 — the compression structure already decided which frames carry the most novel information, and we lean on that. If you want temporal coverage instead of compression-driven sparsity, pass` sample_interval_seconds=1.0` and Daft picks the first frame at or after each second:


```text
df   =   daft.read_video_frames(
path  =  "  s3://bucket/videos/*.mp4  "  ,
image_height  =  480  ,
image_width  =  640  ,
sample_interval_seconds  =  1.0  ,
)
```


Both filters can stack — keyframes sampled at one-second intervals — so you keep the compression-aware sparsity and get a predictable temporal grid.


## Filter before you decode


` read_video_frames` is the path of least resistance. When you need to make decisions per-video — skip anything over an hour, only process 1080p or higher, dispatch by codec — wrap the path in[video_file](https://docs.daft.ai/en/stable/api/functions/video_file/) and let[video_metadata](https://docs.daft.ai/en/stable/api/functions/video_metadata/) inspect headers without decoding a frame:


```text
import   daft
from   daft.functions   import   unnest, video_file, video_metadata


df   =   (
daft.from_files(  "  s3://bucket/videos/**/*.mp4  "  )
.with_column(  "  video_file  "  , video_file(daft.col(  "  file  "  )))
.with_column(  "  video_meta  "  , video_metadata(daft.col(  "  video_file  "  )))
.select(  "  video_file  "  , unnest(daft.col(  "  video_meta  "  )))
.where(daft.col(  "  duration_seconds  "  )   <   3600  )
.where(daft.col(  "  height  "  )   >=   1080  )
)
```


Same pattern as[Week 2's file_path() filter](https://www.daft.ai/blog/daft-file-lazy-metadata-filtering) : cheap operations narrow the set, expensive operations run on the survivors. Header reads are HTTP range requests — no full download.


## Targeted decode with` video_frames`


Once you've narrowed the set,[video_frames](https://docs.daft.ai/en/stable/api/functions/video_frames/) decodes a specific time range from a` VideoFile` column. One row per video, with the decoded frames as a list of structs you can` .explode()` into per-frame rows:


```text
import   daft
from   daft.functions   import   video_file, video_frames


df   =   (
daft.from_files(  "  s3://bucket/videos/*.mp4  "  )
.with_column(  "  videofile  "  , video_file(daft.col(  "  file  "  ),   verify  =  True  ))
.with_column(
"  frames  "  ,
video_frames(
daft.col(  "  videofile  "  ),
start_time  =  0.0  ,
end_time  =  10.0  ,
),
)
.explode(  "  frames  "  )
)
```


` start_time` and` end_time` are seconds. The decoder seeks to the nearest preceding keyframe and walks forward — so a 10-second window from a 90-minute video reads roughly 10 seconds of bytes, not the whole file. That's the "stream-based" promise: every worker pulls only what it needs from object storage.


[video_keyframes](https://docs.daft.ai/en/stable/api/functions/video_keyframes/) is the convenience version when you only want keyframes per file:


```text
from   daft.functions   import   video_keyframes


df   =   df.with_column(  "  keyframes  "  , video_keyframes(daft.col(  "  video  "  )))
```


## From frames to inference


Frames as DataFrame rows compose with the rest of Daft like any other image column. Send them to a vision model with[prompt](https://docs.daft.ai/en/stable/ai-functions/overview/) , classify with[classify_image](https://docs.daft.ai/en/stable/ai-functions/overview/) , or write the embeddings to Iceberg:


```text
from   daft.functions   import   prompt,   format   as   daft_format


df_captioned   =   df.with_column(
"  caption  "  ,
prompt(
daft_format(  "  Describe this scene in one sentence:   {}  "  , daft.col(  "  data  "  )),
model  =  "  openai/gpt-5.5  "  ,
),
)
```


Same DataFrame, no new infrastructure — the file decode step disappears into the column.


Reference example in[daft-examples/examples/files](https://github.com/Eventual-Inc/daft-examples/tree/main/examples/files) :


- [daft_videofile.py](https://github.com/Eventual-Inc/daft-examples/blob/main/examples/files/daft_videofile.py) — full pattern with` video_file` ,` video_metadata` ,` video_keyframes`
