---
schema_version: "1.0.0"
document_id: "590f0d89a1e5c48586b05c02f54dab946810409ad81940962dfad93cf38353cd"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/stablys-new-file-upload-feature"
published_at: "2024-12-08T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c65863c7ca97bf303a602d381e37e760bd5da825b4224578c593fae170d7ddf5"
---

# Testing File Uploads Just Got Easier

Great news, fellow testers and developers! 🎉 We're thrilled to announce a highly-requested addition to Stably's AI-powered testing framework: comprehensive support for file upload testing. If you've ever tried to automate tests involving file uploads, you know this has been a significant gap in many testing solutions – but not anymore.


## Why This Matters


Think about how many crucial user flows involve file uploads in modern web applications. From updating your profile picture during onboarding to sharing documents in a cloud storage platform, file uploads are everywhere. Yet, testing these features has traditionally been a pain point. You might have found yourself asking: "How do I reliably test that users can upload their profile pictures?" or "How can I verify our document sharing system works across different file types?"


Until now, testing these scenarios either required complex workarounds or, more commonly, fell back to manual testing. Neither solution is ideal when you're aiming for comprehensive, automated testing coverage.


## The Stably Solution: Simple Yet Powerful


We've approached this challenge with our usual philosophy: making complex testing scenarios simple and reliable. Here's how our new file upload testing feature works:


### Project-Level File Management


First, we've introduced a new file management system at the project level. Think of it as your test asset library. You'll find it in your project settings, where you can upload and manage all the files you need for your tests. This project-level approach means you can reuse the same files across different test groups – no need to upload the same image multiple times for different scenarios.


### Seamless Recording Integration


Here's where the magic happens. When you're recording a test in our no-code editor and click on a file upload button, something new appears: a Stably file selector window. This window gives you access to all the files you've uploaded to your project. Select your file, and watch as it gets uploaded to your website in real-time during the recording!


Let's break down what's happening behind the scenes:


1. You click a file upload trigger on your website
2. Stably recognizes this as a file upload operation
3. Our file selector window appears, showing your project's uploaded files
4. You select the appropriate file
5. Stably handles the technical complexity of the file upload
6. The file appears on your website, just as it would for a real user


### Real-World Example


Imagine you're testing a user registration flow that includes uploading a profile picture. Here's how you'd set it up:


1. Upload your test profile image to your Stably project
2. Start recording your test
3. Navigate to the registration page
4. When you click the "Upload Profile Picture" button, select your test image from the Stably file selector
5. Complete the rest of your test steps


That's it! No complex configuration, no dealing with file paths or system dialogs – just straightforward, reliable file upload testing.


## Looking Ahead


This new file upload feature is just one example of how we're continuously expanding Stably's capabilities. We're committed to making website testing as comprehensive and painless as possible, and we're rolling out new features at an incredible pace to support an ever-growing range of testing scenarios.


Remember, our goal at Stably isn't just to make testing possible – it's to make it easier and more reliable than ever before. File upload testing is a significant step forward, but it's just one of many exciting developments in our pipeline. Stay tuned for more features that will continue to transform how you approach website testing.


Want to try out our new file upload testing feature? Log into your Stably account and head to your project settings. We can't wait to see how you'll use this new capability to create even more comprehensive test suites for your applications! 🚀
