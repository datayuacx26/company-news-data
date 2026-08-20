---
schema_version: "1.0.0"
document_id: "a7b07de0d598e9a0c2aeb1ec626260477c889c1a550c59bdf6f60716345ee7b1"
company_key: "yc-screenleap-inc"
company: "Screenleap"
source_id: "yc-screenleap-inc-rss-5ba46d8925b4"
canonical_url: "https://blog.screenleap.com/2021/05/14/installation-free-sharing-is-now-available-for-the-screenleap-api/"
published_at: "2021-05-15T06:56:45+00:00"
first_seen_at: "2026-07-25T22:26:02.059328+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:4950d19dbf70140d1c03db8f974baf37633278134c7d66ad75425f9f33f619fd"
---

# Installation-Free Sharing Is Now Available for the Screenleap API

You are here:


[Home](https://blog.screenleap.com/) ›


Installation-Free Sharing Is Now Available for the Screenleap API


# Installation-Free Sharing Is Now Available for the Screenleap API


By


[Screenleap](https://blog.screenleap.com/author/admin/)


on


May 14, 2021 in


[Screenleap](https://blog.screenleap.com/category/screenleap/)


We are excited to share that the installation-free sharing option that has been available on the Screenleap site is now available in our API. You will now be able to offer your users the ability to start share sessions from your website and allow them to interact with other users through screen sharing, 2-way audio conferencing, and video conferencing—all without needing anyone to install any software. All they need is a modern web browser.


**Installation-Free Sharing**


With this release, you will now be able to offer installation-free sharing and viewing to all your users who are using a modern desktop browser (Chrome, Edge, Firefox, and Safari). Viewers will be able to continue to take advantage of the convenience of installation-free viewing while presenters will now also benefit from the convenience and benefits of installation-free sharing. This includes never needing to worry about needing to install a native app in order to join a share session or having to worry about a mandatory app update at an inopportune time right before an important meeting.


**Default Presenter UI**


To make it even easier for you to integrate with installation-free sharing, we have added a default presenter UI that you can use to get started with. The default presenter UI will show toggles for all enabled options as well as buttons for change the screen source and for stopping the share session.


If you want to create a completely custom UI for your integration, you can disable the default presenter UI by passing` useDefaultPresenterUI=false` in your request to create a new share session.


**Audio Conferencing and Video Conferencing**


Our long-term goal at Screenleap has always been to make it ridiculously easy for you to enable live-collaboration on your website or web app. As a step towards achieving that goal, we are excited to also announce that you can now enable 2-way audio conferencing and video conferencing when using our new installation-free sharing option. This will allow you to give your presenters the ability to chat with their viewers through their computer’s microphone and speakers and also share their screen and webcam video with them. 2-way audio conferencing is currently in beta and video conferencing is currently in an experimental state, so they are not always guaranteed to work. Please alert us of any issues that you discovered so we can investigate them.


To enable 2-way audio conferencing, add` enableComputerAudio=true` to your create share session request. To enable video conferencing, add` enableVideoConferencing=true` to your create screen share request.


**New API Defaults**


We have updated some of the defaults that you can specify in your requests to create an API share session and in your viewer URL. Please note the following changes to the defaults:


- **includeDefaultPresenterUI** – We are now supporting a default presenter UI for installation-free API share sessions. It will be enabled by default. If you would like to disable it so you can create your own custom presenter UI using our lower-level API calls, you will need to include` includeDefaultPresenterUI=false` in your create share session request.
- **showResize** – This viewer URL parameter will now default to true. If you do not want the resize controls to be visible, please include` showResize=false` in the viewer URL.
- **showStop** – This viewer URL parameter will now default to true. If you do not want the stop button to be shown to viewers, please include` showResize=false` in the viewer URL.


**Even Easier Integration Coming Soon**


Currently, we do not recommend using a JavaScript-only integration as it exposes your API credentials to everyone, which allows nefarious users to create share sessions using your account. To make it even easier to integrate with your website in the future, we are working to support a JavaScript-only integration that does not expose your API credentials so you don’t need to make a server-side call to create the share session. Please stay tuned for more information about this simpler integration option.


**We Would Love Your Feedback**


Please let us know what you think about our new installation-free sharing option and if they are any changes we can make to make it even more useful to you!


[Screenleap API](https://blog.screenleap.com/tag/screenleap-api/)


[← Screenleap’s Next Evolution: Installation-Free Sharing With Add-On Support](https://blog.screenleap.com/2021/02/17/installation-free-sharing-with-add-on-support/)


[Full-Screen Video Conferencing Now Available For Your Meetings →](https://blog.screenleap.com/2021/07/07/full-screen-video-conferencing-now-available-for-screenleap-meetings/)


##### No comments yet.


### Leave a Reply[Click here to cancel reply.](https://blog.screenleap.com/2021/05/14/installation-free-sharing-is-now-available-for-the-screenleap-api/#respond)


### Stay Updated!
