---
schema_version: "1.0.0"
document_id: "a1ed79cc7c53e499f4190349d25131f12f653457b525ce29211d9f5d5cabdc8d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/edge-functions-background-tasks-websockets"
published_at: "2024-12-03T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:3d49b3b80b54275a8cf5aa832356efb9e5f2a3fff2ec7dd8b0851355a070a36c"
---

# Supabase Edge Functions: Introducing Background Tasks, Ephemeral Storage, and WebSockets

We are excited to announce three long-awaited features: Background Tasks, Ephemeral File Storage, and WebSockets.


Starting today, you can use these features in any project. Let's explore what exciting things you can build with them.


## Background Tasks#


Sometimes you need a backend logic to do more than respond to a request. For example, you might want to process a batch of files and upload the results to Supabase Storage. Or read multiple entries from a database table and generate embeddings for each entry.


With the introduction of background tasks, executing these long-running workloads with Edge Functions is super easy.


We've introduced a new method called` EdgeRuntime.waitUntil` , which accepts a promise. This ensures that the function isn't terminated until the promise is resolved.


Free projects can run background tasks for a maximum of 150 seconds (2m 30s). If you are on a paid plan, this limit increases to 400 seconds (6m 40s). We plan to introduce more flexible limits in the coming months.


You can subscribe to notifications when the function is about to be shut down by listening to` beforeunload` event. Read the guide for more details on[how to use background tasks](https://supabase.com/docs/guides/functions/background-tasks) .


## Ephemeral Storage#


Edge Function invocations now have access to ephemeral storage. This is useful for background tasks, as it allows you to read and write files in the` /tmp` directory to store intermediate results.


Check the guide on how to access[ephemeral storage](https://supabase.com/docs/guides/functions/ephemeral-storage) .


### Example: Extracting a zip file and uploading its content to Supabase Storage#


Let's look at a real-world example using Background Tasks and Ephemeral Storage.


Imagine you're building a Photo Album app. You want your users to upload photos as a zip file. You would extract them in an Edge Function and upload them to storage.


One of the most straightforward ways to implement is using streams:


`
_35


import { ZipReaderStream } from 'https://deno.land/x/zipjs/index.js'


_35


import { createClient } from 'jsr:@supabase/supabase-js@2'


_35


_35


const supabase = createClient(


_35


Deno.env.get('SUPABASE_URL'),


_35


Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')


_35


)


_35


_35


Deno.serve(async (req) => {


_35


const uploadId = crypto.randomUUID()


_35


_35


const { error } = await supabase.storage.createBucket(uploadId, {


_35


public: false,


_35


})


_35


_35


for await (const entry of await req.body.pipeThrough(new ZipReaderStream())) {


_35


// write file to Supabase Storage


_35


const { error } = await supabase.storage


_35


.from(uploadId)


_35


.upload(entry.filename, entry.readable, {})


_35


_35


console.log('uploaded', entry.filename)


_35


}


_35


_35


return new Response(


_35


JSON.stringify({


_35


uploadId,


_35


}),


_35


{


_35


headers: {


_35


'content-type': 'application/json',


_35


},


_35


}


_35


)


_35


})


`


If you test out the streaming version, it will run into memory limit errors when you try to upload zip files over 100MB. This is because the streaming version has to keep every file in a zip archive in memory.


We can modify it instead to write the zip file to a temporary file. Then, use a background task to extract and upload it to Supabase Storage. This way, we only read parts of the zip file to the memory.


`
_73


import { BlobWriter, ZipReader, ZipReaderStream } from 'https://deno.land/x/zipjs/index.js'


_73


_73


import { createClient } from 'jsr:@supabase/supabase-js@2'


_73


_73


const supabase = createClient(


_73


Deno.env.get('SUPABASE_URL'),


_73


Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')


_73


)


_73


_73


let numFilesUploaded = 0


_73


_73


async function processZipFile(uploadId, filepath) {


_73


const file = await Deno.open(filepath, { read: true })


_73


const zipReader = new ZipReader(file.readable)


_73


const entries = await zipReader.getEntries()


_73


_73


await supabase.storage.createBucket(uploadId, {


_73


public: false,


_73


})


_73


_73


await Promise.all(


_73


entries.map(async (entry) => {


_73


// read file entry


_73


const blobWriter = new BlobWriter()


_73


const blob = await entry.getData(blobWriter)


_73


_73


if (entry.directory) {


_73


return


_73


}


_73


_73


// write file to Supabase Storage


_73


await supabase.storage.from(uploadId).upload(entry.filename, blob, {})


_73


_73


numFilesUploaded += 1


_73


console.log('uploaded', entry.filename)


_73


})


_73


)


_73


_73


await zipReader.close()


_73


}


_73


_73


// you can add a \`beforeunload\` event listener to be notified


_73


// when Function Worker is about to terminate.


_73


// use this to do any logging, save states.


_73


globalThis.addEventListener('beforeunload', (ev) => {


_73


console.log('function about to terminate: ', ev.detail.reason)


_73


console.log('number of files uploaded: ', numFilesUploaded)


_73


})


_73


_73


async function writeZipFile(filepath, stream) {


_73


await Deno.writeFile(filepath, stream)


_73


}


_73


_73


Deno.serve(async (req) => {


_73


const uploadId = crypto.randomUUID()


_73


await writeZipFile('/tmp/' + uploadId, req.body)


_73


_73


// process zip file in a background task


_73


// calling EdgeRuntime.waitUntil() would ensure


_73


// function worker wouldn't exit until the promise is completed.


_73


EdgeRuntime.waitUntil(processZipFile(uploadId, '/tmp/' + uploadId))


_73


_73


return new Response(


_73


JSON.stringify({


_73


uploadId,


_73


}),


_73


{


_73


headers: {


_73


'content-type': 'application/json',


_73


},


_73


}


_73


)


_73


})


`


## WebSockets#


Edge Functions now support establishing both inbound (server) and outbound (client) WebSocket connections. This enables a variety of new use cases.


### Example: Building an authenticated relay to OpenAI Realtime API#


OpenAI recently introduced a[Realtime API](https://openai.com/index/introducing-the-realtime-api/) , which uses WebSockets. This is tricky to implement purely client-side because you'd need to expose your OpenAI key publicly. OpenAI[recommends](https://platform.openai.com/docs/guides/realtime/overview?text-generation-quickstart-example=audio) building a server to authenticate requests.


With our new support for WebSockets, you can easily do this in Edge Functions without standing up any infrastructure. Additionally, you can use[Supabase Auth](https://supabase.com/docs/guides/auth) to authenticate users and protect your OpenAI usage from being abused.


`
_80


import { createClient } from 'jsr:@supabase/supabase-js@2'


_80


_80


const supabase = createClient(


_80


Deno.env.get('SUPABASE_URL'),


_80


Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')


_80


)


_80


const OPENAI_API_KEY = Deno.env.get('OPENAI_API_KEY')


_80


_80


Deno.serve(async (req) => {


_80


const upgrade = req.headers.get('upgrade') || ''


_80


_80


if (upgrade.toLowerCase() != 'websocket') {


_80


return new Response("request isn't trying to upgrade to websocket.")


_80


}


_80


_80


// WebSocket browser clients does not support sending custom headers.


_80


// We have to use the URL query params to provide user's JWT.


_80


// Please be aware query params may be logged in some logging systems.


_80


const url = new URL(req.url)


_80


const jwt = url.searchParams.get('jwt')


_80


if (!jwt) {


_80


console.error('Auth token not provided')


_80


return new Response('Auth token not provided', { status: 403 })


_80


}


_80


const { error, data } = await supabase.auth.getUser(jwt)


_80


if (error) {


_80


console.error(error)


_80


return new Response('Invalid token provided', { status: 403 })


_80


}


_80


if (!data.user) {


_80


console.error('user is not authenticated')


_80


return new Response('User is not authenticated', { status: 403 })


_80


}


_80


_80


const { socket, response } = Deno.upgradeWebSocket(req)


_80


_80


socket.onopen = () => {


_80


// initiate an outbound WebSocket connection to OpenAI


_80


const url = 'wss://api.openai.com/v1/realtime?model=gpt-4o-realtime-preview-2024-10-01'


_80


_80


// openai-insecure-api-key isn't a problem since this code runs in an Edge Function


_80


const openaiWS = new WebSocket(url, \[


_80


'realtime',


_80


\`openai-insecure-api-key.${OPENAI_API_KEY}\`,


_80


'openai-beta.realtime-v1',


_80


\])


_80


_80


openaiWS.onopen = () => {


_80


console.log('Connected to OpenAI server.')


_80


_80


socket.onmessage = (e) => {


_80


console.log('socket message:', e.data)


_80


// only send the message if openAI ws is open


_80


if (openaiWS.readyState === 1) {


_80


openaiWS.send(e.data)


_80


} else {


_80


socket.send(


_80


JSON.stringify({


_80


type: 'error',


_80


msg: 'openAI connection not ready',


_80


})


_80


)


_80


}


_80


}


_80


}


_80


_80


openaiWS.onmessage = (e) => {


_80


console.log(e.data)


_80


socket.send(e.data)


_80


}


_80


_80


openaiWS.onerror = (e) => console.log('OpenAI error: ', e.message)


_80


openaiWS.onclose = (e) => console.log('OpenAI session closed')


_80


}


_80


_80


socket.onerror = (e) => console.log('socket errored:', e.message)


_80


socket.onclose = () => console.log('socket closed')


_80


_80


return response // 101 (Switching Protocols)


_80


})


`


## Performance and stability#


In the past few months, we have made many[performance, stability,](https://supabase.com/blog/edge-functions-faster-smaller) and[DX improvements](https://github.com/orgs/supabase/discussions/30307) to Edge Functions. While these improvements often aren't visible to the end-users, they are the foundation of the new features we are announcing today.


## What's next?#


We have a very exciting roadmap planned for 2025. One of the main priorities is to provide customizable compute limits (memory, CPU, and execution duration). We will soon announce an update on it.


Stay tuned for the upcoming launches this week. You will see how all these upcoming pieces fit like Lego bricks to make your developer life easy.
