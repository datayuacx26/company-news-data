---
schema_version: "1.0.0"
document_id: "f38b81f990fd9d0c907842116f6f1de6e73d44d7e7c75dcf1563a5d07f6c1642"
company_key: "yc-uberduck"
company: "Uberduck"
source_id: "yc-uberduck-news-import-d130d9d061b4"
canonical_url: "https://www.uberduck.ai/post/how-to-convert-audio-to-video-for-hosting-on-youtube-and-other-free-video-hosting-sites"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-24T05:08:07.007877+00:00"
fetched_at: "2026-08-19T23:25:46.161203+00:00"
content_hash: "sha256:55e13028982eef873c9718a8505c1e6045b8cc8dbf372a93fdc6a866ecd4977c"
---

# How to convert audio to video for hosting on YouTube and other free video hosting sites

# How to convert audio to video for hosting on YouTube and other free video hosting sites


Use ffmpeg to convert voice ai outputs to video for easier hosting and embedding.


If you want to upload audio to the internet so that you can easily share and embed it, you typically want to use a free video hosting site like YouTube. (As a voice AI site, we have to do this all the time!)


Unfortunately, sites like YouTube don't support uploading audio files directly. Here's how you can create a video with your audio and a static background image so that you can upload it and embed it for free.


1. You need an image to use as the background to your video. You can create an AI image using Dall-E through ChatGPT, or use a tool like Canva.


2. Download the image to the same directory as your audio.


3. Combine the audio and image into a video using ffmpeg (run this command from the same directory as your audio and image files):


` ffmpeg -loop 1 -i background-image.png -i audio.wav -c:v libx264 -tune stillimage -pix_fmt yuv420p -shortest output.mp4`


Now you can upload output.mp4 to YouTube or any other video hosting site.
