---
schema_version: "1.0.0"
document_id: "7abbef6c2b10368eed60d09612d7e8d267b197c2b6f17dad599ba2c651c22136"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-react-native"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:907df0580be85024b93c07e48c1560fcf968ec152b7811fd04a5f7d3608737d4"
---

# Add iMessage to Your React Native App

In your React Native app, call your backend API — never Sendblue directly:


` // services/messaging.ts const API_BASE = 'https://your-backend.com'; export async function sendIMessage( number: string, content: string, mediaUrl?: string ) { const token = await getAuthToken(); // your auth implementation const response = await fetch(\`${API_BASE}/api/send-message\`, { method: 'POST', headers: { 'Content-Type': 'application/json', 'Authorization': \`Bearer ${token}\`, }, body: JSON.stringify({ number, content, mediaUrl }), }); if (!response.ok) { throw new Error(\`Send failed: ${response.status}\`); } return response.json(); }`


` // components/SendMessageScreen.tsx import React, { useState } from 'react'; import { View, TextInput, Button, Alert } from 'react-native'; import { sendIMessage } from '../services/messaging'; export function SendMessageScreen() { const \[number, setNumber\] = useState(''); const \[content, setContent\] = useState(''); const \[sending, setSending\] = useState(false); const handleSend = async () => { setSending(true); try { await sendIMessage(number, content); Alert.alert('Sent!', 'iMessage delivered successfully.'); setContent(''); } catch (err) { Alert.alert('Error', 'Failed to send message.'); } finally { setSending(false); } }; return ( <View style={{ padding: 20 }}> <TextInput placeholder="Phone number (+15551234567)" value={number} onChangeText={setNumber} keyboardType="phone-pad" style={{ borderBottomWidth: 1, marginBottom: 16, padding: 8 }} /> <TextInput placeholder="Message" value={content} onChangeText={setContent} multiline style={{ borderBottomWidth: 1, marginBottom: 16, padding: 8 }} /> <Button title={sending ? 'Sending...' : 'Send iMessage'} onPress={handleSend} disabled={sending || !number || !content} /> </View> ); }`
