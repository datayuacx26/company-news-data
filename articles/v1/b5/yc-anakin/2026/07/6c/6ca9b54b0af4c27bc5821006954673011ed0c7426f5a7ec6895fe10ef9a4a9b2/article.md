---
schema_version: "1.0.0"
document_id: "6ca9b54b0af4c27bc5821006954673011ed0c7426f5a7ec6895fe10ef9a4a9b2"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/http-429-errors-scrapers-despite-delays"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-21T06:45:54.754455+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:1ffd55f6bcb3b5b3199c50079c5176f78ed102d7b46d13dedf1d1f50a7c785c9"
---

# Why Scrapers Still Hit HTTP 429 Errors Despite Delays

Adding delays between scraper requests often fails to prevent HTTP 429 rate-limit errors. Servers enforce limits by *identity signals* - browser fingerprints, session tokens, and concurrent request patterns - not just request timing.


## Key takeaways


- HTTP 429 errors persist despite delays because servers group requests by fingerprint (user-agent, TLS cipher, screen resolution) and session state - not just timestamps.
- Rate-limit algorithms like token buckets and sliding windows count requests *per identifier* (IP address, session token, fingerprint hash), making timing adjustments insufficient.
- Rotating proxies without rotating fingerprints exposes scrapers as automated - servers see multiple IPs sharing the same fingerprint hash, triggering persistent 429 responses.
- Session cookies and localStorage persist across proxy rotations unless explicitly cleared, causing servers to group requests under one identity.
- Infrastructure solutions that maintain consistent fingerprints within sessions and clear state between sessions eliminate 429 errors without manual management.


## Why HTTP 429 errors happen even with request delays


Adding delays between requests addresses *timing* but not *identity* . Servers enforce rate limits using a sliding window algorithm that counts requests within the last N seconds, but they group those requests by fingerprint - IP address, session token, TLS signature, or browser attributes - not just timestamp. When your scraper's identity signals cluster together, the server sees one persistent actor making many requests, even if those requests arrive seconds apart. Three hidden causes trigger this clustering: fingerprint inconsistency (each request exposes a slightly different User-Agent or header set, flagging the traffic as non-human), session-state leakage (cookies or tokens from one request bleed into the next, linking them), and concurrent request bursts (parallel threads share the same IP or credential, collapsing delays into simultaneous hits).


### What HTTP 429 Too Many Requests really means


[HTTP 429 Too Many Requests](https://zuplo.com/learning-center/http-429-too-many-requests-guide) is the server's signal that a client exceeded the allowed request rate within a specific time window. Websites implement rate limiting to protect server resources and prevent abuse from automated scrapers. The enforcement mechanism operates as a rate-limit bucket: token bucket systems allocate a fixed number of request tokens that refresh at regular intervals, while sliding window approaches count requests over rolling time periods rather than fixed minute boundaries. When your identifier - IP address, API key, or session token - exhausts its quota, the server blocks additional requests and returns a 429 response, often including a Retry-After header that specifies when you can try again.


### The delay fallacy: timing vs identity


Slowing your request rate assumes the server counts only timestamps. Production rate limiters count requests *per identifier* , not per second. If your scraper sends ten requests over sixty seconds but all ten share the same IP address and identical TLS fingerprint, the server groups them into a single bucket. The delay spreads the timeline but doesn't split the identity. Fingerprint-aware systems track User-Agent consistency, header order, cipher suite negotiations, and session continuity. When those signals remain static across requests, adding sleep calls between them changes nothing. The server still recognizes one persistent actor exceeding the per-identity threshold. This is why scrapers that rotate proxies but reuse the same session cookies, or vary User-Agent strings but keep identical Accept-Language headers, continue hitting 429 errors despite respecting timing intervals.[Anakin's infrastructure](https://anakin.io/blog/cloudflare-blocking-automation-system-fix) decouples timing from identity by rotating both IP addresses and browser fingerprints per request, ensuring each call appears as a distinct user rather than a throttled script.


[Get an API key](https://anakin.io/) to test zero-block proxy routing with automatic fingerprint diversification.


## How server-side rate limiting actually works


Client-side delays address only half the rate-limit problem. The server doesn't see pauses between requests - it sees arrival timestamps and identity clusters. Most production systems layer multiple enforcement buckets, each tracking a different dimension of identity.


### Sliding window vs token bucket algorithms


Rate-limit enforcement typically uses one of two models. A sliding window algorithm counts requests within the last N seconds and rejects once the threshold is reached. Anakin enforces limits this way: 60 requests per minute for standard submit endpoints and 10 per minute for AI evaluation endpoints, with GET polling endpoints exempt from rate limits. The window slides continuously, so a burst at second 58 and another at second 2 of the next minute still counts both inside the same 60-second span.


A token bucket model refills capacity at a fixed rate - five tokens per second, for example. A burst consumes tokens instantly; when the bucket is empty, requests block until replenishment. This model permits short bursts but enforces a sustained average rate. Choosing between the two changes retry behavior: sliding windows require spreading requests across the full period, while token buckets reward clustered bursts followed by pauses.


### Per-IP vs per-session vs per-fingerprint limits


Modern servers enforce rate limits across three independent identity layers:


1. **Per-IP bucket** - the coarsest signal. A distributed scraper moving through thousands of addresses rotates through this bucket without triggering it, but a single residential proxy shared by many users hits the limit quickly.
2. **Per-session bucket** - keyed by cookies or session tokens. Rotating proxies doesn't clear this counter if the session persists across IP changes. A scraper that discards cookies after each request never builds session history, bypassing this layer entirely.
3. **Per-fingerprint bucket** - TLS handshake patterns and HTTP/2 frame order group requests into recognizable cohorts even when IP and session rotate. A headless browser running Puppeteer shares a fingerprint signature with thousands of other Puppeteer instances, so one abusive actor can exhaust the bucket for the entire cohort.


Rotating one dimension - switching proxies, clearing cookies, or spoofing user-agent strings - doesn't reset the others. The server tracks all three in parallel, and whichever bucket fills first triggers the 429. This is why scrapers still hit rate limits despite delays: the identity clustering reveals the pattern before the request-rate threshold is reached.


## Three hidden causes of persistent 429 errors


Most scraping guides prescribe delays and proxy rotation as the universal 429 fix. When those measures fail, the root cause is not request volume - it is identity consistency signals that group requests by automation fingerprint even when IPs rotate.


### Browser fingerprint inconsistency across requests


Anti-bot systems probe browser artifacts to identify automation: user-agent strings, screen resolution, timezone, installed fonts, WebGL renderer configuration, and TLS cipher-suite order. When a scraper rotates proxies but sends the same fingerprint profile - or worse, sends inconsistent fingerprint attributes that no real browser would produce - the server groups those requests into a single automated identity cluster and applies rate limits at the cluster level. The[FP-Inconsistent study (ACM IMC 2025)](https://dl.acm.org/doi/10.1145/3730567.3732919) measured evasive bot traffic across 20 commercial bot services and found evasion rates of 44-53% against major anti-bot systems - fingerprint mismatches, not IP addresses, were the dominant detection signal.


### Session state leakage (cookies and localStorage persistence)


Rotating IPs does not clear localStorage or cookies unless the scraper explicitly manages session lifecycle. When a request carries a session token or tracking cookie from a previous IP's session, the server correlates the new IP with the old identity and applies cumulative rate limits. The[FP-Inconsistent study](https://arxiv.org/abs/2406.07647) also measured inconsistency across time - the same attribute observed at two different points - meaning session state leakage creates a time-series fingerprint that survives IP rotation.


### Concurrent request clustering and batch patterns


Servers detect automated batch patterns when request timing clusters together even with random jitter. Research on[bot detection prevalence across the Alexa Top 10K](https://arxiv.org/html/2606.14525v1) found that around a third of sites that block crawlers rely on browser fingerprinting - not simple per-IP thresholds. When multiple requests from one identity (fingerprint plus session cluster) arrive in a narrow time window, the pattern triggers batch-detection heuristics regardless of per-request delays. Reducing detection risk requires proxy routing paired with session isolation, not just spacing.


## Why browser fingerprint inconsistency triggers rate limits


### How servers fingerprint requests


Servers construct a session identifier by hashing request characteristics - user-agent string, TLS cipher suite, screen resolution, timezone, installed fonts, and WebGL renderer - into a persistent fingerprint that survives IP changes. Even when scrapers rotate proxies to present fresh source addresses, the fingerprint hash remains constant if browser attributes don't rotate in lockstep. Research on[fingerprinting AI browsing agents](https://arxiv.org/abs/2605.01247) confirms that behavioral and browser fingerprints persist across sessions, allowing servers to link requests from apparently different IPs back to a single automated source.


### The proxy rotation trap


Rotating proxies without rotating fingerprints exposes scrapers as automated - servers see multiple IPs sharing the same fingerprint hash, a pattern typical of bot infrastructure. Rate-limit enforcement using sliding window algorithms buckets requests by fingerprint, not IP, so each new proxy connection adds to the same quota counter. When the per-fingerprint threshold trips, the server returns HTTP 429 across all IPs tied to that hash, nullifying the proxy rotation strategy entirely.


## How leaked session state groups your requests


Rotating proxies changes the IP fingerprint, but session-layer identity persists unless explicitly cleared. Cookies, localStorage, and IndexedDB outlive proxy rotation, grouping seemingly independent requests under one session bucket. Anti-bot systems read these artifacts to recognize returning clients, even when the IP has changed.


### Session cookies that outlive proxy rotation


Session cookies carrying JWT tokens or authentication state remain in the scraper's context across proxy hops unless explicitly dropped. A single login cookie can bind hundreds of requests to one identity. When a 403 signals session or proxy trouble, clearing cookies before retry prevents the next request from inheriting the flagged session.


### localStorage and IndexedDB persistence


Headless browser scrapers inherit localStorage and IndexedDB entries from prior runs. These storage APIs persist client-side state that targets use for fingerprinting - device IDs, feature flags, interaction timestamps. Proxy rotation alone does not clear browser storage; each new IP carries the same client-side footprint until the context is torn down.


Three session-lifecycle management steps mitigate state leakage:


1. Clear cookies and storage before each proxy rotation to drop inherited session artifacts.
2. Isolate browser contexts using separate profiles or container tabs so that parallel requests do not share state.
3. Validate session expiry before retry to avoid reusing a flagged credential after a failed request.


Anakin's intelligent caching sidesteps the session-state problem by serving repeat requests from cache when the request fingerprint matches a recent successful fetch. Cached responses return significantly faster than fresh scrapes and cost zero credits, eliminating the retry cycle entirely.


## Reading and acting on Retry-After headers


When a server returns HTTP 429, it often includes a Retry-After header specifying how long to wait before retrying. Scrapers that ignore this header and retry immediately trigger further rate-limit responses, compounding the problem. Adaptive retry logic that parses and honors Retry-After prevents this loop.


### Parsing Retry-After header values


The *Retry-After* header ships in two formats: delay-seconds (integer, e.g. *120* ) or an HTTP-date (absolute timestamp, e.g. *Wed, 21 Oct 2026 07:28:00 GMT* ). Robust scrapers must handle both. Try parsing as an integer first, then fall back to an HTTP-date parser.


```text
import time
import requests
from email.utils import parsedate_to_datetime
from datetime import datetime, timezone


def parse_retry_after(header_value: str) -> float:
# Return seconds to wait, from either a delay-seconds or HTTP-date value.
try:
return max(0, int(header_value))
except ValueError:
retry_at = parsedate_to_datetime(header_value)
delta = (retry_at - datetime.now(timezone.utc)).total_seconds()
return max(0, delta)


def fetch_with_backoff(url: str, max_retries: int = 5):
for attempt in range(max_retries):
r = requests.get(url)
if r.status_code != 429:
return r
wait = parse_retry_after(r.headers.get('Retry-After', '1'))
time.sleep(wait)
raise RuntimeError(f'Exhausted retries for {url}')
```


### Exponential backoff implementation


When *Retry-After* is absent, exponential backoff is the recommended fallback: start with a short delay, then double on each retry. The[tenacity](https://tenacity.readthedocs.io/) library handles this cleanly for HTTP clients.


```text
import requests
from tenacity import retry, stop_after_attempt, wait_exponential, retry_if_exception_type


class RateLimited(Exception):
pass


@retry(
retry=retry_if_exception_type(RateLimited),
wait=wait_exponential(multiplier=1, min=1, max=60),
stop=stop_after_attempt(6),
reraise=True,
)
def scrape(url: str):
r = requests.get(url, timeout=30)
if r.status_code == 429:
raise RateLimited(r.headers.get('Retry-After'))
r.raise_for_status()
return r.json()
```


Backoff alone is not enough when the server groups every retry under the same fingerprint - the retry requests still land in the same identity bucket, and the backoff timing becomes irrelevant. In those cases, rotate user-agents, cookies, or IP addresses between retries to break fingerprint correlation.


## How Anakin's infrastructure eliminates 429 errors


The three root causes - fingerprint inconsistency, rate-bucket accumulation, and session-state leakage - converge into one infrastructure problem. Manual scrapers address symptoms one at a time. Anakin's proxy routing, caching layer, and session-lifecycle management eliminate all three at the platform level.


### Zero-block proxy routing and fingerprint consistency


Anakin routes requests through residential proxies across 207 countries and territories, maintaining consistent browser fingerprints within each session. Switching proxies mid-session doesn't trigger the fingerprint-mismatch patterns that cause defensive 429 blocks. The platform handles geo selection, timezone alignment, and locale matching automatically. See the[URL Scraper docs](https://anakin.io/products/url-scraper) for the full request surface.


```text
curl -X POST https://api.anakin.io/v1/url-scraper \
-H "X-API-Key: $ANAKIN_API_KEY" \
-H "Content-Type: application/json" \
-d '{
"url": "https://example.com/product/123",
"country": "us"
}'


# 202 Accepted
# {
#   "jobId": "job_01H8QZ4M7ABCXYZ",
#   "status": "processing"
# }
```


### Intelligent caching for repeat requests


Repeat requests for the same URL return from cache at zero credit cost. Cached responses bypass the target server entirely, so no rate-limit bucket is touched. This solves rate-bucket accumulation without requiring developers to build their own cache-invalidation logic or track which requests are safe to repeat.


### Session-lifecycle management at the infrastructure layer


Anakin handles authenticated scraping through[Browser Sessions](https://anakin.io/products/browser-sessions) that isolate each user's session context automatically. Session clearing, context isolation, and credential rotation are managed at the platform layer, removing session-state leakage from the developer's responsibility. The infrastructure prevents stale-session 429 errors without requiring manual session teardown after every workflow run.


## Troubleshooting 429 errors: common questions


### Diagnosing your 429 root cause


A 429 error can be tied to different request identities - IP address, session, account, API key, cookie, or client fingerprint. Follow this decision tree to isolate the root cause:


1. Check for a Retry-After header. If present, honor it. Retry-After: 120 means wait 120 seconds.
2. Test whether rotating IP alone resolves the block. If yes, the limit is per-IP.
3. If IP rotation doesn't fix it, test fingerprint rotation - TLS signature, user-agent, header order.
4. If still failing, check session state. The limit may be tied to cookies or localStorage.


### When to use exponential backoff vs infrastructure solutions


Choose exponential backoff when:


- The 429 is infrequent and you have engineering capacity to implement retry logic with jitter.
- Your volume stays well under the published rate limit and you can afford to slow down requests.


Choose an infrastructure service like Anakin when:


- The limit is tied to fingerprint or session state - IP rotation alone won't fix it.
- You need zero engineering effort for proxy rotation, anti-bot handling, and retry orchestration.


Manual fingerprint rotation and session clearing cost nothing in infrastructure but require engineering effort to build and maintain. Infrastructure services like Anakin eliminate that effort at the cost of per-request pricing. Exponential backoff works when 429s are purely timing-based (per-IP buckets with no fingerprinting) but fails when servers cluster requests by identity - diagnosing your 429 root cause determines which solution fits.


As server-side bot detection continues layering behavioral signals - mouse movement entropy, request timing clustering, TLS fingerprint evolution - rate-limit enforcement will increasingly rely on identity clustering rather than simple per-IP buckets, making infrastructure that manages fingerprint consistency and session lifecycle the default for reliable scraping at scale.


[Try Anakin's URL scraper](https://anakin.io/products/url-scraper) to eliminate 429 errors with zero-block proxy routing and intelligent caching - no manual fingerprint management required.


## Frequently asked questions


### Why do I still get 429 errors after adding random delays between requests?


Delays address timing but not identity clustering. Servers enforce rate limits using sliding-window algorithms that count requests per fingerprint, IP address, session token, or browser signature - not per second. Even with delays, requests sharing the same fingerprint accumulate in the same rate-limit bucket, triggering 429 errors when the bucket threshold is exceeded.


### What's the difference between per-IP and per-session rate limits?


Per-IP limits count requests from one IP address within a time window, regardless of session identity. Per-session limits count requests from one session cookie or fingerprint hash, regardless of the IP address used. When servers layer both, rotating proxies solves only the per-IP constraint - per-session buckets still accumulate unless fingerprints and session state are cleared.


### Does rotating proxies fix HTTP 429 errors?


Rotating proxies alone doesn't fix 429 errors when servers enforce per-fingerprint or per-session buckets. Production rate limiters count requests *per identifier* , not per second. You must also rotate browser fingerprints and clear session state (cookies, localStorage) to distribute requests across separate rate-limit buckets and avoid accumulation under one identity.


### How do I parse the Retry-After header in Python?


Check whether *Retry-After* is present in *response.headers* . Parse as an integer (delay-seconds) or as an HTTP-date string with *email.utils.parsedate_to_datetime* . See the *parse_retry_after* helper earlier in this post for a reference implementation.


### What browser fingerprinting signals do servers track?


Servers track six core signals: user-agent string, screen resolution, timezone, installed fonts, WebGL renderer, and TLS cipher suite. These characteristics are hashed into a persistent fingerprint that survives IP changes. Anti-bot systems probe these artifacts to identify automation - inconsistent or generic fingerprints (for example, headless Chrome defaults) expose scrapers even when proxies rotate.


### When does exponential backoff fail to fix 429 errors?


Exponential backoff fails when the server groups all retry attempts under one session fingerprint. The backoff logic doesn't help because the server sees retries as the same automated source, accumulating in the same rate-limit bucket. This failure mode occurs when scrapers don't rotate fingerprints or clear session state between retries, causing identity persistence across backoff intervals.


### How does Anakin's intelligent caching avoid rate limits?


Intelligent caching serves repeat requests from cache when the request fingerprint matches a recent successful fetch. Cached requests bypass the rate-limit bucket entirely and cost zero credits. This approach sidesteps session-state accumulation because the server never sees the cached request - the response is served from Anakin's infrastructure, not the target origin.


---


[Back to blog](https://anakin.io/blog)
