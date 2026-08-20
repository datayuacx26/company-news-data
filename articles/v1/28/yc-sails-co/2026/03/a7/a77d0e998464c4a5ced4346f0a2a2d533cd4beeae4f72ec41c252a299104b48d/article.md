---
schema_version: "1.0.0"
document_id: "a77d0e998464c4a5ced4346f0a2a2d533cd4beeae4f72ec41c252a299104b48d"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/migrating-a-boring-stack-test-to-sounding/"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:58:45.685125+00:00"
content_hash: "sha256:c102b94aee822fec63f84db9985c483baaef00942cb9f9802a9e25ae582ebfd8"
---

# Migrating a Boring Stack Test to Sounding: From Split Tools to One Calm Trial

When you live with a testing setup long enough, you stop noticing the seams.


You stop noticing that one test style reaches for Playwright, another reaches for a custom Sails singleton, another needs a homegrown database helper, and another needs special knowledge just to assert on an Inertia response.


Then one day you try to explain the suite to someone else and realize the truth:


The tests work, but the DX is fragmented.


That was the situation in[The African Engineer](https://africanengineer.com/) , a real app built on The Boring JavaScript Stack.


This is the story of moving that codebase to **Sounding** , the new Sails-native testing framework I am building, and why the migration immediately made the tests easier to read.


## The old shape of the suite


Before Sounding, African Engineer had a few moving parts:


- the Node test runner for some tests
- Playwright for browser flows
- a custom` getSails()` singleton to boot the app without lifting it
- extra scaffolding around auth and data setup
- a separate mental model for Inertia-style assertions


The old custom Sails bootstrap looked like this:


```text
const   Sails   =   require  (  'sails'  ).  constructor


let   sailsInstance   =   null
let   initPromise   =   null


async   function   getSails  () {
if   (sailsInstance) {
return   sailsInstance
}


if   (initPromise) {
return   initPromise
}


initPromise   =   new   Promise  ((  resolve  ,   reject  )   =>   {
const   sailsApp   =   new   Sails  ()
sailsApp.  load  (
{ environment:   'test'  , hooks: { shipwright:   false  , content:   false   } },
(  err  ,   sails  )   =>   {
if   (err) {
return   reject  (err)
}
sailsInstance   =   sails
resolve  (sails)
}
)
})


return   initPromise
}
```


That file was practical. But it was also one more thing every developer had to remember before they could even write a useful test.


The same thing happened on the page-testing side. A simple guest-protection check looked like this:


```text
import   { test, expect }   from   '@playwright/test'


test.  describe  (  'Guest Protection'  , ()   =>   {
test  (  'dashboard redirects unauthenticated users to login'  ,   async   ({   page   })   =>   {
await   page.  goto  (  '/dashboard'  )
await   expect  (page).  toHaveURL  (  /  login  /  )
})
})
```


That works. But it also means the browser is involved even when the behavior we actually care about is just a redirect contract.


That is a heavier tool than the behavior needs.


## What we wanted instead


The goal was not to replace everything with magic. The goal was to make the tests read like the app.


That meant a few concrete choices:


- one` test()` API
- ` sails` at the center of the trial context
- ` get()` and` post()` for request-level behavior
- ` visit()` for Inertia-aware responses
- ` page` only when the browser truly matters
- ` sails.sounding.mailbox` for transactional email assertions
- worlds under` tests/` so setup reads like business state, not plumbing


Once that shape exists, the migration stops being about tooling and starts being about clarity.


## The first easy win: guest protection


Here is the Sounding version of the guest-protection test:


```text
const   {   test   }   =   require  (  'sounding'  )


const   protectedRoutes   =   [
'/dashboard'  ,
'/settings/profile'  ,
'/settings/security'  ,
'/settings/team'  ,
]


for   (  const   route   of   protectedRoutes) {
test  (  `guest is redirected from ${  route  }`  ,   async   ({   get  ,   expect   })   =>   {
const   response   =   await   get  (route)


expect  (response).  toHaveStatus  (  302  )
expect  (response).  toRedirectTo  (  '/login'  )
})
}
```


Three things improve immediately.


### 1. The test now matches the behavior


The behavior is not “a browser can reach this URL and end up somewhere else.”


The behavior is:


**this route redirects guests to` /login` .**


That is exactly what` get()` and` toRedirectTo()` describe.


### 2. The browser stops paying rent where it is not needed


Browser flows are great when we care about real page interaction. They are wasteful when we just need to assert on a redirect response.


Sounding defaults to a Sails-native virtual transport for this kind of trial, so the test stays close to the server behavior without carrying browser overhead.


### 3. The context gets calmer


There is no custom bootstrap import. No question about where the app instance lives. No second mental model for this one class of tests.


The trial context already gives us what matters.


## The second win: Inertia stops feeling awkward


A lot of modern Sails apps are not pure JSON APIs and not pure server-rendered HTML. They are Sails + Inertia apps.


That means tests need to understand component identity and props, not just status codes.


Here is a page-level contract from African Engineer using Sounding:


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'home page returns the expected Inertia payload'  ,   async   ({   visit  ,   expect   })   =>   {
const   page   =   await   visit  (  '/'  )


expect  (page).  toHaveStatus  (  200  )
expect  (page).  toBeInertiaPage  (  'index'  )
expect  (Array.  isArray  (page.data.props.deepDiveIssues)).  toBeTruthy  ()
expect  (Array.  isArray  (page.data.props.lowdownIssues)).  toBeTruthy  ()
})
```


The old version of this check used Playwright and asserted on the browser title and the first heading. That is fine for smoke testing, but it is not the actual contract of the page.


The real contract is:


- which Inertia component was returned
- which props came with it
- whether the payload shape is right


That is what` visit()` gives us.


This is one of the biggest benefits of the migration. A whole class of tests moves from “browser-shaped smoke tests” to “app-shaped response tests.”


## The third win: email tests go through the real app path


African Engineer uses magic-link authentication. That means the email path is not incidental. It is core product behavior.


With Sounding, the test is straightforward:


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'requesting a magic link sends a usable email'  ,   async   ({   auth  ,   sails  ,   expect   })   =>   {
const   result   =   await   auth.  requestMagicLink  (  ' [email protected]  '  )
const   email   =   sails.sounding.mailbox.  latest  ()


expect  (result.response).  toHaveStatus  (  302  )
expect  (result.response.  header  (  'location'  )).  toMatch  (  '/check-email?type=magic-link'  )
expect  (email.to).  toContain  (  ' [email protected]  '  )
expect  (email.subject).  toContain  (  'magic link'  )
expect  (email.ctaUrl).  toContain  (  '/magic-link/'  )
})
```


The important thing here is not the matcher syntax. It is that the mail assertion now lives inside the same testing system as the rest of the app.


No separate mail mock story. No test-only controller route. No weird side channel.


That continuity matters.


## The browser still matters, but now it is obvious why


Sounding is not anti-browser. It just wants the browser to show up for the right reasons.


A real magic-link sign-in flow still deserves a browser-capable trial:


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'magic link login reaches the dashboard in a real browser'  , { browser:   true   },   async   ({   auth  ,   page  ,   expect   })   =>   {
await   auth.login.  as  (  ' [email protected]  '  , page)


await   expect  (page).  toHaveURL  (  /  \/  dashboard  $  /  )
await   expect  (page.  getByRole  (  'heading'  , { level:   1  , name:   /  browser-reader  /  i   })).  toBeVisible  ()
})
```


That is a healthier split:


- request helpers for request behavior
- ` visit()` for Inertia contracts
- browser trials for genuine interaction


## The suite structure got better too


One of the quieter benefits of moving to Sounding is that the test tree becomes more obvious again.


Instead of a special` tests/sounding/` island and extra utility files, the suite can read like a normal codebase:


```text
tests/
unit/
helpers/
e2e/
pages/
auth/
billing/
dashboard/
issues/
scenarios/
```


A good testing framework should make the file structure clearer, not more exotic.


## What actually improved


After doing the migration work, I think the best way to describe Sounding is this:


it reduces the number of mental context switches per test.


That is the real win.


You do not have to keep asking:


- which test style is this?
- which boot helper do I need?
- do I need the browser here?
- where do mail assertions go?
- how do I assert on an Inertia response again?


The answers are closer together now. And because they are closer together, the tests are easier to teach, easier to review, and easier to extend.


## The migration did not make everything magically easier


A good migration should not tell fairy tales.


Moving African Engineer to Sounding surfaced real framework work too:


- which hooks should stay enabled in test mode
- how browser-capable trials should lift the app cleanly
- how test datastores should be managed under one predictable folder
- how mail capture should stay in-memory by default while still reflecting the real Sails mail flow


That work was worth doing precisely because it made the framework more honest. Dogfooding against a real app always does that.


## Why this matters for The Boring JavaScript Stack


The Boring Stack already had the right ingredients for a strong testing story. What it did not have was a framework that made those ingredients feel like one coherent product.


That is the gap Sounding fills.


The migration in African Engineer matters not because one app changed its test files. It matters because it shows the shape of a calmer testing future for the stack:


- one` test()` API
- one Sails-centered context
- one place to reach for helpers, requests, Inertia, mail, and browser work
- fewer seams
- more intent


That kind of change compounds. And once you feel it in a real codebase, it is hard to want the old split-brain setup back.
