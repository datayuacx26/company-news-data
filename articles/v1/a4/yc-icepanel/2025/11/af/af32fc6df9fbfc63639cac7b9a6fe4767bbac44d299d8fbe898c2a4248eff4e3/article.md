---
schema_version: "1.0.0"
document_id: "af32fc6df9fbfc63639cac7b9a6fe4767bbac44d299d8fbe898c2a4248eff4e3"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2025-11-18-design-youtube-using-icepanel"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:3ade39b6fe8b41aa1a6f54046f0213a9cb7bf292a74e47181f497c88c20458c3"
---

# Design YouTube using IcePanel

## 📝 Introduction


In this post, we’ll share an example architecture for **YouTube** – the largest video-sharing and streaming platform where users can watch and upload videos. We’ll design this as software architects and create four hierarchical diagrams: Context, Container, Component, and Code (not sure what these are? Read[this](https://icepanel.io/blog/2024-07-18-what-is-the-c4-model) ). We’ll annotate the building blocks and highlight user interaction (data flow) within the system using IcePanel[Flows](https://docs.icepanel.io/visual-storytelling/flows) .


Let’s first set the scope by defining the behaviour of the system (functional requirements) and the qualities the system should have (non-functional requirements). Afterwards, we’ll go through each layer of the C4 model.


You can view the final architecture at this link:[https://s.icepanel.io/DWnaysJ3cbCQqg/qmVa](https://s.icepanel.io/DWnaysJ3cbCQqg/qmVa)


---


## 🔎 Scope


We have two simple requirements for a video-sharing platform, users should be able to:


1. Watch videos
2. Upload videos


For non-functional requirements, the system should be:


- 99.99% available for watching videos.
- Eventually consistent for uploads (will handle video processing asynchronously).
- Highly performant with different resolutions for video streaming.


Let’s start with the first diagram, Context.


## Level 1 - Context


This high-level view defines the main actors interacting with our internal system (YouTube). In this case, we have two:


- Viewer: End user who watches videos on the platform.
- Content Creator: End user who uploads videos on the platform.


Given our defined scope, we don’t have any external systems in this view. The core business logic is running within the YouTube system.


---


## Level 2 - Container


This diagram is where we model a collection of independently deployable or runnable applications or data stores that are essential for the overall software system to function. This could be a web application, server, datastore, or a serverless function like[AWS lambda](https://aws.amazon.com/pm/lambda) .


We’ll design this system as an **event-based** architecture. This approach gives us several benefits that align with our non-functional requirements, such as:


1. Asynchronous processing: Long-running tasks like video transcoding and analysis can be processed in the background without blocking user requests or other services.
2. Improved scalability: Each service can scale independently based on its specific workload, and message queues naturally handle load buffering during traffic spikes.
3. Better resource utilization: Workers can process tasks from queues at their own pace, preventing system overload and enabling more efficient use of computing resources.
4. Easier debugging and monitoring: Event flows create a clear audit trail, making it simpler to track video processing stages and identify bottlenecks.


Our system is composed of the following Containers:


1. **Upload Service** : Manages video uploads to pre-processed storage and writing metadata to the database.
2. **Raw Videos / YouTube Videos** : S3 buckets for storing raw / processed videos.
3. **Chunker** : Event-based worker responsible for segmenting raw videos into 5-10 sec clips for later processing.
4. **Video Processing Service** : Orchestrates the video processing workflow by calling validation services and kicking off processing jobs in parallel.
5. **Video Inspection Service** : Handle video processing using[AWS Rekognition](https://aws.amazon.com/rekognition/) service.
6. **Policy Validation Service:** AI-powered solution for inspecting video context (text, audio, etc.) using AWS[Bedrock](https://docs.aws.amazon.com/bedrock) .
7. **Asynchronous Workers** : A group of AWS Lambdas that scale up or down based on demand and event traffic. It is a virtual cluster of transcoding, audio processing, and transcript generation jobs.
8. **Watch Service** : Handles video playback requests and serves video metadata to viewers.
9. **Video Metadata Database** : Stores video metadata (e.g., title, description, thumbnail, date, etc.) and URLs of processed videos at different resolutions (e.g., 420p, 780p, etc.)
10. **Video Metadata Cache** : Caches frequently accessed videos for faster load time.


In this architecture, we use the following technologies:


1. **AWS API Gateway** : A scalable and secure entry point for all API requests coming from the frontend.
2. **AWS EC2:** Web service that provides reliable and secure compute for the different microservices.
3. **AWS Lambda** : A lightweight, serverless compute service that is cost-effective, event-driven, and automatically scales on demand.
4. **PostgreSQL** : An ACID-compliant relational database that ensures data integrity and strong consistency. Ideal for pre-defined schemas like a video metadata table.
5. **Redis** : A fast in-memory cache that improves response times and reduces database load.


We’ve designed two common data flows using IcePanel. Check out these flows and play them step by step to see how our system works.


1. Upload a new YouTube video:[https://s.icepanel.io/DWnaysJ3cbCQqg/h1Au](https://s.icepanel.io/DWnaysJ3cbCQqg/h1Au)
2. Watch a YouTube video:[https://s.icepanel.io/DWnaysJ3cbCQqg/eyRu](https://s.icepanel.io/DWnaysJ3cbCQqg/eyRu)


---


## Level 3 - Component


In the C4 model, a component is a grouping of related functionality encapsulated behind a well-defined interface. For example, a collection of classes behind an interface. Let’s look at the Components in YouTube.


### 1. Upload Service


The diagram below shows how a user interacts with the Upload service to upload a new video. When a user sends a POST /video request, the Upload API first creates an entry for the upload status and metadata to the database, then calls the S3 Client to generate a[pre-signed URL for secure raw video storage](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html) . This pre-signed URL is returned to the user, who uses it to upload their video directly to the Raw Videos S3 bucket. Once the upload is finished, the Upload Repository stores the new S3 url to the Video Metadata Database (PostgreSQL), storing other information like upload date, thumbnail, other metadata, and the initial processing status for subsequent workflow stages.


### 2. Video Processing Service


This service operates as an event-driven orchestrator for video processing workflows. When a new video is uploaded to the Raw Videos S3 bucket, it triggers an event that sends chunked videos (5-10 sec clips) for processing via a message queue. The Processing Worker listens to S3 bucket events and initiates the workflow by calling the Video Orchestrator. The Video Orchestrator manages and coordinates all video processing tasks in a sequential manner, starting with the Video Analyser for content inspection. If the video fails the inspection stage, the workflow is blocked and processing terminates. Upon passing inspection, the orchestrator then calls the Policy Checker for content validation against community guidelines. If the video fails the policy check stage, the workflow is similarly blocked, and no further processing occurs. Only after successfully passing both inspection and policy checks does the orchestrator dispatch jobs to the Job Manager. The Job Manager sends video processing jobs to Asynchronous Workers via message queues, which handle the compute-intensive transcoding, audio processing, and transcript generation tasks without blocking other operations. The processed video is then stored into the YouTube S3 bucket with different resolutions and updated with S3 URLs to the database for serving users.


### 3. Watch Service


The Watch Service handles all video streaming requests in our YouTube system. When a viewer sends a GET request to watch a video, the Watch API receives the request and first checks if the requested manifest file for the video is already in the cache. If not available in cache, the Watch API calls the Watch Repository to query the manifest files and video metadata from the Video Metadata Database (PostgreSQL). Once the manifest file is retrieved, the user then watches the video by repeatedly requesting video segments in the manifest file from the main S3 bucket through dynamic HTTP streaming protocols (e.g.,[DASH or HLS](https://blog.eleven-labs.com/en/video-live-dash-hls/) ). The user does not download the whole video, they download the chunks sequentially as they’re watching.


Let’s go one level deeper with the source code in the Code layer.


---


## Level 4 - Code


This is where we can view implementation details at the code level. We don’t recommend creating extensive diagrams for this; instead, we link directly to the code. However, here’s the general structure of these components for reference. We’ll briefly go over two Components: **Upload** and **Watch** .


### Code Classes in “Upload Component”


This component handles the video upload workflow. The Upload API class provides RESTful endpoints for users to initiate video uploads and obtain secure upload URLs. The S3 Client manages interactions with the S3 bucket, generating pre-signed URLs that allow users to upload videos directly to the Raw Videos bucket without routing the file through the application server. The Upload Repository serves as the data layer for creating and managing video metadata entries in the database.


#### Upload API


- POST /upload - Initiate a new video upload and receive pre-signed URL
- GET /upload/:videoId/status - Check upload status for a specific video
- POST /upload/:videoId/complete - Notify system that upload is complete


#### S3 Client


- generatePresignedUrl(videoId, expiryTime) - Create secure S3 upload URL with expiration


#### Upload Repository


- createVideoEntry(userId, videoId, metadata) - Create new video record in database
- updateVideoStatus(videoId, status) - Update video upload state (pending, completed, failed)


### Code Classes in Watch Component


This component manages video streaming and playback requests. The Watch API class exposes RESTful endpoints for retrieving video manifest files and metadata. The API first checks if the requested video files are already cached in Redis before querying the database. The Watch Repository serves as the data layer for fetching video information and streaming URLs from the Video Metadata Database.


These classes have roughly the following methods:


#### Watch API


- GET /watch/:videoId - Retrieve video details (title, description, etc.) and manifest files.
- GET /videos/:videoId/:segmentId - Serve individual video segment files directly from S3 (or CDN).


#### Video Cache Manager


- getCachedVideo(videoId) - Retrieve video metadata & manifest file from cache
- cacheVideo(videoId, videoData, ttl) - Store video metadata & manifest file with time-to-live


#### Watch Repository


- getVideoById(videoId) - Fetch video metadata from database.
- getVideoManifestFile(videoId) - Retrieve manifest file for streaming.


---


## Conclusion


In this post, we designed a video-streaming platform using the C4 model on IcePanel. We began with the core requirements and modeled the system from the top down, starting with the Context layer, followed by the Container, Component, and finally the Code layer.


P.S. We’ve also designed Ticketmaster on IcePanel! Check out:[https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel](https://icepanel.io/blog/2025-10-20-design-ticketmaster-using-icepanel)


Let us know which system you’d like to see modeled next on IcePanel!


## 📚 Resources


- [https://aws.amazon.com/api-gateway](https://aws.amazon.com/api-gateway/)
- [https://aws.amazon.com/sqs](https://aws.amazon.com/sqs/)
- [https://aws.amazon.com/rekognition/](https://aws.amazon.com/rekognition/)
- [https://docs.aws.amazon.com/bedrock](https://docs.aws.amazon.com/bedrock)
- [https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)
- [https://redis.io/open-source](https://redis.io/open-source/)
- [https://aws.amazon.com/microservices](https://aws.amazon.com/microservices)
- [https://aws.amazon.com/pm/lambda](https://aws.amazon.com/pm/lambda/)
