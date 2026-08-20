---
schema_version: "1.0.0"
document_id: "97b2217d821a95b483f6e1d2c2a9866a26f5648fc91230a9b7b7c3b398b52f5c"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/powering-toast-notifications-with-flash-messages/"
published_at: "2025-08-14T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:7ea2a35aa2b3813013e40b475ced145f2f5fbc95899dc193c5f7cc3f1d27cf18"
---

# Powering toast notifications in The Boring JavaScript Stack with Flash messages

If you’ve ever built a web app with forms, you know the pain: handling notifications for every action quickly gets messy. Success and error messages end up scattered across server and client code, leading to duplication, inconsistent styles, and maintenance headaches.


In this post, I’ll show how I unified flash messages and toast notifications in a The Boring JavaScript Stack project—making notifications automatic, consistent, and easy to manage.


## The Problem


While building[Hagfish](https://hagfish.io/) with **The Boring JavaScript Stack** (Sails.js, Inertia.js, Vue, Tailwind), I noticed a pattern: I was writing the same toast notification code for every form submission.


For example, creating an expense meant:


- Optionally sending a success flash message from the server to display in a global` <Message />`
- Then **also** writing toast logic in the client’s` onSuccess` /` onError` callback


**Before refactor** – Server side was fine:


```text
// api/controllers/expense/create-expense.js
this  .req.  flash  (  'success'  ,   'Expense created successfully!'  )
return   '/expenses/new'
```


But client-side was bloated:


```text
const   toast   =   useToast  ()


form.  post  (  '/expenses'  , {
onSuccess  : ()   =>   {
toast.  add  ({
severity:   'success'  ,
summary:   'Expense added'  ,
detail:   'Expense added successfully.'  ,
life:   3000
})
form.  reset  ()
},
onError  : ()   =>   {
toast.  add  ({
severity:   'error'  ,
summary:   'Error adding expense'  ,
detail:   'Could not add expense. Please try again.'  ,
life:   3000
})
}
})
```


I repeated this pattern in:


- Expense create/edit/delete
- Client create/update
- Profile updates
- Invoice operations


It caused:


- **Duplication** — same message in two places
- **Inconsistency** — toast styles/config varied
- **Maintenance pain** — had to update messages in multiple files
- **Mixed concerns** — components handled both logic and notifications


## The Solution: Unifying Flash Messages and Toasts with useFlashToast


I decided **flash messages** from the server should be the **single source of truth** . If the server sends` req.flash()` , the UI should automatically display it as a toast.


Here’s the composable:


```text
// assets/js/composables/flashToast.js
import   { watch }   from   'vue'
import   { usePage }   from   '@inertiajs/vue3'
import   { useToast }   from   'primevue/usetoast'


export   function   useFlashToast  () {
const   toast   =   useToast  ()
const   page   =   usePage  ()


watch  (
()   =>   page.props.flash,
(  flash  )   =>   {
if   (  !  flash)   return


const   types   =   {
success: { severity:   'success'  , summary:   'Success'  , life:   4000   },
error: { severity:   'error'  , summary:   'Error'  , life:   5000   },
message: { severity:   'info'  , summary:   'Info'  , life:   4000   }
}


for   (  const   [  type  ,   config  ]   of   Object.  entries  (types)) {
if   (flash[type]   &&   Array.  isArray  (flash[type])) {
flash[type].  forEach  (  message   =>   {
toast.  add  ({   ...  config, detail: message })
})
}
}
},
{ deep:   true  , immediate:   true   }
)


return   { toast }
}
```


With the composable in place, integrating flash-driven toast notifications into my app was straightforward. Here are the steps I followed to refactor my layout and components to take full advantage of this pattern:


## Step 1 – Integrate into Your Layout


```text
<  script   setup  >
import Toast from 'primevue/toast'
import { useFlashToast } from '@/composables/flashToast'


useFlashToast()
</  script  >


<  template  >
<  header  ><!-- navigation --></  header  >
<  main  >
<  slot  ></  slot  >
<  Toast   />
</  main  >
</  template  >
```


## Step 2 – Clean Up Components


**Before** – Manual toast handling in every form. **After** – No toast code at all:


```text
function   addExpense  () {
form.  post  (  '/expenses'  , {
onSuccess  : ()   =>   {
// Flash messages now automatically become toasts
form.  reset  ()
}
})
}
```


## Step 3 – Migrate CRUD Operations


Pattern:


1. **Server** — Always use` req.flash()` for success/error/info
2. **Client** — Remove all toast logic


Example – Creating a client:


**Server**


```text
this  .req.  flash  (  'success'  ,   'Client added successfully.'  )
return   '/clients'
```


**Client**


```text
form.  post  (  '/clients'  , {
onSuccess  : ()   =>   form.  reset  ()
})
```


## Results


- **~135 lines** of toast code deleted
- **12` useToast` imports** removed
- **8 form components** simplified
- All toasts consistent in style, duration, and position
- No more “forgot to add a toast” bugs


## Key Benefits


1. **Single Source of Truth** — All messages come from server flash data
2. **Cleaner Components** — No notification logic in forms
3. **Consistent UX** — Uniform styling and timing
4. **Easier Maintenance** — Update message in one place only
5. **Automatic for New Features** — No extra work to get notifications


## Handling Flash Errors with Redirects


Sometimes, you want to show an error message without returning a specific status code—just redirect the user back and display the error as a toast. With this unified system, it’s simple:


**Server-side pattern:**


```text
// api/controllers/expense/create-expense.js
if   (  !  expenseDataIsValid) {
this  .req.  flash  (  'error'  ,   'Please fill out all required fields.'  )
return   this  .res.  redirect  (  'back'  )
}
```


**Client-side:** No changes needed. The error message will automatically appear as a toast when the page reloads.


This approach is perfect for validation errors or other cases where you want to inform the user but keep the flow simple. All you need is a flash message and a redirect—the toast notification is handled for you.


## Conclusion


Switching to a unified` useFlashToast` system gave me cleaner code, a more consistent UI, and fewer bugs. If you’re building with **The Boring JavaScript Stack** , I highly recommend it — your future self will thank you.


*See it live in[Hagfish](https://github.com/sailscastshq/hagfish) — a Work OS for modern creators.*
