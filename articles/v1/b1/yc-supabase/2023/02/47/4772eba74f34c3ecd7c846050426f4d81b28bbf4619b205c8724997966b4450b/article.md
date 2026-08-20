---
schema_version: "1.0.0"
document_id: "4772eba74f34c3ecd7c846050426f4d81b28bbf4619b205c8724997966b4450b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/openai-embeddings-postgres-vector"
published_at: "2023-02-06T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:0905f608f72ed4c62f13223d781a95b840703bc551fe8929161e1dd91aa68b58"
---

# Storing OpenAI embeddings in Postgres with pgvector

A new PostgreSQL extension is now available in Supabase:[pgvector](https://github.com/pgvector/pgvector) , an open-source vector similarity search.


The exponential progress of AI functionality over the past year has inspired many new real world applications. One specific challenge has been the ability to store and query *embeddings* at scale. In this post we'll explain what embeddings are, why we might want to use them, and how we can store and query them in PostgreSQL using` pgvector` .


🆕 Supabase has now released an open source toolkit for developing AI applications using Postgres and pgvector. Learn more in the[AI & Vectors docs](https://supabase.com/docs/guides/ai) .


## What are embeddings?#


Embeddings capture the “relatedness” of text, images, video, or other types of information. This relatedness is most commonly used for:


- **Search:** how similar is a search term to a body of text?
- **Recommendations:** how similar are two products?
- **Classifications:** how do we categorize a body of text?
- **Clustering:** how do we identify trends?


Let's explore an example of text embeddings. Say we have three phrases:


1. “The cat chases the mouse”
2. “The kitten hunts rodents”
3. “I like ham sandwiches”


Your job is to group phrases with similar meaning. If you are a human, this should be obvious. Phrases 1 and 2 are almost identical, while phrase 3 has a completely different meaning.


Although phrases 1 and 2 are similar, they share no common vocabulary (besides “the”). Yet their meanings are nearly identical. How can we teach a computer that these are the same?


## Human language#


Humans use words and symbols to communicate language. But words in isolation are mostly meaningless - we need to draw from shared knowledge & experience in order to make sense of them. The phrase “You should Google it” only makes sense if you know that Google is a search engine and that people have been using it as a verb.


In the same way, we need to train a neural network model to understand human language. An effective model should be trained on millions of different examples to understand what each word, phrase, sentence, or paragraph could mean in different contexts.


So how does this relate to embeddings?


## How do embeddings work?#


Embeddings compress discrete information (words & symbols) into distributed continuous-valued data (vectors). If we took our phrases from before and plot them on a chart, it might look something like this:


Phrases 1 and 2 would be plotted close to each other, since their meanings are similar. We would expect phrase 3 to live somewhere far away since it isn't related. If we had a fourth phrase, “Sally ate Swiss cheese”, this might exist somewhere between phrase 3 (cheese can go on sandwiches) and phrase 1 (mice like Swiss cheese).


In this example we only have 2 dimensions: the X and Y axis. In reality, we would need many more dimensions to effectively capture the complexities of human language.


## OpenAI embeddings#


OpenAI offers an[API](https://platform.openai.com/docs/guides/embeddings) to generate embeddings for a string of text using its language model. You feed it any text information (blog articles, documentation, your company's knowledge base), and it will output a vector of floating point numbers that represents the “meaning” of that text.


Compared to our 2-dimensional example above, their latest embedding model` text-embedding-ada-002` will output 1536 dimensions.


Why is this useful? Once we have generated embeddings on multiple texts, it is trivial to calculate how similar they are using vector math operations like cosine distance. A perfect use case for this is search. Your process might look something like this:


1. Pre-process your knowledge base and generate embeddings for each page
2. Store your embeddings to be referenced later (more on this)
3. Build a search page that prompts your user for input
4. Take user's input, generate a one-time embedding, then perform a similarity search against your pre-processed embeddings.
5. Return the most similar pages to the user


## Embeddings in practice#


At a small scale, you could store your embeddings in a CSV file, load them into Python, and use a library like` numPy` to calculate similarity between them using something like cosine distance or dot product. OpenAI has a cookbook[example](https://github.com/openai/openai-cookbook/blob/main/examples/Semantic_text_search_using_embeddings.ipynb) that does just that. Unfortunately this likely won't scale well:


- What if I need to store and search over a large number of documents and embeddings (more than can fit in memory)?
- What if I want to create/update/delete embeddings dynamically?
- What if I'm not using Python?


### Using PostgreSQL#


Enter[pgvector](https://github.com/pgvector/pgvector) , an extension for PostgreSQL that allows you to both store and query vector embeddings within your database. Let's try it out.


First we'll enable the **Vector** extension. In Supabase, this can be done from the web portal through` Database` →` Extensions` . You can also do this in SQL by running:


`
_10


create extension vector;


`


Next let's create a table to store our documents and their embeddings:


`
_10


create table documents (


_10


id bigserial primary key,


_10


content text,


_10


embedding vector(1536)


_10


);


`


` pgvector` introduces a new data type called` vector` . In the code above, we create a column named` embedding` with the` vector` data type. The size of the vector defines how many dimensions the vector holds. OpenAI's` text-embedding-ada-002` model outputs 1536 dimensions, so we will use that for our vector size.


We also create a` text` column named` content` to store the original document text that produced this embedding. Depending on your use case, you might just store a reference (URL or foreign key) to a document here instead.


Soon we're going to need to perform a similarity search over these embeddings. Let's create a function to do that:


`
_21


create or replace function match_documents (


_21


query_embedding vector(1536),


_21


match_threshold float,


_21


match_count int


_21


)


_21


returns table (


_21


id bigint,


_21


content text,


_21


similarity float


_21


)


_21


language sql stable


_21


as $$


_21


select


_21


documents.id,


_21


documents.content,


_21


1 - (documents.embedding <=> query_embedding) as similarity


_21


from documents


_21


where documents.embedding <=> query_embedding < 1 - match_threshold


_21


order by documents.embedding <=> query_embedding


_21


limit match_count;


_21


$$;


`


` pgvector` introduces 3 new operators that can be used to calculate similarity:


Operator Description


` <->` Euclidean distance


` <#>` negative inner product


` <=>` cosine distance


OpenAI recommends cosine similarity on their embeddings, so we will use that here.


Now we can call` match_documents()` , pass in our embedding, similarity threshold, and match count, and we'll get a list of all documents that match. And since this is all managed by Postgres, our application code becomes very simple.


### Indexing#


Once your table starts to grow with embeddings, you will likely want to add an index to speed up queries. Vector indexes are particularly important when you're ordering results because vectors are not grouped by similarity, so finding the closest by sequential scan is a resource-intensive operation.


Each distance operator requires a different type of index. We expect to order by cosine distance, so we need` vector_cosine_ops` index. A good starting number of lists is 4 * sqrt(table_rows):


`
_10


create index on documents using ivfflat (embedding vector_cosine_ops)


_10


with


_10


(lists = 100);


`


You can read more about indexing on` pgvector` 's GitHub page[here](https://github.com/pgvector/pgvector#indexing) .


### Generating embeddings#


Let's use JavaScript to generate embeddings and store them in Postgres:


`
_29


import { createClient } from '@supabase/supabase-js'


_29


import { Configuration, OpenAIApi } from 'openai'


_29


import { supabaseClient } from './lib/supabase'


_29


_29


async function generateEmbeddings() {


_29


const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })


_29


const openAi = new OpenAIApi(configuration)


_29


_29


const documents = await getDocuments() // Your custom function to load docs


_29


_29


// Assuming each document is a string


_29


for (const document of documents) {


_29


// OpenAI recommends replacing newlines with spaces for best results


_29


const input = document.replace(/\\n/g, ' ')


_29


_29


const embeddingResponse = await openai.createEmbedding({


_29


model: 'text-embedding-ada-002',


_29


input,


_29


})


_29


_29


const \[{ embedding }\] = embeddingResponse.data.data


_29


_29


// In production we should handle possible errors


_29


await supabaseClient.from('documents').insert({


_29


content: document,


_29


embedding,


_29


})


_29


}


_29


}


`


### Building a simple search function#


Finally, let's create an[Edge Function](https://supabase.com/docs/guides/functions) to perform our similarity search:


`
_45


import { serve } from 'https://deno.land/std@0.170.0/http/server.ts'


_45


import 'https://deno.land/x/xhr@0.2.1/mod.ts'


_45


import { createClient } from 'jsr:@supabase/supabase-js@2'


_45


import { Configuration, OpenAIApi } from 'https://esm.sh/openai@3.1.0'


_45


import { supabaseClient } from './lib/supabase'


_45


_45


export const corsHeaders = {


_45


'Access-Control-Allow-Origin': '*',


_45


'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',


_45


}


_45


_45


serve(async (req) => {


_45


// Handle CORS


_45


if (req.method === 'OPTIONS') {


_45


return new Response('ok', { headers: corsHeaders })


_45


}


_45


_45


// Search query is passed in request payload


_45


const { query } = await req.json()


_45


_45


// OpenAI recommends replacing newlines with spaces for best results


_45


const input = query.replace(/\\n/g, ' ')


_45


_45


const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })


_45


const openai = new OpenAIApi(configuration)


_45


_45


// Generate a one-time embedding for the query itself


_45


const embeddingResponse = await openai.createEmbedding({


_45


model: 'text-embedding-ada-002',


_45


input,


_45


})


_45


_45


const \[{ embedding }\] = embeddingResponse.data.data


_45


_45


// In production we should handle possible errors


_45


const { data: documents } = await supabaseClient.rpc('match_documents', {


_45


query_embedding: embedding,


_45


match_threshold: 0.78, // Choose an appropriate threshold for your data


_45


match_count: 10, // Choose the number of matches


_45


})


_45


_45


return new Response(JSON.stringify(documents), {


_45


headers: { ...corsHeaders, 'Content-Type': 'application/json' },


_45


})


_45


})


`


### Building a smarter search function#


ChatGPT doesn't just return existing documents. It's able to assimilate a variety of information into a single, cohesive answer. To do this, we need to provide GPT with some relevant documents, and a prompt that it can use to formulate this answer.


One of the biggest challenges of OpenAI's` text-davinci-003`[completion model](https://beta.openai.com/docs/guides/completion) is the 4000 token limit. You must fit both your prompt and the resulting completion within the 4000 tokens. This makes it challenging if you wanted to prompt GPT-3 to answer questions about your own custom knowledge base that would never fit in a single prompt.


Embeddings can help solve this by splitting your prompts into a two-phased process:


1. Query your embedding database for the most relevant documents related to the question
2. Inject these documents as context for GPT-3 to reference in its answer


Here's another Edge Function that expands upon the simple example above:


`
_100


import { serve } from 'https://deno.land/std@0.170.0/http/server.ts'


_100


import 'https://deno.land/x/xhr@0.2.1/mod.ts'


_100


import { createClient } from 'jsr:@supabase/supabase-js@2'


_100


import GPT3Tokenizer from 'https://esm.sh/gpt3-tokenizer@1.1.5'


_100


import { Configuration, OpenAIApi } from 'https://esm.sh/openai@3.1.0'


_100


import { oneLine, stripIndent } from 'https://esm.sh/common-tags@1.8.2'


_100


import { supabaseClient } from './lib/supabase'


_100


_100


export const corsHeaders = {


_100


'Access-Control-Allow-Origin': '*',


_100


_100


}


_100


_100


serve(async (req) => {


_100


// Handle CORS


_100


if (req.method === 'OPTIONS') {


_100


return new Response('ok', { headers: corsHeaders })


_100


}


_100


_100


// Search query is passed in request payload


_100


const { query } = await req.json()


_100


_100


// OpenAI recommends replacing newlines with spaces for best results


_100


const input = query.replace(/\\n/g, ' ')


_100


_100


const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })


_100


const openai = new OpenAIApi(configuration)


_100


_100


// Generate a one-time embedding for the query itself


_100


const embeddingResponse = await openai.createEmbedding({


_100


model: 'text-embedding-ada-002',


_100


input,


_100


})


_100


_100


const \[{ embedding }\] = embeddingResponse.data.data


_100


_100


// Fetching whole documents for this simple example.


_100


//


_100


// Ideally for context injection, documents are chunked into


_100


// smaller sections at earlier pre-processing/embedding step.


_100


const { data: documents } = await supabaseClient.rpc('match_documents', {


_100


query_embedding: embedding,


_100


match_threshold: 0.78, // Choose an appropriate threshold for your data


_100


match_count: 10, // Choose the number of matches


_100


})


_100


_100


const tokenizer = new GPT3Tokenizer({ type: 'gpt3' })


_100


let tokenCount = 0


_100


let contextText = ''


_100


_100


// Concat matched documents


_100


for (let i = 0; i < documents.length; i++) {


_100


const document = documents\[i\]


_100


const content = document.content


_100


const encoded = tokenizer.encode(content)


_100


tokenCount += encoded.text.length


_100


_100


// Limit context to max 1500 tokens (configurable)


_100


if (tokenCount > 1500) {


_100


break


_100


}


_100


_100


contextText += \`${content.trim()}\\n---\\n\`


_100


}


_100


_100


const prompt = stripIndent\`${oneLine\`


_100


You are a very enthusiastic Supabase representative who loves


_100


to help people! Given the following sections from the Supabase


_100


documentation, answer the question using only that information,


_100


outputted in markdown format. If you are unsure and the answer


_100


is not explicitly written in the documentation, say


_100


"Sorry, I don't know how to help with that."\`}


_100


_100


Context sections:


_100


${contextText}


_100


_100


Question: """


_100


${query}


_100


"""


_100


_100


Answer as markdown (including related code snippets if available):


_100


\`


_100


_100


// In production we should handle possible errors


_100


const completionResponse = await openai.createCompletion({


_100


model: 'text-davinci-003',


_100


prompt,


_100


max_tokens: 512, // Choose the max allowed tokens in completion


_100


temperature: 0, // Set to 0 for deterministic results


_100


})


_100


_100


const {


_100


id,


_100


choices: \[{ text }\],


_100


} = completionResponse.data


_100


_100


return new Response(JSON.stringify({ id, text }), {


_100


headers: { ...corsHeaders, 'Content-Type': 'application/json' },


_100


})


_100


})


`


### Streaming results#


OpenAI API responses take longer to depending on the length of the “answer”. ChatGPT has a nice UX for this by streaming the response to the user immediately. You can see a similar effect for the Supabase docs:


The OpenAI API supports[completion streaming](https://platform.openai.com/docs/api-reference/completions/create#completions/create-stream) with Server Side Events. Supabase Edge Functions are run Deno, which also supports[Server Side Events](https://deno.com/blog/deploy-streams#server-sent-events) . Check out[this commit](https://github.com/supabase/supabase/pull/12056/commits/bd83e9ba2f7263440888228e3b29007604d94841) to see how we modified the Function above to build a streaming interface.


## Wrap up#


Storing embeddings in Postgres opens a world of possibilities. You can combine your search function with telemetry functions, add an user-provided feedback (thumbs up/down), and make your search feel more integrated with your products.


The[pgvector extension](https://supabase.com/docs/guides/ai/vector-columns) is available on all new Supabase projects today. To try it out, launch a new Postgres database:[database.new](https://database.new/)


## More pgvector and AI resources#


- [Supabase Clippy: ChatGPT for Supabase Docs](https://supabase.com/blog/chatgpt-supabase-docs)
- [Hugging Face is now supported in Supabase](https://supabase.com/blog/hugging-face-supabase)
- [How to build ChatGPT Plugin from scratch with Supabase Edge Runtime](https://supabase.com/blog/building-chatgpt-plugins-template)
- [Docs pgvector: Embeddings and vector similarity](https://supabase.com/docs/guides/database/extensions/pgvector)
- [Choosing Compute Add-on for AI workloads](https://supabase.com/docs/guides/ai/choosing-compute-addon)
- [pgvector v0.5.0: Faster semantic search with HNSW indexes](https://supabase.com/blog/increase-performance-pgvector-hnsw)
