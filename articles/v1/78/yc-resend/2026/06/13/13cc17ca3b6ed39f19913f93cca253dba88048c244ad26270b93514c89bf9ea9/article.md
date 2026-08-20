---
schema_version: "1.0.0"
document_id: "13cc17ca3b6ed39f19913f93cca253dba88048c244ad26270b93514c89bf9ea9"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/editor-emoji-picker"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:8ee89d14637ced6c3073322bffc2d0895c17eff0e1ff9b03b8b22572fec04084"
---

# Editor Emoji Picker

One of our engineers recently asked if we could add emojis to emails built in our editors.


Since our editor is extensible, we were able to add an emoji picker to our[Broadcast](https://resend.com/features/broadcasts) and[Template](https://resend.com/features/templates) email editors.


## How to use


Type` :` followed by an emoji name to add any emoji from a searchable list.


## Build your own email editor


We open sourced our[React Email editor](https://resend.com/blog/react-email-6) so you can build custom experiences in your own applications. Importantly, the editor includes a default core that is[extensible via custom components](https://react.email/docs/editor/advanced/extensions) .


We've used this same architecture to add our emoji extension.


The composable API exposes` EmailNode` so you can build any custom block your users need: uploading images to a CDN, embedding social posts, rendering charts inline in an email. Each custom node defines both its HTML representation and its React Email output via` renderToReactEmail` .


```text
import     {     EmailNode     }     from     '@react-email/editor/core'  ;     import     {   mergeAttributes   }     from     '@tiptap/core'  ;
const     Callout     =     EmailNode  .  create  (  {      name  :     'callout'  ,      group  :     'block'  ,      content  :     'inline*'  ,
parseHTML  (  )     {          return     [  {   tag  :     'div[data-callout]'     }  ]  ;        }  ,
renderHTML  (  {     HTMLAttributes     }  )     {          return     [            'div'  ,            mergeAttributes  (  HTMLAttributes  ,     {     'data-callout'  :     ''     }  )  ,            0  ,          ]  ;        }  ,
renderToReactEmail  (  {   children  ,   style   }  )     {          return     (            <  div     style  =  {  {     ...  style  ,   padding  :     '12px 16px'  ,   backgroundColor  :     '#f4f4f5'     }  }  >              {  children  }            </  div  >          )  ;        }  ,     }  )  ;
```


For more help, view[editor examples](https://react.email/editor/examples) or read the[full editor documentation](https://react.email/docs/editor/overview) .
