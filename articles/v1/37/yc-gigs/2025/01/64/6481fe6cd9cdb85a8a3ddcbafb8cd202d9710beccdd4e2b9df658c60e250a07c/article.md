---
schema_version: "1.0.0"
document_id: "6481fe6cd9cdb85a8a3ddcbafb8cd202d9710beccdd4e2b9df658c60e250a07c"
company_key: "yc-gigs"
company: "Gigs"
source_id: "yc-gigs-news-import-0fc2b850cd38"
canonical_url: "https://gigs.com/blog/how-connect-uses-typescripts-asserts"
published_at: "2025-01-17T16:28:40.850+00:00"
first_seen_at: "2026-07-21T21:37:08.403736+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:2a8562d75157f5e06e04361a67c4a4a5157692ae9126555621059e6531c222f0"
---

# How Connect uses TypeScript asserts

At Gigs, we care about writing clean, ergonomic, and maintainable code. A well-structured codebase allows us to iterate and ship faster. It requires less refactoring; it’s easier to extend and quicker to review. What makes a codebase great to work with isn’t just the big architectural decisions. It's also the many small, thoughtful practices that make a difference. This article is about one of those small practices.


In TypeScript 3.7, a new set of function signatures called “assertions” was introduced. These enable us to define functions that ensure either a condition is true or that a value matches a specific type.


To ensure a condition is true, we can write a function like this:


This lets us use assertions similarly to how Node.js handles them:


To ensure a value is of a specific type, the function signature changes slightly:


We can then use this function as a drop-in replacement in our logArray


function:


##


Why we like it


Before we talk about *how* we use assertions, let's quickly discuss *why* we use them. Assertions play a significant role in Connect's codebase: They make implicit assumptions explicit, simplify logic and ensure our code remains easy to read. These qualities enhance developer productivity and reduce the likelihood of bugs, both of which are crucial for maintaining a robust application.


Take the logArray()


function as an example. We could inline the Array.isArray


check directly in the function:


This code works, but the intent is not immediately clear and sacrifices readability. Developers reading the if


statement must mentally parse why the condition exists and what assumptions the code is making. In contrast, using an assertion function makes these assumptions explicit.


By replacing the inline check with assertArray(array)


, the code immediately communicates that from this point forward, the array


is guaranteed to be of type Array


. This simplifies the cognitive load for developers, improving maintainability and readability, for example during code reviews.


With these advantages in mind, let’s explore specific examples of how we use assertions in Connect.


## assertDefined


One function we use extensively (in 69 files at the time of writing) is assertDefined


. Here's the implementation:


Before we talk about why we use it, let’s look at the function itself.


The function ensures that a given value is neither undefined


nor null


(i.e., NonNullable


in TypeScript) which is reflected in its signature: asserts value is NonNullable<T>.


The additional name


and message


arguments let callers customize error messages for better debugging context.


To understand the purpose of name


and message


, it helps to know how we approach error handling in Connect. We use several error classes to categorize issues by their recoverability and severity.


For example, we often throw AppError


when the user can resolve the issue themselves, such as during the checkout process if a SIM card is already in use. These errors are typically caught and handled by the caller, allowing us to provide a meaningful way forward to the user.


In contrast, GeneralAppError


(used in assertDefined


)is our fallback for unexpected errors where recovery is unlikely. These errors are usually not handled by the caller, are logged to Sentry and result in a generic error modal, prompting the user to essentially "try again." This is where name


and message


become important: they provide additional context to help us debug issues, even when the error itself is generic.


Here’s an example from our code:


In this case, assertDefined


ensures the order


is defined before proceeding with a payment request. Providing the message places the missing order


in a meaningful context. When this error appears in Sentry, we can identify the problem without needing to dig through the stack trace.


## How we use it


We use assertDefined


when our application logic guarantees that a value exists, but TypeScript cannot verify it due to static analysis limitations. One common example is the project


object in Connect.


The project


is essential for the app's functionality and is fetched during the very first interaction with the application, even before the user signs in. Since the project


cannot change for a given Connect instance, the request to fetch it is cached for reuse throughout the application. Despite this, the function signature of fetchCurrentProject


allows for the possibility of null


, which TypeScript rightly flags as a potential issue.


We need the project


in many places because it contains configuration data. Instead of bypassing TypeScript's safety with a non-null assertion like project!.id


, we prefer to use assertDefined


:


This also ensures that even in rare cases where the project


is undefined, we can handle the issue gracefully by notifying the user and logging the error for debugging.


## assertStripeLoaded


This function is very similar to asserDefined


discussed above, but it focuses specifically on ensuring that the Stripe client is loaded in the browser.


Occasionally, Stripe may fail to load for a user, often due to external factors outside our control. However, Stripe is critical for enabling users to make purchases, so ensuring it is properly loaded is essential.


We aim to handle these errors as close to their source as possible, informing users of the issue and logging occurrences to PostHog for monitoring. For this reason, assertStripeLoaded


differs slightly from assertDefined


. It specifically asserts the presence of a Stripe


object (or lack thereof) and throws an AppError


with the stripeNotLoaded


error code when the assertion fails:


This allows us to check for the error code further down in the application and handle it appropriately, for example:


##


Closing thoughts


We believe that small practices like using assertions make a big difference. They reflect our broader philosophy of building software that is clean, maintainable, and a joy to work with, ultimately allowing us to solve complex problems not only faster but with greater reliability.


We’re constantly refining how we approach engineering to make our work easier, faster, and more reliable. If you share this passion and enjoy working on challenging problems, Gigs might be the place for you: Head over to[our careers page](https://gigs.com/careers) , we’re excited to hear from you!
