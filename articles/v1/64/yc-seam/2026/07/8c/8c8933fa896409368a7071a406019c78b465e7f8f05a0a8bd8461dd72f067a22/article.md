---
schema_version: "1.0.0"
document_id: "8c8933fa896409368a7071a406019c78b465e7f8f05a0a8bd8461dd72f067a22"
company_key: "yc-seam"
company: "Seam"
source_id: "yc-seam-news-import-2c1b4f47501f"
canonical_url: "https://www.seam.co/blog/announcing-seam-instant-keys"
published_at: null
first_seen_at: "2026-07-22T12:56:26.420023+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:bfbde26dc42a691fc19dccefbe0306c1d90e445ee9a55bf941aef7cf1a2704da"
---

# Announcing Seam Instant Keys

This week, we're excited to announce the beta release of Instant Keys! This powerful new product enables you to create and share mobile keys with a single API call. These keys are shared with just a link and leverage App Clip on iOS to completely eliminate the hassle of app installation.


# What is Seam Instant Key?


Instant Key is the fastest way to share access to spaces—whether for hotel guests, real estate visitors, or on-site contractors. With a single API call, you can create a mobile key and deliver it through text, email, or your own app.


Users simply tap a link to get a lightweight, native experience using iOS App Clips or Android Instant Apps. The entire process is designed to feel instant, intuitive, and secure.


Further, because Instant Key is hosted by Seam, this prebuilt solution enables you to delight your users, without distracting your development efforts from your core mission—connecting your hotel or apartment building. Using Seam Instant Keys means that you can go from 0 to 1 in minutes, with no additional DevOps or mobile development needed.


# No app, no account, just tap to unlock


Many access systems that use Bluetooth mobile keys ask guests to download an app, create an account, and complete a setup process—often just for a one-time visit. This hassle is a significant deterrent to adoption.


Instant Key skips all of that. Users receive a secure link that opens directly in their mobile browser, launches a native unlock screen, and connects over Bluetooth to open the door.


Instant Key is the most streamlined mobile access experience for Bluetooth-enabled locks available today.


# Flexible and embeddable


Once you use the Seam API or Seam Console to grant access to a user, Seam returns an Instant Key as a shareable URL. You can deliver the Instant Key in several ways, depending on your product flow and guest experience.


For most use cases (hotels, self-touring, contractors), the simplest way to deliver a Instant Key is using a link in a text message or email. This method works well if you already collect the guest’s phone number or email at booking.


If you have a guest-facing web app or booking portal, you can embed the Instant Key directly into your interface. For example, you could show it in a reservation detail page or check-in flow with a **Mobile Key** or **Unlock Door** button.


# Simple, intuitive API


With a single API call, create an access grant and issue an Instant Key. Share the returned Instant Key URL with your user.


```text
1  const   accessGrant   =     await   seam  .  accessGrants  .  create  (  {
2      user_identity  :     {
3  	    email_address  :     "jane_doe@example.com"
4      }  ,
5      space_ids  :     [
6      room_101_space_id  ,
7      common_space_id
8      ]  ,
9      requested_access_methods  :     [
10        {     mode  :     "mobile_key"     }  ,
11      ]  ,
12      starts_at  :     "2025-07-13T15:00:00.000Z"  ,
13      ends_at  :     "2025-07-16T11:00:00.000Z"  ,
14   }  )  ;
15
16   const   instantKeyUrl   =   accessGrant  .  instant_key_url  ;
17   // https://ik.seam.co/SRTCDE --> Send this Instant Key URL to your users.
```


# Customizable and available in your users’ languages (Coming soon!)


We know that hotels and apartments care about adding custom branding and localization. With Instant Key, you’ll be able to customize your users’ experience.


# Supports all major access systems


Seam Instant key supports the following access systems:


- Salto Space
- Salto KS
- ASSA ABLOY Visionline (coming soon)
- ASSA ABLOY Vostio (coming soon)


# Ready to unlock with Instant Key?


Whether you’re building a self-check-in flow or adding mobile access to your guest app, Instant Key makes it simple to get started.


→ Learn more in[our docs](https://docs.seam.co/latest/capability-guides/instant-keys) .


→[Talk to Sales](https://www.seam.co/contact-us) to explore your use case or request a demo.
