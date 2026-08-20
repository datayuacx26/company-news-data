---
schema_version: "1.0.0"
document_id: "8b6ad78ff7f29f3f1f16bd8aaec3f3444cddf253ed0cf0492a3b7f08b303b65c"
company_key: "yc-galactic-resource-utilization-space-inc-gru-space"
company: "Galactic Resource Utilization Space, Inc. (GRU Space)"
source_id: "yc-galactic-resource-utilization-space-inc-gru-space-news-import-c1c54996ad6c"
canonical_url: "https://www.gru.space/blog/siting-the-first-lunar-hotel-a-view-of-home"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-25T06:06:34.816183+00:00"
fetched_at: "2026-07-28T22:22:42.278869+00:00"
content_hash: "sha256:706e669569693c4a609ae064cc071fd41b6cb2b2b1ea84a47869b232726e1cd7"
---

# Siting the First Lunar Hotel: A View of Home

Where should the first hotel on the Moon be located? In a series of upcoming newsletter posts, we’ll walk you through some of the factors that go into selecting the spot on the Moon where we’ll deploy GRU Space’s first hotel as soon as 2032.


Safety is our number one priority, but the guest experience is paramount. If you trekked all the way to the Moon and couldn’t look back on Earth, or the planet was upside down or obscured by the sun, you’d probably want a refund, and rightly so. We can go beyond the basic question of “will you be able to see Earth?” and build software models to show exactly how you’ll be able to see the Earth from inside our hotel and during Moonwalks outside.


That first factor is important though: from a given location on the Moon, is the Earth even visible? The Moon is **tidally locked** to Earth, which means the same side of the Moon is always pointed at the Earth. Everywhere on the Moon has a roughly 28-day day/night cycle: 14 days of daylight followed by 14 days of night. There is no “dark side” of the Moon, but rather a “near side” pointed towards us, and a “far side” pointed away:


Because of this, most places on the Moon’s near side have an almost continuous view of Earth: it’s always hanging in the sky, 4× the size that the Moon is in our sky. Towards the limbs of the Moon or towards the poles, the Earth is only visible some of the time, based on the Moon’s orbit and obscuration from topography. We can map out these effects and ask what fraction of time the Earth is visible from every point on the Moon’s surface:


The map shows clearly that we want the hotel to be on the near side (0° longitude is pointed right at Earth), and ideally not at polar latitudes.


If the Earth is just above or below the horizon, guests would only be able to see a portion of it some of the time, and it’s at risk of being blocked by topography. On the other hand, if you’re right at the Moon’s equator, the Earth will be directly overhead, forcing you to crane your neck straight up to see it. This might not even be possible in spacesuits that have a limited range of motion. An elevation of 20-45 degrees above the horizon is ideal, and we can model which locations on the Moon will provide this angle most of the time:


Finally, if the Earth is always in the exact same position, all the photos you’ll come back with will look the same. We want guests to be able to experience Earthrise during their stay, and see our home planet from different perspectives. We’ve modeled the “elevation diversity”, in other words how many different angles above the horizon the Earth can be seen from over time:


By weighing these different factors, we can start to narrow down the best locations on the Moon’s surface to site our hotel such that guests have the best views of Earth. Not only that, we can also model exactly what the Earth will look like during a given stay, any time in the future. For example, a hotel stay starting on July 1, 2032, at a lunar location of 30°N, 0°E will see the Earth like this over a 5 day stay:


The elevation, orientation, and rotation of the Earth are all accurately captured by our model. In future posts we’ll go through other factors that influence the hotel’s location: temperature swings, radiation and micrometeoroid hazards, how scenic the surrounding terrain is, and locations of lunar pits and caves.
