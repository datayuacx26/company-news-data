---
schema_version: "1.0.0"
document_id: "360f27bb133023d60b0d15ffc6a3733b4104ef909a8a696417bb24e705eefb67"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/generate-subtitles-with-zapier"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:358a5a9b6b0b6267ef4da4fbe6610c3e30e65739deb45745804fb86da9a2e115"
---

# Generate subtitles with AssemblyAI and Zapier

With the AssemblyAI app for Zapier you can apply Voice AI to your audio and video files. In this tutorial, you'll learn how to transcribe a video, then generate SRT subtitles for it, and finally, upload the subtitles to Google Drive.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#prerequisites) Prerequisites


You'll need these to follow along:


- Basic experience with creating Zaps on Zapier
- A Zapier account with an upgraded Zapier plan to create multi-step Zaps
- A Chromium-based browser ([Google Chrome](https://www.google.com/chrome/?ref=assemblyai.com) /[Microsoft Edge](https://www.microsoft.com/en-us/edge/download?form=MA13FJ&ref=assemblyai.com) /etc.)
- [The Zapier Chrome extension](https://zapier.com/l/install-chrome-extension?ref=assemblyai.com)


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#create-your-zap) Create your Zap


[Log into your Zapier account](https://zapier.com/app/login?ref=assemblyai.com) and create a new Zap.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#configure-trigger) Configure trigger


Click on the empty trigger and search for the "Zapier Chrome extension".


This trigger lets you trigger your Zap at will using the Chrome extension, and provide additional fields that your Zap needs.


Select "New Push" as the **Event** , and click **Continue** .


On the **Trigger** tab, add two new **Input Fields** :


- **Video File** : You will use this field to pass the URL to the video file you want to transcribe
- **Subtitles File Name** : You will use this field to configure the name of the subtitles file in Google Drive.


Click **Continue** .


Click **Find new records** , and select the most recent record in the list.
This record should include your two new fields. If it doesn't, click on **Find new records** again.


Click on the pencil icon at the bottom right to create a duplicate record that you can edit.
Update your custom fields with values of your choice, or use our sample video:


- **Fields Video File** :[https://storage.googleapis.com/aai-web-samples/hanniballecter.mp4](https://storage.googleapis.com/aai-web-samples/hanniballecter.mp4?ref=assemblyai.com)
- **Fields Subtitles File Name** : hanniballecter.srt


Click on **Continue with selected record** .


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#transcribe-audio) Transcribe audio


Click the empty action, search for AssemblyAI, and click on the AssemblyAI app.


Select the "Transcribe" **Event** .


If this is your first time using AssemblyAI with Zapier, you'll need to create a new connection.
Create a new connection and enter your AssemblyAI API key. If you don't have an AssemblyAI account, first[sign up for free](https://www.assemblyai.com/dashboard/signup?ref=assemblyai.com) , then copy and paste your API key from the[account settings](https://www.assemblyai.com/app/account?ref=assemblyai.com) . Finally, click **Yes, Continue to AssemblyAI** .


Now that your connection is configured, click **Continue** .


Next, map the` Fields Video File` from the Chrome extension trigger into the **Audio File** field, then select the **Language Code** of your video or set **Language Detection** to` True` . You can find the[list of supported languages and features](https://www.assemblyai.com/docs/getting-started/supported-languages?ref=assemblyai.com) in the docs.


Click **Continue** .


Test the action. Remember that we're always returning sample data during testing due to Zapier limitations.
During a real Zap run, the action will be executed, and real data will be returned.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#generate-subtitles) Generate subtitles


Add another action, select the AssemblyAI app, but select the "Get Transcript Subtitles" **Event** , and click **Continue** . The previously created AssemblyAI connection should already be selected. Click **Continue** again.


Map the` ID` field from the transcribe step into the **Transcript ID** field. Click **Continue** .


Test the action. This action also returns sample data during testing.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#upload-subtitles-to-google-drive) Upload subtitles to Google Drive


Add one last action. Search for the Google Drive app and select it. Select the "Create File From Text" **Event** . Click **Continue** .


If you haven't already, create a connection to your Google Drive, and click **Continue** .


Configure the action as you wish. Then map the` Fields Subtitles File Name` into the **File Name** field, and map the` Subtitles` into the **File Content** field. Click **Continue** .


Feel free to test the action. Finally, publish the Zap.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#run-your-zap) Run your Zap


To run your Zap, click on the Zapier Chrome extension in your browser. Log in if you haven't already.


Then, click on your Zap, fill out the fields with your video file URL and subtitles file name, or use these for testing:


- **Video File** :[https://storage.googleapis.com/aai-web-samples/hanniballecter.mp4](https://storage.googleapis.com/aai-web-samples/hanniballecter.mp4?ref=assemblyai.com)
- **Subtitles File Name** : hanniballecter.srt


Click **Send** .


Shortly, your Zap will run, and you will see the subtitles file appear in your Google Drive. You can see the result of your Zap run under "Zap runs" of your Zap.


## [#](https://www.assemblyai.com/blog/generate-subtitles-with-zapier/#next-steps) Next steps


You successfully put together a Zap that generates subtitles for your videos, using the Chrome extension trigger, the AssemblyAI transcribe & get transcript subtitles action, and the Google Drive Create File From Text action.


You can integrate AssemblyAI's Voice AI with many other services in Zapier. The Transcribe action that you used has a lot more parameters that you can configure to enable AI models and change the output of the transcript. You can learn more about[the AssemblyAI app for Zapier in our documentation](https://www.assemblyai.com/docs/integrations/zapier?ref=assemblyai.com) .


‍
