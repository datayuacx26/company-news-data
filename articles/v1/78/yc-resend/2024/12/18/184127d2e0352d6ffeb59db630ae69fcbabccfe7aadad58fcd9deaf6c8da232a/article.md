---
schema_version: "1.0.0"
document_id: "184127d2e0352d6ffeb59db630ae69fcbabccfe7aadad58fcd9deaf6c8da232a"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/design-engineering-an-x-component"
published_at: "2024-12-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:56cd40e41f40a710d3ca7738fccff27180594bd0817673148c767bffe730fec3"
---

# Design Engineering an X Component

Both development and design share a common task: solving problems. As a Design Engineer, I often combine tools from both worlds to solve problems for our customers. Let's explore the process of adding a new component to Broadcasts,[our no-code editor](https://resend.com/features/broadcasts) , from challenge to solution.


I'll expose my research, the thought process for developing the component, and the challenges and lessons I learned along the way.


## Possibilities and Limitations


As with everything we create, we want the Broadcast editor to be second to none. The editing experience should be easy, intuitive, and as simple as writing a document.


Emails may benefit from embedded media like videos, social posts, and third-party integrations, but this media is often dynamic, while email is static.


## Technical Challenge


As one example, for nearly two decades, 𝕏 (formerly Twitter) has played a key role in online communication. As a result, it has become a common practice to embed 𝕏 posts.


Since email is static, we faced the challenge of embedding something dynamic (an 𝕏 post). Given that email-safe HTML cannot embed an 𝕏 post, we needed to find a way to convert any post to an email-friendly form.


𝕏 post


→ **Email**


Dynamic


→ **Static**


## Identifying a Solution


### Stage 1: Research


We began exploring possible options and dismissed the following:


1. **Render a post-like component using email-safe HTML** : While possible, building a component using email-safe HTML (i.e., tables, etc.) introduces unnecessary complexity.
2. **Capture a screenshot using JavaScript** : Using a headless browser with Puppeteer or some other library, we could render an 𝕏 post using JavaScript to capture it in a screenshot. Rendering in a web engine is less reliable and requires more complexity and resource usage.
3. **Text-only render** : Rending a text-only 𝕏 post wouldn't meet user expectations since it provides a degraded experience only slightly better than copy-and-paste.


### Stage 2: Solution


Luckily, we found the solution using three excellent tools made by Vercel. In short, we use 𝕏 post data and dynamically create an image via a Next.js function for use in the Broadcast editor.


The three tools we used were:


1. **React Tweet** : We were inspired by how[react-tweet](https://github.com/vercel/react-tweet) addresses the problem of rendering posts outside X's website and utilizes many of its features. These include fetching post content, layout designs, and parsing, all of which make our work easier.
2. **Next.js ImageResponse** : Next.js introduces the ability to easily generate dynamic images using JSX and CSS with the[ImageResponse](https://nextjs.org/docs/app/api-reference/functions/image-response) constructor.
3. **Satori lib** : Satori is a library used by Next.js` ImageResponse` to create images using JSX syntax. It renders our custom post UI to an SVG. The Next.js response will then capture the rendering and return a JPG.


Finally, we save the rendered JPG into the static host (S3 bucket) and embed it into the email HTML body, wrapping it with an anchor tag directed to the 𝕏 post.


**What does it look like?**


```text
// app/api/twitter/post.ts
import     {   ImageResponse   }     from     'next/og'  ;     import     {   getTweet   }     from     'react-tweet/api'  ;
export     async     function     GET  (  request  :   Request  )     {        const     {   searchParams   }     =     new     URL  (  request  .  url  )  ;        const   tweetId   =   searchParams  .  get  (  'id'  )  ;
const   data   =     await     getTweet  (  tweetId  )
return     new     ImageResponse  (           <  div style  =  {  {     ...     }  }  >              {  data  .  user  .  name  }           <  /  div  >  ,           {             width  :     600  ,             height  :     300           }        )     }
```


## Key Challenges


The journey to a solution included several challenges and edge cases. These issues allowed the output to be more polished and visually closer to an 𝕏 post layout. There were a few key challenges:


### 1. Image Dimensions


The[ImageResponse constructor](https://nextjs.org/docs/app/api-reference/functions/image-response) receives a` ReactElement` and an` options` object, which allows you to set the width and height of the image.


```text
import     {   ImageResponse   }     from     'next/og'
new     ImageResponse  (        element  :     <  >     ...     <  /  >  ,        options  :     {          width  :     1200  ,          height  :     630        }     )
```


Since the constructor requires passing both the element and the dimensions simultaneously, it produces a sequencing challenge, as we need to know the dimensions of the rendered image *before* it is rendered.


Thankfully, we found a way to predict the dimension of the image by:


- Setting a fixed height for all known elements (e.g., profile, name, images, etc.)
- Calculating the text height based on the number of characters


We calculate the final dimensions of the image by adding the dimensions of these elements beforehand. We then pass these calculated dimensions to the` ImageResponse` and the JSX to render the image.


### 2. Dark/light theme


Due to its special use case, the Satori library used by Next.js` ImageResponse` supports a limited subset of HTML and CSS. For one, it does not support css variables for switching themes. Additionally, Satori requires styling all elements with the style attribute.


We felt it was important to provide light and dark themes for maximum flexibility in the editor to match the users' content theme. We created a custom theme object to switch between themes from the Broadcast quickly.


```text
const     {   searchParams   }     =     new     URL  (  request  .  url  )  ;     const   currentTheme   =   searchParams  .  get  (  'theme'  )  ;
const     THEMES     =     {        light  :     {          background  :     'white'  ,          text  :     '#0f1419'  ,        }  ,        dark  :     {          background  :     '#15202b'  ,          text  :     '#f7f9f9'  ,        }  ,     }  ;
<  div style  =  {  {     backgroundColor  :     THEMES  [  currentTheme  ]     }  }  >     ...     <  /  div  >
```


We faced various other challenges, like supporting different screen densities, styling mentions, hashtags, links, and more, but we couldn't be happier with the final result.


## Building the best editor


Sending professional, modern emails is crucial for our customers. A modern email editor should make it easy to stand out from the crowd and communicate in various formats.


We want the editor to get out of the way so you can focus on what matters most to you: connecting with your readers. We're striving to make it easy to create engaging emails, communicate your brand, and bring value to your readers.


In the future, we want to support all possible post variations, including reposts, responses, and more.


We are confident the lessons we learned while creating this component will continue to benefit you as we develop additional media types, including posts, videos, and more on other social media platforms.
