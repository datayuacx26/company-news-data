---
schema_version: "1.0.0"
document_id: "81be91605ad244ee6cca7e3e71592f9bc12cbf633ef8028f14d9c22e1a5cc23b"
company_key: "yc-integuru"
company: "Integuru"
source_id: "yc-integuru-news-import-ab81679661d6"
canonical_url: "https://www.integuru.com/blog/reverse-engineer-website-private-api"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-24T00:14:53.521545+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:6fa4588713a0779d36c1f9fb5ebcc5ec2b89d0e18f1bfcad2c83654c50c4283b"
---

# How to Reverse-Engineer a Website's Private API: A Developer's Guide

Every modern web application has two surfaces. There is the visual interface rendered in the browser, and there is the API layer the browser calls to fetch data and perform actions. The frontend you see is just a client. When you open a shipping portal, an EHR system, or a financial dashboard, the React or Vue application running in your tab is calling real HTTP endpoints behind the scenes, returning structured JSON, just like any API you have integrated before.


When a platform has no official public API, those internal endpoints are still there. They are just undocumented. Reverse-engineering a website's private API means finding them, understanding their auth requirements, and calling them directly, without a browser in the middle. This guide walks through the full process. Updated June 2026.


> **Before you start:** Check the platform's Terms of Service before reverse-engineering their endpoints. Some platforms explicitly prohibit automated access via unofficial API calls. Review the ToS and consult your legal team if the use case is commercial or involves regulated data.


### **Why Most Web Apps Have an Accessible Internal API**


Any web application built as a Single-Page Application (SPA) after roughly 2016 communicates with its backend through` XHR` or` Fetch` requests. React, Vue, Angular, and every major frontend framework works this way: the browser loads a JavaScript bundle, which then makes HTTP calls to fetch and write data. Those HTTP calls are real API requests with documented endpoints, structured request bodies, and predictable response schemas.


The distinction that matters here is that these endpoints are HTTP, not browser. They accept HTTP requests from any client, not just from the browser that the platform expects you to use. A` curl` command or an HTTP library in your language of choice can call the same endpoints, as long as it presents the correct headers and authentication tokens. The browser is not special from the server's perspective; it is just another HTTP client.


This is the foundation for the entire reverse-engineering process: the endpoint exists, it is reachable, and you already have a client that calls it correctly. That client is the browser running in front of you. Your job is to learn what it is doing.


### **Step 1: Capturing the Right Requests in Chrome DevTools**


Chrome DevTools' Network tab is where the work starts. Open DevTools with` F12` (or right-click anywhere on the page and select Inspect), then navigate to the **Network** tab. Before you trigger any action, filter by **Fetch/XHR** . This hides static asset requests (images, CSS, JavaScript bundles) and surfaces only the API calls, which is the data you care about.


The workflow for capturing a target request looks like this:


1.


Open DevTools and navigate to the Network tab


2.


Apply the **Fetch/XHR** filter


3.


Perform the specific action you want to automate, such as submitting a form, loading a record, or triggering a data export


4.


Watch the requests that fire immediately after your action


5.


Click into each request and examine the Headers, Payload, and Response tabs


You are looking for the request that carries the business logic. For a shipment system, it might be a` POST` to` /api/v2/shipments` . For a healthcare portal, a` GET` to` /api/patients/schedule` . The URL structure, HTTP method, and response shape tell you what each endpoint does.


Once you have identified the right request, right-click it in the Network panel and select **Copy as cURL** . This exports the full request, including headers and request body, as a ready-to-run terminal command. That export is your starting point for step three.


What to look for in each request:


-


**Request URL:** the base path and any query parameters


-


**HTTP method:**` GET` ,` POST` ,` PUT` ,` DELETE` ,` PATCH`


-


**Request headers:** especially` Authorization` ,` Cookie` ,` Content-Type` ,` X-CSRF-Token` , and any custom headers like` X-Client-Version`


-


**Request body:** the JSON payload or form data the frontend sends


-


**Response schema:** the structure of the data you will receive back


### **Step 2: Identifying the Auth Pattern**


Authentication is the detail most reverse-engineering guides gloss over. Getting it right is the difference between a` 200 OK` and a` 401 Unauthorized` . Modern web applications use a small set of auth patterns, and identifying which one a platform uses early saves considerable time.


The common patterns:


-


**Session cookie:** The browser stores a session token in a cookie set after login. Subsequent requests include that cookie automatically. In the Network tab, you will see a` Cookie` header on authenticated requests. Copy it and include it in your replayed requests.


-


**Bearer token:** After login, the platform returns a JWT or opaque token stored in` localStorage` or memory. Requests include it as` Authorization: Bearer <token>` . Look for this header in the request headers panel.


-


**API key in header:** Some platforms authenticate via a fixed header like` X-API-Key` or` X-App-Token` . These are typically long, static strings set once during account setup.


-


**OAuth 2.0 with token refresh:** The session token has a short expiry and a separate refresh token is used to obtain a new one. Look for requests to an` /oauth/token` or` /auth/refresh` endpoint firing periodically.


-


**2FA-gated sessions:** The session is not fully valid until a second factor is verified. The auth flow has an intermediate step, usually a` POST` to an endpoint like` /auth/verify` or` /auth/totp` , that upgrades the session.


For a long-running integration, the token you copy from the Network tab is valid only for that session. You will need to implement token refresh, which means capturing the login flow in addition to the action flow. The pattern is: perform the full login sequence in the Network tab, identify the request that returns a session token, replicate it, and store the token for use in subsequent calls.


> Bearer tokens and session cookies captured from DevTools are tied to the account you are logged in as. For integrations that need to act on behalf of multiple users, you will need to replicate the full auth flow per account.


### **Step 3: Replaying and Testing Requests**


With a captured` curl` command and the auth pattern identified, the next step is to replay the request outside the browser and confirm it works. This is the proof of concept: if the replayed request returns the same response the browser received, the endpoint is stable and callable without any browser involvement.


A` curl` command exported from Chrome DevTools looks roughly like this:


` curl 'https://platform.example.com/api/v2/shipments' \\ -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \\ -H 'Content-Type: application/json' \\ -H 'X-Client-Version: 4.2.1' \\ --data-raw '{"origin":"LAX","destination":"JFK","weight_kg":12.5}'`


Run that command in a terminal. A` 200 OK` with the expected JSON response means the endpoint works. At that point, you can import the same request into[Postman](https://www.postman.com/) or[Insomnia](https://insomnia.rest/) for easier iteration and collection management. Both tools accept` curl` imports directly.


What the response codes tell you:


-


**200 / 201:** Success. The endpoint is callable without a browser.


-


**401 Unauthorized:** The auth token is missing, expired, or malformed. Check that you copied the correct header value and that the token has not expired.


-


**403 Forbidden:** The account does not have permission, or the platform is verifying something beyond the token, such as request origin, IP allow-list, or CSRF validation.


-


**429 Too Many Requests:** Rate limiting is in place. Note the` Retry-After` header and the rate limit headers if present.


A` 403` is the most common complication and usually means the platform is doing additional verification. The four scenarios that make manual replay significantly harder:


-


**Request signing:** Headers include an` HMAC` or timestamp-based signature computed from the request body and a secret. The signature changes per request and must be freshly computed. Look for headers named` X-Signature` ,` X-Request-Timestamp` , or similar.


-


**Anti-CSRF tokens:** A token from the page's HTML or a prior` GET` request must be included in state-changing` POST` requests. Look for` X-CSRF-Token` or` _csrf` in request headers.


-


**Anti-bot headers:** Platforms running Akamai, Cloudflare, or similar anti-bot infrastructure may require tokens like` X-Recaptcha-Token` or challenge-response values that are generated client-side by JavaScript. These cannot be trivially replicated without running the JS.


-


**IP restrictions:** Some enterprise platforms allow requests only from known IP ranges. A` 403` that disappears when you test from a different network is a sign of this.


For the first two, the fix is capturing and replicating the signing or token-generation step as part of your integration. For the latter two, the workaround is more involved and typically requires a deeper look at how the platform's anti-bot JS generates those values.


### **What Makes Manual Reverse-Engineering Hard to Maintain at Scale**


The technique works. For a single integration built for internal use, capturing requests in DevTools, replaying them with` curl` , and scripting the auth flow is a completely valid approach. Teams do it routinely.


The problem is maintenance. A manual integration is accurate at the point in time you built it. Platforms change. When the team at the target platform refactors their API, updates their auth flow, migrates from session cookies to bearer tokens, or adds a new signing requirement, your integration breaks. At that point, the reverse-engineering process starts over: open DevTools again, re-capture the new request pattern, update the auth handling, re-test, re-deploy.


For one platform, this is manageable. For teams integrating with several platforms simultaneously, it becomes the job. Based on Integuru's data across production deployments, roughly **40% of integration breakages come from platform API and UI changes** . Auth changes account for a significant share of the remainder.


The other scaling challenge is coverage. A quick DevTools session captures the happy path. Production integrations encounter different account states, permission tiers, error conditions, and branching logic that only surface in real-world usage. Building a robust integration means testing and handling all of those paths, which requires considerably more than reproducing the one flow you observed.


-


**~40%** of integration breakages come from platform API and UI changes


-


Each new platform requires a full reverse-engineering cycle from scratch


-


Auth token rotation requires automated refresh logic, not a one-time copy


### **How Integuru Automates the Full Process**


Integuru is a Y Combinator-backed platform (founded 2024) that automates the reverse-engineering process end to end. You connect a platform by authenticating your account, and Integuru's agent analyzes the platform's network traffic, maps the API structure, handles the auth flow, and delivers production-ready endpoints within 10 to 20 minutes.


The output is a documented HTTP API for the target platform: structured endpoints, request and response schemas, and authentication handled automatically, including 2FA and session token refresh. The same technique described in this guide, applied systematically and kept current as the target platform evolves.


Where manual reverse-engineering stops after the initial build, Integuru continues:


-


**Edge case coverage:** The agent captures branching logic, error states, and variation across account types, not just the happy path you trigger in DevTools.


-


**Auth auto-healing:** On the Production plan, Integuru's system detects session expiry and re-authenticates automatically before requests fail. No manual token refresh required.


-


**24/7 on-call maintenance:** When a target platform updates its API structure or auth flow, Integuru's on-call team handles the repair. Your integration continues working.


-


**Full API documentation:** Generated endpoints ship with documentation, so downstream consumers know the request shape and expected response without reading source code.


Key metrics for Integuru-generated integrations:


-


**10-20 minutes** to generate production-ready endpoints per platform


-


**Under 3 seconds** average response time (no browser load cycle)


-


**99.9%+ reliability** across production deployments


-


**10M+ API calls per month** supported at the Production tier


> The Network tab shows you the API. The question is how much effort you want to spend maintaining it yourself, and whether a platform update should be your team's emergency or Integuru's.


For teams building AI agent tools, internal automation, or product integrations that depend on platforms with no official API, the manual technique is the right first step for validation. For production use across multiple platforms and accounts, Integuru handles the process that would otherwise run indefinitely on your engineering backlog.


For context on why direct HTTP integration outperforms browser-based alternatives once you have the endpoints, see[Browser Automation vs. Direct HTTP: A Reliability and Speed Comparison](https://www.integuru.com/blog/browser-automation-vs-direct-http) . For a head-to-head on the maintenance overhead of browser automation, see[Integuru vs. Puppeteer: Which Is Right for Production Integrations?](https://www.integuru.com/blog/integuru-vs-puppeteer) .


---


*Last verified: June 2026*


### **Get Started**


Integuru generates production-ready HTTP integrations for platforms that have no official API, in 10 to 20 minutes. The fastest way to start is the CLI:


` npm install -g integuru`


Or open the web app at[app.integuru.com](https://app.integuru.com/) . To talk through your specific platform and use case, book a call[here](https://calendly.com/d/cqb8-d9x-nbf/integuru) oremail us .
