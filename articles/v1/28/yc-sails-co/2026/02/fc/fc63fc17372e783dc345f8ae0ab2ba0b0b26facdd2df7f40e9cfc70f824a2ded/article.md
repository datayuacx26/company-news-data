---
schema_version: "1.0.0"
document_id: "fc63fc17372e783dc345f8ae0ab2ba0b0b26facdd2df7f40e9cfc70f824a2ded"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/durable-ui-patterns/"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:57:41.767482+00:00"
content_hash: "sha256:f6c0e89c2c4bbc035612972468062807be7d4c421b5cc308a16807cd61d035c2"
---

# Durable UI Patterns

A durable UI isn’t just UI that remembers things. It’s UI that **behaves correctly** — under refresh, under navigation, under sharing, under real users doing things you didn’t plan for, and under codebases that grow over months and years.


Durability covers where state lives, yes — but also how components clean up after themselves, how navigation contracts work, how interactions degrade gracefully, and how the whole thing stays debuggable as it scales.


This post walks through the core patterns. They’re framework-agnostic — Vue is used for examples, but everything here applies to React, Svelte, Angular, or anything else. (If you’re new to the term, start with[Introducing Durable UI](https://blog.sailscasts.com/introducing-durable-ui) .)


## Pattern 1: State Placement


Every piece of state has a right place to live. Put it in the wrong place and the UI “works” but breaks under real conditions.


The decision framework:


If the state is… It belongs in… Example


Shareable via link URL query params Filters, search, sort, pagination, active tab


Personal to the device localStorage Sidebar open/closed, theme, dismissed banners


Ephemeral to the page Memory (` ref` /` useState` ) Modal open, animation step, hover state


The source of truth Server / database User data, invoices, enrollment


Calculable from other state` computed` / derived Filtered list, totals, formatted dates


**The litmus test:** “If someone else clicks this link, should they see this state?”


- Yes → URL.
- No, but it should persist across sessions → localStorage.
- No, and it’s temporary → memory.
- It comes from the server → let the server own it.
- It can be calculated → don’t store it at all.


```text
// Placement bug: filters in memory
const   status   =   ref  (  'active'  )
// URL says /users. Refresh: gone. Share: meaningless.


// Fixed: filters in the URL
// URL: /users?status=active
// Refresh: preserved. Share: works. Debug: visible.
```


This seems simple, but in practice most SPAs get it wrong. The default instinct is` ref()` for everything interactive. That instinct is the source of most durability bugs.


## Pattern 2: Click-Outside Dismissal


You open a dropdown. You click somewhere else on the page. The dropdown stays open. You press Escape. Nothing happens. You click the trigger button again just to close it — or worse, you navigate away because you can’t figure out how to dismiss it.


This is one of the most common interaction bugs in any UI. The dropdown opens fine, but nobody built the dismissal.


A durable dropdown closes when you click outside it, press Escape, or when the component unmounts. All three. Every time.


```text
import   { onMounted, onBeforeUnmount }   from   'vue'


function   useClickOutside  (  elementRef  ,   callback  ) {
function   onPointerDown  (  event  ) {
if   (elementRef.value   &&   !  elementRef.value.  contains  (event.target)) {
callback  ()
}
}


function   onKeyDown  (  event  ) {
if   (event.key   ===   'Escape'  ) {
callback  ()
}
}


onMounted  (()   =>   {
document.  addEventListener  (  'pointerdown'  , onPointerDown)
document.  addEventListener  (  'keydown'  , onKeyDown)
})


onBeforeUnmount  (()   =>   {
document.  removeEventListener  (  'pointerdown'  , onPointerDown)
document.  removeEventListener  (  'keydown'  , onKeyDown)
})
}
```


Usage is one line:


```text
const   dropdownRef   =   ref  (  null  )
useClickOutside  (dropdownRef, ()   =>   { isOpen.value   =   false   })
```


This pattern applies to every dismissible element: dropdowns, context menus, popovers, modals, slide-over panels. If a user can open it, they need an obvious way to close it — and “click outside” and Escape are the two ways every user expects to work.


The` onBeforeUnmount` cleanup is critical. Without it, navigating away and back stacks duplicate listeners. But that’s the implementation detail — the pattern is about the user expectation: **if I click outside it, it closes.**


## Pattern 3: Navigation Durability


Users have three implicit contracts with navigation that most UIs violate:


**The Back Button Contract.** If a user navigates away from a form and hits back, they expect their input to still be there. Breaking it feels like a bug — even though you never promised to preserve it.


The durable approach: auto-save form drafts to localStorage with a TTL. When the user returns, restore the draft and show a subtle banner (“Draft restored”). Clear the draft on successful submission.


```text
const   DRAFT_KEY   =   'myapp:expense-form:v1'


// Restore on mount
const   saved   =   localStorage.  getItem  (  DRAFT_KEY  )
if   (saved) {
Object.  assign  (form,   JSON  .  parse  (saved))
}


// Auto-save on change
watch  (form, (  val  )   =>   {
localStorage.  setItem  (  DRAFT_KEY  ,   JSON  .  stringify  (val))
}, { deep:   true   })


// Clear on submit
function   onSuccess  () {
localStorage.  removeItem  (  DRAFT_KEY  )
}
```


**The Refresh Contract.** Whatever the user was looking at before refresh should be what they see after. View state (filters, tabs, sort) belongs in the URL. Preference state (sidebar, panel widths) belongs in localStorage. If refresh resets anything the user set, that’s a durability failure.


**The Share Contract.** If a user copies the URL and sends it to someone, the recipient should land on the same *view* . Not the same page — the same view. Filters applied, tab selected, search populated. If your URL says` /dashboard` regardless of what the user is looking at, you’ve broken this contract.


## Pattern 4: Derived State, Not Duplicated State


Every piece of state that can be computed from other state **must** be computed, not stored.


This eliminates an entire class of sync bugs where two representations of the same data drift apart.


```text
// Bug-prone: stored derived state
const   items   =   ref  ([  ...  ])
const   total   =   ref  (  0  )


watch  (items, ()   =>   {
total.value   =   items.value.  reduce  ((  sum  ,   i  )   =>   sum   +   i.price,   0  )
})
// If the watcher misses a change or runs out of order,
// total disagrees with items. Silent data corruption.


// Durable: computed derived state
const   items   =   ref  ([  ...  ])
const   total   =   computed  (()   =>
items.value.  reduce  ((  sum  ,   i  )   =>   sum   +   i.price,   0  )
)
// Can never be wrong. Always in sync by definition.
```


**Rule:** if it can be computed, compute it. Don’t store it. “Is the form valid” is derived from field values + validation rules — don’t store a separate` isValid` ref. “Is the list empty” is derived from` items.length` — don’t maintain a separate boolean.


## Pattern 5: Explainability


A durable UI is one where any engineer can point at the interface and explain:


- Where each piece of state lives
- How it persists (or doesn’t)
- How it can be reproduced


This means:


- **No hidden global state.** If state is stashed in a closure or module-level variable that isn’t exposed anywhere, future you won’t find it.
- **No mystery watchers.** A chain of watchers that triggers side effects across components — A watches B which triggers C which updates D — is impossible to debug.
- **URL as documentation.** When state lives in the URL, it’s self-documenting. A developer can look at` /users?status=active&sort=name&page=3` and immediately understand the view state. No devtools spelunking required.


**The Explainability Test:** Pick any page in your app. Can someone look at the URL, the component code, and the devtools, and within 5 minutes explain where every piece of interactive state lives? If not, you have a durability problem.


## Pattern 6: UI Honesty


A durable UI never lies about what’s happening. The interface should reflect the true state of user intent — not an assumed or default state.


The most common violation: **an always-enabled “Update” button on edit forms** .


When the update button is always enabled, the user has no idea whether they’ve changed anything. They click “Update” without making changes — wasted server request, pointless loading spinner, and confusion: “did it actually save something?” They can’t tell the difference between “I haven’t touched this” and “my changes are ready to save.”


Worse: the app can’t warn them about unsaved changes when navigating away, because it doesn’t know there are any.


The durable version: track dirty state. Disable the button until the form has actually changed.


```text
// Vue with Inertia — useForm tracks isDirty automatically
const   form   =   useForm  ({
name: props.user.name,
email: props.user.email,
})
```


```text
<  button
type  =  "submit"
:disabled  =  "!form.isDirty || form.processing"
>
Update
</  button  >
```


Now the UI communicates honestly:


- Button disabled → “nothing has changed”
- Button enabled → “you have unsaved changes”
- Button loading → “saving your changes”


This extends beyond buttons. Any UI element that communicates state should reflect **actual** state, not assumed state:


- A “Save” indicator should only appear when there’s something to save
- A badge count should be computed from the actual list, not maintained separately
- A “changes saved” toast should only fire when changes were actually persisted, not on every form submission


UI honesty is a subset of the derived state pattern — the button’s disabled state is *derived* from whether the form has changed. Don’t store a separate` canSubmit` flag. Compute it.


## Pattern 7: Graceful Degradation


Durable UIs handle failure without losing user work.


**Network failures:** If a form submission fails, the form data must still be there. The user should not re-enter anything.


**Storage failures:** localStorage can throw (private browsing, quota exceeded). Catch the error and fall back to memory. The feature degrades — state won’t persist across sessions — but the app doesn’t crash.


```text
function   safePersist  (  key  ,   value  ) {
try   {
localStorage.  setItem  (key,   JSON  .  stringify  (value))
}   catch   {
// Storage unavailable — degrade to memory-only
}
}
```


**Stale data:** If localStorage contains data from a previous schema version, don’t crash — ignore it. Version your storage keys so breaking changes silently discard old data rather than parsing garbage.


## Pattern 8: Testable Durability


If you can’t test that your UI survives, it won’t — especially as the codebase grows and people refactor without understanding the durability contracts.


Three essential Playwright tests for any page with interactive state:


**The Refresh Test:**


```text
await   page.  goto  (  '/users'  )
await   page.  click  (  '[data-filter="active"]'  )
await   expect  (page).  toHaveURL  (  /  status=active  /  )
await   page.  reload  ()
await   expect  (page.  locator  (  '[data-filter="active"]'  )).  toHaveClass  (  /  selected  /  )
```


**The Share Test:**


```text
await   page.  click  (  '[data-filter="active"]'  )
const   url   =   page.  url  ()
const   newPage   =   await   browser.  newPage  ()
await   newPage.  goto  (url)
await   expect  (newPage.  locator  (  '[data-filter="active"]'  )).  toHaveClass  (  /  selected  /  )
```


**The Back Button Test:**


```text
await   page.  fill  (  '#expense-name'  ,   'Office supplies'  )
await   page.  goto  (  '/dashboard'  )
await   page.  goBack  ()
await   expect  (page.  locator  (  '#expense-name'  )).  toHaveValue  (  'Office supplies'  )
```


These tests are cheap to write and catch regressions that manual QA misses.


## The Checklist


For any page or component you’re building:


- Every piece of state has a deliberate home (URL, localStorage, memory, server, or computed)
- Shareable state is in the URL
- Preferences persist in localStorage (namespaced, versioned)
- All event listeners, timers, and subscriptions are cleaned up on unmount
- No derived state is stored — it’s all computed
- Form drafts survive navigation (back button contract)
- Refresh preserves view state
- Storage failures are caught and degraded gracefully
- The UI is honest — buttons, indicators, and signals reflect actual state (e.g., submit disabled until form is dirty)
- The UI is explainable — anyone can trace where state lives


## Going Deeper


These patterns are the foundation of a course I’m building on Sailscasts: **[Building Durable UIs in the Era of Vibe Coding](https://sailscasts.com/courses/durable-ui)** .


The course goes further — you’ll build production composables (` useDurableUrl()` ,` useDurableStorage()` ,` useFormDraft()` ,` useWizardDraft()` ), work through anti-patterns that AI tools generate by default, and test durability systematically with Playwright. It also ships with a Durable UI skill for Claude Code that encodes these patterns into your AI workflow.


If this post gave you a new way to think about even one piece of your UI, the course will change how you think about all of them.


**[Learn more →](https://sailscasts.com/courses/durable-ui)**
