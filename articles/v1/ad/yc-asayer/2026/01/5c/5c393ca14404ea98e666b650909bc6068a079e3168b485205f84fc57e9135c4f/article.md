---
schema_version: "1.0.0"
document_id: "5c393ca14404ea98e666b650909bc6068a079e3168b485205f84fc57e9135c4f"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/popup-child-window-not-recorded-in-openreplay-v16/556"
published_at: "2026-01-15T11:48:45+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:d2e68766dfe3cbd8d3d87dd61e6006ab46df329ea8e062063793f0d0878f18a0"
---

# Popup (Child Window) Not Recorded in OpenReplay v16

[akhileshulhe](https://forum.openreplay.com/u/akhileshulhe)


January 15, 2026, 11:48am 1


Is popup window recording supported in OpenReplay v16 integrated in react?
Summary:
OpenReplay is not recording activity in a child window (popup). Only the parent window is captured.


Current Behavior:


-


Parent window is recorded when` start()` is called.


-


Popup window is not recorded at all.


Reason / Analysis:


-


` start(true)` is executed in the parent window context, so tracking restarts only in the parent, not the popup.


-


The popup runs in a separate JavaScript context and does not initialize its own OpenReplay tracker instance.


-


Attempting to initialize OpenReplay in the popup results in a “one tracker instance has been initialized already” error, suggesting a limitation in OpenReplay v16 regarding multiple tracker instances across windows.


[mehdi](https://forum.openreplay.com/u/mehdi)


January 17, 2026, 12:35am 2


Does the popup contain an iFrame? Also can you share your tracker configuration?


[akhileshulhe](https://forum.openreplay.com/u/akhileshulhe)


January 20, 2026, 4:48am 3


No, the popup doesn’t contain an iframe. It’s a separate DOM, a child window opened using` window.open` , while the state and overall logic are managed by the parent application.


[mehdi](https://forum.openreplay.com/u/mehdi)


January 21, 2026, 1:02am 4


Is your app publicly accessible so we can try to reproduce it?


[akhileshulhe](https://forum.openreplay.com/u/akhileshulhe)


January 21, 2026, 5:51am 5


Unfortunately, it’s not publicly accessible. it’s internal application


[mehdi](https://forum.openreplay.com/u/mehdi)


January 21, 2026, 5:02pm 6


Theoretically it should show up as a` tab` in the session replay. Did you disable tab tracking in the tracker constructor/options?


If you can join our[Slack](https://slack.openreplay.com/) , this way it’s quicker to chat and resolve the issue.
