---
schema_version: "1.0.0"
document_id: "61d42798054d7751fea9f17783c7f1d8462a7ed4b0952c7ccf09c1a347bdbf15"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/detect-residential-proxies-with-new-ip-intelligence"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-27T11:16:58.513640+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:8106bee0ece58ccb089e76b2abd541f0725ec98188a48ef6dbd83f1f147c39d2"
---

# Detect residential proxies with new IP intelligence

[Back to All](https://docs.castle.io/changelog)


Added


Castle has long provided **Proxy IP** and **Tor IP** signals to flag suspicious IP activity. With this release, we're adding a new **Residential Proxy IP** signal and richer IP tunnel intelligence.


Residential proxies route traffic through real consumer ISPs, making them invisible to traditional datacenter-based proxy detection. The new` residential_proxy_access` signal fires when Castle has observed an IP in a residential proxy provider's network within the last 7 days. It complements the existing` proxy_ip` signal, which covers datacenter proxies, VPNs, and other known proxy IP ranges.


###


New fields on the` ip` object


- **` ip.connection`** - the connection type:` residential` ,` datacenter` ,` corporate` , or` mobile`
- **` ip.tunnels`** - an array of observed tunnels, each with:


- ` type` -` proxy` ,` vpn` ,` tor` , or` relay`
- ` operator` - provider name when known (e.g. "Geonode residential proxies")
- ` last_seen_at` - when the tunnel activity was last confirmed
- ` proxy_type` - for proxy type tunnels:` residential` ,` datacenter` , or` isp`


You can filter on tunnel data in Explore and build Policies around it, for example targeting events from IPs with recent residential proxy activity.


###


Relation to other IP signals


IP signals answer different questions and can fire simultaneously:


Signal What it tells you


` datacenter_ip` The IP belongs to a hosting provider or datacenter (based on` ip.connection` )


` proxy_ip` The IP matches known proxy IP ranges or non-residential proxy tunnels


` residential_proxy_access` The IP was recently observed on a residential proxy network (based on` ip.tunnels` with` proxy_type: residential` )


For example, a datacenter IP that a residential proxy provider routes traffic through will correctly trigger both` datacenter_ip` and` residential_proxy_access` .


###


API response example


A datacenter IP with residential proxy activity:


```text
{
"ip": {
"address": "172.58.121.153",
"connection": "datacenter",
"tunnels": [
{
"type": "proxy",
"operator": "Geonode residential proxies",
"proxy_type": "residential",
"last_seen_at": "2026-05-12T18:00:00Z"
}
],
"privacy": {
"anonymous": false,
"datacenter": true,
"proxy": false,
"tor": false
}
},
"signals": {
"datacenter_ip": {},
"residential_proxy_access": {}
}
}
```


Note:` ip.privacy.proxy` remains` false` for residential proxies as this field reflects the existing proxy detection. Use the` residential_proxy_access` signal or query` ip.tunnels` directly for residential proxy detection.
