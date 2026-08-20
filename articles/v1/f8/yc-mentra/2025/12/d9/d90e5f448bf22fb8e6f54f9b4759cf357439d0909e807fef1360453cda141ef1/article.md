---
schema_version: "1.0.0"
document_id: "d90e5f448bf22fb8e6f54f9b4759cf357439d0909e807fef1360453cda141ef1"
company_key: "yc-mentra"
company: "Mentra"
source_id: "yc-mentra-atom-1275414bf67e"
canonical_url: "https://mentraglass.myshopify.com/blogs/blog/making-mentra-live"
published_at: "2025-12-21T19:54:03+00:00"
first_seen_at: "2026-07-27T03:44:49.649440+00:00"
fetched_at: "2026-08-20T00:50:23.357001+00:00"
content_hash: "sha256:c411e10e8ad78753550dee7ea14bdc7bd8506105cf92a3e1dc09bb212ab1a2dd"
---

# Making Mentra Live

## Making Mentra Live


Thank you to everyone who has ordered Mentra Live, attended hackathons, and given your feedback and requests – you’ve helped shape the direction of Mentra Live. This post is a deeper look at the technical decisions we made and challenges we faced in the journey of building Mentra Live.


We originally planned to ship Mentra Live in summer 2025. As we approached our shipping date and used our own glasses, we saw some critical issues that we needed to address before we could ship them out. Our goal throughout has always been to build an incredible pair of smart glasses that we ourselves wear all-day everyday. In the last 6 months, we made that happen by upgrading every aspect of the experience. Here are the changes we made:


## Upgraded Camera


We wanted top notch image quality, and the first camera that we selected wasn’t cutting it, so we moved from an 8MP camera to a 12MP smart phone grade camera.


The new camera is significantly smaller, which decreased weight and bulk.


## Completely redesigned the frame to be light and sexy


We dropped from ~50 grams to 43 grams, and designed new hinges and nosepads for all-day wearability.


## Upgraded the processor to support live streaming


We originally used an MTK6765 processor. This worked decent for taking pictures, videos, but we encountered an issue with live streaming: after 15+ minutes of high quality streaming, the processor would get too hot and overheat. We solved this in a few ways:


- Upgraded to a more efficient MTK 8766 processor
-


Redesigned Mentra Live’s main circuit board to physically separate the WiFi chip and the processor to better disperse heat


## Upgraded the WiFi antenna for fast image transfers and live streaming


We previously could take pictures and videos, but transferring them was slow. Live streaming was also bandwidth-limited. So we upgraded the WiFi antenna to support WiFi-5Ghz, 1080p video streaming, and fast photo/video sync.


## Upgraded the speakers for awesome sound playback


We love taking calls and listening to music/podcasts on Mentra Live. So we swapped the speakers to premium open-ear hardware and tuned them in a pro audio lab.


## Designed a custom magnetic charging cable for all day battery


From talking with the Mentra community, it was obvious Mentra Live needed these two things:


1. All-day battery life while taking pictures, videos, and live streaming
2. The ability to be programmed easily


To enable both of these, we developed a custom magnetic charging cable. It’s designed to be thin and lightweight like earbuds, and magnetically clips on to the back of Mentra Live. You can use it to charge Mentra Live you’re wearing it. You can plug it into your iPhone, a power bank, or any other USB-C power source. It also works as a USB cable, so developers are free to access, modify, and replace Mentra Live’s software.


## How we got the glasses microphone to work all the time with iOS and Android


Existing AI glasses connect to your smart phone as normal Bluetooth headphones. That’s great, because it allows you to take calls, listen to music, and watch videos using your glasses speakers and microphone.


However, this has a major drawback. If you’re watching a video, taking a call, listening to music, or another app is using sound on your phone, then your glasses apps can no longer hear.


To solve this, we designed a custom audio pathway in MentraOS. That means that no matter what you’re doing on your phone, your glasses apps can always use the glasses microphone.


Here’s how it works: Mentra Live has a special microphone set aside for this custom channel. On the glasses, that microphone is recorded, compressed to be very small (using LC3), and then sent directly to the Mentra app over Bluetooth Low Energy (BLE GATT). From the phone’s perspective, no microphone is active. This avoids system mic permissions, OS ownership, UI indicators, and platform restrictions, while allowing continuous audio streaming and instant start/stop behavior.


We run both paths in parallel—standard Bluetooth audio for calls and media, plus our custom BLE mic channel for apps—so no matter what your phone is doing, the glasses can always hear.


## How we almost fumbled (and then fixed) the Charging Case


After finalizing our charging case design and moving forward for our first production run of 1,000 units, we found a problem. If the case was held upside down and shaken (as might happen in a backpack) then the charging would stop. We halted case production until we could find a solution.


First, we made the case spring stronger. This ensures the glasses stay in place and don’t move fall away when upside down. Then, to ensure continuous connection, we and made the charging pins longer.


## Conclusion


Early this year, we set out to build a great pair of AI Glasses with an open app ecosystem.


Through this year, we’ve worked endlessly on that vision. Now, half our team is wearing Mentra Live every day as their normal glasses. We’re taking calls, filming our weekend adventures, live streaming to X, and exploring new cities with AI tour guides, and we can’t wait for you to join us.


- Cayden, Israelov, and the Mentra team
