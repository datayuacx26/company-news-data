---
schema_version: "1.0.0"
document_id: "b7a2a6ba4a15a61ed72bd4811bc6471b27e3df08a0097c53aa2192eb938455d2"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/ai-sales-agent-imessage"
published_at: "2026-04-05T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:89cc9cd0e7005e3b419e8e4e5cf7159c040dfa8c7068bd4feea19f58df818d46"
---

# Build an AI Sales Agent That Texts via iMessage (2026 Guide)

When a prospect replies, Sendblue POSTs to your webhook. The agent generates a contextual response and checks whether to escalate:


` from fastapi import FastAPI from pydantic import BaseModel from typing import Optional import sqlite3 app = FastAPI() # Simple SQLite store for conversation history # In production, use PostgreSQL + pgvector or Redis def get_history(phone: str) -> list\[dict\]: conn = sqlite3.connect("conversations.db") conn.execute( "CREATE TABLE IF NOT EXISTS messages " "(phone TEXT, role TEXT, content TEXT, ts DATETIME DEFAULT CURRENT_TIMESTAMP)" ) rows = conn.execute( "SELECT role, content FROM messages WHERE phone=? ORDER BY ts", (phone,) ).fetchall() conn.close() return \[{"role": r\[0\], "content": r\[1\]} for r in rows\] def save_message(phone: str, role: str, content: str): conn = sqlite3.connect("conversations.db") conn.execute( "INSERT INTO messages (phone, role, content) VALUES (?,?,?)", (phone, role, content) ) conn.commit() conn.close() ESCALATION_KEYWORDS = \[ "pricing", "contract", "legal", "urgent", "not interested", "stop", "remove", "unsubscribe", "lawsuit" \] def needs_escalation(text: str) -> bool: return any(kw in text.lower() for kw in ESCALATION_KEYWORDS) def alert_human_rep(phone: str, reply: str): """In production: post to Slack, update CRM, send email to rep.""" print(f"\[ESCALATION\] Human needed for {phone}: {reply}") class SendblueWebhook(BaseModel): isOutbound: bool status: str messageHandle: str fromNumber: str number: str content: Optional\[str\] = None mediaUrl: Optional\[str\] = None @app.post("/webhook/sendblue") async def handle_reply(payload: SendblueWebhook): if payload.isOutbound or not payload.content: return {"status": "ok"} phone = payload.fromNumber reply = payload.content save_message(phone, "user", reply) if needs_escalation(reply): alert_human_rep(phone, reply) bridge = "Thanks for your message — I'm looping in a colleague who can help you directly." send_imessage(phone, bridge) save_message(phone, "assistant", bridge) return {"status": "escalated"} history = get_history(phone) ai_response = claude.messages.create( model="claude-3-5-sonnet-20241022", max_tokens=200, system=( "You are an AI sales assistant following up via iMessage. " "Be helpful, concise, and human. " "Goal: qualify interest and book a 15-minute call. " "Max 2 sentences per reply. Never mention you are an AI unless directly asked." ), messages=history, ).content\[0\].text.strip() send_imessage(phone, ai_response) save_message(phone, "assistant", ai_response) return {"status": "ok"} # Run: uvicorn agent:app --reload --port 8000`
