---
schema_version: "1.0.0"
document_id: "f9eed777330f06e8ff6aa52f39e0b9b41838a0ab4f2fc74646bbc3f57d1d49bd"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/detect-vpn-traffic-with-new-ip-intelligence"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-27T11:16:58.513640+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:878bb5f35b4d5169249ae9bb6d3a50f4528346b5929c256518da7d6064915446"
---

# Detect VPN traffic with new IP intelligence

[Back to All](https://docs.castle.io/changelog)


Added


Recently we launched[Residential Proxy detection and richer IP tunnel intelligence](https://docs.castle.io/changelog/detect-residential-proxies-with-new-ip-intelligence) . Now we're extending that same foundation to VPNs. VPN use isn't proof of fraud on its own, but together with other indicators like a high risk score or a new device, it's a strong sign of an attacker trying to cover their tracks.


The new` vpn_access` signal triggers when a request comes from an IP recently seen on a known VPN provider's network. It's visible and filterable in the Dashboard, and exposed in the API response signals.


Like with proxies, VPNs show up in the` ip.tunnels` payload, with the provider as` operator` and a` last_seen_at` timestamp for when we last confirmed VPN activity on that IP. We already cover a large share of the most popular providers, and we're adding more over time.


Check out this and all the other signals in our[documentation](https://docs.castle.io/docs/signals-reference) .


Example API response:


```text
{
"ip": {
"address": "185.253.162.22",
"connection": "datacenter",
"tunnels": [
{
"type": "proxy",
"operator": "Private Internet Access",
"proxy_type": "vpn",
"last_seen_at": "2026-06-16T18:00:00Z"
}
],
// ...
},
"signals": {
"vpn_access": {}
}
}
```
