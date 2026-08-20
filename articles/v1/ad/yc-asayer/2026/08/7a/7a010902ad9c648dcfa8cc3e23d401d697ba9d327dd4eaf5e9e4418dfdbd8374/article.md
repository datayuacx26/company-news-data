---
schema_version: "1.0.0"
document_id: "7a010902ad9c648dcfa8cc3e23d401d697ba9d327dd4eaf5e9e4418dfdbd8374"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/force-https-htaccess/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T18:37:30.227787+00:00"
fetched_at: "2026-08-18T18:37:31.619257+00:00"
content_hash: "sha256:8c1069d50b0c304b1c81f4e12593c1bcd12a6f2819487e48a3e7766d93079c35"
---

# How to Force HTTPS with .htaccess

To force HTTPS on all traffic in Apache, add three lines to the` .htaccess` file in your site root:` RewriteEngine On` ,` RewriteCond %{HTTPS} off` , and` RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} \[L,R=301\]` .


If you have ever pasted a redirect rule from a forum answer and then watched the browser bounce until it gave up, the rule itself was probably fine. What usually breaks it is whatever sits in front of your server.


That rule catches every request arriving over plain HTTP and issues a permanent redirect to the identical URL on HTTPS. It works on Apache with` mod_rewrite` enabled and an SSL certificate already installed, and it fails in specific, predictable ways when a CDN or load balancer sits in front of your origin. This guide gives you the copy-paste rule first, then the domain, folder, and www variations, then the redirect-loop fixes, and finally what to do if you aren’t on Apache at all.


## Key Takeaways


- The canonical rule tests` RewriteCond %{HTTPS} off` and rewrites to` https://%{HTTP_HOST}%{REQUEST_URI}` , which preserves the exact domain and path the visitor requested instead of hardcoding a single domain.
- ` R=301` issues a permanent redirect and` L` stops rewrite processing; while testing, use` R` (a temporary 302) first, because browsers cache 301s aggressively.
- Forcing HTTPS only works if a valid TLS/SSL certificate is already installed. Redirecting without one makes the site unreachable, not secure.
- Behind a TLS-terminating proxy,` %{HTTPS}` is never` on` , so the rule loops with` ERR_TOO_MANY_REDIRECTS` ; test` %{HTTP:X-Forwarded-Proto}` instead.
- A Cloudflare **Flexible** SSL loop is a Cloudflare-side misconfiguration fixed by changing the encryption mode, not by editing` .htaccess` .


## Before you start: SSL certificate and mod_rewrite


Forcing HTTPS only works if a valid TLS/SSL certificate is already installed on the domain. Redirecting to HTTPS without a certificate doesn’t secure the site, it makes it unreachable behind a browser security warning. (“SSL certificate” is the common industry term; the protocol is actually TLS.) Confirm the certificate is live by loading` https://yourdomain.com` directly in a browser and checking for the padlock before you touch` .htaccess` .


The rule below depends on Apache’s[mod_rewrite module](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html) , which is enabled by default on most shared and cPanel hosting. The` .htaccess` file lives in your site root, typically` public_html` or the document root for the domain. Edit it through the cPanel **File Manager** (enable “Show Hidden Files” to see dotfiles), over FTP, or via SSH. Back up the file before editing so you can restore it if a rule misfires.


## The .htaccess rule to force HTTPS on all traffic


Paste this into the` .htaccess` file in your site root to redirect every HTTP request to HTTPS:


```text
RewriteEngine On
RewriteCond   %{HTTPS}   off
RewriteRule   ^(.*)$   https://%{HTTP_HOST}%{REQUEST_URI}   [L,R=  301  ]
```


Line by line:` RewriteEngine On` turns the rewrite engine on.` RewriteCond %{HTTPS} off` fires the rule only when the connection is not already encrypted.` RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI}` rebuilds the URL on HTTPS, and the` %{HTTP_HOST}` and` %{REQUEST_URI}` variables preserve the exact domain and path the visitor requested, so the rule works across multiple domains and never silently forces` www` .


In the flags` \[L,R=301\]` ,` R=301` issues a permanent redirect and` L` stops rewrite processing at that rule. While testing, use` R` alone (a temporary 302) first, because browsers cache 301s hard and a wrong one is painful to unwind; switch to` R=301` only once you’ve confirmed the redirect resolves cleanly.


**Do not repeat` RewriteEngine On` .** If the line already exists in the file, add only the` RewriteCond` and` RewriteRule` beneath it.


## Variations: specific domain, folder, and www canonicalization


To force HTTPS on only one domain when several point at the same document root, add a condition on` HTTP_HOST` :


```text
RewriteEngine On
RewriteCond   %{HTTP_HOST}   ^yourdomain\.com   [NC]
RewriteCond   %{HTTPS}   off
RewriteRule   ^(.*)$   https://%{HTTP_HOST}%{REQUEST_URI}   [R=  301  ,L]
```


The` NC` flag makes the host match case-insensitive. To combine HTTPS with` www` /non-` www` canonicalization,[htaccessbook](https://htaccessbook.com/htaccess-redirect-https-www/) documents wrapping both redirects in a` mod_rewrite` guard:


```text
<  IfModule   mod_rewrite.c  >
RewriteEngine On


RewriteCond   %{HTTPS}   off
RewriteRule   (.*)   https://%{HTTP_HOST}%{REQUEST_URI}   [L,R=  301  ]


RewriteCond   %{HTTP_HOST}   !^www\.   [NC]
RewriteRule   (.*)   https://www.%{HTTP_HOST}%{REQUEST_URI}   [L,R=  301  ]
</  IfModule  >
```


The published version of this block assumes` RewriteEngine On` appears earlier in the file, so it is added above to make the snippet run on its own. The opening rule moves the request to HTTPS. The second one adds` www` to a host that lacks it. Note that` \[L\]` ends processing as soon as a rule matches, so a plain HTTP request without` www` costs two redirects, not one: first to HTTPS, then to the` www` host. Wrapping the rules in` <IfModule mod_rewrite.c>` makes the site fail open (serving over HTTP) instead of throwing a 500 error if` mod_rewrite` isn’t loaded.


Stacking these rules by hand gets fiddly once you’re combining HTTPS, www canonicalization, a handful of redirects and a caching block in the same file, and a stray flag or a rule in the wrong order will bite you in production. OpenReplay’s[htaccess generator](https://openreplay.com/tools/htaccess-generator/) assembles the file for you from a set of toggles: force HTTPS, add or strip` www` , add 301 or 302 redirects, switch on gzip and browser caching, block IPs, map custom error pages. The output is commented, updates as you change the options, and runs entirely in your browser, so you can copy it out or download it and diff it against what you already have.


## Fixing ERR_TOO_MANY_REDIRECTS behind a CDN or load balancer


If your redirect causes` ERR_TOO_MANY_REDIRECTS` , the browser has followed too many hops and given up, and the usual cause is a TLS-terminating proxy or load balancer. TLS ends at the proxy, so` %{HTTPS}` is never` on` at the origin, the rule fires on every request, and the loop never breaks. The fix is to trust the forwarded scheme instead:


```text
<  IfModule   mod_rewrite.c  >
RewriteEngine On
RewriteCond   %{HTTP:X-Forwarded-Proto}   !https
RewriteRule   ^(.*)$   https://%{HTTP_HOST}%{REQUEST_URI}   [L,R=  301  ]
</  IfModule  >
```


Here` RewriteCond %{HTTP:X-Forwarded-Proto} !https` reads the` X-Forwarded-Proto` header the proxy sets, so the rule fires only when the *original* visitor connection was HTTP. Test with` R` before promoting to` R=301` .


A Cloudflare **Flexible** SSL loop is a different problem with a different fix. Under[Flexible encryption](https://developers.cloudflare.com/ssl/troubleshooting/too-many-redirects/) , the hop between Cloudflare and your server is unencrypted, so an origin rule that insists on HTTPS keeps sending the request back around. Cloudflare offers two ways out: drop the HTTPS redirect at the origin, or move the zone up to Full or stricter, which requires a certificate on the origin itself. Either change happens in the Cloudflare dashboard, not in` .htaccess` . A Flexible-mode visitor is still browsing over HTTPS, and the[scheme Cloudflare reports in X-Forwarded-Proto](https://developers.cloudflare.com/fundamentals/reference/http-headers/) mirrors the visitor’s own connection, so the header test above won’t misfire here. Correcting the encryption mode is still the real fix.


After any change, clear your browser cache and cookies before retesting, since a cached 301 can mask a fix. A conditional loop that only affects users arriving through one proxied path is invisible in a single manual test from your own browser. Session replay of a post-migration site can surface that intermittent redirect loop, and mixed-content breakage, as an infinite-bounce pattern real users hit.


## When .htaccess isn’t the right tool


` .htaccess` is Apache-only, and it’s read only on Apache-based hosting. On[Nginx](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html) there is no` .htaccess` file. You force HTTPS with a server block that listens on port 80 and returns a redirect:


```text
server   {
listen   80  ;
server_name yourdomain.com www.yourdomain.com;
return   301   https://$  host  $  request_uri  ;
}
```


Keep` return 301` in the port-80 block only; putting it inside the 443 block recreates the loop. On modern stacks, HTTPS enforcement often belongs at the CDN, platform, or load-balancer layer rather than in server config at all.


Once your redirect works, harden it with an HSTS header so browsers connect over HTTPS automatically and skip the insecure HTTP request entirely.[HSTS is defined in RFC 6797](https://datatracker.ietf.org/doc/html/rfc6797) ; the[OWASP HSTS cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) recommends` Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` . Treat` preload` as a one-way door: getting a domain removed from the list is slow, and while you wait, visitors can be shut out of the domain and everything under it if you ever have to fall back to HTTP.


Pick the rule that matches your setup, deploy it with a temporary 302, verify the redirect resolves in one hop, then promote it to a permanent 301 and layer HSTS on top. That sequence forces HTTPS without the redirect loops and cached mistakes that turn a five-minute change into an outage.


## FAQs


What is the difference between a 301 and a 302 redirect when forcing HTTPS?


A 301 is a permanent redirect and a 302 is temporary. Browsers cache 301s aggressively and hold them for a long time, so a wrong 301 is hard to reverse. While testing an HTTPS redirect, use R (a 302) in the RewriteRule flags first, confirm the redirect resolves cleanly in one hop, then promote it to R=301 for the permanent version.


How do I verify my HTTPS redirect resolves correctly instead of just clearing my browser cache?


Run curl -IL http://yourdomain.com from the command line. The -I flag requests headers only and -L follows redirects, so you see the full chain. A correct setup returns a single 301 with a Location header pointing to the https URL, then a 200 on the secure address. If you see repeated 301s or a redirect back to http, you have a loop. This is deterministic, unlike inspecting a cached browser tab.


Why does my HTTPS redirect throw a 500 error instead of redirecting?


A 500 error usually means mod_rewrite is not loaded but your rule calls RewriteEngine or RewriteRule directly. Wrap the rules in an IfModule mod_rewrite.c guard so Apache skips them and serves over HTTP instead of failing when the module is absent. On most shared and cPanel hosting mod_rewrite is enabled by default, but the guard is the safe pattern if you cannot confirm it.


Does the .htaccess HTTPS rule work on AWS Application Load Balancer or other TLS-terminating proxies?


Not the standard %{HTTPS} off rule. When an AWS ALB or similar load balancer terminates TLS, the encrypted connection ends at the proxy and your Apache origin always sees plain HTTP, so %{HTTPS} is never on and the rule loops with ERR_TOO_MANY_REDIRECTS. Instead, test RewriteCond %{HTTP:X-Forwarded-Proto} !https, which reads the header the proxy sets to report the original visitor protocol.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
