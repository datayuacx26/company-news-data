---
schema_version: "1.0.0"
document_id: "4f65dde1802952214da60157ac3134f28216d4051fd4c0df93bfb7541cd58aa9"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/nuxt-keycloak-integration/"
published_at: "2025-10-10T10:00:00+00:00"
first_seen_at: "2026-07-24T11:21:12.712803+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:4402d44f7f9651a2d3a9edb35a3d2002b5068ce148e13d5523beb543193e8997"
---

# Nuxt & Keycloak: A Simple Guide to SSR Integration | BLUESHOE

Intermediate


## Table of Contents


- SSR, Hybrid, Generate - Authentication per Mode
- Option 1: Integration with nuxt-auth-utils - New and improved
- Option 2: Integration with @sidebase/nuxt-auth - Battle-tested and ready to use
- Conclusion: nuxt-auth-utils or @sidebase/nuxt


---


## Meet the Author


[Robert Stein](https://www.blueshoe.io/team/robert-stein/) Blueshoe uses Keycloak for authentication in many Nuxt applications. When selecting the right Nuxt module, we paid special attention to developer and user-friendliness.


Last updated:


2025-10-10


---


Nuxt


Vue.js


Keycloak


---


2025-10-10


# User Authentication for Nuxt Applications with Keycloak


Nuxt & Keycloak: A Simple Guide to SSR Integration


Integrating Keycloak into a Nuxt application for robust authentication can be a challenge, especially with regard to Server-Side Rendering (SSR). In this article, we compare two leading modules, nuxt-auth-utils and @sidebase/nuxt-auth, and provide a step-by-step guide to help you choose the right solution.


## Prerequisites


You should have the following knowledge to use the article optimally:


- [Keycloak](https://www.keycloak.org/) and[OAuth2](https://en.wikipedia.org/wiki/OAuth)
- [Nuxt Rendering Modes](https://nuxt.com/docs/4.x/guide/concepts/rendering)


If you have any questions or if anything is unclear, feel free to use the comment function below the article.


## SSR, Hybrid, Generate - Authentication per Mode


First, it's important to identify the mode in which the Nuxt application is running. This determines the different requirements for authentication. A blog article on the different[modes can be found here](https://www.blueshoe.io/blog/nuxt-generate-vs-server/) .


**Server-Side Rendering -** If user information such as permissions, names, etc., is processed during server-side rendering, the Nitro server needs access to it. Accordingly, the Nitro/Nuxt server must be able to validate whether a session is authenticated or authorized.


**Client-Side Rendering** - There is no server logic. Everything is processed on the client-side (the browser) - accordingly, the handling of user information/session also runs exclusively on the client-side.


**Hybrid-Rendering** - Depending on whether the SSR routes require user session information or not, the Keycloak integration must be considered.


## Option 1: Integration with nuxt-auth-utils - New and improved


Integrating Keycloak and Nuxt with[nuxt-auth-utils](https://github.com/atinux/nuxt-auth-utils) could hardly be simpler. A little configuration in` nuxt.config.js` and the basic framework is ready:


```text
runtimeConfig: {
oauth  : {
keycloak  : {
serverUrl  :   'https:  //keycloak.blueshoe.io',
realm:   'Blueshoe'  ,
clientId  :   'blueshoe-website'  ,
// clientSecret: '',
redirectURL  :   'https:  //blueshoe.io/auth/keycloak',
},
},
},
modules: [
'nuxt-auth-utils'  ,
]


```


Now, convenient[Composables](https://github.com/atinux/nuxt-auth-utils/?tab=readme-ov-file#user-session) are available:


```text
const   { loggedIn, user, session, fetch, clear, openInPopup }   =   useUserSession  ()


```


With this information, user-specific information can be rendered quickly and easily:


```text
<  template  >
<  span   v-if  =  "  loggedIn  "  >Hello, {{ user.firstName }} {{ user.lastName }} span  >
<  span   v-else  >Hello Guest span  >
template  >


```


Likewise,[useful utils are available](https://github.com/atinux/nuxt-auth-utils/?tab=readme-ov-file#session-management) on the SSR side. If a route absolutely requires a valid user session, this can be easily achieved with the following composable:


```text
const   session   =   await   requireUserSession  (event)


```


So far, so good - but how does the login work?


Through the environment variables, the nuxt-auth-utils module has all the information available to generate the login redirect and to use the session on the redirect back to the Nuxt application.


> 💡 **Note:** The` runtimeConfig` is not an environment variable itself, but the way Nuxt provides access to environment variables. More on this in the[Nuxt documentation](https://nuxt.com/docs/4.x/guide/going-further/runtime-config#environment-variables) .


For this, a server-side route is simply created under` server/auth/keycloak.get.ts` :


```text
export   default   defineOAuthKeycloakEventHandler  ({
async   onSuccess  (  event  , {   user   }) {
await   setUserSession  (event, {
user: {
keycloak: user.preferred_username,
},
loggedInAt: Date.  now  (),
})


return   sendRedirect  (event,   '/'  )
},
})


```


If you now call /auth/keycloak/, you will be automatically redirected to the running Keycloak instance and receive a session after a successful login.


The module is by[Sébastien Chopin](https://github.com/atinux) - the creator of Nuxt himself - and has a solid standing with (as of Sep 2025) 95,000 downloads per month, regular updates, and 1,400 stargazers, and can be recommended with confidence.


## Option 2: Integration with @sidebase/nuxt-auth - Battle-tested and ready to use


Another way to bring Nuxt and Keycloak together is the` @sidebase/nuxt-auth` package. With a few adjustments in` nuxt.config.js` , the module can be easily configured:


```text
runtimeConfig: {
public  : {
authOrigin  :   'http:  //localhost:3000',
},
},


modules: [
'@sidebase/nuxt-auth'  ,
],


auth: {
isEnabled  :   true  ,
disableServerSideAuth  :   false  ,
provider  : {
type  :   'authjs'  ,
trustHost  :   false  ,
defaultProvider  :   'keycloak'  ,
addDefaultCallbackUrl  :   true  ,
},
sessionRefresh  : {
enablePeriodically  :   true  ,
enableOnWindowFocus  :   true  ,
},
},


```


As the configuration already shows,` @sidebase/nuxt-auth` takes care of refreshing the session automatically. Keycloak exists as a pre-configured provider. We create the following file at` server/api/auth/\[...\].ts` and configure the provider:


```text
import   KeycloakProvider   from   'next-auth/providers/keycloak'
import   { NuxtAuthHandler }   from   '#auth'


export   default   NuxtAuthHandler  ({
secret:   'your-secret-here'  ,
providers: [
KeycloakProvider.  default  ({
clientId: process.env.KEYCLOAK_ID,
clientSecret: process.env.KEYCLOAK_SECRET,
issuer: process.env.KEYCLOAK_ISSUER,
})
]
})


```


It is important to note here that the URL to the Keycloak realm must be specified as the issuer:[https://my-keycloak-domain.com/realms/My_Realm](https://my-keycloak-domain.com/realms/My_Realm) .


Additionally, callbacks can be defined to react to various events:


```text
export   default   NuxtAuthHandler  ({
...
callbacks: {
/* on before signin */
async   signIn  ({   user  ,   account  ,   profile  ,   email  ,   credentials   }) {
return   true
},
/* on redirect to another url */
async   redirect  ({   url  ,   baseUrl   }) {
return   baseUrl
},
/* on session retrieval */
async   session  ({   session  ,   user  ,   token   }) {
return   session
},
/* on JWT token creation or mutation */
async   jwt  ({   token  ,   user  ,   account  ,   profile  ,   isNewUser   }) {
return   token
}
}
})


```


` @sidebase/nuxt-auth` automatically creates a page that provides the login and integrates with the given providers.


The user's data is then available in the application as follows:


```text
<  script   setup  >
const   {
status,
data,
lastRefreshedAt,
getCsrfToken,
getProviders,
getSession,
signIn,
signOut
}   =   useAuth  ()
script  >


```


The status indicates whether the user is authenticated or not.` data` contains user-specific data.


The module is SSR compatible and allows rendering different information based on the session.


` @sidebase/nuxt-auth` is developed by[sidebase](https://sidebase.io/) and has established itself as a reliable solution for authentication in Nuxt applications. With a well-established company behind it and regular updates, it offers a solid foundation for production applications and can be recommended with confidence.


## Conclusion: nuxt-auth-utils or @sidebase/nuxt


As is often the case, the devil is in the details of the use case. 😉 Both modules have a good track record regarding further development, the integration is simple, and the composables are very good.


If you want to customize the typical authentication pages yourself,` @sidebase/nuxt-auth` is more suitable. The module also comes with a configurable simple refresh logic for the user session.` nuxt-auth-utils` , on the other hand, "feels" a bit more lightweight. A[request](https://github.com/atinux/nuxt-auth-utils/issues/356) for automatic session refresh is still pending. However, this can usually be easily retrofitted yourself.


We can warmly recommend both modules and look forward to feedback on your experiences with Nuxt and Keycloak in the comments!


How did you "plug together" Nuxt and Keycloak? Are there better ways? What are your experiences with the modules?


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[Nuxt 4 is here: A Deep Dive into the New Features and What They Mean for Developers After months of speculation and intensive beta testing, the Nuxt team has officially released Nuxt 4. by Lukas Rüsche](https://www.blueshoe.io/blog/nuxt4-new-features/)


[The Solution for SSR Sliders with Nuxt We spent a long time searching for a sensible solution for implementing sliders with Nuxt. Many developers struggle with performance issues, hydration mismatches, and poor SEO performance with conventional slider libraries. In this comprehensive guide, we show you how we use Embla Carousel in Nuxt to achieve top SSR performance. by Lukas Rüsche](https://www.blueshoe.io/blog/nuxt-ssr-slider-top-performance/)


Intermediate


[Optimal Use of TailwindCSS in Nuxt 3 TailwindCSS is a high-performance solution for styling in Nuxt 3 projects. Learn how you optimally integrate it, create design systems, and maximize your Nuxt app's performance. We'll show you best practices for efficient usage. by Lukas Rüsche](https://www.blueshoe.io/blog/nuxt3-tailwindcss-best-practices/)


[Vue Vapor: Goodbye Virtual DOM, Hello Performance! Load times, complex component structures and unnecessary overhead — the Virtual DOM is reaching its limits. With Vue Vapor comes the answer: maximum performance, minimal JavaScript ballast — whether classic SPA or Server-Side Rendering (SSR). by Lukas Rüsche](https://www.blueshoe.io/blog/vue-vapor-performance-without-virtual-dom/)


[Avoiding Prop Drilling in Vue.js - How to Make It Work Sharing data between components in Vue.js everyday life – but what sounds simple quickly becomes messy. The reason: Prop Drilling. Here, you pass props through multiple components, even if they are only used at the end. This works – but is anything but elegant. If you want to avoid prop drilling in Vue, you're in the right place. We'll show you best practices and tools like provide/inject and State Management with Pinia to keep your app clean, maintainable, and scalable. by Lukas Rüsche](https://www.blueshoe.io/blog/vuejs-avoiding-prop-drilling/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
