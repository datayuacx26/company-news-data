---
schema_version: "1.0.0"
document_id: "7ce3125d831bd88fb60fb60a41816e06ce041cb6d379565f653135343ef6908f"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/getting-to-know-the-trycatch-step/"
published_at: "2025-04-22T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:e40f86a26ec8a17ec3cdda6c19ab6700655a9859630b9384b7935844de0b0056"
---

# Getting to Know the Try/Catch Step - Tips and Tricks

A critical concept to understand development or automation building is how to handle errors within your workflows in a way that does not cause the workflow to stop. The idea of a Try/Catch has existed in programming for a long time and this is something that we brought into axiom.ai to help you manage your automations.


The[Try/Catch](https://axiom.ai/docs/no-code-tool/reference/steps/try-catch) step is a step within axiom.ai that can be used within your automations to gracefully catch any errors that you may encounter. Let’s look at an example of when this may be applicable:


You have an automation that visits a website and clicks a button - however, this button does not always appear on the page. Set up without the “Try/catch” step, this would cause an error within your automation and the automation will stop. Instead, if you wrap the “Click element” step within a “Try/catch” step you can add additional steps inside the “catch” portion that will be run when the error occurs - you could try to click an additional button or simply just send an email letting you know that the error occurred. The steps after the “Try/catch” step will still run in this case.


## Usage


To get started, add a “Try/catch” step to your automation. You’ll notice that this step has two sections, a “try” and a “catch” section. You can use the “+ Add substep” button to add steps into these sections.


There are a couple of ways that you can implement this step into your automation - either proactively or reactively. Proactively adding this to your automation allows you to ensure that you are more likely to have a successful run the first time the automation is run, if you believe that there may be issues or if you are aware that the site is likely to change quite regularly. Adding these reactively would mean that you add these to your automation based on errors that you have experienced during the running of your automation.


If you are familiar with development, the “Try/catch” step works exactly like it would when you are writing code yourself so you can likely understand the concept and best practices.


### Try


The “try” portion of this step can be used to add steps that you believe may cause an error within your automation. The steps will run as normal in the flow of your automation and have access to the data tokens from the rest of the automation. You can imagine these running as if the “Try/catch” step was not present.


### Catch


The “catch” portion of this still can be used to add steps that are run if an error occurs within the steps included in the “try” section. These steps will not be run during normal operation of the automation and will only run when an error occurs with the steps in the “try” section, if no error is encountered, these steps will never run within your automation. We don't recommend adding steps that you need to run every time your automation runs as this may cause additional errors in your automation outside the "Try/catch" step if the steps aren't run every run.


## Handling errors


When an error occurs within your automation, there are a few methods that you can use to handle it. We are going to concentrate on the “catch” portion of the “Try/catch” step for this blog. There are many things that you can do once your automation has encountered an error.


> 💡 To learn more about error handling, you can check out our[Error Handling](https://axiom.ai/docs/no-code-tool/troubleshooting/error-handling) documentation where you will find more advice on handling errors.


## Notifications


If you have your automation scheduled, you’re going to want to know if the automation runs into any errors that occur so you can jump in and make any changes that are required. There are a few steps that you can make use of to alert you or another system, such as:


- [Send an email](https://axiom.ai/docs/no-code-tool/reference/steps/send-an-email)
- [Trigger a webhook](https://axiom.ai/docs/no-code-tool/reference/steps/trigger-webhook)


Personally, the “Trigger a webhook” can be quite useful for alerting other systems that there has been an issue, for example, you could have this trigger a[Slack workflow](https://axiom.ai/guides/slack) webhook that can alert your team, if you have multiple people who require the results of the automation.


Simply adding these steps and configuring them in the “catch” portion of the “Try/catch” step will be sufficient to catch any errors.


## Ending the run


There may be situations where you wish to end the run in the event of the error rather than continue with the other steps in your workflow. For example, if you want the automation to finish if a button is not present on a page. To do this, you can use the[End run](https://axiom.ai/docs/no-code-tool/reference/steps/end-run) step within the “catch” portion of the automation - once your automation reaches this step it will stop the automation run. This will be paired with a[notification](https://axiom.ai/docs/no-code-tool/reference/settings/run-options/notifications) to alert you to this.


## Triggering additional steps


It’s important to remember that when an error occurs, it is caught by the “catch” portion of the “Try/catch” step, once the steps inside the “catch” portion are completed the rest of the steps within the automation will run.


However, there may be instances where you wish to trigger additional steps when an error occurs with a certain step. For example, if you have a button being clicked inside the “try” portion, and if this is not found an error is caught by the “catch” portion, you may want to attempt to click another button - worth noting though that any errors inside the “catch” portion will not be caught and will cause your automation to fail, consider nesting “Try/catch” steps if you are worried about this.


You may also want to skip certain steps within your workflow, you can use the[Jump to another step](https://axiom.ai/docs/no-code-tool/reference/steps/jump-step) step in the “catch” section to navigate between your steps in your workflow to create a more dynamic workflow.


## Wrapping up


Handling errors gracefully can help ensure that your automations can dynamically adapt to errors that it may encounter when working with websites that may be changing. The “Try/catch” step allows you to decide the actions that are performed in the event of an error so that your automation can continue to run and achieve your goals. This is a great tool for error handling within your automations to prevent them from stopping when they encounter an error, if required.
