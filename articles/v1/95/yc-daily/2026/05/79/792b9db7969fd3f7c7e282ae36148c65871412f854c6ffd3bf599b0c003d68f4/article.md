---
schema_version: "1.0.0"
document_id: "792b9db7969fd3f7c7e282ae36148c65871412f854c6ffd3bf599b0c003d68f4"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/recording-improvements-next-gen-raw-tracks-and-new-compositor-with-layout-animations/"
published_at: "2026-05-22T17:12:05+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:32020764a6ecd56cf2fd202aa32a5221eb953a2a1c8a046327bc7bdbc61f315b"
---

# Recording improvements: next-gen raw tracks and new compositor with layout animations

We’re happy to announce some major upgrades to video recording on Daily for both raw-tracks and single-file cloud recording.


These updates will become defaults for all accounts on **May 26th, 2026** . Before that date, you can already turn on these features today by using properties (either for your entire domain or for an individual room). We have tested these extensively in production with customers, so there shouldn't be any disruption to your recording experience.


0:00


/ 0:13


## Event-driven raw-tracks compositing


The raw-tracks recording mode on Daily does exactly what it says on the tin: instead of a single composite MPEG-4 video, you get the meeting’s raw participant track data as individual media files. Nothing lost, nothing added. But the full story is not that simple. Because these track files are really just data packets captured in realtime over a network, they can have packet loss, resolution changes, etc. — all kinds of things that are very different from the traditional stable camera-originated media that post-processing applications like video editors are designed to operate on.


For easier post-processing, we provide an open source Github repo called[raw-tracks-tools](https://github.com/daily-co/raw-tracks-tools) . But its capabilities have been limited by what’s available in the files. The tracks on their own don’t provide essential context about what was actually happening in the meeting room: users turning on and off their cameras and microphones, active speaker status changes, application level messages, etc.


To remedy this, some time ago we added a[dataOutputs](https://docs.daily.co/reference/daily-js/instance-methods/start-recording#control-cloud-recording-layouts) configuration option to startRecording calls that lets you capture events and transcripts as data files alongside the recorded media. This event JSON data output is so useful for raw-tracks that we’re making it the default for all raw-tracks sessions.


The event JSON is a small file that gets written in the same S3 bucket as the raw-tracks media files. The raw-tracks-tools scripts have also been updated to make use of these events for more reliable meeting reconstruction and more features. So you can now create much richer post-processing composites.


Let’s say you’ve got a long meeting recorded in raw-tracks mode. There are people coming and going, pausing and resuming their video, live transcription was turned on, and people are chatting too. All this data is present in the event JSON and can be rendered into a video composite after the fact, with visual results that are identical to what you’d get by running a regular cloud recording during the meeting. You can use[all VCS features](https://docs.daily.co/guides/products/live-streaming-recording/vcs) like switching layouts, rendering captions (for transcriptions), sidebars for chat messages, etc.


To enable the event JSON output today, you can set the property` enable_raw_tracks_event_json: true` on either your domain or a room. And if you prefer not to get this file in your S3 bucket, you can use this same property to explicitly turn it off (even after it becomes the default on May 26).


💡


When you fetch an access link for a raw-tracks recording via the[Daily REST API](https://docs.daily.co/reference/rest-api/recordings/get-recording-link) , the response object includes the event JSON separately from the media tracks. The S3 key for the event JSON can be found under` dataOutputs\[‘event-json’\]` while track keys are in their usual place, as an array under the` tracks` property.


### Transcoded gapless audio (WAV / AAC) for raw-tracks


As was mentioned above, recording raw client tracks really means that you're capturing every artifact of the connection. If there’s packet loss or the participant is not sending, the media file will simply not contain any data at that time. For audio specifically, this “no data” situation is subtly different from silence. Actual silence means that zeros have been encoded, while a gap in data means it could be either silence or missing data. (This is basically analogous to boolean false vs. a boolean|null type.)


Many of our customers do post-processing on their raw-tracks audio recordings. To make it easier to use these files, we’ve added a “transcoded gapless” setting that lets you record audio files that are already transcoded into a standard format that all audio processing applications can easily deal with. There are no data gaps. Silence has already been mixed in, so the audio data is always uniform.


To enable this mode, you can set the property` enable_raw_tracks_transcoded_audio` on either your domain or a specific room. This property’s value must be either null or one of these strings:


- ` wav-48k-mono` - WAV files at 16-bit 48kHz sample rate, mono
- ` wav-44k1-mono` - WAV files at 16-bit 44.1kHz sample rate, mono
- ` wav-48k-stereo` - WAV files at 16-bit 48kHz sample rate, stereo
- ` wav-44k1-stereo` - WAV files at 16-bit 44.1kHz sample rate, stereo
- ` wav` - alias for` wav-48k-stereo` , the highest quality WAV setting
- ` aac` - AAC (.m4a) files at 160kbps - a good compromise if you don’t need uncompressed audio
- null - turns off transcoding, audio tracks will be recorded as raw WebM (default)


💡


You can find out the audio format of a raw-tracks recording session by inspecting the event JSON file saved alongside the media files. Each time a file is written to S3, a` recording-media-started` event is logged. These events have a` contentType` field that will tell you whether it’s WebM, AAC, or WAV.


## New cloud compositor with animation support


We also have improvements to cloud recording mode. It has been upgraded to use the VCSRender compositor, which is the exact same code that powers the compositing features in raw-tracks-tools that was discussed above. This means you have a guarantee that the rendering output will be identical whether your composite is rendered on the fly (using cloud recording) or as a post-processing action using raw-tracks.


💡


This upgrade also applies to RTMP/HLS live streaming which uses the same cloud rendering infrastructure.


The new compositor offers better performance overall, so even large rooms with dozens of participants visible in the recording won’t stutter. (Well, due to CPU limitations at least! If a participant isn’t sending video frames or there’s network packet loss, the cloud recording compositor can’t fix that...)


A significant new feature enabled by the performance upgrade is support for full frame rate layout animations. Video layer positions and opacities can now be changed at 30fps with negligible overhead. VCS contains[a declarative animation system](https://github.com/daily-co/daily-vcs/blob/main/js/ANIMATIONS.md) that lets you build entirely custom animations if you want, but there’s also an easy way to get animations automatically in your recordings with a single setting.


You can turn on the new composition param` enableLayoutAnims` to get smooth animations every time the video layout changes. For example, when a participant is added to a grid, the new video layer will fade in, and the other participant’s layer positions will animate to new positions as the grid changes.


These animations work with any layout mode and can be enabled at the start of recording, or in an update call during the session. For example:


```text
startRecording({
layout: {
preset: "custom",
composition_params: {
mode: "grid",
enableLayoutAnims: true
}
}
})


```


To enable the new compositor today, you can set the property` enable_legacy_compositor: false` on either your domain or a room. If for any reason you prefer to remain on the legacy compositor after the May 26 date, you can set this property to` true` (and please tell us why, so we can fix whatever is keeping you on the old code).


#### Never miss a story


Get the latest direct to your inbox.
