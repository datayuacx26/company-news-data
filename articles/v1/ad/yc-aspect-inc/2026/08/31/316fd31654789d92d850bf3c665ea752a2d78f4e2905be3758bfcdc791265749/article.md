---
schema_version: "1.0.0"
document_id: "316fd31654789d92d850bf3c665ea752a2d78f4e2905be3758bfcdc791265749"
company_key: "yc-aspect-inc"
company: "Aspect"
source_id: "yc-aspect-inc-news-import-ad28d86e40e8"
canonical_url: "https://aspect.inc/blog/technical-solutions/how-to-build-an-ffmpeg-transcode-server-with-watchfolders"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T21:00:56.600033+00:00"
fetched_at: "2026-08-09T21:00:58.529020+00:00"
content_hash: "sha256:f03ed68b482163bb3c7e3690d41c0d96d487c4ea91f3fcaac2b52dee741f73d0"
---

# How to Build an FFmpeg Transcode Server with Watchfolders

Start with the boring decision: build the server around the outputs you need every day, not around the biggest camera files you might occasionally receive.


Plan the server around routine deliverables, not rare worst-case source files. If the job is editorial proxies, you want reliable throughput, preserved metadata, predictable naming, and fast enough turnaround for assistants to trust the system. If the job is review files, you want consistent H.264 or H.265 deliverables, audio that plays everywhere, and output sizes that don't crush your upload pipe. If the job is finishing media or mezzanine transcodes, you need to be much more careful about color, bit depth, codec support, and quality settings.


A watchfolder transcode server has a simple concept: files arrive in an input folder, a watcher detects that the copy is finished, FFmpeg creates an output, logs the job, and moves the source or writes a marker so the same file isn't processed twice. The trouble comes from real post workflows: camera cards copied over the network, files still growing while the watcher sees them, mixed audio layouts, timecode requirements, codecs that need external libraries, and servers that get overloaded because five people dropped 800 GB at once.


This article walks through a practical FFmpeg watchfolder setup that you can run on a Linux workstation, rack server, or cloud VM. The examples use shell scripts,` inotifywait` ,` ffmpeg` ,` ffprobe` , and` systemd` , because those are easy to reason about and easy to replace later with a queue, API, or orchestration layer.


## Decide what kind of transcode server you're building


Don't start by asking “CPU or GPU?” Start by defining the server’s normal job.


Most post teams fall into one of these buckets:


- Editorial proxy generation from camera originals or mezzanine media
- Review file generation from exports or graded rough cuts
- Standardization of mixed incoming media before edit
- Web or streaming output generation
- Archive normalization or rewrapping


These are different workloads. Proxy generation may need burned-in LUTs, resize filters, source timecode, reel names, and filename consistency for relinking. Review file generation may need speed, H.264 compatibility, loudness normalization, and small file sizes. Standardization may involve fewer creative choices but more stream handling, because you'll see weird containers, extra audio tracks, subtitles, and mismatched frame rates.


FFmpeg’s own documentation makes an important distinction here: transcoding means[decoding and encoding again](https://ffmpeg.org/ffmpeg.html) . That's computationally expensive and usually lossy. If you only need to change the container, remove streams, copy audio, or adjust metadata, stream copy is faster and avoids generation loss.


That gives you the first rule for the server:


- Transcode only streams that need to change.
- Stream copy everything else when the output target allows it.
- Use[explicit stream mapping](https://ffmpeg.org/ffmpeg.html#Stream-selection) instead of trusting defaults.
- Preserve source identifiers needed for conform whenever proxies are involved.


That last point matters because in offline editorial workflows, proxies are useful only if they reconnect cleanly to the originals later.[Source timecode](https://www.arri.com/en/learn-help/learn-help-camera-system/pre-postproduction/editorial-workflow) , clip names, reel names, and consistent naming are how you avoid a painful conform.


## Pick hardware for the bottleneck you actually have


FFmpeg can use CPU and, depending on your build and hardware,[GPU acceleration](https://ffmpeg.org/ffmpeg-all.html) . But “GPU transcode server” isn't automatically better than “CPU transcode server.”


CPU encoding is usually the safest default for a post facility because it's predictable, flexible, and supports a wide range of codecs and filters. More cores usually mean more parallel work, especially when you're encoding several files at once. GPU encoding is useful when you need high throughput for supported codecs like H.264 or H.265 and can accept the quality, compatibility, and driver constraints of the hardware encoder.


A practical sizing model looks like this:


- CPU: prioritize core count for parallel jobs, but keep per-core performance in mind for heavy filters and codecs.
- RAM: 16 GB is comfortable for a small server, 32 GB or more is safer for multiple concurrent transcodes and large files.
- Storage: use fast local SSD or NVMe for working directories, not only for the OS.
- Network: 10 GbE matters quickly if camera originals live on shared storage.
- GPU: useful for high-volume H.264/H.265 outputs, less useful if your bottleneck is disk, network, ProRes encoding, or color-managed processing.


For an assistant editor proxy box, an 8 to 16 core CPU, 32 GB RAM, local NVMe scratch, and 10 GbE is a very reasonable starting point. For a review file server that makes a lot of H.264 MP4s, a supported NVIDIA, Intel, or AMD hardware encoder may be worth it. For cloud instances, test with your real source media before committing. Cloud CPU, storage, and network behavior can be very different from an on-prem workstation.


Also think about failure domain because a single huge machine is simple, but when it's down, nobody gets files. Two smaller servers with separate watchfolders or a shared queue may be more useful than one monster box.


## Use a folder structure that prevents duplicate and partial jobs


The biggest watchfolder mistake is treating “file exists” as “file is ready.” When someone copies a 200 GB camera file into a shared folder, the watcher may see the filename long before the copy is complete. If FFmpeg starts reading too early, you get corrupt outputs, failed jobs, or partial transcodes that look valid until someone hits the bad section.


Use a folder layout that separates incoming, processing, finished, failed, and output states.


Separate folder states help prevent duplicate jobs and partial-file processing. A simple layout:


```text
/transcode
/watch
/processing
/done
/failed
/output
/logs


```


The preferred ingest pattern is atomic move: copy the file somewhere temporary, then move it into` /watch` only after the copy is complete. On the same filesystem, a move is effectively instant, so the watcher sees a complete file rather than a growing one.


If users or other systems will copy directly into the watchfolder, add a[stability check](https://docs.ffmate.io/docs/watchfolder.md) before processing. The script can compare file size twice with a delay and only continue when the size is unchanged.


Example stability function:


```text
wait_until_stable() {
local file="$1"
local delay="${2:-20}"


while true; do
size1=$(stat -c%s "$file" 2>/dev/null || echo 0)
sleep "$delay"
size2=$(stat -c%s "$file" 2>/dev/null || echo 0)


if [ "$size1" -eq "$size2" ] && [ "$size1" -gt 0 ]; then
break
fi
done
}


```


That doesn't prove the file is perfect, but it prevents the most common partial-read failure. For camera cards and folder-based formats, you may need to watch for a sidecar marker file instead of individual media files. For example, your copy tool can write` CARD_001.complete` after the whole card is verified, and the watcher can trigger from that marker.


## Install FFmpeg with the codecs you actually need


The FFmpeg command line is only as capable as the build behind it. Native decoders are commonly available, but some encoders and hardware features require external libraries or build-time options. Before you automate anything, confirm that the server’s FFmpeg build supports your required codecs.


Useful inspection commands:


```text
ffmpeg -version
ffmpeg -encoders
ffmpeg -decoders
ffmpeg -hwaccels
ffmpeg -filters


```


Also use` ffprobe` on[real source files](https://ffmpeg.org/ffprobe.html) . It reports container, streams, codecs, timecode-related tags, audio layouts, frame rate, duration, and other details in human-readable or machine-readable formats.


Example:


```text
ffprobe -hide_banner -show_format -show_streams "input.mov"


```


For scripts, JSON output is easier to parse:


```text
ffprobe -v error -print_format json -show_format -show_streams "input.mov"


```


Don't skip this part because FFmpeg’s automatic stream selection can be convenient for one-off jobs, but you should make automated commands explicit. Media files can contain multiple video streams, audio stems, subtitles, timecode tracks, attachments, and data streams. Container formats also have limits on what stream types they can hold. A command that works on yesterday’s file may silently choose the wrong stream tomorrow unless you map streams deliberately.


## Build the FFmpeg commands around output targets


The cleanest watchfolder setup has named presets, even if they're just shell functions or separate scripts. Avoid one giant command with a dozen conditional branches unless you enjoy debugging at 2 a.m.


Named presets turn one source into different deliverables for different uses. Output target Typical codec or wrapper Preset priorities Watch out for


Editorial proxies ProRes Proxy MOV, DNxHR LB, sometimes H.264 Relink safety, source timecode, reel or clip names, edit performance Changing frame rate, losing metadata, collapsing useful audio channels


Review files H.264 MP4, H.265 MP4 where approved Broad playback compatibility, reasonable file size, AAC audio, fast start H.265 compatibility, aggressive compression, hardware encoder quality differences


Mezzanine transcodes ProRes 422 HQ, ProRes 4444, DNxHR HQX, image sequences Bit depth, color tags, alpha support, audio fidelity, predictable wrapper Unintended pixel format conversion, incorrect color metadata, channel layout changes


Standardization before edit Project-approved MOV or MXF codec, PCM audio Consistent container, stream layout, frame size, and naming Accidentally transcoding when rewrap or stream copy would be safer


Web or streaming outputs H.264 or H.265 ladder, AAC audio Target bitrate, resolution ladder, player requirements, upload size Variable frame rate sources, audio drift, unsupported profiles or levels


For editorial proxies, the priorities are edit performance, relink safety, and consistent naming. A typical target might be ProRes Proxy, DNxHR LB, or H.264 proxy depending on the NLE, platform, and storage. ProRes and DNxHR are often friendlier in editorial than long-GOP H.264, though they create larger files.


Example ProRes Proxy style command:


```text
ffmpeg -hide_banner -y \
-i "$INPUT" \
-map 0:v:0 -map 0:a? \
-c:v prores_ks \
-profile:v 0 \
-vf "scale=1280:-2" \
-vendor apl0 \
-c:a pcm_s16le \
-movflags +faststart \
"$OUTPUT"


```


For review MP4s, compatibility matters more than edit performance. H.264 with AAC audio is still the safest broad target.


Example review command:


```text
ffmpeg -hide_banner -y \
-i "$INPUT" \
-map 0:v:0 -map 0:a:0? \
-c:v libx264 \
-preset medium \
-crf 20 \
-pix_fmt yuv420p \
-vf "scale='min(1920,iw)':-2" \
-c:a aac \
-b:a 192k \
-movflags +faststart \
"$OUTPUT"


```


For mezzanine transcodes, be conservative. Know whether you need 10-bit, alpha, timecode, specific color tags, or a particular wrapper. Don't collapse audio channels or convert pixel formats casually.


Example ProRes 422 HQ command:


```text
ffmpeg -hide_banner -y \
-i "$INPUT" \
-map 0:v:0 -map 0:a? \
-c:v prores_ks \
-profile:v 3 \
-pix_fmt yuv422p10le \
-c:a pcm_s24le \
"$OUTPUT"


```


For GPU-assisted H.264 review outputs, test quality and compatibility before using it everywhere. Hardware encoders can be fast, but they have their own options and limitations.


Example NVIDIA-style command:


```text
ffmpeg -hide_banner -y \
-hwaccel cuda \
-i "$INPUT" \
-map 0:v:0 -map 0:a:0? \
-c:v h264_nvenc \
-preset p5 \
-cq 23 \
-vf "scale=1920:-2" \
-c:a aac \
-b:a 192k \
-movflags +faststart \
"$OUTPUT"


```


These are starting points. Base your real presets on the NLE, finishing path, delivery specs, storage limits, and source media you actually receive.


## Write a watcher that can survive normal post chaos


On Linux,` inotifywait` is a practical way to trigger jobs when files arrive. Use` close_write` or moved-file events rather than a simple polling loop when possible.


Here is a basic watcher:


```text
#!/usr/bin/env bash
set -u


WATCH_DIR="/transcode/watch"
PROCESSING_DIR="/transcode/processing"
DONE_DIR="/transcode/done"
FAILED_DIR="/transcode/failed"
OUTPUT_DIR="/transcode/output"
LOG_DIR="/transcode/logs"


mkdir -p "$WATCH_DIR" "$PROCESSING_DIR" "$DONE_DIR" "$FAILED_DIR" "$OUTPUT_DIR" "$LOG_DIR"


inotifywait -m -e close_write -e moved_to --format '%w%f' "$WATCH_DIR" | while read -r FILE
do
[ -f "$FILE" ] || continue


BASENAME=$(basename "$FILE")
STEM="${BASENAME%.*}"
JOB_ID="$(date +%Y%m%d-%H%M%S)-$STEM"
LOG_FILE="$LOG_DIR/$JOB_ID.log"


echo "Starting job: $FILE" >> "$LOG_FILE"


wait_until_stable "$FILE" 20


INPUT="$PROCESSING_DIR/$BASENAME"
OUTPUT="$OUTPUT_DIR/${STEM}_review.mp4"


mv "$FILE" "$INPUT"


ffmpeg -hide_banner -nostdin -y \
-i "$INPUT" \
-map 0:v:0 -map 0:a:0? \
-c:v libx264 \
-preset medium \
-crf 20 \
-pix_fmt yuv420p \
-vf "scale='min(1920,iw)':-2" \
-c:a aac \
-b:a 192k \
-movflags +faststart \
"$OUTPUT" >> "$LOG_FILE" 2>&1


STATUS=$?


if [ "$STATUS" -eq 0 ]; then
echo "Job succeeded: $OUTPUT" >> "$LOG_FILE"
mv "$INPUT" "$DONE_DIR/$BASENAME"
else
echo "Job failed with status $STATUS" >> "$LOG_FILE"
mv "$INPUT" "$FAILED_DIR/$BASENAME"
fi
done


```


A few details matter here.


The` -nostdin` flag is important when FFmpeg runs in the background or under a service manager. FFmpeg normally listens for console input, such as` q` to quit. In unattended scripts, you don't want FFmpeg trying to interact with a terminal that isn't really there.


The script moves files into` /processing` before running FFmpeg. That keeps the watchfolder clean and prevents reprocessing if the watcher restarts.


The command redirects both standard output and standard error into a per-job log. FFmpeg[logs to stderr](https://ffmpeg.org/ffmpeg.html#Generic-options) by default, so capturing only stdout will miss the useful information.


This example processes one job at a time, which is intentional for a first version. Parallel processing is useful, but only after you know the server’s real throughput and bottlenecks.


## Run the watcher as a service


A watchfolder is only useful if it keeps running after you close your SSH session. On Linux, use` systemd` for a simple always-on service.


Example unit file:


```text
[Unit]
Description=FFmpeg Watchfolder Transcode Service
After=network-online.target
Wants=network-online.target


[Service]
Type=simple
User=transcode
Group=transcode
ExecStart=/usr/local/bin/watch-transcode.sh
Restart=always
RestartSec=5
WorkingDirectory=/transcode
Environment=AV_LOG_FORCE_NOCOLOR=1


[Install]
WantedBy=multi-user.target


```


Enable it with:


```text
systemctl daemon-reload
systemctl enable ffmpeg-watchfolder
systemctl start ffmpeg-watchfolder


```


Then inspect it with:


```text
systemctl status ffmpeg-watchfolder
journalctl -u ffmpeg-watchfolder -f


```


Run the service as a dedicated user, not as root. Give that user read/write permissions only where needed: watch, processing, output, logs, done, and failed. If the server reads from shared storage, make sure the server mounts it before the service starts and that network disconnects don't leave jobs half-running.


## Log enough to debug without watching the terminal


You need two levels of logging: service logs and job logs.


Service logs tell you whether the watcher is alive, whether directories are mounted, and whether the script is crashing. Job logs tell you which FFmpeg command ran, what FFmpeg detected, and why a file failed.


For routine operation,` -hide_banner` keeps logs readable. For failures, FFmpeg’s` -loglevel` option lets you control verbosity. Common values include` error` ,` warning` ,` info` , and` debug` . You can also use FFmpeg’s report feature to dump command-line and log output to a timestamped report file, which is useful when chasing strange decode or muxing issues.


Include these fields in a practical job log:


- Job ID
- Input path
- Output path
- Preset name
- Full FFmpeg command
- FFmpeg exit code
- Start and end timestamps
- ` ffprobe` summary for the input
- Final output file size
- Any move to` done` or` failed`


For failed jobs, don't delete the input automatically. Move it to a failed folder and keep the log next to it or reference the same job ID. Assistants need to know whether to retry, send the file to a different preset, or escalate because the source is damaged.


Failed jobs should preserve the input and diagnostic record for retry or review.


## Handle errors as workflow states


Most watchfolder failures are predictable, so build for them.


Common failure modes include:


- File copied before it was complete
- Unsupported codec or missing FFmpeg library
- Invalid stream mapping because expected audio or video is missing
- Not enough disk space
- Corrupt source media


Some of these should fail the job, but your scripts can handle others gracefully. For example,` -map 0:a?` makes audio optional, which is useful for silent graphics exports. But for camera proxy generation, missing audio may be a real problem. Make the preset reflect the workflow expectation.


Before the encode, use` ffprobe` to reject files that clearly don't match the preset. If a review preset requires one video stream, check for it. If an editorial proxy preset requires timecode metadata, log whether it exists. This avoids producing “successful” outputs that fail later in edit.


## Monitor load before adding concurrency


Once one-at-a-time processing is stable, measure, then decide whether to run multiple jobs in parallel.


Watch these signals:


- CPU usage and load average
- GPU encoder and decoder usage, if applicable
- RAM and swap
- Disk read/write throughput
- Free disk space in processing and output volumes
- Network throughput to shared storage
- Job duration by preset
- Queue depth or number of files waiting
- Failure rate by source type and preset


Basic Linux tools like` top` ,` htop` ,` iostat` ,` df` ,` du` , and` nvidia-smi` can tell you a lot. For a facility server, your team should push metrics into a monitoring system so post supervisors can see whether the box is idle, busy, or stuck.


Don't assume 16 cores means you should run 16 FFmpeg jobs. Encoding can saturate memory bandwidth, storage, network, or a hardware encoder before CPU reaches 100 percent. Start with two concurrent jobs, measure, then increase. For proxy generation from shared storage, the network or source volume may become the bottleneck before the transcode server does.


If you need proper concurrency, add a queue rather than launching unlimited background jobs from the watcher. Even a simple spool directory with a worker limit is better than letting every file start immediately.


## Keep presets boring and documented


The server will become part of the facility’s workflow. Give presets names people understand.


Good preset names describe the intended use:


- ` editorial_prores_proxy_720p`
- ` editorial_dnxhr_lb_1080p`
- ` review_h264_1080p`
- ` mezzanine_prores422hq`
- ` audio_wav_48k`


Store preset scripts in version control if possible. When you change a CRF value, audio layout, scale rule, or codec profile, you want a record of what changed and when. That matters when someone asks why today’s review files are larger than last week’s, or why a proxy batch no longer relinks the same way.


Also keep sample source files for testing: one camera file with timecode, one mixed-audio file, one no-audio file, one weird phone file, one long duration file, and one intentionally bad file. Run them through presets after updates to FFmpeg, GPU drivers, OS packages, or storage mounts.


## When to move beyond a shell-script watchfolder


A shell-script watchfolder is a good first server. It's transparent, cheap, and easy to debug, but it has limits.


You may want a more formal job system when:


- Multiple users submit jobs from different locations
- You need authentication or role-based access
- Several servers should share one queue
- Failed jobs need retry rules
- You need audit history across productions


At that point, keep FFmpeg as the media engine but put a[real queue](https://docs.ffmate.io/docs/ffmate-internals) , database, web UI, or API in front of it. The workflow concepts stay the same: stable ingest, explicit presets, careful stream handling, good logs, and resource-aware scheduling.


## The practical build path


The fastest reliable path is to start small and make each layer boring.


Set up the server with fast local working storage and enough CPU for your expected daily load. Install an FFmpeg build that supports your codecs. Create a watchfolder structure that separates incoming, processing, completed, failed, output, and logs. Use atomic moves or file stability checks so you don't process partial copies. Start with one or two presets that match real workflow targets. Use explicit` -map` options. Capture per-job logs. Run the watcher under` systemd` . Measure load before adding concurrency.


That gets you a useful transcode server without pretending it's a full media asset platform. For a lot of editorial teams, that's exactly what they need: a dependable box that turns incoming files into the right kind of media with enough logging to fix problems when real-world footage gets messy.


## FAQ


Use CPU encoding when you need broad codec support, predictable quality, complex filters, or mezzanine and editorial proxy workflows. Use GPU encoding when the main requirement is high-volume H.264 or H.265 output and you have tested the quality, driver stability, and compatibility of the hardware encoder with your real source media.


The safest method is to copy files to a temporary location first, then move them into the watchfolder only after the copy is complete. If users copy directly into the watchfolder, add a stability check that waits until the file size stops changing before FFmpeg starts. For camera cards or folder-based media, trigger from a marker file such as CARD_001.complete after the full card has been verified.


For editorial proxies, common choices include ProRes Proxy, DNxHR LB, or another edit-friendly codec that preserves relink-critical metadata and performs well in the NLE. For review files, H.264 MP4 with AAC audio is still the safest broad compatibility target. Presets should be based on the edit system, finishing path, storage limits, upload bandwidth, and source media, not on generic defaults.


Start with one job at a time, measure performance, then increase gradually. Multiple concurrent jobs can saturate CPU, storage, network, memory bandwidth, or a GPU hardware encoder before the server appears fully loaded. A queue or worker limit is safer than launching every incoming file immediately.


Each job log should include the job ID, input path, output path, preset name, full FFmpeg command, start and end timestamps, FFmpeg exit code, an ffprobe summary, final output size, and whether the source was moved to done or failed. Failed jobs should keep the source file and log so an assistant can retry, use a different preset, or escalate the issue.


Use clear version naming in the output path, write the preset and source filename into the job log, and avoid sending review files as loose attachments. Aspect keeps revisions together with version stacking, so notes and approvals stay connected to the current cut instead of drifting across separate exports.
