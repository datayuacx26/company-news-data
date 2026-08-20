---
schema_version: "1.0.0"
document_id: "7606f12bf03d562738ec76b09759956af38fd81daf86df60def56e79d6fed5b9"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/handling-attachment-restrictions-gracefully/ba-p/3260026"
published_at: "2025-05-11T22:48:42+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:3ac36361b300560bb7962b1a26d65cdd475bb7c7d7c8fa520ae55c1fca2278bb"
---

# Handling Attachment restrictions gracefully

Service Bridge on Remote Task definitions can be configured to send attachments between tasks, based on the following settings.


[https://www.servicenow.com/docs/bundle/yokohama-service-bridge/page/product/tmt-service-bridge-2/tas...](https://www.servicenow.com/docs/bundle/yokohama-service-bridge/page/product/tmt-service-bridge-2/task/service-bridge-v2-create-remote-tasks-defs.html)


However, at both the Consumer and Provider end, there are attachment file restrictions, which could differ between based on security settings of the instance, namely in the system property 'glide.attachment.extensions', which are :List of file extensions (comma-separated) that can be attached to documents via the attachment dialog. Extensions should not include the dot (.) e.g. xls,xlsx,doc,docx. Leave blank to allow all extensions.


I have found the Sub-Flow 'Push Attachments to Remote System' that does not handle a status code, say a 400 error when either the Provider or Consumer does not allow the attachment to go through, and provide a comment or error back on the Task where it was inserted.


So this Sub-Flow has been extended with an action, to comment on the task when we get a 400 error.


Additional, errors could handled in similar ways.


This has only been tested when sending an attachment from Consumer to Producer.


The pseudocode is as follows:


- Look up table & sys_id of attachment - which should be 'sn_transport_queue'


- Then gather the from the 'sn_transport_queue' payload the table_name and sys_id of the task


- Create a comment on the task, checking first that the comment doesn't exist


The additional action, script steps


```text
(  function   execute(inputs, outputs) {      // Find table_name and sys_id     var   attachmentGR =   new    GlideRecord  (  'sys_attachment'  );     attachmentGR.  get  (inputs.attachment_id);     // now get payload     var   tqGR =   new    GlideRecord  (attachmentGR.getValue(  'table_name'  ));     if   (tqGR.  get  (attachmentGR.getValue(  'table_sys_id'  )) ){        // now get Remote Task Parent to put comment in        var   tqPayload =   JSON  .parse(tqGR.getValue(  'payload'  ));        var   rtGR =   new    GlideRecord  (tqPayload.changes.adds[  0  ].table_name);        if   (rtGR.  get  (tqPayload.changes.adds[  0  ].table_sys_id)) {           // now get Parent            var   taskGR =   new    GlideRecord  (  'task'  );           if   (taskGR.  get  (rtGR.getValue(  'parent'  ))){              var   commentString =   'Attachment '   + attachmentGR.getValue(  'file_name'  ) +   ' could not be transported due to status code : '   + inputs.status_code;              if   (!taskGR.comments.getJournalEntry(  1  ).match(commentString)){                 taskGR.comments=  'Attachment '   + attachmentGR.getValue(  'file_name'  ) +   ' could not be transported due to status code : '   + inputs.status_code;                 taskGR.update();              }           }        }     }    })(inputs, outputs);
```
