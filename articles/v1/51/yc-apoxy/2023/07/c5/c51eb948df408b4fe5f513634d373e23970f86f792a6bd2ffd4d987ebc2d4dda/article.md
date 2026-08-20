---
schema_version: "1.0.0"
document_id: "c51eb948df408b4fe5f513634d373e23970f86f792a6bd2ffd4d987ebc2d4dda"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/riffing-on-nginx-playground"
published_at: "2023-07-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:988411a17b18450ad6ce34bfbc75a5f3f10eed7102b29737c070f7b587260569"
---

# Riffing on Nginx Playground

We’ve been working with Envoy Proxy a lot and when we first learned about Julia Evans’[Nginx Playground](https://nginx-playground.wizardzines.com/) we though it would be cool to build a similar playground for Envoy too. Envoy is an incredibly powerful L4/L7 proxy but the flip side is that writing configs for it really is a pain (we’d argue even more so than Nginx).


Since Julia open-sourced Nginx Playground recently, we thought we’d give it a shot. We figured it would be pretty easy to fork Nginx Playground and convert it to Envoy. And so it begins:


First, the “hard” part` git clone`[https://github.com/jvns/nginx-playground/](https://github.com/jvns/nginx-playground/)


` sed s/nginx/envoy/`


Next, try to compile it, fail - try again.


Remember that[func-e](https://func-e.io/) is awesome and built to vendor and run Envoy quicly.


Doh. func-e is unable to download binary from the[bubblewrap](https://github.com/containers/bubblewrap) sandbox.


Vendor func-e in the Docker image.


Realize that the Envoy team for some reason suffixed the binary with` aarch_64` instead of` aarch64` so fix that.


On to the UI - run sed again.


Fix a bug in CodeMirror and[upstream the fix](https://github.com/jvns/nginx-playground/pull/3/files) .


Serve UI assets directly from Go using[http.FileServer](https://pkg.go.dev/net/http#FileServer) so that everything could be run from one process.


**Ship it.**


Deploy to Fly via` flyctl launch` which builds and starts the Docker container automatically!


CNAME` envoy-playground.apoxy.dev` to` envoy-playground.fly.dev`


Create the TLS certificate with` flyctl certs create envoy-playground.apoxy.dev`


**Done! It’s[live](https://envoy-playground.apoxy.dev/) !**


Dmitry Ilyevsky


Co-founder & CTO


Dmitry is co-founder and CTO of Apoxy. He previously built and operated infrastructure at Google, Cruise, and Mux (where he and Matt met).


[GitHub ↗](https://github.com/dilyevsky)


[← Back to all posts](https://apoxy.dev/blog)
