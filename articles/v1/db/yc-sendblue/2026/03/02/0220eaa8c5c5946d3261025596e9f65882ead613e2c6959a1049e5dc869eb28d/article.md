---
schema_version: "1.0.0"
document_id: "0220eaa8c5c5946d3261025596e9f65882ead613e2c6959a1049e5dc869eb28d"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-go"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:01843db894bb35fb2f1a7233050ce1ba6a7001d80c87712c9bedd9c76ab55684"
---

# How to Send iMessages from Go (Golang)

Implement the send method and use it in a main function:


` func (c *Client) SendMessage(req SendMessageRequest) (*SendMessageResponse, error) { body, err := json.Marshal(req) if err != nil { return nil, fmt.Errorf("marshal request: %w", err) } httpReq, err := http.NewRequest( "POST", baseURL+"/api/send-message", bytes.NewReader(body), ) if err != nil { return nil, fmt.Errorf("create request: %w", err) } httpReq.Header.Set("Content-Type", "application/json") httpReq.Header.Set("sb-api-key-id", c.apiKey) httpReq.Header.Set("sb-api-secret-key", c.apiSecret) resp, err := c.http.Do(httpReq) if err != nil { return nil, fmt.Errorf("send request: %w", err) } defer resp.Body.Close() if resp.StatusCode != http.StatusOK { return nil, fmt.Errorf("API returned status %d", resp.StatusCode) } var result SendMessageResponse if err := json.NewDecoder(resp.Body).Decode(&result); err != nil { return nil, fmt.Errorf("decode response: %w", err) } return &result, nil } // Usage in main.go: func main() { client := sendblue.NewClient() result, err := client.SendMessage(sendblue.SendMessageRequest{ Number: "+15551234567", Content: "Hello from Go!", SendStyle: "celebration", }) if err != nil { log.Fatal(err) } fmt.Printf("Message sent: %s (ID: %s)\\n", result.Status, result.MessageID) }`


The` send_style` field is optional — it adds iMessage effects like` celebration` ,` fireworks` , and` confetti` . Sendblue automatically falls back to RCS or SMS if the recipient doesn't have iMessage.
