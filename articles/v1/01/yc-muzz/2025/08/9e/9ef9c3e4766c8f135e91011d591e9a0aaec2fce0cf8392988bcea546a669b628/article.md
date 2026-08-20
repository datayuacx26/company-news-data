---
schema_version: "1.0.0"
document_id: "9ef9c3e4766c8f135e91011d591e9a0aaec2fce0cf8392988bcea546a669b628"
company_key: "yc-muzz"
company: "Muzz"
source_id: "yc-muzz-news-import-4a23516b0f87"
canonical_url: "https://engineering.muzz.com/how-a-bad-release-saved-us-thousands-a-month-and-reduced-latency"
published_at: "2025-08-06T15:08:00+00:00"
first_seen_at: "2026-07-25T16:24:04.083021+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:a784bb262830bc890f485b2d11eecc94383481cbf22661cd541096c181f898c5"
---

# How a bad release saved us thousands a month and reduced latency

On a rainy day of October we decided to release a feature that we had spent all summer working on: a massive refactor of how our matching process works, from an unlimited number of matches leading to ghosting and low-quality connections to a fixed amount of slots that makes users more thoughtful on who they are matching with.


Minutes after the release to production our lead devops sends an alarming message on our slack alerts channel :


And followed with the following graph:


**We were getting 10x the usual traffic !**


At the same time as the release we had a script going through hundreds of millions of matches ever created by our users and unmatching automatically all who were inactive for the past 30 days.


Of course our initial suspicion was that the script was being too aggressive on the database and we reduced the number of workers from 20 to 5.


That barely had any effect.


We proceeded to stop the script completely. The massive increase in load did not subside. That left us scratching our heads.


The database was maxed out :


All the metrics were showing that this was not a DDos attack but real legit traffic, with sessions showing the usual patterns of behaviour, hitting the endpoints in a predictable and well-known sequence.


Was it due to the marketing campaign that was launched globally at the same time to inform users of the new feature ?


No, metrics were clearly showing that the number of users had not dramatically changed pre and post release.


**Strategic Retreat**


As the problem was not subsiding and app experience was deteriorating we decided to rollback the feature, the latency had degraded too much and matches were taking seconds to load vs milliseconds usually.


Thankfully we put this feature behind a config flag on the backend as well as iOS and Android so rolling back was done at the click of a button and a smooth app experience was restored instantly.


**A closer look**


It was clear that the new feature was the source of the increased latency and load on the database.


A minute analysis of the network calls showed that the new iOS and Android versions were doing 5X and 2X respectively the usual number of calls to get the list of matches. We were onto something. If you are a Muzz user you certainly have seen this screen before :


If the user goes above the limit (10 matches max) then they are prevented from browsing new profiles.


The app needs to know at all times how many matches the current user has to know whether browsing should be limited. This led to the multiplication of calls to the endpoint that fetches matches the number of unread messages. This multiplication of calls went unnoticed during code review and QA.


The fix seemed clear: apps should make fewer backend calls… However this was not a small task and would require days of work and changes to code shared with other teams.


That’s when we looked at the other side of the equation, the backend and in particular the database query. Was there any way we could improve it ?


**Left on Read: When a JOIN meets a 10 Billion‑row table**


Chat feature is central to Muzz and it is one of the first functionality that was implemented. It worked smoothly and we didn’t think to review it. Muzz has grown exponentially since our debut and as the time of this article handles 20M users.


Upon reviewing the database metrics while the feature was enabled, the get-unread-messages was the top offender in terms of Load by Wait and by far.


Muzz users have generated hundreds of millions of matches and have sent each other billions messages to this day. The get-unread-messages query was performing a JOIN between matches and messages and at scale this created a lot of contention and meant the database was going through an immense amount of data to find matching rows despite indexing. Essentially it was doing the following steps :


SELECT B.id AS conversation_id, COUNT(*) AS unread_count


FROM A


JOIN B ON A.conversation_id = B.id


WHERE A.conversation_id IN (1,2,3)


AND A.sender_id != :user_id


AND A.created_at > B.last_read_at


GROUP BY B.id;


1. Go to the big messages table and JOINed on match ID to pull messages.
2. Compare each message’s time to the last read time by the current user coming from the match table.
3. If it was newer, we counted it as “unread.”


It is pretty straightforward and it served us very well for over a decade but it was time to update it and make it more efficient.


**A more efficient way**


To make this query more efficient we needed to :


- **Optimise the common case.** Most matches have zero unread, don’t even touch the messages table for them.


- **Eliminate the join entirely.** If the only reason to involve the matches table is the last read timestamp, get that value from matches through code and without involving the SQL planner.


The new query looks like this :


SELECT A.conversation_id, COUNT(*) AS unread_count


FROM A


WHERE A.conversation_id IN (…)


AND A.sender_id != :user_id


AND A.created_at > CASE


WHEN A.conversation_id = :id1 THEN :ts1


WHEN A.conversation_id = :id2 THEN :ts2


— …


END


GROUP BY A.conversation_id;


1. For each match, we do a first quick check: is the last read message timestamp older than last sent message timestamp ?


1. If not, we **stop there** for that match. We know there are no new messages
2. If **yes** , we know for sure there are new messages, in a separate query we then grab the last read message timestamp for each match, build a CASE list and we count messages that are newer.


The result was immediate and dramatic!


Read IOPS dropped from ~16k to ~2-3k within minutes and stayed low.


The get-unread‑messages query disappeared from the top 10 most intensive queries.


We now were way below our provisioned IOPS meaning we can right‑size some of our database capacity. Our conservative estimate: about **$5,000 per month** saved. Not bad for removing a JOIN and adding some upfront thinking.


**Lessons learned**


1. Measure first, database metrics pointed to the one query that mattered.
2. Optimise for the common case, most matches have zero unread.
3. Refactor, small, well‑placed changes can move big numbers.
4. Keep product options ready, feature flags give you flexibility.
5. Make load and performance testing systematic rather than ad-hoc


That buggy release wasn’t fun at the moment, but it forced us to look hard at a query we had learned to live with. One refactor later, we’re serving a better product experience with a fraction of the database effort, and we’re saving a small fortune over time.
