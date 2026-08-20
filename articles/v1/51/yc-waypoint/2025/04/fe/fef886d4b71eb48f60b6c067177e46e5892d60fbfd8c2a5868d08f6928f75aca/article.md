---
schema_version: "1.0.0"
document_id: "fef886d4b71eb48f60b6c067177e46e5892d60fbfd8c2a5868d08f6928f75aca"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/email-message-metadata/"
published_at: "2025-04-17T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:f113124b0bdec3b89f18998f523ee0846556ff25491b8eb46be733e673de4713"
---

# Email message metadata

A small but mighty improvement for dev teams –` metadata` is now supported on Waypoint email messages. This allows teams to attach additional context for internal use and reference.


For example, when[sending an email](https://www.usewaypoint.com/docs/api-reference/#send-an-email-with-a-template) API


via the API, you might want to include extra context. In the snippet below, we’re passing in a` customerId` ,` companyId` , and` transactionId` :


POST


` v1/email_messages`


Terminal window


```text
curl     "  https://live.waypointapi.com/v1/email_messages  "     \        -H     "  Content-Type: application/json  "     \        -u     "  API_KEY_USERNAME:API_KEY_PASSWORD  "      \        -d     '  {           "to": "jordan@usewaypoint.com",           "templateId": "wptemplate_tuVKkNrLgZfM6KyQ",           "variables": {             "displayName": "Jordan"           },           "metadata": {              "customerId": "customer_10928q3093840aap",              "companyId": "company_sdafljaw09330r36",              "transactionId": "transaction_a20394a339aa390a"            }          }  '
```


This` metadata` isn’t just available when[fetching individual emails](https://www.usewaypoint.com/docs/api-reference/#retrieve-an-email) API


— it’s also included visible on[message detail pages](https://www.usewaypoint.com/changelog/email-message-metadata/docs/email-event-logs/) in your dashboard.


Additionally, message` metadata` is also accessible from[webhook payloads](https://www.usewaypoint.com/docs/webhooks) . Example:


```text
{        "createdAt"  :   "  2025-04-21T14:03:01.357Z  "  ,        "emailMessage"  : {          ...          "metadata"  : {            "customerId"  :   "  customer_10928q3093840aap  "  ,            "companyId"  :   "  company_sdafljaw09330r36  "  ,            "transactionId"  :   "  transaction_a20394a339aa390a  "           }         },        "emailMessageId"  :   "  em_wUpG2hHHvfncz8EY  "  ,        "id"  :   "  log_vJayu2MEA7ZZXTuQ  "  ,        "message"  :   "  Email successfully delivered to jordan@usewaypoint.com. Delivery time: 0.2 seconds.  "  ,        "updatedAt"  :   "  2025-04-21T14:03:01.357Z  "    }
```
