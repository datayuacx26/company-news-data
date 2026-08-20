---
schema_version: "1.0.0"
document_id: "212f515ed2d178af9bf12dfc6fc31eb92e95de0d144ba67c3120a9a65b0df74f"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/ai-chatbot-imessage-integration"
published_at: "2026-04-05T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:b93da14b7767bbbaf163d2c8b06da575a8e4e81373576d4d25dd946616ee0100"
---

# AI Chatbot iMessage Integration: Connect Any LLM to Text Messaging

This is a complete, working implementation. You can copy this, set your environment variables, deploy it, and have a working AI chatbot on iMessage in under 10 minutes.


### 1. Install dependencies


` mkdir imessage-ai-chatbot && cd imessage-ai-chatbot npm init -y npm install express sendblue openai @anthropic-ai/sdk`


### 2. Set environment variables


` # .env SENDBLUE_API_KEY=your_sendblue_api_key SENDBLUE_API_SECRET=your_sendblue_api_secret OPENAI_API_KEY=your_openai_api_key ANTHROPIC_API_KEY=your_anthropic_api_key PORT=3000`


### 3. Complete server with OpenAI integration


` import express from 'express'; import Sendblue from 'sendblue'; import OpenAI from 'openai'; const app = express(); app.use(express.json()); // Initialize clients const sendblue = new Sendblue( process.env.SENDBLUE_API_KEY, process.env.SENDBLUE_API_SECRET ); const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY, }); // In-memory conversation store (use Redis or a DB in production) const conversations = new Map(); // System prompt for your AI chatbot const SYSTEM_PROMPT = \`You are a helpful assistant that communicates over iMessage. Keep responses concise (under 300 characters when possible) since this is a text conversation. Be friendly and natural. Do not use markdown formatting — plain text only.\`; // Webhook endpoint — Sendblue POSTs here when a message arrives app.post('/webhook/inbound', async (req, res) => { // Acknowledge immediately so Sendblue doesn't retry res.status(200).json({ status: 'received' }); const { content, number, media_url } = req.body; if (!content || !number) return; try { // Send typing indicator while LLM processes await sendblue.sendTypingIndicator({ number }); // Retrieve or initialize conversation history if (!conversations.has(number)) { conversations.set(number, \[\]); } const history = conversations.get(number); // Add the user's message history.push({ role: 'user', content }); // Keep last 20 messages to manage token usage if (history.length > 20) { history.splice(0, history.length - 20); } // Call OpenAI const completion = await openai.chat.completions.create({ model: 'gpt-4o', messages: \[ { role: 'system', content: SYSTEM_PROMPT }, ...history, \], max_tokens: 500, }); const reply = completion.choices\[0\].message.content; // Store assistant response in history history.push({ role: 'assistant', content: reply }); // Send reply via iMessage through Sendblue await sendblue.sendMessage({ number, content: reply, }); console.log(\`Replied to ${number}: ${reply.substring(0, 50)}...\`); } catch (error) { console.error('Error processing message:', error); // Send a fallback message so the user isn't left hanging await sendblue.sendMessage({ number, content: "Sorry, I hit a snag. Try again in a moment.", }); } }); // Health check app.get('/health', (req, res) => { res.json({ status: 'ok', uptime: process.uptime() }); }); app.listen(process.env.PORT || 3000, () => { console.log(\`AI chatbot server running on port ${process.env.PORT || 3000}\`); });`


### 4. Swap to Claude (Anthropic) — drop-in replacement


Want to use Claude instead of GPT-4? Replace the LLM call. Everything else stays the same:


` import Anthropic from '@anthropic-ai/sdk'; const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY, }); // Replace the OpenAI call with this: async function generateReply(history) { const response = await anthropic.messages.create({ model: 'claude-sonnet-4-20250514', max_tokens: 500, system: SYSTEM_PROMPT, messages: history, // Same format: { role, content } }); return response.content\[0\].text; }`


### 5. Configure your Sendblue webhook


In your[Sendblue dashboard](https://dashboard.sendblue.com/) , set your inbound message webhook URL to your server's public URL:


` https://your-server.com/webhook/inbound`


For local development, use[ngrok](https://ngrok.com/) to expose your local server:


` ngrok http 3000 # Then set the webhook URL to: https://abc123.ngrok.io/webhook/inbound`
