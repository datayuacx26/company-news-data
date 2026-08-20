---
schema_version: "1.0.0"
document_id: "3ac35a8f1bde3d0e936dfdc7300b1654e4d621e3be85b3f3ee8d38183251941b"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/subreddits-no-longer-showing-up-as-story-tags/13766"
published_at: "2026-07-04T01:52:12+00:00"
first_seen_at: "2026-07-25T16:10:32.307561+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:99d2993b0ebee2c43deaf1558b319c54a049c8e06ac95ca8086693efb3e585c5"
---

# Subreddits no longer showing up as story tags

[chris79](https://forum.newsblur.com/u/chris79)


July 3, 2026, 6:33pm 1


… making all my filters fail


Reddit’s “r/example” story tags are no longer working. For example, the RSS tag` <category term="worldnews" label="r/worldnews"/>` no longer shows` r/worldnews` as a story tag


Using` old.reddit.com` for the feeds


Help? Please?
– Chris


[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 3, 2026, 7:42pm 2


Fixed, the Reddit API fetched stories now include the subreddit as a story tag again, so filters like world news should work.


Here’s the commit:[https://github.com/samuelclay/NewsBlur/commit/3df609772b4b32103aa7158de7df247fe6d1dac7](https://github.com/samuelclay/NewsBlur/commit/3df609772b4b32103aa7158de7df247fe6d1dac7)


[chris79](https://forum.newsblur.com/u/chris79)


July 4, 2026, 1:52am 3


Thanks so much, man!


– Chris


1 Like
