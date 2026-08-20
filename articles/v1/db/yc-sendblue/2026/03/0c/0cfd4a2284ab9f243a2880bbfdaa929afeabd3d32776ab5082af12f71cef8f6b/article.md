---
schema_version: "1.0.0"
document_id: "0cfd4a2284ab9f243a2880bbfdaa929afeabd3d32776ab5082af12f71cef8f6b"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/build-imessage-ai-agent-claude-sendblue"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:49fb52f94b6950bcd5cc1af8018291100dd43edcbf4a736dacd5676acb55fd1e"
---

# How to Build an iMessage AI Agent with Claude + Sendblue

` import express from 'express'; import Sendblue from 'sendblue'; import Anthropic from '@anthropic-ai/sdk'; const app = express(); app.use(express.json()); const sendblue = new Sendblue( process.env.SENDBLUE_API_KEY, process.env.SENDBLUE_API_SECRET ); const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY, }); // Store conversation history per phone number const conversations = new Map(); const SYSTEM_PROMPT = \`You are a helpful business assistant for Acme Corp. You help customers with questions about products, pricing, and support. Keep responses concise — under 300 characters when possible. Be friendly and professional.\`; app.post('/webhook/receive', async (req, res) => { const { from_number, content } = req.body; // Get or create conversation history if (!conversations.has(from_number)) { conversations.set(from_number, \[\]); } const history = conversations.get(from_number); // Add user message to history history.push({ role: 'user', content }); // Keep last 20 messages for context if (history.length > 20) history.splice(0, history.length - 20); try { // Send typing indicator while processing await sendblue.sendTypingIndicator({ number: from_number }); // Get Claude's response const response = await anthropic.messages.create({ model: 'claude-sonnet-4-20250514', max_tokens: 500, system: SYSTEM_PROMPT, messages: history, }); const reply = response.content\[0\].text; // Add assistant response to history history.push({ role: 'assistant', content: reply }); // Send the response via iMessage await sendblue.sendMessage({ number: from_number, content: reply, }); } catch (error) { console.error('Error:', error); await sendblue.sendMessage({ number: from_number, content: "Sorry, I'm having trouble right now. A human will follow up shortly.", }); } res.status(200).json({ status: 'ok' }); }); app.listen(3000);`
