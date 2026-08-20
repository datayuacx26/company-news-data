---
schema_version: "1.0.0"
document_id: "d36e702ee46a3a120e90eda252090989701b1cae852e1df90c5ffc767143e7c0"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/react-email-6"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:b34b67e698b8e88855250524002b077248fbde10f42b16e580059e391a23b5a8"
---

# React Email 6.0

We're excited to announce[React Email 6.0](https://react.email/) , featuring:


- **Open-Source Editor**
- **Build your own Extensions**
- **New Templates**
- **Unified Package**


Get started today or check theupgrade instructions below.


```text
npm   i react-email@latest @react-email/ui@latest
```


React Email now has **2M weekly downloads** on[npm](https://www.npmjs.com/package/react-email) , that is a **108% increase** since the last major release 5 months ago.


We would like to thank all the **[196 open source contributors](https://github.com/resend/react-email/graphs/contributors)** who made this possible.


## Open-Source Editor


The centerpiece of React Email 6 is a **new open-source visual editor** , available as a standalone package.


```text
npm   i @react-email/editor
```


You can embed the editor directly in your app with a few lines of code.


```text
import     {     EmailEditor     }     from     '@react-email/editor'
export     default     function     MyEditor  (  )     {        return     <  EmailEditor     />     }
```


The editor outputs semantically correct, email-ready HTML that renders correctly across every major inbox provider.


## Editor Architecture


Built on the fundamental learnings from React Email, the editor is designed to be easy to use, extensible, and flexible.


The editor is built in two layers.


1. **Core** works out of the box with no configuration.
2. **Extensions** are custom features you can build using the composable API.


This architecture allows the core to remain simple and focused, while providing a composable API for building your own extensions.


To style the editor, import a default theme to get started quickly.


```text
import     '@react-email/editor/themes/default.css'
```


You can also build your own theme to style the editor to look and feel like your own app.


## Build your own Extensions


The composable API exposes` EmailNode` so you can build any custom block your users need: uploading images to a CDN, embedding social posts, rendering charts inline in an email.


The opportunities are as endless as your imagination.


Each custom node defines both its HTML representation and its React Email output via` renderToReactEmail` .


```text
import     {     EmailNode     }     from     '@react-email/editor/core'  ;     import     {   mergeAttributes   }     from     '@tiptap/core'  ;
const     Callout     =     EmailNode  .  create  (  {      name  :     'callout'  ,      group  :     'block'  ,      content  :     'inline*'  ,
parseHTML  (  )     {          return     [  {   tag  :     'div[data-callout]'     }  ]  ;        }  ,
renderHTML  (  {     HTMLAttributes     }  )     {          return     [            'div'  ,            mergeAttributes  (  HTMLAttributes  ,     {     'data-callout'  :     ''     }  )  ,            0  ,          ]  ;        }  ,
renderToReactEmail  (  {   children  ,   style   }  )     {          return     (            <  div     style  =  {  {     ...  style  ,   padding  :     '12px 16px'  ,   backgroundColor  :     '#f4f4f5'     }  }  >              {  children  }            </  div  >          )  ;        }  ,     }  )  ;
```


For more help, view[editor examples](https://react.email/editor/examples) or read the[full editor documentation](https://react.email/docs/editor/overview) .


## New Templates


React Email 6 also ships with a new collection of templates created by our friends at[Character Studio](https://character.studio/) for common use cases, including authentication flows and e-commerce sequences.


Use them as a starting point, or pull individual sections into your own templates. The templates are available as[React Email templates](https://demo.react.email/) or as Figma files ([Authentication templates](https://www.figma.com/community/file/1626682167713928769) ,[E-commerce templates](https://www.figma.com/community/file/1626680546446620209) ).


## Unified Package


To simplify the development experience, we've unified all React Email components into a single package. This means you can now import everything you need from` react-email` without worrying about multiple packages.


```text
import     {     Button  ,     Container  ,     Html  ,     Heading  ,     Tailwind     }     from     'react-email'
```


This does not include the Editor, which needs to be installed separately.


We've also moved what was` @react-email/preview-server` into what's now` @react-email/ui` .


## Upgrade instructions


Here's how to update from React Email 5.0 to 6.0.


**A) Remove the old packages:**


```text
npm     rm   @react-email/components @react-email/preview-server
```


**B) Install the new packages:**


```text
npm   i react-email@latest @react-email/ui@latest
```


**C) Update your imports:**


Instead of importing from` @react-email/components` :


```text
import     {     Button  ,     Html  ,     Head  ,   render   }     from     "@react-email/components"  ;
```


You can now import from` react-email` :


```text
import     {     Button  ,     Html  ,     Head  ,   render   }     from     "react-email"  ;
```


For a thorough explanation, see the[updating guide](https://react.email/docs/getting-started/updating-react-email#update-from-react-email-5-0-to-6-0) .


## Conclusion


We're excited to see the new possibilities that the open-source editor and the new templates will bring to the community.


If you have any questions, feel free to reach out to us on[GitHub](https://github.com/resend/react-email) .
