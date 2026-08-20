---
schema_version: "1.0.0"
document_id: "ea97cf32cf62e7f9d63e585842fc54ff42abd38a0f6e54cd453846aa97a6c178"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-add-code-blocks-and-markdown-to-the-framer-cms/"
published_at: "2023-04-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:99a3b7d3866bbbaf35e6b03eb3a94934e095588abf86cd9e15086e1a67b37204"
---

# How to add Code blocks & Markdown to the Framer CMS

We recently made the switch from Webflow to Framer for the Basedash website. A big reason for that switch was the need to have a better CMS for our blog content. We write a ton of technical posts here and use Notion to compose and collaborate around them. We also use Notion for tracking our goals, taking notes, capturing user interviews, and we even have our[docs](https://www.basedash.com/docs) in Notion running on a[Super](https://super.so/) site so that we can keep it up-to-date without much effort. There may be better authoring experiences out there, but we don’t want to add more tools to our stack if we don’t have to.


There also may be more specific CMSs out there, that are made for technical posts, but I wanted to make sure that our site was unified with our Marketing content and had as much flexibility as possible, so tools that didn’t work with Framer were out of the running.


## The old workflow in Webflow


Previously in Webflow, we could copy the page content over to their CMS, fill in the fields, but a lot of the rich content like callouts, code blocks, inline code, and media embeds didn’t work well. I’d have to manually copy each image from Notion, and then upload it to the Webflow CMS, add custom code blocks for each code snippet, add opening and closing tags to those snippets so they’d render, and then find any` in-line snippets` and bold them so that our CMS block component would make them` look like this` . It was a major pain. Probably about 30 minutes per blog post.


Also, then end result wasn’t great, even with all of that manual work. Code embeds were iffy, rich content like tweets and external embeds were hard to control, and, if I’m being honest, the act of editing content itself in the Webflow CMS was totally frustrating. Notion and newer tools have spoiled me with / menus, keyboard shortcuts and dropping the need to save my content constantly.


Here’s a tweet showing what that looked like:


[https://twitter.com/tomjohndesign/status/1603827393558355969?s=20](https://twitter.com/tomjohndesign/status/1603827393558355969?s=20)


I loved the ease of editing data in the Framer CMS, the fact that I didn’t have to kick my teammates out when I needed to work, but the Framer rich text component didn’t give me everything I wanted.


The best part about Framer though, is that I’m not limited to their components. We can build our own.


## How it’s set up in Framer


So, we decided to write a custom markdown component using[react-markdown](https://github.com/remarkjs/react-markdown) , and make the technical post component we’ve always wanted without relying on the CMS or site builder we were using to add support for features we want. This way we can improve it over time as our blog needs change. The component then renders that content without even needing to use the built in rich text block that Framer provides.


Here’s what that CMS collection looks like in Framer:


[Framer Setup.mp4](https://www.basedash.com/blog-assets/how-to-add-code-blocks-and-markdown-to-the-framer-cms/Framer_Setup.mp4)


Basically, if you can’t watch the video I made, we export the content from Notion (more on that later) and add it to a` plan text` field in Framer called` post` .


Then we paste the markdown into that post and add all of the other content like SEO tags, image, image alt text, description, etc. There’s no automated way I’ve found to do this, but overall it’s down to about a minute from ready to publish → live.


Also, because we’re authoring the content inside of Notion, we use this AI based prompt to generate the summary (for the list page), SEO title tag, and a meta description for each. Also a huge time saver. I’m sure other tools will add things like this over time, but Notion has it now and so do we.


### Notion AI prompt:


```text
Give me a   200   character summary, a title tag for   SEO  , and a meta description for   SEO   of   this   blog post.
```


And without further ado, here’s the full Markdown code we are using for the blog content:


```text
import   React, { useState }   from   "react"
import   ReactMarkdown   from   "https://esm.sh/react-markdown@7"
import   { Prism   as   SyntaxHighlighter }   from   "https://esm.sh/react-syntax-highlighter@15"
import   remarkGfm   from   "https://esm.sh/remark-gfm@3"
import   TweetEmbed   from   "https://esm.sh/react-tweet-embed"
import   ReactDom   from   "react-dom"
import   { addPropertyControls, ControlType }   from   "framer"


// Available styles: https://github.com/react-syntax-highlighter/react-syntax-highlighter/blob/master/AVAILABLE_STYLES_PRISM.MD
import   { atomDark   as   style }   from   "https://esm.sh/react-syntax-highlighter@15/dist/esm/styles/prism"


export   default   function   Markdown  (  props  ) {
return   (
<  ReactMarkdown
children  =  {props.content}
className  =  "blog-post"
remarkPlugins  =  {[remarkGfm]}
components  =  {{
h1  (  props  ) {
return   (
<  h1
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   40  ,
marginTop:   "3rem"  ,
marginBottom:   "0.5rem"  ,
}}
>
{props.children}
</  h1  >
)
},
h2  (  props  ) {
return   (
<  h2
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   28  ,
marginTop:   "2rem"  ,
marginBottom:   "0.5rem"  ,
}}
>
{props.children}
</  h2  >
)
},
h3  (  props  ) {
return   (
<  h3
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   20  ,
marginTop:   "1rem"  ,
marginBottom:   "0.5rem"  ,
}}
>
{props.children}
</  h3  >
)
},
p  (  props  ) {
return   (
<  p
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   16  ,
margin:   "21px 0"  ,
lineHeight:   1.5  ,
}}
>
{props.children}
</  p  >
)
},
ul  (  props  ) {
return   (
<  ul
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   16  ,
margin:   "21px 0"  ,
lineHeight:   1.5  ,
}}
>
{props.children}
</  ul  >
)
},
ol  (  props  ) {
return   (
<  ol
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   16  ,
margin:   "21px 0"  ,
lineHeight:   1.5  ,
}}
>
{props.children}
</  ol  >
)
},
a  (  props  ) {
if   (
props.href.  match  (
/  ^  https  ?  :  \/\/  twitter  .  com  \/  (  [a-zA-Z0-9_]  +  )  \/  (d  +  )  /  g
)
) {
const   tweetId   =   props.href.  split  (  "/"  ).  at  (  -  1  )
return   <  TweetEmbed   tweetId  =  {tweetId} />
}


return   (
<  a
href  =  {props.href}
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   16  ,
color:   "#3a50fc"  ,
textDecoration:   "underline"  ,
}}
>
{props.children}
</  a  >
)
},
li  (  props  ) {
return   (
<  li
style  =  {{
fontFamily:   "Inter"  ,
fontSize:   16  ,
}}
>
{props.children}
</  li  >
)
},
img  (  props  ) {
return   (
<  img
className  =  "blog-post-image"
style  =  {{
display:   "block"  ,
margin:   "0 auto"  ,
maxWidth:   "100%"  ,
}}
{  ...  props}
/>
)
},
blockquote  (  props  ) {
return   (
<  blockquote
style  =  {{
margin:   0  ,
marginBottom:   "1rem"  ,
fontSize:   "12px"  ,
background:   "rgba(21, 21, 21, 0.04)"  ,
padding:   "1px 1rem "  ,   // Non-zero vertical padding is a hack
borderLeft:   "1px dotted #A3A3A3"  ,
}}
>
{props.children}
</  blockquote  >
)
},
code  ({   node  ,   inline  ,   className  ,   children  ,   ...  props   }) {
const   match   =   /  language-(  \w  +  )  /  .  exec  (className   ||   ""  )
const   codeToCopy   =   String  (children).  replace  (  /  \n  $  /  ,   ""  )
const   [  copySuccess  ,   setCopySuccess  ]   =   useState  (  false  )
function   handleCopy  () {
navigator.clipboard.  writeText  (children)
setCopySuccess  (  true  )
setTimeout  (()   =>   {
setCopySuccess  (  false  )
},   1500  )
}


return   !  inline   &&   match   ?   (
<  div   style  =  {{ position:   "relative"   }}>
<  button
style  =  {{
position:   "absolute"  ,
top:   "8px"  ,
right:   "8px"  ,
background:   "#151515"  ,
border:   "1px solid rgba(255, 255, 255, 0.15)"  ,
borderRadius:   "4px"  ,
color:   "white"  ,
fontFamily:   "Inter"  ,
fontSize:   "14px"  ,
padding:   "4px 8px 4px 24px"  ,
cursor:   "pointer"  ,
transition:
"background-color 50ms ease-in-out"  ,
}}
onClick  =  {handleCopy}
>
<  svg
width  =  "15"
height  =  "15"
viewBox  =  "0 0 15 15"
fill  =  "none"
xmlns  =  "http://www.w3.org/2000/svg"
style  =  {{
position:   "absolute"  ,
left:   "6px"  ,
top:   "5px"  ,
}}
>
<  g   opacity  =  "0.6"  >
<  path
fillRule  =  "evenodd"
clipRule  =  "evenodd"
d  =  "M2 2.5C2 2.22386 2.22386 2 2.5 2H9.5C9.77614 2 10 2.22386 10 2.5V9.5C10 9.77614 9.77614 10 9.5 10H2.5C2.22386 10 2 9.77614 2 9.5V2.5ZM3 3V9H9V3H3ZM11 5.5C11 5.22386 11.2239 5 11.5 5H12.5C12.7761 5 13 5.22386 13 5.5V12.5C13 12.7761 12.7761 13 12.5 13H5.5C5.22386 13 5 12.7761 5 12.5V11.5C5 11.2239 5.22386 11 5.5 11C5.77614 11 6 11.2239 6 11.5V12H12V6H11.5C11.2239 6 11 5.77614 11 5.5Z"
fill  =  "#ffffff"
/>
</  g  >
</  svg  >
{copySuccess   ?   "Text copied!"   :   "Copy text"  }
</  button  >
<  SyntaxHighlighter
children  =  {codeToCopy}
style  =  {style}
language  =  {match[  1  ]}
PreTag  =  "div"
customStyle  =  {{
borderRadius:   "10px"  ,
background:   "#151515"  ,
fontWeight:   "bold"  ,
}}
{  ...  props}
/>
</  div  >
)   :   (
<  code
className  =  {className}
{  ...  props}
style  =  {{
background:   "#ffe9df"  ,
borderRadius:   "4px"  ,
padding:   "1px 4px"  ,
color:   "#cf521c"  ,
fontSize:   "14px"  ,
fontFamily:
"'Fira Code', 'Fira Mono', Menlo, Consolas, 'DejaVu Sans Mono', monospace"  ,
border:   "1px solid #fcc9b3"  ,
}}
>
{children}
</  code  >
)
},
}}
/>
)
}


Markdown.defaultProps   =   {
content:   `# Markdown component


[Here's a link](https://new.basedash.com)


[Here's Max's Twitter profile](https://twitter.com/MaxMusing)


\`  [Here's a code link 1](https://new.basedash.com)  \`
[  \`  Here's a code link 2  \`  ](https://new.basedash.com)


> 💡 This post is the first part of a series talking about my experience applying to and taking part in the Sumer 2020 batch of Y Combinator with my company, Basedash. Other posts in the series dive deeper into the YC interview, whether YC is worth it, how to take advantage of the batch, and the post-batch experience.


### Header


Here's a Twitter embed:


[https://twitter.com/MaxMusing/status/1600616312165863424](https://twitter.com/MaxMusing/status/1600616312165863424)


Here's a code block for the   \`  add  \`   function:
\`\`\`  js
function add(a, b) {
return a + b;
}
\`\`\`  `  ,
}


addPropertyControls  (Markdown, {
content: {
title:   "Content"  ,
type: ControlType.String,
},
})
```


To use this code, create a custom code component in Framer and then paste it in. Our blog uses some custom fonts, but I’ve replaced those with` Inter` so you should be good to go.


## Getting content out of Notion


The tricky part about writing content in Notion is the images. Notion hosts all embedded content on their servers, and doesn’t have any way to host them elsewhere. Because of this, if you just copy and paste any image from Notion to Framer, it’ll break. Public Notion pages use a public url, but exported Markdown always has the internal url which can’t be viewed by a non-authenticated user.


So, we had to figure out a quick way to get the Notion images hosted elsewhere.


When you export a Notion page as markdown, if there’s images or other content, it will include those in a separate folder. Here’s this post’s folder structure as an example:


I can manually upload those files somewhere, but the easiest way to do this turned out to be writing a Raycast script to do it for me.


Here’s how we export the content and paste it into Framer:


[https://twitter.com/tomjohndesign/status/1633292115449978882?s=20](https://twitter.com/tomjohndesign/status/1633292115449978882?s=20)


Here’s the[Raycast](https://www.raycast.com/) extension that we made to help with this:


[https://github.com/Basedash/raycast-markdown-image-upload](https://github.com/Basedash/raycast-markdown-image-upload)


To use this extension, you’ll have to add your own credentials to your own S3 bucket, but if you have any issues or questions about it, feel free to send a tweet our way.


Here’s how to to install it to[Raycast](https://www.raycast.com/) :


[RaycastExtension.mp4](https://www.basedash.com/blog-assets/how-to-add-code-blocks-and-markdown-to-the-framer-cms/RaycastExtension.mp4)


After you have the extension installed and pointing at your S3 bucket, you just need to copy the pathname of the root Markdown file (by holding opt+right click), and then running the extension and pasting it in.


After this, run the script and it will upload all of the images to the S3 bucket, and update all of the links in the markdown file to point to the new urls. Crazy fast.


Now that the content is formatted properly, you can paste that markdown content into Framer.


Hope this is helpful, be sure to let us know if you have any questions by tweeting at Basedash or Me and we’ll be sure to lend a hand.
