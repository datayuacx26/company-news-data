---
schema_version: "1.0.0"
document_id: "0f7e0db8a6d3b95463df8a8d36c98fe53818f3ad3d79a20c5b412a68476c85b2"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/mozilla-llamafile-in-supabase-edge-functions"
published_at: "2024-08-21T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:c4ff7b729831d7913d5e320babc2085ed0ea412a3710fd1c3a1cecb5479203ee"
---

# Mozilla Llamafile in Supabase Edge Functions

A few months back, we introduced support for running[AI Inference directly from Supabase Edge Functions](https://supabase.com/blog/ai-inference-now-available-in-supabase-edge-functions) .


Today we are adding[Mozilla Llamafile](https://github.com/Mozilla-Ocho/llamafile) , in addition to[Ollama](https://supabase.com/docs/guides/functions/ai-models#using-large-language-models) , to be used as the Inference Server with your functions.


Mozilla Llamafile lets you distribute and run LLMs with a single file that runs locally on most computers, with no installation! In addition to a local web UI chat server, Llamafile also provides an OpenAI API compatible server, that is now integrated with Supabase Edge Functions.


Want to jump straight into the code? You can find the examples on[GitHub](https://github.com/supabase/supabase/blob/master/examples/ai/llamafile-edge) !


## Getting started#


Follow the[Llamafile Quickstart Guide](https://github.com/Mozilla-Ocho/llamafile?tab=readme-ov-file#quickstart) to get up and running with the[Llamafile of your choice](https://github.com/Mozilla-Ocho/llamafile?tab=readme-ov-file#other-example-llamafiles) .


Once your Llamafile is up and running, create and initialize a new Supabase project locally:


`
_10


npx supabase bootstrap scratch


`


If using VS Code, when promptedt` Generate VS Code settings for Deno? \[y/N\]` select` y` and follow the steps. Then open the project in your favoiurte code editor.


## Call Llamafile with functions-js#


Supabase Edge Functions now comes with an OpenAI API compatible mode, allowing you to call a Llamafile server easily via` @supabase/functions-js` .


Set a function secret called AI_INFERENCE_API_HOST to point to the Llamafile server. If you don't have one already, create a new` .env` file in the` functions/` directory of your Supabase project.


supabase/functions/


.env


`
_10


AI_INFERENCE_API_HOST=http://host.docker.internal:8080


`


Next, create a new function called` llamafile` :


`
_10


npx supabase functions new llamafile


`


Then, update the` supabase/functions/llamafile/index.ts` file to look like this:


supabase/functions/llamafile/


index.ts


`
_31


import 'jsr:@supabase/functions-js/edge-runtime.d.ts'


_31


const session = new Supabase.ai.Session('LLaMA_CPP')


_31


_31


Deno.serve(async (req: Request) => {


_31


const params = new URL(req.url).searchParams


_31


const prompt = params.get('prompt') ?? ''


_31


_31


// Get the output as a stream


_31


const output = await session.run(


_31


{


_31


messages: \[


_31


{


_31


role: 'system',


_31


content:


_31


'You are LLAMAfile, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests.',


_31


},


_31


{


_31


role: 'user',


_31


content: prompt,


_31


},


_31


\],


_31


},


_31


{


_31


mode: 'openaicompatible', // Mode for the inference API host. (default: 'ollama')


_31


stream: false,


_31


}


_31


)


_31


_31


console.log('done')


_31


return Response.json(output)


_31


})


`


## Call Llamafile with the OpenAI Deno SDK#


Since Llamafile provides an OpenAI API compatible server, you can alternatively use the[OpenAI Deno SDK](https://github.com/openai/openai-deno) to call Llamafile from your Supabase Edge Functions.


For this, you will need to set the following two environment variables in your Supabase project. If you don't have one already, create a new` .env` file in the` functions/` directory of your Supabase project.


supabase/functions/


.env


`
_10


OPENAI_BASE_URL=http://host.docker.internal:8080/v1


_10


OPENAI_API_KEY=sk-XXXXXXXX # need to set a random value for openai sdk to work


`


Now, replace the code in your` llamafile` function with the following:


supabase/functions/llamafile/


index.ts


`
_54


import OpenAI from 'https://deno.land/x/openai@v4.53.2/mod.ts'


_54


_54


Deno.serve(async (req) => {


_54


const client = new OpenAI()


_54


const { prompt } = await req.json()


_54


const stream = true


_54


_54


const chatCompletion = await client.chat.completions.create({


_54


model: 'LLaMA_CPP',


_54


stream,


_54


messages: \[


_54


{


_54


role: 'system',


_54


content:


_54


_54


},


_54


{


_54


role: 'user',


_54


content: prompt,


_54


},


_54


\],


_54


})


_54


_54


if (stream) {


_54


const headers = new Headers({


_54


'Content-Type': 'text/event-stream',


_54


Connection: 'keep-alive',


_54


})


_54


_54


// Create a stream


_54


const stream = new ReadableStream({


_54


async start(controller) {


_54


const encoder = new TextEncoder()


_54


_54


try {


_54


for await (const part of chatCompletion) {


_54


controller.enqueue(encoder.encode(part.choices\[0\]?.delta?.content || ''))


_54


}


_54


} catch (err) {


_54


console.error('Stream error:', err)


_54


} finally {


_54


controller.close()


_54


}


_54


},


_54


})


_54


_54


// Return the stream to the user


_54


return new Response(stream, {


_54


headers,


_54


})


_54


}


_54


_54


return Response.json(chatCompletion)


_54


})


`


Note that the model parameter doesn't have any effect here! The model depends on which Llamafile is currently running!


## Serve your functions locally#


To serve your functions locally, you need to install the[Supabase CLI](https://supabase.com/docs/guides/cli/getting-started#running-supabase-locally) as well as[Docker Desktop](https://docs.docker.com/desktop) or[Orbstack](https://orbstack.dev/) .


You can now serve your functions locally by running:


`
_10


supabase start


_10


supabase functions serve --env-file supabase/functions/.env


`


Execute the function


`
_10


curl --get "http://localhost:54321/functions/v1/llamafile" \\


_10


--data-urlencode "prompt=write a short rap song about Supabase, the Postgres Developer platform, as sung by Nicki Minaj" \\


_10


-H "Authorization: $ANON_KEY"


`


## Deploying a Llamafile#


There is a great guide on how to[containerize a Lllamafile](https://www.docker.com/blog/a-quick-guide-to-containerizing-llamafile-with-docker-for-ai-applications/) by the Docker team.


You can then use a service like[Fly.io](https://fly.io/) to deploy your dockerized Llamafile.


## Deploying your Supabase Edge Functions#


Set the secret on your hosted Supabase project to point to your deployed Llamafile server:


`
_10


supabase secrets set --env-file supabase/functions/.env


`


Deploy your Supabase Edge Functions:


`
_10


supabase functions deploy


`


Execute the function:


`
_10


curl --get "https://project-ref.supabase.co/functions/v1/llamafile" \\


_10


_10


-H "Authorization: $ANON_KEY"


`


## Get access to Supabase Hosted LLMs#


Access to open-source LLMs is currently invite-only while we manage demand for the GPU instances. Please[get in touch](https://forms.supabase.com/supabase.ai-llm-early-access) if you need early access.


We plan to extend support for more models.[Let us know](https://forms.supabase.com/supabase.ai-llm-early-access) which models you want next. We're looking to support fine-tuned models too!


## More Supabase Resources#


- Edge Functions:[supabase.com/docs/guides/functions](https://supabase.com/docs/guides/functions)
- Vectors:[supabase.com/docs/guides/ai](https://supabase.com/docs/guides/ai)
- [Semantic search demo](https://github.com/supabase/supabase/tree/master/examples/ai/edge-functions)
- [Store and query embeddings](https://supabase.com/docs/guides/ai/vector-columns#querying-a-vector--embedding) in Postgres and use them for Retrieval Augmented Generation (RAG) and Semantic Search
