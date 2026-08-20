---
schema_version: "1.0.0"
document_id: "0b95e7849b50bae05e2639afe20231dfb8785c79560f0836e80700debc1dbef7"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/processing-large-jobs-with-edge-functions"
published_at: "2025-09-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:02aa588be728b193792bd4325bc596e8d03a42ece5b91320b001b402c5c2a7a8"
---

# Processing large jobs with Edge Functions, Cron, and Queues

When you're building applications that process large amounts of data, you quickly run into a fundamental problem: trying to do everything at once leads to timeouts, crashes, and frustrated users. The solution isn't to buy bigger servers. It's to break big jobs into small, manageable pieces.


Supabase gives you three tools that work beautifully together for this:[Edge Functions](https://supabase.com/edge-functions) for serverless compute,[Cron](https://supabase.com/modules/cron) for scheduling, and database[queues](https://supabase.com/modules/queues) for reliable job processing.


Here's how to use them to build a system that can handle serious scale.


## The three-layer pattern#


The architecture is simple but powerful. Think of it like an assembly line:


**Collection** : Cron jobs run Edge Functions that discover work and add tasks to queues


**Distribution** : Other cron jobs route tasks from main queues to specialized processing queues


**Processing** : Specialized workers handle specific types of tasks from their assigned queues


This breaks apart the complexity. Instead of one giant function that scrapes websites, processes content with AI, and stores everything, you have focused functions that each do one thing well.


## Real example: Building an NFL news aggregator#


Let's say you want to build a dashboard that tracks NFL (American football) news from multiple sources including NFL-related websites and NFL-related videos on YouTube, automatically tags articles by topic, and lets users search by player or team. When they see an article they’re interested in, they can click on it and visit the website that hosts the article. It’s like a dedicated Twitter feed for the NFL without any of the toxicity.


This sounds straightforward, but at scale this becomes complex fast. You need to monitor dozens of news sites, process hundreds of articles daily, make API calls to OpenAI for content analysis, generate vector embeddings for search, and store everything efficiently. Do this wrong and a single broken webpage crashes your entire pipeline.


We need to build a more resilient approach. With Supabase Edge Functions, Cron, and Queues, we have the building blocks for a robust content extraction and categorization pipeline.


## Setting up the foundation#


Everything starts with the[database](https://supabase.com/database) , and Supabase is Postgres at its core. We know what we’re getting: scalable, dependable, and standard.


The database design for the application follows a clean pattern. You have content tables for storing articles and videos, queue tables for managing work, entity tables for NFL players and teams, and relationship tables linking everything together. For example:


`
_10


create table articles (


_10


url text unique not null,


_10


headline text,


_10


content text,


_10


embedding vector(1536)


_10


);


`


## Collection: Finding new content#


The collection layer seeks out new NFL-related content and runs on a schedule to discover new articles and videos. We create a collector for every site we want to search. A cron job triggers every 30 minutes to begin collection:


`
_10


SELECT cron.schedule(


_10


'nfl-collector',


_10


'*/30 * * * *',


_10


$$SELECT net.http_post(url := '\[https://your-project.supabase.co/functions/v1/collect-content')$$\](https://your-project.supabase.co/functions/v1/collect-content')$$)


_10


);


`


The Edge Function does the actual scraping. The trick is being selective about what you collect:


`
_10


function isRelevantArticle(url: string): boolean {


_10


return url.includes('/news/') && !url.includes('/video/')


_10


}


`


This simple filter prevents collecting promotional content or videos. You only want actual news articles.


When parsing HTML, you need to handle relative URLs properly:


`
_10


if (href.startsWith('/')) {


_10


href = BASE_URL + href


_10


}


`


And always deduplicate within a single scraping session:


`
_10


const seen = new Set<string>()


_10


if (!seen.has(href)) {


_10


seen.add(href)


_10


articles.push({ url: href, site: 'nfl' })


_10


}


`


For database insertion, let the database handle duplicates rather than checking in your application:


`
_10


const { error } = await supabase.from('articles').insert({ url, site })


_10


if (error && !error.message.includes('duplicate')) {


_10


console.error(\`Error inserting: ${url}\`, error)


_10


}


`


This approach is more reliable than complex application-level deduplication logic.


## Distribution: Smart routing#


The distribution layer identifies articles that need processing and routes them to appropriate queues. The key insight is using separate queue tables for different content sources.[NFL.com](http://nfl.com/) articles need different parsing than ESPN articles, so they get routed to specialized processors. It runs more frequently than collection: every 5 minutes:


`
_10


SELECT cron.schedule(


_10


'distributor',


_10


'*/5 * * * *',


_10


$$SELECT net.http_post(url := '\[https://your-project.supabase.co/functions/v1/distribute-work')$$\](https://your-project.supabase.co/functions/v1/distribute-work')$$)


_10


);


`


The Edge Function finds unprocessed articles using a simple SQL query:


`
_10


const { data } = await supabase


_10


.from('articles')


_10


.select('url, site')


_10


.is('headline', null) // Missing headline means unprocessed


_10


.limit(50)


`


Then it routes based on the source site:


`
_10


if (\[article.site\](http://article.site) === "nfl") {


_10


await supabase.from("nfl_queue").insert({ url: article.url });


_10


} else if (\[article.site\](http://article.site) === "espn") {


_10


await supabase.from("espn_queue").insert({ url: article.url });


_10


}


`


This separation is crucial because each site has different HTML structures and parsing requirements.


## Processing: The heavy lifting#


Each content source gets its own processor that runs on its own schedule.[NFL.com](http://nfl.com/) gets processed every 15 seconds because it's high-priority:


`
_10


SELECT cron.schedule(


_10


'nfl-processor',


_10


'*/15 * * * *',


_10


$$SELECT net.http_post(url := '\[https://your-project.supabase.co/functions/v1/process-nfl')$$\](https://your-project.supabase.co/functions/v1/process-nfl')$$)


_10


);


`


The processor handles one article at a time to stay within Edge Function timeout limits:


`
_10


const { data } = await supabase


_10


.from('nfl_queue')


_10


.select('id, url')


_10


.eq('processed', false)


_10


.order('created_at')


_10


.limit(1)


`


Content extraction requires site-specific CSS selectors:


`
_10


const headline = $('h1').first().text().trim()


_10


const content = $('.article-body').text().trim() || $('article').text().trim()


`


Date parsing often needs custom logic for each site's format:


`
_10


const dateText = $('.publish-date').text()


_10


const match = dateText.match(/(\\w+ \\d+, \\d{4})/)


_10


if (match) {


_10


publication_date = new Date(match\[1\])


_10


}


`


After scraping, the article gets analyzed with AI to extract entities:


`
_10


const result = await classifyArticle(headline, content)


_10


const playerIds = await upsertPlayers(supabase, result.players)


_10


const teamIds = await upsertTeams(supabase, result.teams)


`


Finally, create the relationships and generate embeddings:


`
_10


await supabase.from("article_players").insert(


_10


\[playerIds.map\](http://playerIds.map)(id => ({ article_url: url, player_id: id }))


_10


);


_10


_10


const embedding = await generateEmbedding(\`${headline}\\n${content}\`);


_10


await supabase.from("articles").update({ embedding }).eq("url", url);


`


The critical pattern is the finally block. We use it to always mark queue items as processed, preventing infinite loops when articles fail to process:


`
_10


try {


_10


// Process article


_10


} finally {


_10


await supabase.from("nfl_queue")


_10


.update({ processed: true })


_10


.eq("id", \[item.id\](http://item.id));


_10


}


`


### Monitoring with Sentry#


While the finally block prevents infinite loops, you still need visibility into what's actually failing. Sentry integration gives you detailed error tracking for your Edge Functions.


First, set up Sentry in your Edge Function:


`
_10


import { captureException, init } from 'https://deno.land/x/sentry/index.js'


_10


_10


init({


_10


dsn: Deno.env.get('SENTRY_DSN'),


_10


environment: Deno.env.get('ENVIRONMENT') || 'production',


_10


})


`


Then wrap your processing logic with proper error capture:


`
_22


try {


_22


const content = await scrapeArticle(url);


_22


const analysis = await classifyArticle(headline, content);


_22


await storeArticle(article, analysis);


_22


} catch (error) {


_22


*// Capture the full context for debugging*


_22


captureException(error, {


_22


tags: {


_22


function: "nfl-processor",


_22


site: article.site


_22


},


_22


extra: {


_22


url: article.url,


_22


queueId: queueItem.id


_22


}


_22


});


_22


console.error(\`Failed to process ${url}:\`, error);


_22


} finally {


_22


await supabase.from("nfl_queue")


_22


.update({ processed: true })


_22


.eq("id", queueItem.id);


_22


}


`


This gives you real-time alerts when processors fail and detailed context for debugging production issues.


## Processing user interactions through the pipeline#


The same pipeline pattern works for user-generated events. When someone clicks, shares, or saves an article, you don't want to block their response while updating trending scores for every player and team mentioned in that article.


Instead, treat interactions like any other job to be processed:


`
_10


// Just record the interaction quickly


_10


await supabase.from('interaction_queue').insert({


_10


article_url: url,


_10


user_id: userId,


_10


interaction_type: 'share',


_10


})


`


Then let a separate cron job process the trending updates in batches:


`
_10


SELECT cron.schedule(


_10


'process-interactions',


_10


'*/2 * * * *', -- Every 2 minutes


_10


$$SELECT net.http_post(url := '\[https://your-project.supabase.co/functions/v1/process-interactions')$$\](https://your-project.supabase.co/functions/v1/process-interactions')$$)


_10


);


`


The processor can handle multiple interactions efficiently:


`
_10


const { data: interactions } = await supabase


_10


.from('interaction_queue')


_10


.select('*')


_10


.eq('processed', false)


_10


.limit(100)


`


This keeps your user interface snappy while ensuring trending scores get updated reliably. If the trending processor goes down, interactions are safely queued and will be processed when it recovers.


## AI-powered content scoring#


To surface the most important content automatically, use AI to analyze article context and assign importance scores.


Define scores for different news types:


`
_10


const CONTEXT_SCORES = {


_10


championship: 9,


_10


trade: 6,


_10


injury: 4,


_10


practice: 1,


_10


}


`


Prompt OpenAI with structured output:


`
_10


const prompt = \`Analyze this headline: "${headline}"


_10


Return JSON: {"context": "trade|injury|etc", "score": 1-9}\`;


_10


_10


const result = await \[openai.chat\](http://openai.chat).completions.create({


_10


model: "gpt-3.5-turbo",


_10


response_format: { type: "json_object" }


_10


});


`


Process articles in batches to manage API costs:


`
_10


const unprocessed = articles.filter((a) => !processedUrls.has(a.url)).slice(0, 10)


_10


_10


for (const article of unprocessed) {


_10


const analysis = await analyzeArticle(article)


_10


await storeAnalysis(article.url, analysis)


_10


}


`


## Background tasks for expensive operations#


Some operations are too expensive to run synchronously, even in your cron-triggered processors. Vector embedding generation and bulk AI analysis benefit from background task patterns.


Edge Functions support background tasks that continue processing after the main response completes:


typescript


`
_20


*// In your article processor*


_20


const article = await scrapeAndStore(url);


_20


_20


*// Start expensive operations in background*


_20


const backgroundTasks = \[


_20


generateEmbedding(article),


_20


analyzeWithAI(article),


_20


updateRelatedContent(article)


_20


\];


_20


_20


*// Run background tasks without blocking the main flow*


_20


Promise.all(backgroundTasks).catch(error => {


_20


captureException(error, {


_20


tags: { operation: "background-tasks" },


_20


extra: { articleUrl: url }


_20


});


_20


});


_20


_20


*// Main processing continues immediately*


_20


await markAsProcessed(queueItem.id);


`


For operations that might take longer than Edge Function limits, break them into smaller background chunks:


typescript


`
_15


async function generateEmbeddingInBackground(article: Article) {


_15


*// Process content in chunks*


_15


const chunks = splitIntoChunks(article.content, 1000);


_15


_15


for (const chunk of chunks) {


_15


await new Promise(resolve => {


_15


*// Use background task for each chunk*


_15


setTimeout(async () => {


_15


const embedding = await generateEmbedding(chunk);


_15


await storeEmbedding(article.id, embedding);


_15


resolve(void 0);


_15


}, 0);


_15


});


_15


}


_15


}


`


This pattern keeps your main processing pipeline fast while ensuring expensive operations complete reliably.


## Why this works#


This pattern succeeds because it embraces the constraints of serverless computing rather than fighting them. Edge Functions have time limits, so you process one item at a time. External APIs have rate limits, so you control timing with cron schedules. Failures happen, so you isolate them to individual tasks.


The result is a system that scales horizontally by adding more cron jobs and queues. Each component can fail independently without bringing down the whole pipeline. Users get fresh content as it becomes available rather than waiting for batch jobs to complete.


Most importantly, it's built entirely with Supabase primitives — no external queue systems or job schedulers required. You get enterprise-grade reliability with startup simplicity.
