---
schema_version: "1.0.0"
document_id: "54f61f71d4f57559462a647c89ac6db63e2e98082e97a4d2c10e5376a03c5936"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/attachments/"
published_at: "2025-05-15T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:f9cf000a5afb407cb5970466a2d1daed6fdb7e408d72da7da086d3e2a5c77bec"
---

# Attachments

[Attachments](https://www.usewaypoint.com/docs/sending-with-attachments) have arrived on Waypoint.


Attachments have been one of our most requested features. After months of beta testing, we’re excited to make them generally available to all Waypoint customers.


You can now send emails with two types of attachments:


1. **[Standard attachments](https://www.usewaypoint.com/docs/sending-with-attachments/#using-standard-attachments)** – Files included with an email but not shown in the message body (for example, PDFs or spreadsheets).
2. **[Inline attachments](https://www.usewaypoint.com/docs/sending-with-attachments/#using-inline-attachments)** – Files, usually images, that are embedded directly within the email content.


Both types are added by including an` attachments` object in your API request. Example below:


```text
const   fs   =   require  (  '  fs  '  );
// Read and encode the file to base64    const   filePath   =   '  ./order-receipt.pdf  '  ;    const   fileContent   =   fs  .  readFileSync  (  filePath  );     const   base64Content   =   fileContent  .  toString  (  '  base64  '  );
// Send the email with attachment    const   authHeader   =        '  Basic   '   +   Buffer  .  from  (  `  ${  API_KEY_USERNAME  }  :  ${  API_KEY_PASSWORD  }  `  )  .  toString  (  '  base64  '  );
const   options   = {         method:   '  POST  '  ,         headers: {          '  Content-Type  '  :   '  application/json  '  ,           Authorization:   authHeader  ,         },         body:   JSON  .  stringify  (  {           templateId:   '  wptemplate_ABc123XYZ  '  ,           to:   '  jordan@usewaypoint.com  '  ,           variables: {             user: {               displayName:   '  Jordan  '  ,             },             product: {               title:   '  Beechers Mac & Cheese  '  ,               id:   '  02934203942  '  ,             },           },           attachments:   [             {               name:   '  order-receipt.pdf  '  ,               contentBlob:   base64Content  ,               contentType:   '  application/pdf  '  ,             },           ]  ,        }  )  ,    }  ;
fetch  (  '  https://live.waypointapi.com/v1/email_messages  '  ,   options  )         .  then  (  (  res  )     =>     res  .  json  ())        .  then  (  (  res  )     =>     console  .  log  (  res  ))         .  catch  (  (  err  )     =>     console  .  error  (  err  ));
```


Additionally, within your activity, messages sent with one or many attachments, will show the attachment name underneath the message preview.
