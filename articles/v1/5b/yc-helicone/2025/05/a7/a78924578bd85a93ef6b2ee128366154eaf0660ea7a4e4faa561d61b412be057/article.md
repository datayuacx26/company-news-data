---
schema_version: "1.0.0"
document_id: "a78924578bd85a93ef6b2ee128366154eaf0660ea7a4e4faa561d61b412be057"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/ai-agent-monitoring-tutorial"
published_at: "2025-05-02T00:00:00+00:00"
first_seen_at: "2026-07-24T11:13:05.720262+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:87f5ac76e7baff3c2c35bbfebb852a693d913938ade1f6cb7507b6edd7c89f45"
---

# Building and Monitoring AI Agents: A Step-by-Step Guide (Part 1)

# Building and Monitoring AI Agents: A Step-by-Step Guide (Part 1)


May 2, 2025


· 10 minute read


Yusuf Ishola


· May 2, 2025


**Time to complete: ~30 minutes**


Your AI agent worked perfectly in testing, but now in production it's making bizarre recommendations and you have no idea why. Sound familiar? As AI agents grow increasingly complex, the black box problem is becoming the number one obstacle to reliable deployment.


In this first part of our **two-part series** on AI agent observability, we'll build a financial research assistant that demonstrates the key components of a modern AI agent. In part two, we'll explore how to effectively monitor it with Helicone's agentic AI observability features.


Let's get started!


## Table of Contents


- Prerequisites
- Quick Start
- How We'll Build Our Financial Assistant
- Key Components of Our AI Agent
- Testing Our Financial Assistant
- Debugging Our Financial Assistant


## Prerequisites


Before we dive in, you'll need:


- **[Node.js 16+](https://nodejs.org/)** installed on your machine
- **[OpenAI API](https://platform.openai.com/api-keys)** key
- **[Alpha Vantage API key](https://www.alphavantage.co/support/#api-key)** (free tier available)
- **[Helicone API key](https://helicone.ai/signup)** (free tier available)


## Quick Start


Want to skip ahead and try the code immediately? Clone the GitHub repository and run the code:


```text
git  clone   https://github.com/Yusu-f/helicone-agent-tutorial.git
cd   helicone-agent-tutorial
npm install


```


Create a` .env` file with your API keys


```text
OPENAI_API_KEY=your_openai_key_here
ALPHA_VANTAGE_API_KEY=your_alpha_vantage_key_here
HELICONE_API_KEY=your_helicone_key_here


```


Run the assistant


```text
npm start


```


This gives you the version of the financial assistant with basic Helicone monitoring.


In part 2, we'll show you how to add comprehensive monitoring to your AI agent with Helicone's Sessions feature.


## How We'll Build Our Financial Assistant


Our financial assistant does two things:


1. Fetches real-time price information and news for specific tickers
2. Uses RAG to answer questions about company information


The agent intelligently determines which approach to take for each query—a pattern applicable to many domains beyond finance, including customer support, healthcare, and legal applications.


## Key Components of Our AI Agent


### 1. Tools


Our agent uses OpenAI's function calling tools system to determine how to handle different queries:


```text
// Define OpenAI tools for function calling
const   tools = [
{
type  :  "function"  ,
function  : {
name  :  "getStockData"  ,
description  :  "Get current price and other information for a specific stock by ticker symbol"  ,
parameters  : {
type  :  "object"  ,
properties  : {
ticker  : {
type  :  "string"  ,
description  :  "The stock ticker symbol, e.g., AAPL for Apple Inc."
}
},
required  : [ "ticker"  ]
}
}
},
...
]


```


This approach allows the model to decide which functions to call based on the user's query.


### 2. Basic Helicone Monitoring


The financial assistant uses Helicone's basic monitoring to track the cost, latency, and error rate of our LLM calls. You can create an account for free[here](https://www.helicone.ai/signup) .


```text
const   openai =  new    OpenAI  ({
apiKey  : process. env  . OPENAI_API_KEY  ,
baseURL  :  "https://oai.helicone.ai/v1"  ,
defaultHeaders  : {
"Helicone-Auth"  :  `Bearer  ${process.env.HELICONE_API_KEY}  `  ,
},
});


```


### 3. RAG & External API access


For company information queries, we use a vector store to retrieve relevant information, while for stock queries, such as real-time price information or news, we connect to the Alpha Vantage API:


```text
async    function    searchCompanyInfo  ( query, vectorStore  ) {
console  . log  ( "Searching for company information in knowledge base..."  );


// Get relevant documents from vector store with similarity scores
const   resultsWithScores =  await   vectorStore. similaritySearchWithScore  (query,  2  );


// Check if we have relevant results with good similarity scores
if   (resultsWithScores. length   ===  0   || resultsWithScores[ 0  ][ 1  ] <  0.9  ) {
console  . log  ( "No relevant company information found in the knowledge base."  );
return   {
found  :  false  ,
message  :  "No relevant company information found in the knowledge base."
};
}


// Extract just the documents from the results for context
const   relevantDocs = resultsWithScores. map  ( ( [doc]  ) =>   doc);


return   {
found  :  true  ,
documents  : relevantDocs
};
}


```


```text
async    function    getStockData  ( ticker  ) {
try   {
const   url =  `https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol= ${ticker}  &apikey= ${ALPHA_VANTAGE_API_KEY}  `  ;
const   response =  await   axios. get  (url);


...
}
}


```


The RAG implementation provides domain-specific knowledge to the agent. However, as we'll see later, **without proper monitoring** , detecting what's causing the system to fail when it does might be difficult.


### 4. Agent Loop (Tool Calling)


We expose three tools to the LLM:


- ` getStockData` : Retrieves current price and market information for a specific ticker
- ` getStockNews` : Fetches the latest news articles related to a stock ticker
- ` searchCompanyInfo` : Queries our vector database for information about company profiles


The loop allows our agent to call tools and process results for as long as needed to generate an appropriate response:


```text
async    function    processQuery  ( userQuery, vectorStore  ) {
let   messages = [
{
role  :  "system"  ,
content  :  `You're a financial assistant. Use tools when needed. If you have enough information to answer, reply normally.`
},
{  role  :  "user"  ,  content  : userQuery }
];


// Add chat history for context if available
if   (chatHistory. length   >  0  ) {
messages. splice  ( 1  ,  0  , ...chatHistory);
}


while   ( true  ) {
console  . log  ( "Sending query to OpenAI..."  );
const   llmResp =  await   openai. chat  . completions  . create  ({
model  :  "gpt-3.5-turbo"  ,
tools,
messages,
temperature  :  0.1  ,
});


const   msg = llmResp. choices  [ 0  ]. message  ;


if   (msg. tool_calls   && msg. tool_calls  . length   >  0  ) {
// Execute the helper and push message into history...


continue  ;
}


// No tool call → LLM has produced the final answer
return   msg. content  ;
}
}


```


## Testing Our Financial Assistant


Now, let's take our financial assistant for a spin!


Run the following command to start the assistant:


```text
npm start


```


We can view the results of our queries in the **Helicone dashboard** .


> Prompt: What is tesla's stock price?


Result:


> Prompt: What is the latest news on tesla?


Result:


```text
What would you like to know? What is the latest news on Tesla?


Researching your question...
Sending query to OpenAI...
Executing getStockNews with args: { ticker: 'TSLA' }
Sending query to OpenAI...


Answer: Here are some of the latest news articles related to Tesla:


...


```


As you can see, the first two queries work as expected—our agent successfully retrieves Tesla's stock price and latest news using the Alpha Vantage API.


However, when we try other prompts, problems emerge:


> Prompt: Who is Elon Musk?


Result:


```text
What would you like to know? Who is Elon Musk?


Researching your question...
Sending query to OpenAI...
Executing searchCompanyInfo with args: { query: 'Elon Musk' }
Searching for company information in knowledge base...
No relevant company information found in the knowledge base.
Sending query to OpenAI...


Answer: Elon Musk is a well-known entrepreneur and business magnate. He is the founder, CEO, and lead designer of SpaceX; early investor, CEO, and product architect of Tesla, Inc.; founder of The Boring Company; co-founder of Neuralink; and co-founder and initial co-chairman of OpenAI. Musk is known for his ambitious vision for the future, including the colonization of Mars and the development of sustainable energy solutions on Earth.


```


> Prompt: What is GreenEnergy's profit margin?


Result:


```text
What would you like to know? What is GreenEnergy's profit margin?


Researching your question...
Sending query to OpenAI...
Executing searchCompanyInfo with args: { query: 'GreenEnergy' }
Searching for company information in knowledge base...
No relevant company information found in the knowledge base.
Sending query to OpenAI...


Answer: I couldn't find specific information on a company named GreenEnergy in the knowledge base. If you have a different company name or if there's any other specific information you need, feel free to let me know!


```


Even though GreenEnergy's profit margin (14%) is clearly **in our knowledge base** , the agent fails to retrieve it.


## The Challenge of Production Debugging 💡


These issues might seem obvious since we're running locally with CLI access, but in a production environment with thousands of queries, spotting these patterns becomes nearly impossible without proper observability tools like Helicone.


## Debugging Our Financial Assistant


Looking at our implementation, there are several blind spots that could potentially cause issues:


- **Hallucinations and retrieval issues** : Our agent failed to answer the query related to GreenEnergy despite having the requisite information—how do we pinpoint the problem?
- **Inappropriate knowledge source selection** : The agent searched its internal knowledge base for information it already had.
- **Cost Visibility** : How many tokens is each component of our agent consuming? Which queries are most expensive?
- **Latency Issues** : If the agent becomes slow, which step is causing the bottleneck?
- **Error Patterns** : Are certain types of queries consistently failing? Where in the pipeline do these failures occur?


In[Part 2 of this tutorial](https://www.helicone.ai/blog/ai-agent-monitoring-tutorial-part-2) on AI agent optimization, we'll add Helicone to our financial assistant to gain comprehensive visibility into every step of the process.


Here's a preview of what you can see:


We'll monitor each step of the agent's workflow, resolve bugs, and gain insights into useful metrics like cost, latency, and error rates.


Stay tuned!


## Observe Your AI Agents with Helicone ⚡️


Stop building AI in the dark. Get complete visibility into every step of your AI workflows, track costs down to the penny, and debug complex issues in minutes instead of days.


### You might also like:


- **[Part 2: Step-by-Step Guide to Building and Optimizing AI Agents](https://www.helicone.ai/blog/ai-agent-monitoring-tutorial-part-2)**
- **[Debugging RAG Chatbots and AI Agents with Sessions](https://www.helicone.ai/blog/debugging-chatbots-and-ai-agents-with-sessions)**
- **[The Full Developer's Guide to Building Effective AI Agents](https://www.helicone.ai/blog/full-guide-to-improving-ai-agents)**
- **[Building Agentic RAG Systems: A Developer's Guide to Smarter Information Retrieval](https://www.helicone.ai/blog/agentic-rag-full-developer-guide)**


## Frequently Asked Questions


### Why do AI agents need specialized observability tools?


AI agents have unique monitoring challenges including non-deterministic execution paths, multi-step LLM calls, complex branching logic, and dependencies on external systems. Unlike traditional applications with fixed flows, agents' decision trees vary with each request. Standard monitoring tools can't track these dynamic workflows or evaluate response quality across interconnected steps, which is why specialized tools like Helicone's session-based tracing are essential for AI agent observability.


### What are the biggest blind spots when deploying AI agents to production?


The most dangerous blind spots include: RAG threshold issues causing information retrieval failures, inappropriate knowledge source selection leading to hallucinations, hidden cost escalations from inefficient prompts, silent failures in multi-step reasoning chains, inconsistent performance across different user segments, and degrading accuracy over time as data or usage patterns change. Without proper observability, these issues can persist for weeks before being discovered, potentially causing significant business impact.


### What metrics should I monitor for any AI agent?


Critical metrics for all AI agents include: end-to-end latency of complete workflows, token usage per step and total cost per request, step completion rates showing where agents get stuck, retrieval quality for RAG implementations, routing accuracy between different processing pathways, error rates for external API calls, and user satisfaction with responses. Tracking these metrics helps identify bottlenecks, optimize costs, and ensure reliable agent performance.


### How do I implement observability across different AI agent frameworks?


Helicone offers flexible integration options for all major AI frameworks. For LangChain, CrewAI, and LlamaIndex, direct integrations are available. For custom agents or other frameworks, you can typically use either Helicone's proxy approach (changing just the base URL) or the SDK integration. The Sessions feature works consistently across most major frameworks to trace multi-step agent workflows regardless of your technology choices, giving you a unified view of all AI operations.


---


### Questions or feedback?


Are the information out of date? Please[raise an issue](https://github.com/Helicone/helicone/issues) or[contact us](https://www.helicone.ai/contact) , we'd love to hear from you!
