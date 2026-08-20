---
schema_version: "1.0.0"
document_id: "8f1f5f252382fa25e0372726f57d9d00a5c100e2ffa694c93388c0ea3044c5ad"
company_key: "yc-pushbullet"
company: "Pushbullet"
source_id: "yc-pushbullet-rss-4ba42a362971"
canonical_url: "https://blog.pushbullet.com/2020/01/21/improving-our-sms-api"
published_at: "2020-01-21T06:00:00+00:00"
first_seen_at: "2026-07-25T20:08:48.518680+00:00"
fetched_at: "2026-08-20T03:16:18.952870+00:00"
content_hash: "sha256:b83ff999ba6d57584cebd3c91b82701bd90c349c3a3c44088f2debbd4148077a"
---

# Improving Our SMS API

**Today we’re making a huge improvement to Pushbullet’s public API by adding support for reliably sending SMS and MMS messages (including picture messages)!**


**[The documentation for our new SMS API calls is right here!](https://docs.pushbullet.com/#text)**


The[new SMS API calls](https://docs.pushbullet.com/#text) we’ve added make it easy to programmatically send text messages, group messages, and even picture messages.


The messages sent using our API come from your own phone and from that phone’s phone number. This means if the recipients have your contact info, they’ll know you sent them the message. Even better, any replies will go right to your phone as expected.


Additionally, you can send as many text messages as you want without needing to worry about paying per message like you would for services like Twilio.


**Important notes** : since the messages come from your own phone, they are covered by your phone plan with your carrier. If you have unlimited texts, you’re set. If you don’t, be aware that sending text messages using our API costs the same as sending texts yourself by hand. Additionally,[Pushbullet Pro](http://pushbullet.com/pro) is required to send more than 100 messages per month through Pushbullet.


**Reliably sending your messages.**


Our new API makes a lot of effort to ensure your messages get sent ASAP. Let’s go through how things work.


First, to send text messages your phone must be:


- Powered on
- Connected to WiFi or have data coverage
- Have Pushbullet signed in with SMS permissions granted


Now, as you may be thinking, having your phone connected to WiFi or data all the time isn’t really possible. Fortunately, we’re ready for that. If your phone is offline when you try to send a text message using our API, it will simply be queued for the next time your phone comes online and syncs.


In order to prevent stale messages from being sent, we do delete queued messages after 1 hour even if they have not been sent. We find this to be a reasonable trade-off between reliably sending messages and avoiding stale messages from suddenly going out if an old device comes back online.


**An simple example API call**


` curl --header 'Access-Token: ACCESS_TOKEN_HERE' --header 'Content-Type: application/json' --data-binary '{"data": { "target_device_iden": "device_iden" "addresses": \["+14159366621"\], "message": "The text message you want to send"}}' --request POST https://api.pushbullet.com/v2/texts`


As you can see above, you only need 3 pieces of information to send a text message or group message:


- **target_device_iden** - The Pushbullet iden of the phone that should send this text message.
- **addresses** - One or more phone numbers you want to receive this message (more than one sends a group message).
- **message** - The message contents you want to send.


**What about sending picture messages?**


Sending picture messages isn’t much harder. All you’ll need to do is upload the picture you want to send using[our Upload API](https://docs.pushbullet.com/#upload) , then include the file_url and file_type in the API call like this:


` curl --header 'Access-Token: ACCESS_TOKEN_HERE' --header 'Content-Type: application/json' --data-binary '{"data": { "target_device_iden": "device_iden" "addresses": \["+14159366621"\], "file_type": "image/jpeg" }, "image_url": "https://dl.pushbulletusercontent.com/example.jpg"}' --request POST https://api.pushbullet.com/v2/texts`


**We hope you like the new and improved SMS API we’ve added today!**


If you have some feedback for us or want to request more API improvements, consider starting a discussion on the[Pushbullet subreddit](https://www.reddit.com/r/PushBullet/) . We’re always checking in to see the new posts.
