---
schema_version: "1.0.0"
document_id: "b62d3c8d4e65d78ae0374deb1afbef42d5d58e84d322898a78f2d6885136dc61"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/building-chatgpt-plugins-template"
published_at: "2023-05-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:5b0a7f8f0856d7edcdb909c028f4840070306b88e4664335bb4f5e78486aa32a"
---

# Building ChatGPT Plugins with Supabase Edge Runtime

ChatGPT Plugins support is[rolling out in beta](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) this week! To help you get up and running quickly, we're releasing a[plugin template](https://github.com/supabase-community/chatgpt-plugin-template-deno) written in TypeScript and running on[Supabase Edge Runtime](https://supabase.com/blog/edge-runtime-self-hosted-deno-functions) !


Want to get started right away?[Fork the template on GitHub](https://github.com/supabase-community/chatgpt-plugin-template-deno) !


## Serving the manifest file#


The` ai-plugin.json`[manifest file](https://platform.openai.com/docs/plugins/getting-started/plugin-manifest) is required for ChatGPT to identify our plugin, know what kind of authentication mechanism is used, understand where to find the OpenAPI definition, and some other details about our plugin. You can find the full list of supported parameters in the[OpenAI docs](https://platform.openai.com/docs/plugins/getting-started/plugin-manifest) .


Supabase Edge Runtime does currently not support hosting/serving of static files, however, we can import JSON files in our function and serve them as a JSON response. As this needs to be at the root of our domain, we add this to our[main function handler](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/functions/main/index.ts#L69-L74) :


`
_11


// functions/main/index.ts


_11


import aiPlugins from './ai-plugins.json' with { type: 'json' }


_11


_11


// \[...\]


_11


_11


// Serve /.well-known/ai-plugin.json


_11


if (service_name === '.well-known') {


_11


return new Response(JSON.stringify(aiPlugins), {


_11


headers: { ...corsHeaders, 'Content-Type': 'application/json' },


_11


})


_11


}


`


Now, when running Edge Runtime locally via Docker, our plugin manifest will be available at[http://localhost:8000/.well-known/ai-plugin.json](http://localhost:8000/.well-known/ai-plugin.json)


## Generating the OpenAPI definition with swagger-jsdoc#


The[OpenAPI definition](https://platform.openai.com/docs/plugins/getting-started/openapi-definition) is required for ChatGPT to know how to underact with our API. Only endpoints included in there will be exposed to ChatGPT, which allows you to selectively make our endpoints available, or add specific endpoints for ChatGPT.


The OpenAPI definition can be either in YAML or JSON format. We’ll be using JSON and the same approach as above to serve it. Writing an OpenAPI definition is not something we will want to do by hand, luckily there is an open source tool called[swagger-jsdoc](https://github.com/Surnet/swagger-jsdoc) which we can use to annotate our endpoints with JSDoc comments and generate the OpenAPI definition with a[little script](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/scripts/generate-openapi-spec.ts) .


`
_22


// /scripts/generate-openapi-spec.ts


_22


import swaggerJsdoc from 'npm:swagger-jsdoc@6.2.8'


_22


_22


const options = {


_22


definition: {


_22


openapi: '3.0.1',


_22


info: {


_22


title: 'TODO Plugin',


_22


description: \`A plugin that allows the user to create and manage a TODO list using ChatGPT. If you do not know the user's username, ask them first before making queries to the plugin. Otherwise, use the username "global".\`,


_22


version: '1.0.0',


_22


},


_22


servers: \[{ url: 'http://localhost:8000' }\],


_22


},


_22


apis: \['./functions/chatgpt-plugin/index.ts'\], // files containing annotations as above


_22


}


_22


_22


const openapiSpecification = swaggerJsdoc(options)


_22


const openapiString = JSON.stringify(openapiSpecification, null, 2)


_22


const encoder = new TextEncoder()


_22


const data = encoder.encode(openapiString)


_22


await Deno.writeFile('./functions/chatgpt-plugin/openapi.json', data)


_22


console.log(openapiString)


`


Since this script is run outside of the function execution, e.g. as a GitHub Action, we can use[npm specifiers](https://deno.com/manual/node/npm_specifiers) to import` swagger-jsdoc` .


Next, we create our` /functions/chatgpt-plugin/index.ts`[file](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/functions/chatgpt-plugin/index.ts) where we use the[Deno oak router](https://deno.land/x/oak) to build our API and annotate it with JSDOC comments.


`
_64


// /functions/chatgpt-plugin/index.ts


_64


import { Application, Router } from 'https://deno.land/x/oak@v11.1.0/mod.ts'


_64


import openapi from './openapi.json' with { type: 'json' }


_64


_64


console.log('Hello from \`chatgpt-plugin\` Function!')


_64


_64


const _TODOS: { \[key: string\]: Array<string> } = {


_64


user: \['Build your own ChatGPT Plugin!'\],


_64


}


_64


_64


/**


_64


* @openapi


_64


* components:


_64


* schemas:


_64


* getTodosResponse:


_64


* type: object


_64


* properties:


_64


* todos:


_64


* type: array


_64


* items:


_64


* type: string


_64


* description: The list of todos.


_64


*/


_64


_64


const router = new Router()


_64


router


_64


.get('/chatgpt-plugin', (ctx) => {


_64


ctx.response.body = 'Building ChatGPT plugins with Deno!'


_64


})


_64


/**


_64


* @openapi


_64


* /chatgpt-plugin/todos/{username}:


_64


* get:


_64


* operationId: getTodos


_64


* summary: Get the list of todos


_64


* parameters:


_64


* - in: path


_64


* name: username


_64


* schema:


_64


* type: string


_64


* required: true


_64


* description: The name of the user.


_64


* responses:


_64


* 200:


_64


* description: OK


_64


* content:


_64


* application/json:


_64


* schema:


_64


* $ref: '#/components/schemas/getTodosResponse'


_64


*/


_64


.get('/chatgpt-plugin/todos/:username', (ctx) => {


_64


const username = ctx.params.username.toLowerCase()


_64


ctx.response.body = _TODOS\[username\] ?? \[\]


_64


})


_64


.get('/chatgpt-plugin/openapi.json', (ctx) => {


_64


ctx.response.body = JSON.stringify(openapi)


_64


ctx.response.headers.set('Content-Type', 'application/json')


_64


})


_64


_64


const app = new Application()


_64


app.use(router.routes())


_64


app.use(router.allowedMethods())


_64


_64


await app.listen({ port: 8000 })


`


With our JSDoc annotation in place, we can now run the generation script in the terminal:


`
_10


deno run --allow-read --allow-write scripts/generate-openapi-spec.ts


`


## Adding the CORS headers#


Lastly, we need to add some CORS headers to make the browser happy. We define them in a` /functions/_shared/cors.ts`[file](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/functions/_shared/cors.ts) so we can easily reuse them across our` main` and` chatgpt-plugins` function.


`
_10


// /functions/_shared/cors.ts


_10


export const corsHeaders = {


_10


'Access-Control-Allow-Origin': 'https://chat.openai.com',


_10


'Access-Control-Allow-Credentials': 'true',


_10


'Access-Control-Allow-Private-Network': 'true',


_10


'Access-Control-Allow-Headers': '*',


_10


}


`


Now we can easily add them to all our` chatgpt-plugin` routes a middleware for our oak application.


`
_18


// /functions/chatgpt-plugin/index.ts


_18


import { Application, Router } from 'https://deno.land/x/oak@v11.1.0/mod.ts'


_18


import { corsHeaders } from '../_shared/cors.ts'


_18


_18


// \[...\]


_18


const app = new Application()


_18


// ChatGPT specific CORS headers


_18


app.use(async (ctx, next) => {


_18


await next()


_18


let key: keyof typeof corsHeaders


_18


for (key in corsHeaders) {


_18


ctx.response.headers.set(key, corsHeaders\[key\])


_18


}


_18


})


_18


app.use(router.routes())


_18


app.use(router.allowedMethods())


_18


_18


await app.listen({ port: 8000 })


`


## Running locally with Docker#


Now that we’ve got all the pieces in place, let’s spin up Edge Runtime locally and test things out. For this, we need a[Dockerfile](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/Dockerfile) and for convenience, we can add a[docker-compose file](https://github.com/supabase-community/chatgpt-plugin-template-deno/blob/main/docker-compose.yml) also.


`
_10


// Dockerfile


_10


FROM ghcr.io/supabase/edge-runtime:v1.2.18


_10


_10


COPY ./functions /home/deno/functions


_10


CMD \[ "start", "--main-service", "/home/deno/functions/main" \]


`


This will pull down Edge Runtime v1.2.18 (you can check the l[atest release here](https://github.com/supabase/edge-runtime/releases) ) and start up the main service (our` /functions/main/index.ts` function).


`
_11


// docker-compose.yml


_11


version: "3.9"


_11


services:


_11


web:


_11


build: .


_11


volumes:


_11


- type: bind


_11


source: ./functions


_11


target: /home/deno/functions


_11


ports:


_11


- "8000:9000"


`


Edge Runtime will serve requests on port` 9000` , so we’re creating a mapping from` \[localhost:8000\](http://localhost:8000)` where we want to serve our requests locally (of course you can adapt this to your needs) to port` 9000` of our Docker container.


Furthermore, we’re using[bind mounts](https://docs.docker.com/storage/bind-mounts/) to mount our functions directory into the container. This allows us to make modifications to our functions without needing to rebuild the container after, making for a great local developer experience.


That’s it, now we can build and spin up our container from the terminal:


`
_10


docker compose up --build


`


Go ahead and try it out by visiting:


- [http://localhost:8000/chatgpt-plugin](http://localhost:8000/chatgpt-plugin)
- [http://localhost:8000/.well-known/ai-plugin.json](http://localhost:8000/.well-known/ai-plugin.json)
- [http://localhost:8000/chatgpt-plugin/openapi.json](http://localhost:8000/chatgpt-plugin/openapi.json)
- [http://localhost:8000/chatgpt-plugin/todos/user](http://localhost:8000/chatgpt-plugin/todos/user)


## Installing and testing the plugin locally#


You can conveniently test your plugin while running it on[localhost](http://localhost/) using the[ChatGPT UI](https://chat.openai.com/) :


1. Select the plugin model from the top drop down, then select “Plugins”, “Plugin Store”, and finally “Develop your own plugin”.
2. Enter` localhost:8000` and click "Find manifest file".
3. Confirm with “Install[localhost](http://localhost/) plugin”.


That’s it, now go ahead and ask some questions, e.g. you can start with “Do I have any todos?”


There you are, now go ahead and build your own plugin as it says on your todo list ;)


## Deploying to Fly.io#


Once you’re happy with the functionality of your plugin, go ahead and deploy it to[Fly.io](http://fly.io/) . After installing the[flyctl cli](https://fly.io/docs/hands-on/install-flyctl/) , it only takes a couple of steps:


- Change` http://localhost:8000` to your Fly domain in the` /main/ai-plugins.json` file
- Open` fly.toml` and update the app name and optionally the region etc.
- In your terminal, run` fly apps create` and specify the app name you just set in your` fly.toml` file.
- Finally, run` fly deploy` .


There you go, now you’re ready to[release your plugin to the world](https://platform.openai.com/docs/plugins/production/faq) \\o/


## Conclusion#


ChatGPT is a powerful new interface and its usage is growing rapidly. With ChatGPT Plugins you can allow your users to access your service directly from ChatGPT, using cutting edge technologies like TypeScript and Deno.


In a next step you can add authentication to your plugin, let us know on[Twitter](https://twitter.com/Supabase) if you’d be interested in a tutorial for that. We can’t wait to see what you will build!


## More AI resources#


- [Hugging Face is now supported in Supabase](https://supabase.com/blog/hugging-face-supabase)
- [pgvector v0.5.0: Faster semantic search with HNSW indexes](https://supabase.com/blog/pgvector-v0-5-0-hnsw)
- [OpenAI ChatGPT Plugin docs](https://platform.openai.com/docs/plugins/introduction)
- [Docs pgvector: Embeddings and vector similarity](https://supabase.com/docs/guides/database/extensions/pgvector)
