---
schema_version: "1.0.0"
document_id: "21ccade8eac3c7b6fb16d54d36e4303fc643c9655ea6fc6a139d9befcb6b74df"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/ip-intelligence-api"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T17:50:10.642270+00:00"
fetched_at: "2026-08-06T17:50:12.970420+00:00"
content_hash: "sha256:9816321c7f1b2cc7ce47f7ad078422f7778c03d847f3306ac9105f5b27127503"
---

# IP Intelligence API

[Back to All](https://docs.castle.io/changelog)


Added


Over the past months, we shipped[residential proxy detection](https://docs.castle.io/changelog/detect-residential-proxies-with-new-ip-intelligence) and[VPN detection](https://docs.castle.io/changelog/detect-vpn-traffic-with-new-ip-intelligence) as signals on the Risk API. The data behind those signals is now a product of its own. Instead of sending an event and reading the signals on the response, you can query an IP directly and get the raw intelligence back:


- ` GET /v1/ips/{value}` looks up a single IP in real time
- ` POST /v1/ips/query` resolves many at once
- ` GET /v1/ips/downloads/{dataset}/{window}.{format}.gz` gives you precomputed snapshots


For any IP, you get the autonomous system it belongs to, a coarse location, and the proxy and VPN tunnels Castle has recently observed on it. Each tunnel carries the` operator` running it, a` tier` for VPN providers, and a` last_seen_at` timestamp for when we last confirmed activity on that network.


Use lookups to enrich your own systems and feed your own risk logic. Download full datasets to use Castle's data for bulk enrichment or offline analysis.


Full details are in the[documentation](https://docs.castle.io/docs/ip-intelligence-api) .


Example request:


```text
curl -sSf -u ":$CASTLE_API_SECRET" \
https://api.castle.io/v1/ips/1.0.105.13
```


```text
{
"address": "1.0.105.13",
"type": "ipv4",
"asn": 18144,
"location": {
"continent_code": "AS",
"country_code": "JP"
},
"tunnels": [
{
"type": "proxy",
"operator": "FloppyData residential",
"tier": null,
"last_seen_at": "2026-06-25T21:38:40.000Z",
"proxy_type": "residential"
},
{
"type": "proxy",
"operator": "AnyIP residential",
"tier": null,
"last_seen_at": "2026-06-21T22:55:42.000Z",
"proxy_type": "residential"
}
]
}


```
