---
schema_version: "1.0.0"
document_id: "909d249f86cb948f327bd5fcb2decef038f4a63f93c2cdd8fb63f1d4e9943910"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/fix-err-too-many-redirects/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T05:07:29.970247+00:00"
fetched_at: "2026-08-12T05:07:31.849756+00:00"
content_hash: "sha256:5fa5622a3cfeb0bd224f8536016282d5289be3d51604564acee5a821b5a3479c"
---

# How to Fix 'ERR_TOO_MANY_REDIRECTS'

` ERR_TOO_MANY_REDIRECTS` means the browser followed a chain of redirects that never resolves — most commonly URL A → URL B → URL A — and gave up after hitting its built-in hop limit.


If you’ve hit this, you probably changed one proxy or SSL setting and then watched every request ping-pong between two URLs instead of loading. It’s a maddening error precisely because the page worked fine an hour ago and nothing looks obviously broken. It is not a browser bug or a transient glitch; it signals that two or more redirect rules on your stack disagree about where a URL should land. This guide is diagnosis-first: you trace the loop before touching config, then fix the real root cause, which for most developers is a protocol mismatch behind a proxy or CDN, not a WordPress plugin.


## Key Takeaways


- ` ERR_TOO_MANY_REDIRECTS` is a redirect loop; Chromium and Firefox stop after 20 hops, and Safari stops sooner, then surface the error instead of the page.
- Diagnose before you change anything:` curl -I -L https://yourdomain.com` prints every hop’s headers, and a loop shows as the same two URLs repeating in successive` Location:` lines.
- The most common loop developers hit is a protocol mismatch: a proxy or CDN terminates TLS and forwards plain HTTP to an origin that force-redirects to HTTPS, so the cycle repeats forever.
- On Cloudflare,` Flexible` SSL plus “Always Use HTTPS” (or an origin that forces HTTPS) guarantees a loop; switch to` Full (strict)` after installing an origin certificate.
- A durable fix owns HTTP→HTTPS and www/non-www redirects in exactly one layer (framework, web server, or CDN), never several at once.


## What does ‘ERR_TOO_MANY_REDIRECTS’ mean?


A redirect loop happens when your site keeps answering one URL with a redirect to another that eventually points back, so the browser never reaches a final` 200` response. Browsers cap how many hops they will follow: Chromium and Firefox both stop at 20, and Safari stops sooner, at which point they abandon the request and show an error.


The wording differs per browser, so any of these describe the same condition:


Browser Message


Chrome` ERR_TOO_MANY_REDIRECTS` / “redirected you too many times”


Firefox ”The page isn’t redirecting properly”


Edge ”This page isn’t working right now”


Safari ”Safari Can’t Open the Page”


The redirects themselves are ordinary[3xx responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Redirections) , usually` 301` or` 302` , each carrying a` Location` header. In a loop, the same two` Location` values alternate until the browser gives up.


## Diagnose first: trace the redirect chain


Before changing any config, trace the chain from the command line.` curl -I -L https://yourdomain.com` prints the response headers for every hop, and a loop shows up as the same two URLs repeating in successive` Location:` lines. In the[curl manual](https://curl.se/docs/manpage.html) ,` -I` (` --head` ) fetches headers only and` -L` (` --location` ) follows each` Location` header to the next URL.


A looping origin produces output like this:


```text
HTTP/2 301
location: https://app.example.com/


HTTP/2 301
location: http://app.example.com/


HTTP/2 301
location: https://app.example.com/
...
curl: (47) Maximum (50) redirects followed
```


The alternating` http://` ↔` https://` here is the signature of a protocol-mismatch loop. If you want response bodies too, use` curl -sSL -o /dev/null -D - https://yourdomain.com` , which dumps every header while discarding the body.


No-install alternatives: the browser DevTools **Network** tab shows the same 301/302 chain with each` Location` , and the[Redirect Path](https://chromewebstore.google.com/detail/redirect-path/aomidfkchockcldhbkggjokdkkebmdll) extension or an online redirect checker will render the chain for a URL. Read whichever output you use for the URL that repeats: that repeating pair is the loop.


## The #1 dev cause: HTTP↔HTTPS loops from SSL termination


The most common redirect loop developers hit isn’t a plugin. It’s a protocol mismatch: a proxy or CDN terminates TLS and forwards plain HTTP to your origin, your app sees` http` , redirects to` https` , and the cycle repeats forever. The browser talks HTTPS to the edge; the edge talks HTTP to your app; your app “helpfully” redirects back to HTTPS.


**On Cloudflare** , setting SSL/TLS to` Flexible` while your origin also forces HTTPS guarantees a loop, because[Flexible always sends HTTP to the origin](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/) . The trigger is often the separate “Always Use HTTPS” toggle layered on top of Flexible. The fix: install a certificate on the origin, then switch the mode to` Full (strict)` . The current mode set is Off, Flexible, Full, Full (strict), and Strict, not the “three modes” older guides describe.


**Behind your own proxy or load balancer** , make the app trust the forwarded-protocol header instead of re-redirecting. The proxy should send[X-Forwarded-Proto](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Forwarded-Proto) with the browser’s original scheme, and the app should read it rather than the plaintext hop. In[Express, enable trust proxy](https://expressjs.com/en/guide/behind-proxies/) so` req.protocol` and` req.secure` reflect the forwarded value:


```text
// Trust the first proxy hop, then req.secure reflects X-Forwarded-Proto
app  .  set  (  '  trust proxy  '  ,   1  );


app  .  use  ((  req  ,   res  ,   next  )   =>   {
if (  !  req  .  secure  ) {
return   res  .  redirect  (  301  ,   `  https://  ${  req  .  headers  .  host  }${  req  .  originalUrl  }`  );
}
next  ();
});
```


Without` trust proxy` ,` req.secure` stays` false` behind a TLS-terminating proxy and this exact middleware loops. In Nginx doing the origin redirect, guard on the forwarded scheme so the rule can’t fire against traffic that already arrived as HTTPS:


```text
# Only redirect when the edge saw plain HTTP
if ($  http_x_forwarded_proto   =   "http"  ) {
return   301   https://$  host  $  request_uri  ;
}
```


A production loop that only fires for logged-in users or only behind the CDN is invisible to an anonymous` curl` run. A session replay of the affected session shows which two URLs were bouncing under the real user’s cookies and edge context, reproducing the condition server logs describe but don’t let you see.


## Cookie and auth-guard loops


Stale cookies and misrouted auth are the second big category. A cookie holding old redirect state, or an HSTS policy the browser cached, can force one client into a loop while everyone else loads the site fine; that’s the “fails in my normal window, works in incognito” symptom. Clear cookies and site data for the affected domain first.


The programmatic version is an auth guard looping on its own login page. If` /login` is itself behind the “redirect unauthenticated users to` /login` ” rule, every visit bounces back to` /login` . Exclude the login route from the guard. The same happens when a login handler redirects to a protected page whose guard immediately sends the user back because the session cookie was never set (a frequent side effect of the` req.secure` mismatch above, where a` Secure` cookie is refused over the proxy’s HTTP hop).


## Redirect-rule mistakes: two layers disagreeing


A redirect loop is almost always two layers disagreeing about the canonical URL (framework, web server, and CDN each enforcing a different rule), so the durable fix is to own HTTP→HTTPS and www/non-www redirects in exactly one layer. Classic cases: one layer forces` www` , another strips it; a rule whose destination still matches its own condition; or the same redirect duplicated across framework, host, and CDN.


Frameworks are a first-class redirect layer, not an afterthought:


- **Next.js** defines redirects with[async redirects() in next.config.js](https://nextjs.org/docs/app/api-reference/config/next-config-js/redirects) , where` permanent: true` emits` 308` and` false` emits` 307` . Note that as of Next.js 16 the old` middleware` file convention is[renamed to Proxy ( proxy.ts )](https://nextjs.org/docs/app/api-reference/file-conventions/proxy) ; a leftover` middleware.ts` still works for Edge runtime use cases but is deprecated and will be removed in a future version, so redirect or auth logic there should be migrated to` proxy.ts` .
- **Nginx** uses a` return 301` directive: guard it, as shown above, so it doesn’t re-fire behind a proxy.
- **Express** uses middleware; keep exactly one HTTPS-redirect middleware in the chain.
- **WordPress** is one instance of the same pattern: a mismatch between the *WordPress Address* and *Site Address* settings is just two layers disagreeing, resolved by making both match.


On the server side, Apache can also throw a distinct “request exceeded the limit of 10 internal redirects” error, whose[internal-rewrite limit of 10](https://httpd.apache.org/docs/current/mod/core.html#limitinternalrecursion) is separate from the browser’s 20-hop cap, a useful tell that the loop lives in` .htaccess` , not the client. On Cloudflare, put the HTTP→HTTPS redirect in a **Redirect Rule** in the modern[Rules engine](https://developers.cloudflare.com/rules/reference/page-rules-migration/) ; Page Rules are being phased out in favor of the modern Rules engine.


## How do you prevent redirect loops?


Most loops are introduced by a config change, so make redirect changes deliberate. Keep this checklist:


1. **One layer owns each redirect.** Decide whether HTTPS and www/non-www canonicalization live in the CDN, the web server, or the app, and remove the duplicates from the other two.
2. **Trust the proxy, don’t re-redirect.** Behind any TLS-terminating hop, read` X-Forwarded-Proto` instead of forcing HTTPS blindly.
3. **Re-trace the chain after every change.** Run` curl -I -L` against the affected URL after any HTTPS, domain, or URL-structure change, and confirm it ends in a single` 200` .


The fastest path out of a redirect loop is always the trace, not the guess. Run` curl -I -L` against the failing URL, find the two URLs ping-ponging in the` Location` headers, then fix the one layer that’s redirecting against the grain, most often a proxy handing HTTP to an origin that insists on HTTPS.


## FAQs


Why does the redirect loop clear in incognito but not in my normal browser window?


Incognito starts with no stored cookies or cached HSTS policy, so a loop that survives normal browsing but disappears in a private window points to client-side state, not a server rule. A stale cookie holding old redirect state, or a cached HSTS policy forcing HTTPS on a misconfigured origin, will loop only the affected profile while other users load the site fine. Clear cookies and site data for the domain, and if HSTS is suspected, inspect chrome://net-internals/#hsts.


What is the difference between Cloudflare Flexible and Full (strict) SSL for redirect loops?


Flexible always sends plain HTTP from Cloudflare to your origin, so if the origin force-redirects HTTP to HTTPS, the request loops forever. Full (strict) sends HTTPS to the origin and validates a trusted certificate there, matching what the origin expects and breaking the loop. Install a valid certificate on the origin first, then switch the SSL/TLS mode from Flexible to Full (strict). The separate 'Always Use HTTPS' toggle layered on Flexible is a common trigger.


Why do curl and the browser show different redirect behavior for the same URL?


curl runs as an anonymous client with no cookies, no cached HSTS policy, and no login session, so it only reproduces loops caused by server or CDN rules that apply to every request. Loops that depend on a specific cookie, an authenticated session, or a particular CDN edge will not appear in an anonymous curl trace. For those, capture the real user's context: browser DevTools on the affected session, or a session replay showing which two URLs were bouncing under that user's cookies and auth state.


Does a Next.js 16 redirect in next.config.js apply to client-side navigation with Link or router.push?


When using the Pages Router, redirects defined in the redirects() function of next.config.js are not applied to client-side routing through Link or router.push unless a Proxy file, formerly middleware, is present and matches the path. next.config.js redirects run on the server for full page loads and initial requests, so client-side transitions can bypass them. In Next.js 16 the middleware file convention was renamed to Proxy (proxy.ts); a leftover middleware.ts should be migrated, since redirect and auth logic left there may stop running.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
