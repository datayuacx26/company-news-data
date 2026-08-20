---
schema_version: "1.0.0"
document_id: "705187fc0363b64af958be2c61c79595dba9564623f9518b78bde6e74539a905"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/bike-vs-tube-commuting-speed"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:29:12.728977+00:00"
fetched_at: "2026-07-31T22:29:14.737206+00:00"
content_hash: "sha256:f99e878802a6524c64170fd5cc0ea5ffaf489354d38a829be1bbaa40da0a7818"
---

# Cycling Is Good for You, But is it Faster Than Public Transport? | Terra

## Wins by Journey Length


I split matched trips into short (<6 km), medium (6–15 km), and long (15–25 km) bands. Win rates and median savings both stay with the bike, but the *opponent* changes with distance. I’m surprised by these results, as I suspected the bike's advantage to be removed over the longer distances.


But this dataset is probably populated by quite serious commuters who aren’t opposed to commuting long distances. We also can’t discount the possibility that more people commute longer distances when there are travel problems with the other options, eg tube strikes.


***Figure 2:** Share of matched morning trips where the recorded bike ride beats TfL door-to-door or Google traffic driving. Short <6 km, medium 6–15 km, long 15–25 km, win rates stay high across lengths, with traffic wins peaking mid-distance.* ***Figure 3:** Median minutes saved on the same origin–destination when cycling versus TfL or Google rush-hour driving. This is a within-trip delta, not a comparison of different absolute journey lengths across modes.*


Short rides have a big win over the Tube but only narrowly beat (or sometimes lose to) a car in traffic. Medium rides are the sweet spot: still ~94% wins versus TfL, and traffic wins jump to 84% with about 9 minutes saved. For longer commutes, the Tube becomes a more competitive option, while the bike’s median save versus rush-hour driving stays large.


## Wins on Busy Corridors


The ten most frequent multi-user corridors are mostly radial rides into the City, Canary Wharf, and central London. On these routes, cycling typically saves about 15 minutes versus TfL and about 8 minutes versus Google rush-hour driving.


***Figure 4:** Busiest multi-user morning bike corridors over a Greater London silhouette with the Thames. Blue routes beat Google rush-hour driving; orange means traffic still wins.* ***Figure 5:** Each corridor has two bars: minutes saved versus public transport and versus Google rush-hour driving. Positive values mean the bike is faster; Ilford → Barking is the main case where Google driving still wins on time.* **Corridor** **Bike** **vs PT** **vs Google drive**


Shepherd's Bush → Kensington 13 min −12 min −1 min


Isle of Dogs → Canary Wharf 10 min −12 min −1 min


Lewisham → City 26 min −14 min −5 min


Hammersmith → City 31 min −25 min −18 min


Hackney → City 24 min +1 min −1 min


Wandsworth → City 31 min −19 min −26 min


Clapham → City 37 min −15 min −7 min


Isle of Dogs → City 24 min −7 min −15 min


Hackney → Central London 21 min −23 min −14 min


Ilford → Barking 47 min −27 min +11 min


Negative minutes mean the bike is faster. Hammersmith → City and Wandsworth → City are standouts against traffic. Hackney → City remains the rare Tube-competitive corridor. Ilford → Barking is the exception versus Google driving, longer outer-London trips are where the car still wins on time.


## Methodology


I used a database of outdoor cycling activities on Weekdays in Greater London with local starts from 07:00–08:59, distances of 1–25 km, and durations of 5–90 minutes. That leaves a commute-like cohort.


For each trip I estimated counterfactual travel times:


- **Public transport:**[TfL Journey Planner API](https://api.tfl.gov.uk/) with door-to-door walking using the same hour/minute on the nearest matching weekday.
- **Driving (rush hour):**[Google Routes API](https://developers.google.com/maps/documentation/routes) for the next matching weekday at the same departure time.


**Important caveat:** I only see the recorded bike activity. Wearables don’t tell me whether that ride is the full door-to-door commute. Someone may walk to a bike share, cycle part of the way, then take the Tube, or start/stop the activity mid-journey, so the bike times can miss walking legs or other modes at either end.


I think this sample is predominantly “serious” cyclists, so most journeys are probably door-to-door. But I am guessing on that point. By contrast, the TfL and Google estimates are built as full origin–destination journeys. Treat the comparison as bike *segment* vs door-to-door alternatives, not a perfect like-for-like door-to-door race.


## Takeaway


On matched morning origin–destinations, the bike wins most of the time, ~92% versus TfL and ~75% versus Google traffic. With the biggest traffic advantage on medium-length radial rides. Short trips still thrash the Tube/public transport but are closer versus the car; long trips keep solid win rates with larger median savings against traffic. Get on your bike!
