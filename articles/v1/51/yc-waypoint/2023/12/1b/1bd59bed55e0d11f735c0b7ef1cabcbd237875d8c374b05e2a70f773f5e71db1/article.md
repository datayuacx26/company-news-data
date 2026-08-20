---
schema_version: "1.0.0"
document_id: "1bd59bed55e0d11f735c0b7ef1cabcbd237875d8c374b05e2a70f773f5e71db1"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/an-email-template-builder-with-a-liquidjs-export"
published_at: "2023-12-19T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:0579c55d48ae04469910c8a1c1169fbd4e0737db5355e578c038772b61ce3c16"
---

# An email template builder with a LiquidJS export

Access to 'raw' templates are now available from our API for teams that prefer to store templates in their own codebase. This means teams can build email templates visually and then use the API to download the LiquidJS template that can be run from within their own application.


## Why teams might use this


Some software teams prefer to build email templates within their codebase. This could be due working with highly sensitive data (eg. HIPAA compliance), legacy templates, or simply because it's the process the devs prefer.


In any case, building these email templates in code is still a pain so devs often turn to using a template builder. The problem with other builders is that it takes an additional step to convert it from static HTML content to a template with a ready-to-process templating system for variables, loops, conditionals and more.


Waypoint simplifies this process by allowing teams to programmatically grab 'raw' templates from Waypoint that include the LiquidJS templating.


## Example


As an example, here is a[template](https://www.usewaypoint.com/templates/post-metrics) built on Waypoint's template builder with LiquidJS set in the template for variables (` {{report.metric.value}}` ):


*Template visually built on Waypoint*


Using the[API to download the template](https://www.usewaypoint.com/) , we can access the raw subject line and raw body.


**Raw subject line**


```text


```


**Raw body** (snippet shown)


```text
<  div    style  = "font-size: 48px; font-weight: bold; padding: 16px 24px 0px 24px; text-align: center; max-width: 100%; box-sizing: border-box;"  >
<  p    style  = "margin-top: 0px; margin-bottom: 0px;"  >
{  {  report  . metric  . value  }  }
</  p  >
</  div  >
```


### Conclusion


If your team manages email templates from your codebase and could benefit from an visual email template builder, be sure to check out Waypoint.


Further reading:


-


[Downloading templates via Waypoint API](https://www.usewaypoint.com/)


-


[Waypoint template gallery](https://www.usewaypoint.com/templates)


-


[Waypoint template basics](https://www.usewaypoint.com/)
