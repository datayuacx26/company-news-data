---
schema_version: "1.0.0"
document_id: "129e3dc8b8df7575f9f697e255ed73202eac01c1512a83d3db702265c5598c3e"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/boring-stack-v-1-5-0/"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:96ee15acf107157430d6cef9464f2194a3640227e2d5c62947b233c83508ca8c"
---

# The Boring Stack 1.5.0

The[Boring JavaScript Stack](https://docs.sailscasts.com/boring-stack/) 1.5.0 is a stack release.


It is not only an[inertia-sails](https://docs.sailscasts.com/inertia-sails/) release. It is the release where the[Sails](https://sailsjs.com/) +[Inertia](https://inertiajs.com/) story gets sharper across the adapter, the templates,[Shipwright](https://github.com/sailshq/sails-hook-shipwright) , testing, documentation, and the developer experience we want every Boring Stack app to inherit.


The last Boring Stack release was[v1.2.3](https://github.com/sailscastshq/boring-stack/releases/tag/v1.2.3) . Since then we have moved the stack onto[stable Inertia v3](https://inertiajs.com/docs/v3/core-concepts/the-protocol) , added the missing protocol metadata, built a dedicated Rsbuild plugin for Inertia apps, added selective server-side rendering, shipped[Inertia Precognition](https://laravel.com/docs/precognition) for live server-side validation, moved template tests toward[Sounding](https://docs.sailscasts.com/sounding/) , and replaced the old development error page with a richer[Youch](https://github.com/poppinss/youch) -powered error experience.


That is a lot, so this post is both a release note and an upgrade guide.


## What is in 1.5.0


This release includes:


- stable Inertia v3 support in[inertia-sails](https://github.com/sailscastshq/boring-stack/tree/main/packages/inertia-sails)
- the new[rsbuild-plugin-inertia](https://github.com/sailscastshq/boring-stack/tree/main/packages/rsbuild-plugin-inertia) package
- selective[Inertia SSR](https://docs.sailscasts.com/boring-stack/server-side-rendering) for Sails apps
- [Inertia Precognition](https://laravel.com/docs/precognition) support
- rescuable deferred props with` rescuedProps`
- ` preserveFragment()` redirect metadata
- v3 history, shared prop, merge prop, and scroll prop metadata
- [Youch](https://github.com/poppinss/youch) -powered development errors
- production Inertia error pages for` 403` ,` 404` ,` 500` , and` 503`
- updated Vue, React, and Svelte templates
- [Sounding](https://docs.sailscasts.com/sounding/) -first test structure in the templates
- [Shipwright 1.4.0](https://github.com/sailshq/sails-hook-shipwright/releases/tag/v1.4.0) in the templates


The thread running through all of this is the same: keep the app a Sails app, keep the UI modern, and remove the small pieces of glue that every project was going to copy by hand.


## Stable Inertia v3


Inertia v3 is now stable, and The Boring Stack speaks the v3 protocol cleanly.


The most visible change is the root page payload. Inertia v3 reads the initial page object from a dedicated JSON script instead of storing it on the` #app` element:


```text
<  div   id  =  "app"  ></  div  >
<  script   type  =  "application/json"   data-page  =  "app"  >
<%- JSON.stringify(page).replace(/</g, '\\u003c') %>
</  script  >
```


That keeps the mounting element clean and gives the client adapters the payload shape they expect.


The templates now use the stable v3 client adapters:


- ` @inertiajs/vue3@^3.1.1`
- ` @inertiajs/react@^3.1.1`
- ` @inertiajs/svelte@^3.1.1`


The Sails adapter is now[\[email protected\]](https://github.com/sailscastshq/boring-stack/releases/tag/v1.5.0) .


## New v3 protocol metadata


` inertia-sails` now emits the[v3 metadata](https://inertiajs.com/docs/v3/core-concepts/the-protocol) that the client adapters need for advanced behavior:


- ` sharedProps` for server-shared props
- ` clearHistory` and` encryptHistory` for history behavior
- ` deferredProps` for deferred data
- ` mergeProps` ,` prependProps` ,` deepMergeProps` , and` matchPropsOn` for richer prop merging
- ` scrollProps` for Inertia’s infinite scroll protocol
- ` rescuedProps` for deferred props that failed but were allowed to fail gracefully
- ` preserveFragment` for redirects that should keep the current hash


This matters because the adapter should not merely “return JSON”. It should understand the protocol well enough that a Sails action can stay boring:


```text
return   {
page:   'dashboard/index'  ,
props: {
user,
notifications: sails.inertia.  defer  (()   =>   Notification.  find  ({ user: user.id }))
}
}
```


The page object is still ordinary Sails-friendly JavaScript, but the client receives the v3 metadata it needs.


## Rescuable deferred props


[Deferred props](https://inertiajs.com/docs/v3/data-props/deferred-props) are great for expensive secondary data: analytics, recommendations, activity feeds, notification counts, reports, and similar things that should not block the first page render.


In 1.5.0, a deferred prop can be marked as rescuable:


```text
analytics  : sails.inertia
.  defer  (  async   ()   =>   {
return   await   Analytics.  getExpensiveReport  ()
})
.  rescue  ()
```


There is also an inline option:


```text
analytics  : sails.inertia.  defer  (
async   ()   =>   {
return   await   Analytics.  getExpensiveReport  ()
},
{ rescue:   true   }
)
```


If that callback fails,` inertia-sails` omits` analytics` from` props` and reports the key in` rescuedProps` . The frontend can render a rescue state instead of taking down the whole deferred response.


That gives you a clean distinction:


- critical page data should fail loudly
- secondary data can be rescued and shown as unavailable


The goal is not to hide real problems. The goal is to let non-critical panels fail without turning the whole page into a bad day.


## Preserved fragments


Some redirects should keep the current URL hash.


Imagine an old article slug redirecting to a new one:


```text
/articles/old-slug#comments
```


If the redirect lands on:


```text
/articles/new-slug
```


the user probably still expects` #comments` to survive.


The new API is explicit:


```text
sails.inertia.  preserveFragment  ()
return   '/articles/new-slug'
```


Fragments are not preserved by default. The redirect action opts in because this is a product decision, not a hidden global behavior.


## rsbuild-plugin-inertia


The new[rsbuild-plugin-inertia](https://github.com/sailscastshq/boring-stack/tree/main/packages/rsbuild-plugin-inertia) package centralizes Inertia-specific Rsbuild behavior for Boring Stack apps.


In the templates,` config/shipwright.js` now looks like this:


```text
const   {   pluginVue   }   =   require  (  '@rsbuild/plugin-vue'  )
const   {   pluginInertia   }   =   require  (  'rsbuild-plugin-inertia'  )


module  .  exports  .shipwright   =   {
build: {
plugins: [  pluginVue  (),   pluginInertia  ()]
}
}
```


Use the React or Svelte Rsbuild plugin for those templates:


```text
const   {   pluginReact   }   =   require  (  '@rsbuild/plugin-react'  )
const   {   pluginInertia   }   =   require  (  'rsbuild-plugin-inertia'  )


module  .  exports  .shipwright   =   {
build: {
plugins: [  pluginReact  (),   pluginInertia  ()]
}
}
```


The plugin handles the Inertia build details that were beginning to leak into every app:


- it injects a default page resolver for` ./pages`
- it supports an Inertia/Vite-style` pages` shorthand when you need a non-standard page directory
- it lazy-loads pages by default for page-level code splitting
- it supports` lazy: false` when an app intentionally wants a single bundle
- it detects Vue, React, or Svelte from the adapter import
- it prepares the SSR build environment when` assets/js/ssr.js` exists
- it stubs the optional Inertia v3 Axios adapter import when Axios is not installed


That last point matters because of[a Rsbuild/Rspack resolution edge case](https://github.com/inertiajs/inertia/issues/3123#event-25972795593) : Inertia v3 uses XHR by default, but its optional Axios adapter can still expose` import('axios')` to the bundler. Starter apps should not need Axios just to satisfy that lookup. If an app installs and uses Axios, the plugin leaves it alone; if it does not, the app can build without carrying Axios as an unused dependency.


This means a normal client entry can be small:


```text
createInertiaApp  ({
setup  ({   el  ,   App  ,   props  ,   plugin   }) {
createApp  ({   render  : ()   =>   h  (App, props) }).  use  (plugin).  mount  (el)
}
})
```


You only set` pages` when the default` ./pages` convention is not what you want:


```text
createInertiaApp  ({
pages: {
path:   './screens'  ,
extension: [  '.jsx'  ,   '.tsx'  ],
lazy:   false
},
setup  ({   el  ,   App  ,   props   }) {
// mount your app
}
})
```


## Selective SSR


The Boring Stack now supports[selective Inertia SSR](https://inertiajs.com/docs/v3/advanced/server-side-rendering) without requiring a separate SSR server process.


Shipwright builds the private SSR bundle.` inertia-sails` imports that bundle in-process and uses it only for the pages you choose.


The Shipwright version matters here.[\[email protected\]](https://github.com/sailshq/sails-hook-shipwright/releases/tag/v1.4.0) changed the tag helpers to emit only the initial assets for a manifest entry, a fix tracked from the[selective Inertia SSR work](https://github.com/sailshq/sails-hook-shipwright/issues/23) . Without that, async page chunks can be injected into every layout and bloat SSR/source HTML before the page actually needs them.


The app writes a source SSR entry at` assets/js/ssr.js` . Shipwright compiles that entry into the private` .tmp/ssr/inertia.mjs` file that Sails imports. You do not write` .tmp/ssr/inertia.mjs` by hand.


For Vue, that source entry looks like this:


```text
// assets/js/ssr.js
import   { createSSRApp, h }   from   'vue'
import   { renderToString }   from   'vue/server-renderer'
import   { createInertiaApp }   from   '@inertiajs/vue3'


export   default   function   render  (  page  ) {
return   createInertiaApp  ({
page,
render: renderToString,
resolve  : (  name  )   =>   require  (  `./pages/${  name  }`  ),
setup  ({   App  ,   props  ,   plugin   }) {
return   createSSRApp  ({   render  : ()   =>   h  (App, props) }).  use  (plugin)
}
})
}
```


React and Svelte use the same idea, but their server renderers are different. The separate entry keeps browser-only setup out of the server bundle while still sharing the same page components.


The default remains no SSR:


```text
module  .  exports  .inertia   =   {}
```


Enable SSR for every Inertia page:


```text
// config/inertia.js
module  .  exports  .inertia   =   {
ssr:   true
}
```


Or use the expanded shape:


```text
module  .  exports  .inertia   =   {
ssr: {
enabled:   true
}
}
```


Enable SSR only for selected pages:


```text
module  .  exports  .inertia   =   {
ssr: {
enabled:   true  ,
pages: [  'index'  ,   'pricing'  ,   'blog/show'  ]
}
}
```


Opt out for a specific response:


```text
return   {
page:   'dashboard/index'  ,
ssr:   false  ,
props: {
user
}
}
```


This is intentionally selective because Boring Stack apps are often hybrid apps. You might have:


- public marketing pages that benefit from SSR
- authenticated dashboards that are better as normal Inertia pages
- classic EJS pages for docs, legal pages, or legacy screens


SSR is useful when it improves first paint, SEO, or social sharing. It is not a moral upgrade for every page.


## Precognition


[Precognition](https://laravel.com/docs/precognition) comes from the Laravel/Inertia ecosystem. The idea is simple: the client can ask the same server action to validate input before the real submit happens.


That means you do not duplicate your server-side validation in the browser.


The request includes the` Precognition: true` header, plus` Precognition-Validate-Only` when the client is validating specific fields. The server validates the requested fields and returns:


- ` 204 No Content` with` Precognition: true` and` Precognition-Success: true` when validation passes
- ` 422` JSON errors when validation fails


In a Boring Stack app, this uses the same Sails Action2 inputs and the same` badRequest` error shape you already use. Validation failures do not need a special success response;` inertia-sails` handles them through` badRequest` and returns the` 422` JSON error shape.


On the client:


```text
const   form   =   useForm  ({
email:   ''
}).  withPrecognition  (  'post'  ,   '/forgot-password'  )
```


Then validate on blur:


```text
<  InputEmail
v-model  =  "  form.email  "
:  error  =  "  form.errors.email  "
@  blur  =  "  form.  validate  (  'email'  )  "
/>
```


For actions with side effects, add a tiny` precognitionSuccess` response so a valid Precognition request can return` 204 No Content` before the real action runs:


```text
// api/responses/precognitionSuccess.js
module  .  exports   =   function   precognitionSuccess  () {
return   this  .req._sails.inertia.  handlePrecognitionSuccess  (  this  .req,   this  .res)
}
```


This is useful when the action’s validation has passed, but the request is only a preview validation request, not the real submit. Without the early return, a blur validation on a forgot password form could accidentally send the reset email.


Return that exit before doing real work like sending email, writing a record, charging a card, enqueueing a job, or calling an external API:


```text
module  .  exports   =   {
inputs: {
email: {
type:   'string'  ,
required:   true  ,
isEmail:   true
}
},


exits: {
success: { responseType:   'redirect'   },
precognitionSuccess: { responseType:   'precognitionSuccess'   }
},


fn  :   async   function   ({   email   },   exits  ) {
if   (sails.inertia.  isPrecognitive  (  this  .req)) {
return   exits.  precognitionSuccess  ()
}


await   sendPasswordResetEmail  (email)
return   '/check-email'
}
}
```


For availability checks such as “username is taken”, use` shouldValidate()` so a blur on one field does not run every expensive rule:


```text
if   (sails.inertia.  shouldValidate  (  'username'  ,   this  .req)) {
const   exists   =   await   User.  count  ({ username })


if   (exists   >   0  ) {
throw   {
badSignupRequest: {
problems: [{ username:   'Username is already taken.'   }]
}
}
}
}
```


Precognition is best for fields where early feedback improves the product:


- signup email
- username availability
- invite codes
- password reset email
- profile settings
- billing setup


It is not a reason to validate every field constantly. Keep it useful and boring.


## Humanized validation errors


While adding Precognition, we also improved validation error messages.


Sails, Anchor, and RTTC can produce technically correct messages that are not the copy you want users to see. For example:


```text
Invalid email: Value ('not-an-email') was not a valid email address
```


` inertia-sails` now humanizes the common validation shapes before they reach Inertia forms. Precognition errors and normal submit-time errors go through the same path, so the user sees consistent messages whether validation happened on blur or on submit.


## Rich development errors with Youch


Development errors now use[Youch](https://github.com/poppinss/youch) .


When a server error happens during an Inertia request, Inertia shows the non-Inertia HTML response in its development error modal. In 1.5.0, that HTML is a Youch page with:


- readable stack frames
- source snippets
- request method and URL
- sanitized headers, params, query, body, and session data


Sensitive values such as cookies, authorization headers, CSRF tokens, passwords, secrets, and session-looking values are redacted before rendering.


This is the development experience we wanted: throw an error from a Sails action and see a proper server-side stack trace without digging through the Network tab.


There is one small upstream detail: Inertia opens the modal and then writes the HTML into the iframe. On a slow paint, you can briefly see the empty modal shell. We opened[an upstream issue](https://github.com/inertiajs/inertia/issues/3130) for the perfect fix: the modal would need to set` iframe.srcdoc` and wait for load before calling` showModal()` .


## Production Inertia error pages


Production errors should not show stack traces.


The templates now ship a framework-matched` error` page:


- ` assets/js/pages/error.vue`
- ` assets/js/pages/error.jsx`
- ` assets/js/pages/error.svelte`


` inertia-sails` renders that page by default for:


- ` 403`
- ` 404`
- ` 500`
- ` 503`


The page receives:


```text
{
status  :   404  ,
title  :   'Page not found'  ,
message  :   'The page you are looking for could not be found.'
}
```


The templates also include` notFound` and` forbidden` responses:


```text
module  .  exports   =   function   notFound  (  error  ) {
return   this  .req._sails.inertia.  handleErrorPage  (  this  .req,   this  .res, {
statusCode:   404  ,
error
})
}
```


Hybrid apps can still keep EJS error pages by opting out:


```text
module  .  exports  .inertia   =   {
errorPage:   false
}
```


By default,` errorPage` is a rendering policy: when it is configured, full-page browser requests can render the same Inertia error component as Inertia visits. If your app wants EJS for non-Inertia requests but Inertia error pages for` X-Inertia` requests, branch in your custom response and call` handleErrorPage()` only for the Inertia side.


Again, the important bit is choice. Inertia apps get in-app error pages. Hybrid apps can keep EJS where EJS is the better fit.


## Sounding in the templates


The templates now move toward[Sounding](https://docs.sailscasts.com/sounding/) as the testing story for Boring Stack apps.


The split is:


- unit tests for small pure logic
- functional tests for Sails actions, responses, auth, mail, and Inertia pages
- browser tests only when browser behavior is the thing under test


That means we stop turning every workflow into a browser automation problem.


Sounding gives us one Sails-centered test API that can test JSON endpoints, Inertia responses, mail capture, auth flows, and browser-capable flows where needed.


## Template updates


The release covers all current[Boring Stack templates](https://github.com/sailscastshq/boring-stack/tree/main/templates) :


- ` ascent-vue`
- ` ascent-react`
- ` mellow-vue`
- ` mellow-react`
- ` mellow-svelte`


The templates have been moved to:


- [inertia-sails@^1.5.0](https://github.com/sailscastshq/boring-stack/tree/main/packages/inertia-sails)
- Inertia client adapters` ^3.1.1`
- [sails-hook-shipwright@^1.4.0](https://github.com/sailshq/sails-hook-shipwright/releases/tag/v1.4.0)
- [rsbuild-plugin-inertia](https://github.com/sailscastshq/boring-stack/tree/main/packages/rsbuild-plugin-inertia)
- updated Vue, React, and Svelte Rsbuild plugins


Precognition is wired into the Vue, React, and Svelte templates. The Mellow templates demonstrate it on the forgot password form. The Ascent Vue template is also the SSR proving ground.


## Upgrade guide


Here is the concrete upgrade path for existing Boring Stack apps.


### 1. Update packages


For Vue:


```text
npm   install   inertia-sails@^1.5.0   @inertiajs/vue3@^3.1.1   rsbuild-plugin-inertia@latest   sails-hook-shipwright@^1.4.0
npm   uninstall   axios
```


For React:


```text
npm   install   inertia-sails@^1.5.0   @inertiajs/react@^3.1.1   rsbuild-plugin-inertia@latest   sails-hook-shipwright@^1.4.0
npm   uninstall   axios
```


For Svelte:


```text
npm   install   inertia-sails@^1.5.0   @inertiajs/svelte@^3.1.1   rsbuild-plugin-inertia@latest   sails-hook-shipwright@^1.4.0
npm   uninstall   axios
```


Only keep Axios if your application code imports Axios directly.


### 2. Update` views/app.ejs`


Move the page JSON out of the` #app` element:


```text
<  div   id  =  "app"  ></  div  >
<  script   type  =  "application/json"   data-page  =  "app"  >
<%- JSON.stringify(page).replace(/</g, '\\u003c') %>
</  script  >
```


If you use SSR, render the SSR body when present:


```text
<  % if (ssr && ssr.body) { %>
<  %- ssr.body %>
<  % } else { %>
<  div   id  =  "app"  ></  div  >
<  % } %>
```


And include SSR head output when present:


```text
<  %- ssr && ssr.head ? ssr.head.join('\n') : '' %>
```


### 3. Update` config/shipwright.js`


Add` pluginInertia()` after your framework plugin:


```text
const   {   pluginVue   }   =   require  (  '@rsbuild/plugin-vue'  )
const   {   pluginInertia   }   =   require  (  'rsbuild-plugin-inertia'  )


module  .  exports  .shipwright   =   {
build: {
plugins: [  pluginVue  (),   pluginInertia  ()]
}
}
```


For React, use` pluginReact()` . For Svelte, use` pluginSvelte()` .


### 4. Simplify` assets/js/app.js`


If your pages live in` assets/js/pages` , you can remove custom page resolution and let` rsbuild-plugin-inertia` inject the default resolver.


Keep only setup:


```text
createInertiaApp  ({
setup  ({   el  ,   App  ,   props  ,   plugin   }) {
}
})
```


If your pages live somewhere else, use the` pages` option:


```text
createInertiaApp  ({
pages:   './screens'  ,
setup  ({   el  ,   App  ,   props  ,   plugin   }) {
}
})
```


### 5. Add production error pages


Add an` error` page under your page directory:


```text
export   default   function   ErrorPage  ({   status  ,   title  ,   message   }) {
return   (
<  main  >
<  p  >Status {status}</  p  >
<  h1  >{title}</  h1  >
<  p  >{message}</  p  >
</  main  >
)
}
```


Then add` api/responses/notFound.js` :


```text
module  .  exports   =   function   notFound  (  error  ) {
statusCode:   404  ,
error
})
}
```


And` api/responses/forbidden.js` :


```text
module  .  exports   =   function   forbidden  (  error  ) {
statusCode:   403  ,
error
})
}
```


If your app should keep Sails EJS status pages, set:


```text
module  .  exports  .inertia   =   {
errorPage:   false
}
```


### 6. Add Precognition where it helps


For actions with side effects, add` api/responses/precognitionSuccess.js` :


```text
module  .  exports   =   function   precognitionSuccess  () {
}
```


Then return early only after validation has passed and before the real work runs. Use this anywhere a valid preview request must not send mail, write data, charge money, enqueue jobs, or call external APIs:


```text
if   (sails.inertia.  isPrecognitive  (  this  .req)) {
return   exits.  precognitionSuccess  ()
}
```


On the client:


```text
const   form   =   useForm  ({ email:   ''   }).  withPrecognition  (  'post'  ,   '/forgot-password'  )
```


Trigger field validation on blur:


```text
form.  validate  (  'email'  )
```


Use Precognition first on the forms where we usually wish we had instant server-backed feedback: signup, forgot password, invite, username, and profile settings.


### 7. Add SSR only where it pays for itself


Create` assets/js/ssr.js` for your framework. That is the source entry; Shipwright compiles it into` .tmp/ssr/inertia.mjs` .


```text
// assets/js/ssr.js
import   { createSSRApp, h }   from   'vue'
import   { renderToString }   from   'vue/server-renderer'
import   { createInertiaApp }   from   '@inertiajs/vue3'


export   default   function   render  (  page  ) {
return   createInertiaApp  ({
page,
render: renderToString,
resolve  : (  name  )   =>   require  (  `./pages/${  name  }`  ),
setup  ({   App  ,   props  ,   plugin   }) {
}
})
}
```


Then enable SSR:


```text
module  .  exports  .inertia   =   {
ssr:   true
}
```


Or selectively:


```text
module  .  exports  .inertia   =   {
ssr: {
enabled:   true  ,
pages: [  'index'  ,   'pricing'  ]
}
}
```


Use SSR for public pages where first paint, SEO, and social cards matter. Skip it for most authenticated dashboards.


### 8. Update redirect fragments where needed


For redirects that should keep the current hash:


```text
sails.inertia.  preserveFragment  ()
return   '/articles/new-slug'
```


Do this only on redirects where preserving the fragment is intentional.


### 9. Use rescuable deferred props for secondary panels


For expensive non-critical props:


```text
report  : sails.inertia.  defer  (()   =>   Analytics.  report  (), { rescue:   true   })
```


Then render a rescue state in the frontend. Do not rescue required data.


### 10. Run the app checks


For each app, run:


```text
npm   install
npm   run   lint
npm   test
npm   run   dev
```


Then manually check:


- initial page source contains the` data-page="app"` JSON script
- Inertia navigation still works
- validation errors still appear after normal submit
- Precognition errors appear on blur where enabled
- production error pages render for 404 and 403
- development server errors show Youch
- SSR pages show real HTML in view-source when enabled
- EJS pages still render if the app is hybrid


## Why this release matters


The Boring Stack is supposed to make full-stack JavaScript feel smaller.


Inertia v3 gives us the protocol foundation.` rsbuild-plugin-inertia` removes build-system glue. Selective SSR gives us server-rendered HTML where it is useful without forcing every page into SSR. Precognition lets us reuse server validation for live feedback. Youch makes development failures readable. Production error pages make the app feel complete.


That is the direction: fewer one-off decisions, better defaults, and the escape hatches still available when an app needs them.


The boring way, but sharper.
