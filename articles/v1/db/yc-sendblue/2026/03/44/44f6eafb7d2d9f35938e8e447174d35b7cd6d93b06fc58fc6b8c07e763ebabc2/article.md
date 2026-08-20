---
schema_version: "1.0.0"
document_id: "44f6eafb7d2d9f35938e8e447174d35b7cd6d93b06fc58fc6b8c07e763ebabc2"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-ruby"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:641f6c8ac0b030e58874912ac51b726cfccbd674f768bd259f36090b9cb3d0f1"
---

# How to Send iMessages from Ruby on Rails

Sendblue's REST API uses simple JSON POST requests. Here's how to set up a reusable Faraday client:


` # lib/sendblue_client.rb require 'faraday' require 'json' class SendblueClient BASE_URL = 'https://api.sendblue.co' def initialize @api_key = ENV\['SENDBLUE_API_KEY'\] @api_secret = ENV\['SENDBLUE_API_SECRET'\] @conn = Faraday.new(url: BASE_URL) do |f| f.request :json f.response :json f.adapter :net_http end end def send_message(number:, content:, send_style: nil, media_url: nil) body = { number: number, content: content } body\[:send_style\] = send_style if send_style body\[:media_url\] = media_url if media_url response = @conn.post('/api/send-message') do |req| req.headers\['sb-api-key-id'\] = @api_key req.headers\['sb-api-secret-key'\] = @api_secret req.headers\['Content-Type'\] = 'application/json' req.body = body end response.body end def evaluate_service(number) response = @conn.get('/api/evaluate-service') do |req| req.headers\['sb-api-key-id'\] = @api_key req.headers\['sb-api-secret-key'\] = @api_secret req.params\['number'\] = number end response.body end end`


This client wraps the two most common API calls. You can extend it to cover[all Sendblue endpoints](https://docs.sendblue.comm/) .
