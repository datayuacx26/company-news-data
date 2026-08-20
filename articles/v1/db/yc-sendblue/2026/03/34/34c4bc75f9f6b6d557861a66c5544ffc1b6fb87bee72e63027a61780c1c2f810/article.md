---
schema_version: "1.0.0"
document_id: "34c4bc75f9f6b6d557861a66c5544ffc1b6fb87bee72e63027a61780c1c2f810"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/ai-text-message-responder"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-24T01:08:51.497224+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:689d102213e01474adf2af673d3a24615a2ad5629e769dc65e748d1be76e93d5"
---

# AI Text Message Responder: Automate Customer Conversations with iMessage

Here is a complete working example using Node.js, Express, Sendblue, and Claude:


` import express from 'express'; import Sendblue from 'sendblue'; import Anthropic from '@anthropic-ai/sdk'; const app = express(); app.use(express.json()); const sendblue = new Sendblue( process.env.SENDBLUE_API_KEY, process.env.SENDBLUE_API_SECRET ); const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY, }); // Conversation history per phone number const conversations = new Map(); const SYSTEM_PROMPT = \`You are a helpful customer service agent for \[Your Company\]. You help with product questions, order status, appointment booking, and general inquiries. Keep responses concise (under 300 characters when possible) since this is a text conversation. If you cannot help with something, offer to connect them with a human agent. Be friendly, professional, and helpful.\`; app.post('/webhook/receive', async (req, res) => { // Respond immediately to acknowledge the webhook res.status(200).json({ received: true }); const { from_number, content } = req.body; if (!content) return; // Skip empty messages (e.g., media-only) // Get or initialize conversation history if (!conversations.has(from_number)) { conversations.set(from_number, \[\]); } const history = conversations.get(from_number); history.push({ role: 'user', content }); // Keep last 20 messages for context window management if (history.length > 20) history.splice(0, history.length - 20); try { // Show typing indicator while AI processes await sendblue.sendTypingIndicator({ number: from_number }); // Get AI response const response = await anthropic.messages.create({ model: 'claude-sonnet-4-20250514', max_tokens: 500, system: SYSTEM_PROMPT, messages: history, }); const reply = response.content\[0\].text; history.push({ role: 'assistant', content: reply }); // Send the response via iMessage await sendblue.sendMessage({ number: from_number, content: reply, }); } catch (error) { console.error('AI responder error:', error); await sendblue.sendMessage({ number: from_number, content: "I'm having trouble right now. Let me connect you with a team member. Someone will reach out shortly.", }); } }); app.listen(3000, () => console.log('AI responder running on port 3000'));`


For a more detailed walkthrough of this architecture, see our full tutorial on[building an AI agent with Claude and Sendblue](https://www.sendblue.com/blog/build-imessage-ai-agent-claude-sendblue) . For OpenAI/GPT integration, see our[ChatGPT + iMessage guide](https://www.sendblue.com/blog/connect-chatgpt-to-imessage) .
