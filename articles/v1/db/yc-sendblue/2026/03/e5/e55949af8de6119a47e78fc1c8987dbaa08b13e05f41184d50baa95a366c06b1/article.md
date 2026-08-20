---
schema_version: "1.0.0"
document_id: "e55949af8de6119a47e78fc1c8987dbaa08b13e05f41184d50baa95a366c06b1"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/connect-chatgpt-to-imessage"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:4db2e0c5a19252948ef7127877ea5e2aa54b418d543ae05128934fd5463e7ac5"
---

# Connect ChatGPT to iMessage: Complete Guide (2026)

` import express from 'express'; import Sendblue from 'sendblue'; import OpenAI from 'openai'; const app = express(); app.use(express.json()); const sendblue = new Sendblue( process.env.SENDBLUE_API_KEY, process.env.SENDBLUE_API_SECRET ); const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY }); const threads = new Map(); // conversation history per number app.post('/webhook', async (req, res) => { const { from_number, content } = req.body; if (!threads.has(from_number)) { threads.set(from_number, \[ { role: 'system', content: 'You are a helpful assistant responding via iMessage. Keep responses brief and conversational.' } \]); } const messages = threads.get(from_number); messages.push({ role: 'user', content }); // Show typing indicator await sendblue.sendTypingIndicator({ number: from_number }); const completion = await openai.chat.completions.create({ model: 'gpt-4o', messages, max_tokens: 500, }); const reply = completion.choices\[0\].message.content; messages.push({ role: 'assistant', content: reply }); await sendblue.sendMessage({ number: from_number, content: reply, }); res.json({ ok: true }); }); app.listen(3000);`
