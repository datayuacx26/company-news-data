---
schema_version: "1.0.0"
document_id: "087ef053d7eaa16e536641aa03c0d6817499bc57e1bb284e73e44f46570328f6"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/introducing-sounding/"
published_at: "2026-03-15T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:58:45.685125+00:00"
content_hash: "sha256:371281a98fc22cd7046a8ccb1ff973eef2522729bf50eaa4de0efc901758c4ab"
---

# Introducing Sounding: A Sails-Native Testing Framework for The Boring JavaScript Stack

Sails has had good testing ingredients for a long time.


The problem was never that we lacked a test runner, a browser automation tool, or a way to boot the app in test mode. The problem was that the story still felt split.


One tool for lightweight tests. Another for browser work. A custom boot helper in one repo. A mail stub in another. A separate mental model again when Inertia got involved.


It all worked. It just did not feel like one product.


That is why I built **Sounding** .


Sounding is a **Sails-native testing framework** for Sails applications and[The Boring JavaScript Stack](https://sailscasts.com/boring) . It gives you one` test()` API, one Sails-centered trial context, and one coherent place to test helpers, JSON endpoints, Inertia pages, transactional email, and browser flows.


## Why the name is Sounding


In maritime navigation, **sounding** means measuring the depth of water beneath a ship before moving forward.


Historically, sailors lowered a weighted sounding line to test the seabed and make sure the ship would not run aground.


That is exactly what a good test suite should do.


Before you ship, you test the waters. Before you commit to the voyage, you check what is underneath. Before the app goes further, you make sure the ground beneath the code is safe.


That makes Sounding feel at home in this ecosystem:


- [Slipway](https://docs.sailscasts.com/slipway/) helps you ship
- [Quest](https://docs.sailscasts.com/sails-quest/) runs work in the background -[Wish](https://docs.sailscasts.com/wish/) handles OAuth
- [Clearance](https://docs.sailscasts.com/clearance/) handles role-based access control
- [Sounding](https://docs.sailscasts.com/sounding/) tells you whether the code is safe to sail further


## The seam problem


The hardest part of testing most Sails apps is not writing assertions. It is living with the seams.


The seams between:


- Sails and the test runner
- virtual requests and real HTTP
- JSON endpoints and Inertia responses
- transactional email and mocks
- browser flows and everything else


A good testing framework should make the business intent loud and the setup quiet. That is the core idea behind Sounding.


## One` test()` API


Sounding does not start by asking you which flavor of test you are writing.


You write one` test()` and destructure what you need from the trial context.


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'signupWithTeam creates a team'  ,   async   ({   sails  ,   expect   })   =>   {
const   result   =   await   sails.helpers.user.  signupWithTeam  ({
fullName:   'Kelvin O'  ,
email:   ' [email protected]  '  ,
tosAcceptedByIp:   '127.0.0.1'  ,
})


expect  (result.user.email).  toBe  (  ' [email protected]  '  )
})
```


That context is intentionally Sails-native.


The canonical surface is still:


- ` sails.helpers`
- ` sails.models`
- ` sails.config`
- ` sails.hooks`
- ` sails.sounding`


Then Sounding adds a few calm conveniences for common work:


- ` get()`
- ` post()`
- ` visit()`
- ` expect()`


The point is not to replace Sails. The point is to make testing feel like an extension of Sails instead of a second app model.


## The trial context


A **trial** is one named behavior being proved. The **trial context** is the object Sounding passes into that behavior.


At the center is` sails` . Around it are the surfaces that matter most often:


- ` expect` for assertions
- ` get` ,` post` ,` put` ,` patch` ,` del` for request work
- ` visit` for Inertia-aware requests
- ` page` for browser-capable trials
- ` mailbox` for transactional email assertions
- ` world` for named business situations


That shape matters. It means the testing runtime looks like the app runtime, just furnished for testing.


## Request testing without ceremony


Sails already ships with a native request primitive:` sails.request()` . So Sounding leans into it.


For most non-browser trials, the default request transport is virtual and Sails-aware.


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'guest is redirected from dashboard'  ,   async   ({   get  ,   expect   })   =>   {
const   response   =   await   get  (  '/dashboard'  )


expect  (response).  toHaveStatus  (  302  )
expect  (response).  toRedirectTo  (  '/login'  )
})
```


When real HTTP parity matters, the shape of the test stays the same:


```text
test  (
'signup should run over real HTTP'  ,
{ transport:   'http'   },
async   ({   post  ,   expect   })   =>   {
const   response   =   await   post  (  '/signup'  , {
fullName:   'Kelvin O'  ,
emailAddress:   ' [email protected]  '  ,
})


expect  (response).  toHaveStatus  (  200  )
}
)
```


That pattern shows up throughout Sounding: one public API, smarter plumbing underneath.


## Inertia gets its own excellent lane


One of the quickest ways to undertest a Sails + Inertia app is to treat an Inertia response like generic HTML.


It is not. An Inertia response carries component identity, props, validation state, redirect semantics, and partial reload behavior.


That is why Sounding ships with` visit()` .


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'pricing returns the right Inertia page'  ,   async   ({   visit  ,   expect   })   =>   {
const   page   =   await   visit  (  '/pricing'  )


expect  (page).  toHaveStatus  (  200  )
expect  (page).  toBeInertiaPage  (  'billing/pricing'  )
expect  (page).  toHaveProp  (  'plans'  )
})
```


That gives Inertia a first-class lane between low-level request checks and full browser flows.


## Mail goes through the real Sails path


Transactional email is product behavior. It should not feel like a detached mock story.


When a trial boots, Sounding wraps` sails.helpers.mail.send` , captures the outgoing message, renders a preview when needed, and stores the normalized result in` sails.sounding.mailbox` .


```text
const   {   test   }   =   require  (  'sounding'  )


test  (  'requesting a magic link sends a usable email'  ,   async   ({   auth  ,   sails  ,   expect   })   =>   {
const   result   =   await   auth.  requestMagicLink  (  ' [email protected]  '  )
const   email   =   sails.sounding.mailbox.  latest  ()


expect  (result.response).  toHaveStatus  (  302  )
expect  (email.to).  toContain  (  ' [email protected]  '  )
expect  (email.ctaUrl).  toContain  (  '/magic-link/'  )
})
```


That continuity is a big deal. Mail assertions now live inside the same testing system as the rest of the app.


## Worlds are named business situations


A **world** in Sounding is not just a bag of fixtures. It is the named business state a trial lives inside.


For example:


- a publisher with a draft issue
- a subscriber with access to a members-only issue
- a reader with a temporary unlock


That lets tests start from intent, not housekeeping.


```text
const   current   =   await   sails.sounding.world.  use  (  'issue-access'  )
```


Then the trial can reach for meaningful handles like:


- ` current.users.subscriber`
- ` current.issues.gatedIssue`
- ` current.issues.freeIssue`


This is one of the biggest changes in the whole framework. The setup starts reading like the product.


## Hook-first by design


Sounding is not trying to sit beside Sails like a foreign tool. It integrates as a hook.


That means the main configuration lives where a Sails developer expects it to live:


```text
// config/sounding.js
module  .  exports  .sounding   =   {
datastore: {
mode:   'inherit'  ,
identity:   'default'  ,
},


request: {
transport:   'virtual'  ,
},


mail: {
capture:   true  ,
},
}
```


And the runtime sits where it belongs:


```text
sails.sounding
```


That is not branding flourish. It is a DX decision. A testing framework for Sails should feel like it belongs to Sails.


## What` 0.0.1` is meant to prove


The first credible release of Sounding does not need to be huge. It needs to prove that the testing story is finally coherent.


For` 0.0.1` , the bar is simple:


- one primary` test()` API
- one Sails-centered trial context
- helper testing
- request testing
- Inertia testing
- mail capture
- browser-capable flows when the browser truly matters
- worlds under` tests/`
- a real app integration story that can power production codebases


That is the shape of a framework teams can actually grow with.


## Why I am excited about Sounding


Every framework eventually has to answer this question:


**What is the most natural way to test an app written in this framework?**


For a long time, Sails had good ingredients but not one excellent answer.


Sounding is my attempt to give Sails that answer.


Not by hiding Node. Not by pretending browser and server tests are identical. Not by replacing Sails primitives.


But by making the right things feel native, and the common things feel calm.


If Shipwright was about giving Sails a modern asset pipeline, Sounding is about giving it a modern testing home.
