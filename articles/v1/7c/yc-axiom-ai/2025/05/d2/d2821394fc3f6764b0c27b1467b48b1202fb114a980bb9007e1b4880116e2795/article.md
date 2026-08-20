---
schema_version: "1.0.0"
document_id: "d2821394fc3f6764b0c27b1467b48b1202fb114a980bb9007e1b4880116e2795"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/making-your-automations-mobile-with-apple-shortcuts/"
published_at: "2025-05-06T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:befec4ad54e46f29a3089e3f21dbf93aa032094482591a7bb23bcd8343024bcd"
---

# Making Your Automations Mobile with Apple Shortcuts

Apple Shortcuts is an application that comes pre-installed on most Apple devices and can provide a powerful solution for performing automations on your devices. It provides access to various system events that can be used to start a Shortcut, we will explore this later in this article.


One of the benefits of using Apple Shortcuts is that the shortcuts that you create will be available on any device that supports Shortcuts, and that you have signed into your iCloud account on. For instance, you can create a shortcut on your Mac that can then be run on your iPhone, or iPad, and vice versa.


Your Shortcuts live and run on your Apple device - which can be great from a privacy and security point of view as it means that none of the data that you are processing with it leaves your device. This is similar to when you run an axiom.ai automation on your computer, no information leaves your device unless you explicitly instruct it to.


As Shortcuts runs on your device, it can tie into the applications on your device - most applications provide steps to Shortcuts that can be used. For example, if you wanted to read data from the Apple Health application, you could do so with a Shortcut and then share this information to another application or service. Or, a Shortcut can be used to write data to another app, such as creating a new calendar event when the Shortcut is run.


## Triggering a Shortcut


There are multiple methods that can be used to trigger your Shortcut that are provided by the Apple Shortcuts application, let’s explore them.


### Manually


Shortcuts can be triggered manually by either navigating to the Shortcuts application and selecting the one you wish to trigger, adding it to your Control Centre, or even adding it to your homescreen as a widget. This can make triggering super convenient and available right at your fingertips. These can be triggered by Siri as a hands-free option by saying, "Hey Siri," followed by the Shortcut name.


Some additional options are available to manually trigger a shortcut on macOS devices, such as adding the Shortcut to your Share Sheet, Quick Actions, or Menu Bar for easy access.


### Automatically


The other option is to have this automatically triggered. Apple provide a wide range of triggers that can be set up to automatically trigger your Shortcuts, such as:


- Time of day
- When your alarm stops
- When you reach or leave a location
- When you receive an email or message
- When a device setting has been changed, such as connecting to a Wi-Fi network
- When an application is opened
- When your battery reaches a certain percentage
- When a focus mode has been activated/deactivated


These can be very convenient when requiring actions to be performed at certain times, or under certain conditions. For example, a personal favourite of mine is when I enable the “Sleep” focus mode on my iPhone, this will prompt me to turn off all of the lights in my home.


*Note: as of time of writing, automatic triggers are currently only available on iOS and iPadOS devices.*


### Via axiom.ai


Even though axiom.ai does not officially integrate with Apple Shortcuts, there is a clever workaround that can be used to trigger your Shortcuts from an axiom.ai automation. To do this, create an automation in axiom.ai as normal. Once you are ready to trigger your Shortcut, add a[Send an email](https://axiom.ai/docs/no-code-tool/reference/steps/send-an-email) step to your automation and set this to send an email to an email address that you have set up on your Apple device.


Next, create a new Shortcut that triggers when an email has been received. We would recommend setting the “Subject Contains” field to match the subject that you have added in your axiom.ai automation. Click “Run” in axiom.ai to test your automation and confirm that the Shortcut has been triggered - you’ll also see the email in your Mail inbox to confirm receipt.


*Note: your axiom.ai automation will continue to run after the email has been sent, it does not have oversight over the Shortcut on your device.*


## Triggering an axiom.ai automation


While axiom.ai does not officially integrate with Shortcuts, as we do not provide an iOS application, you can still trigger your automations from Shortcuts. This means that you can automatically trigger your axiom.ai automations as part of your workflow.


To get started, create your Shortcut as normal, when you are ready to trigger your axiom.ai automation, add a “Get Contents of URL” step to your Shortcut. Change the method to “POST” and add two key/value pairs in the “headers” section of this step, as follows:


- key:` <your api key>`
- name:` <your automation name>`


Once you run your Shortcut, you will see your axiom.ai automation run a few moments later.


You can learn more about this in our[How to use Apple Shortcuts to trigger an axiom.ai automation](https://axiom.ai/guides/apple-shortcuts) guide.


## Use cases


There are a bunch of use cases that can be used to take advantage of this combination of tools, such as:


### Social media automation


Shortcuts have the ability to interact with most social media applications that are installed on your device. Automatically posting to your social media account via your Apple device can be even more useful when you trigger your Shortcut via axiom.ai. This can be a useful alternative to automating social media when using multiple accounts without relying on sessions or steps to log in and out of multiple accounts.


### Workout tracking


Your iPhone has the ability to trigger a Shortcut when a workout has been started or ended on your Apple Watch - this can be used to share information with your axiom.ai automation using the “Health” steps provided, this could be used to log this data in a[Google Sheet](https://axiom.ai/docs/no-code-tool/integrations/google-sheets) for further analysis, or other services such as[Supabase](https://axiom.ai/guides/supabase) .


### Credit card transactions


Transactions can be used to automate the triggering of Shortcuts - these will be triggered when an Apple Pay linked card has been used to make a purchase. You can configure this based on what type of card you would like notifications about, and if you only want certain types of transactions to trigger the Shortcut. This can be useful to log Apple Pay transactions for later analysis on spending.


## Wrapping up


Combining the power of Apple Shortcuts with the power of axiom.ai allows you to create a chain of automation that can be triggered from your Apple devices, or can trigger events to occur on your Apple devices.


With Apple Shortcuts' ability to integrate with system applications, and downloaded applications, on your device, this gives you an opportunity to expand the functionality offered to your automations. From automatically triggering an axiom.ai automation based on a change in device settings via Shortcuts, to triggering a Shortcut based on criteria set within your axiom.ai automation, the possibilities are endless.


We’re excited to hear about what you build with Apple Shortcuts and axiom.ai, let us know over in our[community](https://reddit.com/r/axiom_ai) .
