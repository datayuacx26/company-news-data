---
schema_version: "1.0.0"
document_id: "1f0f464c4ab077032c112163080c2925c083986ae9e791f1ee24b5a05db6f9e1"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/see-any-x-accounts-exact-follower-count"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:e0fbb5b3318a359ab6668dfe10beafc7661a0895c20582ec3ffaa79d80fba7d5"
---

# How to See Any X Account's Exact Follower Count

Once an account passes ten thousand followers, X stops telling you the exact number. A profile with 9,847 followers gets all four digits, but NASA gets "92.2M", on the profile and everywhere else the count appears. Your browser already has the full number, though: X's API returns it on every page load, and the interface rounds it off before it reaches the screen.


Even the rounded number lives in one place. To check how many followers someone has, you leave whatever you were reading, open their profile or wait for a hover card, then find your way back. Repeat for every "who is this?" moment in a day of scrolling.


[X Followers Count on Every Post](https://www.tweeks.io/t/81b31e494c8a4a3d9e8895aa) closes both gaps. It's a tweek that writes each author's follower count directly into the post header, between the handle and the timestamp, for every post you scroll past. And because it's a tweek, turning that rounded 92.2M into the exact figure is a one-sentence edit, covered below.


## Follower Counts in Every Post Header


Once installed, each post header gains one gray label: name, handle, the follower count, then the usual dot and timestamp. It uses X's own font and secondary text color, so it reads like something X shipped. Here it is on a post from NASA's profile:


It works on x.com and twitter.com, in the home timeline, in replies, and on profile pages, with counts appearing a moment after each post renders.


It changes how replies read. A confident correction lands differently coming from an account with 43 followers than from one with 43,000, and until now that context cost a profile visit per reply. Engagement farmers are easier to spot too: the pattern of a large following and nothing but generic replies stands out when the number sits next to every post.


## Setup


1. Install the[Tweeks extension](https://www.tweeks.io/get-extension) for Chrome or Firefox.
2. Open the[X Followers Count on Every Post](https://www.tweeks.io/t/81b31e494c8a4a3d9e8895aa) share page. The source is 567 lines, and all of it is on the page if you want to read what you're installing. Click install.
3. Open x.com, signed in, and scroll. Counts start filling in within a couple of seconds.


Being signed in matters: the tweek looks counts up through your own X session, which is also why it works without any extra account or API key.


## Show the Exact Follower Count, Not 92.2M


Out of the box, the badge matches X's compact style: 92.2M, 48.2K, full digits only below a thousand. But the lookup behind it returns the count as a plain integer, and one small formatting function rounds it for display, the same way X does.


To un-round it, open the tweek in Tweeks and describe the change:


> show the exact follower count with commas instead of rounding to K and M


That's the whole edit; if describing a change in plain English is new to you, the[Tweeks vs. Tampermonkey](https://www.tweeks.io/blog/tweeks-vs-tampermonkey) post walks through that workflow. Here is the same NASA post with the formatting change applied:


The profile page still says 92.2M. The badge says 92,161,818, a number precise enough to watch move. It moved while we worked on this post: an hour before that screenshot, the same lookup returned 92,161,547, about 270 followers earlier. That churn comes with one caveat. The tweek caches each count for 24 hours, so the number is exact as of the last lookup, not live to the second. A shorter cache is another one-sentence edit; the trade is more lookups against X's rate limits.


## How It Works


When a post appears, the tweek asks X's profile API for that author, the same lookup X performs to build a hover card, authenticated with your own session. The response carries the follower count as an exact integer, and the badge is built from that. No other server is involved: the requests run between your browser and X, and results are cached in local storage.


The lookups are deliberately throttled, at most two at a time and spaced most of a second apart, backing off for thirty seconds if X rate limits. With the 24-hour cache, an author you see often costs one lookup a day no matter how much they post.


The limitations:


- It needs a signed-in X session. Logged out, no badges.
- A timeline full of unfamiliar authors fills in over several seconds rather than all at once, because of the throttling.
- It reads X's page structure and calls an endpoint X doesn't document, so a redesign or API change can break it until the tweek is updated.


X is one of the most modified sites in the Tweeks library;[browse everything published for x.com](https://www.tweeks.io/sites/x.com) . If you read long-form there, the[X Article Export Button](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c) fixes a bigger gap the same way: X Articles have no download option, and it adds one that saves any article as a PDF or a Markdown file.


The exact number was on X's servers the whole time. Now it's in your feed.


## Sources


- [X Followers Count on Every Post tweek](https://www.tweeks.io/t/81b31e494c8a4a3d9e8895aa)
- [X Article Export Button tweek](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c)
- [Tweeks for x.com](https://www.tweeks.io/sites/x.com)
