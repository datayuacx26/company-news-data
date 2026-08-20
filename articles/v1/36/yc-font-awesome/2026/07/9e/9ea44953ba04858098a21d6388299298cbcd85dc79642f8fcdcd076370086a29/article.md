---
schema_version: "1.0.0"
document_id: "9ea44953ba04858098a21d6388299298cbcd85dc79642f8fcdcd076370086a29"
company_key: "yc-font-awesome"
company: "Font Awesome"
source_id: "yc-font-awesome-atom-991752a735d6"
canonical_url: "https://blog.fontawesome.com/wa-3-11/"
published_at: "2026-07-30T18:41:24+00:00"
first_seen_at: "2026-07-31T23:16:20.418483+00:00"
fetched_at: "2026-07-31T23:16:21.635478+00:00"
content_hash: "sha256:2fa1fd67f10772f9aa3f2fa0f0a0db03834c5fe8cc636b44c1d0d3ef978191e8"
---

# WA 3.11: Data Grid, New Components, Plus Free Toast

*Everything in its place. 3.11 is about giving rows, pages, characters and parts the structure they’ve been missing.*


📊 **A Place for Your Data**


[<wa-data-grid>](https://webawesome.com/docs/components/data-grid) has arrived as experimental for Pro users. Sort, filter, search, select, reorder, resize and paginate; every row and column falls into place. Large datasets are virtualized automatically, only the rows on screen ever hit the DOM, so scrolling stays smooth whether you’ve got 50 rows or 50,000.


Set` data` and` columns` , use the features you need, and let the grid do the heavy lifting.


Don’t have Pro?[Get it here](https://webawesome.com/purchase)


🔢 **A Place for Every Page , and A Place for Every Character**


Two new experimental components have been added to core, each one giving content some structure:


[<wa-pagination>](https://webawesome.com/docs/components/pagination) – set` total` and` page-size` , and watch long lists get carved into clean, numbered pages with previous/next controls, ready to sync via the` wa-page-change` event. Pairs naturally` wa-data-grid` , but works anywhere you’re paging through results.


[<wa-otp-input>](https://webawesome.com/docs/components/otp-input) – a proper segmented input that gives verification code exactly as many slots as you need. With paste handling and keyboard navigation built in, set` length` and your’e off.


**🍞 Toast Finds a New Home in Core**


[<wa-toast>](https://webawesome.com/docs/components/otp-input) and[<wa-toast-item>](https://webawesome.com/docs/components/toast-item) moved from Pro to Core. Now everyone gets stackable, top layer notifications for success messages, errors, and status updates right out of the box. No upgrade required.


Drop a single` <wa-toast>` on your page and call` .create()` whenever you need to say something.


🎠 **Carousels Slides Fall in Line**


[<wa-carousel>](https://webawesome.com/docs/components/carousel) now has` addSlide()` and` removeSlide()` methods. So you can rearrange slides in and out on the fly, even with loop enabled. Perfect for carousels that use live data instead of a fixed set of slots.


🧩 **Every Part Gets a Proper Name**


Every component that renders a wrapper element now exposes a CSS part named after the component itself (button, details, carousel, etc.) alongside the current` base` part. Same goes for form control labels, which now expose` form-control-label` .` base` is softly deprecated, it’s not going anywhere for now. This update simply gives you a clearer name to reach for going forward.


🧰 **Loose Ends? Tidied Up**


As always, WA 3.11 ships with a bunch of fixes, color picker alpha handling, tree keyboard navigation, dialogs staying open when ad blockers hide them, and much more. Full details in the[changelog](https://webawesome.com/docs/resources/changelog#wa_3110) .


Until next time! ✌️
