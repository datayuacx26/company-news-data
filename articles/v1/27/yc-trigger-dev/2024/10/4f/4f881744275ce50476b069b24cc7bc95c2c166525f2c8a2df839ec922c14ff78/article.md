---
schema_version: "1.0.0"
document_id: "4f881744275ce50476b069b24cc7bc95c2c166525f2c8a2df839ec922c14ff78"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/supabase-and-trigger-dev"
published_at: "2024-10-17T12:00:00+00:00"
first_seen_at: "2026-07-24T05:26:40.243460+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:58d1f622eb7b2e1ae25eaf9951a3f34c3cd3b949f032829281327b0d8f172bde"
---

# Trigger anything from a database change using Supabase with Trigger.dev

## Introduction


In this article we'll be learning how to use Trigger.dev with Supabase:


- [What are the benefits of using Trigger.dev with Supabase?](https://trigger.dev/blog/supabase-and-trigger-dev#what-are-the-benefits-of-using-triggerdev-with-supabase)
- [Unlocking the power of Supabase and Trigger.dev](https://trigger.dev/blog/supabase-and-trigger-dev#unlocking-the-power-of-supabase-and-triggerdev)


- [Triggering tasks from Supabase Edge Functions](https://trigger.dev/blog/supabase-and-trigger-dev#triggering-tasks-from-supabase-edge-functions)
- [Performing database operations](https://trigger.dev/blog/supabase-and-trigger-dev#performing-database-operations)
- [Interacting with Supabase storage](https://trigger.dev/blog/supabase-and-trigger-dev#interacting-with-supabase-storage)
- [Triggering tasks from database events: creating an advanced video processing pipeline](https://trigger.dev/blog/supabase-and-trigger-dev#creating-an-advanced-video-processing-pipeline)


- [Next steps](https://trigger.dev/blog/supabase-and-trigger-dev#next-steps)


## What are the benefits of using Trigger.dev with Supabase?


[Supabase](https://supabase.com/) is an open-source Firebase alternative, providing a backend-as-a-service with a full suite of tools you can use in your projects, including a Postgres-compatible relational database, Database Webhooks, Edge Functions, authentication, instant APIs, realtime and storage.


Used in conjunction with Trigger.dev, you can trigger tasks from database events, and then offload the execution to Trigger.dev’s elastic infrastructure.


This makes it easy to build event-driven workflows, written in normal code, that interact with your application, database and external services.


`
// Add a new user subscription


export const createUserSubscription = task({


id: "create-user-subscription",


run: async (payload: { userId: string; plan: string }) => {


const { userId, plan } = payload;


logger.log(\`Inserting new user ${userId} to user_subscriptions table\`);


const { error } = await supabase.from("user_subscriptions").insert({


user_id: userId,


plan: plan,


});


if (error) throw new Error(\`Failed to insert new user: ${error.message}\`);


logger.log(\`New user added successfully: ${userId} on ${plan} plan\`);


return { userId, plan };


},


});


`


Every run is observable in the Trigger.dev dashboard, giving you live trace views and detailed logs to help with development and debugging.


## Unlocking the power of Supabase and Trigger.dev


We have created a series of examples and guides which cover a range of common use cases. They can be extended and modified to get the best out of Supabase and Trigger.dev.


### Triggering tasks from Supabase Edge Functions


Supabase Edge functions run close to your users for faster response times. They are executed in a secure environment and have access to the same APIs as your application. They can be used to trigger a task from an external event such as a webhook, or from a database event with the payload passed through to the task.


This very basic implementation demonstrates how to trigger a 'Hello World' task from a Supabase Edge Function when the Edge Function URL is visited.


`
Deno.serve(async () => {


await tasks.trigger<typeof helloWorldTask>(


// Your task id


"hello-world",


// Your task payload


"Hello from a Supabase Edge Function!"


);


return new Response("OK");


});


`


To learn how to build this example yourself, check out our[Triggering tasks from Supabase Edge Functions](https://trigger.dev/docs/guides/frameworks/supabase-edge-functions-basic) guide.


### Performing database operations


You can also run CRUD operations directly from your tasks. This is useful for updating your database in response to an event, e.g. a user signing up, or adding data to a database on a repeating schedule, e.g. every day at 10am, etc.


The task below demonstrates updating a user's subscription in a table by either inserting a new row or updating an existing one with the new plan, depending on whether the user already has a subscription.


It takes a payload with a` userId` and` newPlan` , and updates the` user_subscriptions` table in Supabase with the new plan.


`
export const supabaseUpdateUserSubscription = task({


id: "update-user-subscription",


run: async (payload: { userId: string; newPlan: PlanType }) => {


const { userId, newPlan } = payload;


// Abort the task run without retrying if the new plan type is invalid


if (!\["hobby", "pro", "enterprise"\].includes(newPlan)) {


throw new AbortTaskRunError(


\`Invalid plan type: ${newPlan}. Allowed types are 'hobby', 'pro', or 'enterprise'.\`


);


}


// Query the user_subscriptions table to check if the user already has a subscription


const { data: existingSubscriptions } = await supabase


.from("user_subscriptions")


.select("user_id")


.eq("user_id", userId);


if (!existingSubscriptions || existingSubscriptions.length === 0) {


// If there are no existing users with the provided userId and plan, insert a new row


const { error: insertError } = await supabase


.from("user_subscriptions")


.insert({


user_id: userId,


plan: newPlan,


updated_at: new Date().toISOString(),


});


// If there was an error inserting the new subscription, throw an error


if (insertError) {


throw new Error(


\`Failed to insert user subscription: ${insertError.message}\`


);


}


} else {


// If the user already has a subscription, update their existing row


const { error: updateError } = await supabase


.from("user_subscriptions")


// Set the plan to the new plan and update the timestamp


.update({ plan: newPlan, updated_at: new Date().toISOString() })


.eq("user_id", userId);


// If there was an error updating the subscription, throw an error


if (updateError) {


throw new Error(


\`Failed to update user subscription: ${updateError.message}\`


);


}


}


// Return an object with the userId and newPlan


return { userId, newPlan };


},


});


`


To learn more, check out our[Supabase database operations](https://trigger.dev/docs/guides/examples/supabase-database-operations) guide in our docs.


### Interacting with Supabase storage


Supabase Storage can be used to store images, videos, or any other type of file.


Tasks using Supabase Storage can be useful for media processing, such as transcoding videos, resizing images, AI manipulations, etc.


This task demonstrates uploading a video from a` videoUrl` to a storage bucket using the Supabase client:


`
export const supabaseStorageUpload = task({


id: "supabase-storage-upload",


run: async (payload: { videoUrl: string }) => {


const { videoUrl } = payload;


const bucket = "my_bucket";


const objectKey = \`video_${Date.now()}.mp4\`;


// Download video data as a buffer


const response = await fetch(videoUrl);


const videoBuffer = await response.buffer();


// Upload the video directly to Supabase Storage


const { error } = await supabase.storage


.from(bucket)


.upload(objectKey, videoBuffer, {


contentType: "video/mp4",


upsert: true,


});


// Return the video object key and bucket


return {


objectKey,


bucket: bucket,


};


},


});


`


To learn more, check out our[Supabase storage upload](https://trigger.dev/docs/guides/examples/supabase-storage-upload) examples in our docs.


### Creating an advanced video processing pipeline


You can also trigger tasks from database events, such as a new row being inserted into a table. This adds realtime functionality to your tasks, allowing you to build workflows that respond to changes in your database.


In this more advanced example, we'll show you how to trigger a task when a row is added to a table in a Supabase database, using a Database Webhook and Edge Function.


This task will download a video from a URL, extract the audio using FFmpeg, transcribe the video using Deepgram and then update the table with the new transcription.


#### Triggering a task from a database event


In Supabase, a database webhook has been created which triggers an Edge Function called` video-processing-handler` when a new row containing a` video_url` is inserted into the` video_transcriptions` table. The webhook will pass the new row data to the Edge Function as a payload.


This Edge Function triggers a` videoProcessAndUpdate` task:


video-processing-handler.ts


`
Deno.serve(async (req) => {


const payload = await req.json();


// This payload will contain the video url and id from the new row in the table


const videoUrl = payload.record.video_url;


const id = payload.record.id;


// Trigger the videoProcessAndUpdate task with the videoUrl payload


await tasks.trigger<typeof videoProcessAndUpdate>(


"video-process-and-update",


{ videoUrl, id }


);


console.log(payload ?? "No name provided");


return new Response("ok");


});


`


#### Creating the video processing task


The` videoProcessAndUpdate` task takes the payload from the Edge Function, and uses it to download the video as a temporary file from the URL, then transcribes the video using Deepgram AI and then updates the` video_uploads` table in Supabase with the new transcription.


videoProcessAndUpdate.ts


`
export const videoProcessAndUpdate = task({


id: "video-process-and-update",


run: async (payload: { videoUrl: string; id: number }) => {


const { videoUrl, id } = payload;


const outputPath = path.join(os.tmpdir(), \`audio_${Date.now()}.wav\`);


const response = await fetch(videoUrl);


// Extract the audio using FFmpeg


await new Promise((resolve, reject) => {


if (!response.body) {


return reject(new Error("Failed to fetch video"));


}


ffmpeg(Readable.from(response.body))


.outputOptions(\[


"-vn", // Disable video output


"-acodec pcm_s16le", // Use PCM 16-bit little-endian encoding


"-ar 44100", // Set audio sample rate to 44.1 kHz


"-ac 2", // Set audio channels to stereo


\])


.output(outputPath)


.on("end", resolve)


.on("error", reject)


.run();


});


logger.log(\`Audio extracted from video\`, { outputPath });


// Transcribe the audio using Deepgram


const { result, error } = await deepgram.listen.prerecorded.transcribeFile(


fs.readFileSync(outputPath),


{


model: "nova-2", // Use the Nova 2 model


smart_format: true, // Automatically format the transcription


diarize: true, // Enable speaker diarization


}


);


if (error) throw error;


// Extract the transcription from the result


const transcription =


result.results.channels\[0\].alternatives\[0\].paragraphs?.transcript;


fs.unlinkSync(outputPath); // Delete the temporary audio file


logger.log(\`Temporary audio file deleted\`, { outputPath });


const { error: updateError } = await supabase


.from("video_transcriptions")


// Update the transcription and video_url columns


.update({ transcription: transcription, video_url: videoUrl })


// Find the row by its ID


.eq("id", id);


if (updateError) {


throw new Error(\`Failed to update transcription: ${updateError.message}\`);


}


return {


message: \`Summary of the audio: ${transcription}\`,


result,


};


},


});


`


### Check out the video processing pipeline in action


To build this example yourself, check out our[Triggering tasks from database events](https://trigger.dev/docs/guides/frameworks/supabase-edge-functions-database-webhooks) guide in our docs.


## Next steps


In this article we've covered how to trigger tasks from Supabase Edge Functions, perform database operations and interact with Supabase Storage. We've also shown you how to trigger tasks from database events, and build a more complex workflow that interacts with your database and external services.


To follow along with the examples in this article, all our guides and examples can be found in our[docs](https://trigger.dev/docs) .


- [Triggering tasks from Supabase Edge Functions](https://trigger.dev/docs/guides/frameworks/supabase-edge-functions-basic)
- [Supabase database operations](https://trigger.dev/docs/guides/examples/supabase-database-operations)
- [Supabase Storage Upload](https://trigger.dev/docs/guides/examples/supabase-storage-upload)
- [Triggering tasks from database events](https://trigger.dev/docs/guides/frameworks/supabase-edge-functions-database-webhooks)


If you found this article useful, please consider sharing it with anyone you think would be interested in using Supabase with Trigger.dev.


[Sign up to Trigger.dev](https://cloud.trigger.dev/) and get started today.
