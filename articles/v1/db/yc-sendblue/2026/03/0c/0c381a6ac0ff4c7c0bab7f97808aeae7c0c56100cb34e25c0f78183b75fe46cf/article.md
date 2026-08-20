---
schema_version: "1.0.0"
document_id: "0c381a6ac0ff4c7c0bab7f97808aeae7c0c56100cb34e25c0f78183b75fe46cf"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-java"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:404cdfd84be3321f59581b421baa9674a591e347460bafb723b8d29b0bc27ae5"
---

# How to Send iMessages from Java / Spring Boot

RestTemplate is the traditional way to make HTTP calls in Spring Boot. Here's a complete service class:


` // src/main/java/com/example/service/SendblueService.java package com.example.service; import org.springframework.beans.factory.annotation.Value; import org.springframework.http.*; import org.springframework.stereotype.Service; import org.springframework.web.client.RestTemplate; import java.util.*; @Service public class SendblueService { @Value("${sendblue.api.key}") private String apiKey; @Value("${sendblue.api.secret}") private String apiSecret; @Value("${sendblue.api.base-url}") private String baseUrl; private final RestTemplate restTemplate = new RestTemplate(); public Map<String, Object> sendMessage( String number, String content, String sendStyle, String mediaUrl) { HttpHeaders headers = new HttpHeaders(); headers.setContentType(MediaType.APPLICATION_JSON); headers.set("sb-api-key-id", apiKey); headers.set("sb-api-secret-key", apiSecret); Map<String, String> body = new HashMap<>(); body.put("number", number); body.put("content", content); if (sendStyle != null) body.put("send_style", sendStyle); if (mediaUrl != null) body.put("media_url", mediaUrl); HttpEntity<Map<String, String>> request = new HttpEntity<>(body, headers); ResponseEntity<Map> response = restTemplate.postForEntity( baseUrl + "/api/send-message", request, Map.class); return response.getBody(); } }`


Use the service in a controller or anywhere Spring can inject it:


` @Autowired private SendblueService sendblue; // Send a message sendblue.sendMessage("+15551234567", "Hello from Java!", "celebration", null);`
