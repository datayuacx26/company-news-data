---
schema_version: "1.0.0"
document_id: "9cbd90281eb426c8629955f494dac8a70cce4dbd0650b12f9d5da6328b798a2e"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-336c2f98fc66"
canonical_url: "https://blog.zeplin.io/collaboration/what-is-the-happy-path/"
published_at: "2023-03-31T19:45:11.540+00:00"
first_seen_at: "2026-07-22T21:02:04.822116+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:ccf0a0d4f6b24a7e8031e7904a88cc5131f5b5965e9c73e0e6b64b06462ffc10"
---

# What is the "happy path"?

The happy path, also called the "sunny day path," is a term used to describe the most direct path a user can take within a product to achieve their desired result. There are multiple error-free paths a user can take to complete a task, but the happy path is the one that takes the least effort and time and ends with the completion of the product's main sales or engagement goal — like making a purchase or consuming a piece of content. The visual map of a happy path is called a happy flow.


What the happy path is *not* is a realistic reflection of user behavior. The happy path, and happy path testing, allow engineers to perform early rounds of validation before running the product in more realistic conditions.


### Happy paths vs. golden paths


Another term that's often used in software development interchangeably with happy path is the "golden path." Though the two are very similar, there's a slight difference in the scope of each path type.


Every[user flow](https://blog.zeplin.io/what-are-user-flows) , no matter what size, has its own happy path. The happy path is the ideal route to accomplish a particular user task. When we talk about the golden path, we're looking at the product in its entirety. Rather than accomplishing individual goals, the golden path is the best route a user can take to get the maximum possible value the product offers, across all of its functions and features. You can think of the golden path as a sort of "mega-happy path."


## What is happy path testing?


One of the earliest tests run on a product is a happy path test. It's a test of the product's function under perfect conditions. Assuming the user does everything exactly as expected, does the product work? Until the answer is an unequivocal yes, there's no point in moving on to testing more complex scenarios.


### Happy path test example


We've just completed the alpha version of our food delivery app, and we're ready to run initial tests on the payment sequence. The happy flow for checkout looks like this:


In a happy path test, we're running the default scenario to determine whether standard inputs generate the expected outputs. In this example, the happy path test case is a completely average customer journey — a user who orders a standard amount of food, inputs their credit card info correctly, and doesn't try to cancel the order while processing.


## What is the "unhappy path"?


There's no consensus on what to call the opposites of happy paths, but popular terms include "unhappy paths," "sad paths," or "exception paths." These are the paths on which the user experiences lots of errors, pain points, and friction.


### Unhappy paths vs. edge cases


Some engineers refer to these paths as edge cases, but there's a slight (but important) difference. Edge cases are true outliers — extreme errors that are unusual, but still possible, for users to encounter. The errors encountered on unhappy paths are the result of common, intuitive user behaviors.


## What is unhappy path testing?


Happy path testing looks at how the product works under perfect conditions; unhappy path tests look at what happens when things go wrong. When the user steps off of the happy path, do they encounter an error, or does the UI guide them back on track? Do all potential errors have designated error messages?


### Unhappy path test example


Let's return to our food delivery app example and see what an unhappy path test might look like. Where before we were testing a "perfect" use case, here we're testing all possible alternative scenarios to find out whether unexpected user behavior still generates valid results.


What are the most likely alternate paths the standard user might take during the checkout process? They might:


- Input the incorrect credit card number
- Cancel their order while it's processing
- Close the app at different points in the middle of the flow


Edge cases are also part of unhappy path testing. What unusual or unexpected bad paths could a user pursue? They might:


- Add thousands of dollars of food to their cart
- Add items from many different sellers to one cart
- Try to remove cart items after their order is processed


The goal isn't to make sure the app actually allows the user to complete these actions but to verify that, under any error conditions, the product will respond by redirecting the user toward a valid user path.


Having a well-built user flow is key to building effective happy paths and completing useful, well-organized happy and unhappy path tests. For more information on how to make the best use of your user flow diagrams, check out our guide to[using the Flows feature](https://support.zeplin.io/en/articles/5877004-getting-started-with-flows) in Zeplin.
