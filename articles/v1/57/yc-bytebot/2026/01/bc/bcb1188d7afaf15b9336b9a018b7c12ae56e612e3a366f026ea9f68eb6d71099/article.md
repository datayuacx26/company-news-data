---
schema_version: "1.0.0"
document_id: "bcb1188d7afaf15b9336b9a018b7c12ae56e612e3a366f026ea9f68eb6d71099"
company_key: "yc-bytebot"
company: "Bytebot"
source_id: "yc-bytebot-news-import-2c444721769b"
canonical_url: "https://bytebot.io/blog/stop-arguing-with-engineers-design-better-systems"
published_at: "2026-01-09T23:24:57.830+00:00"
first_seen_at: "2026-07-27T08:16:37.499212+00:00"
fetched_at: "2026-07-28T22:23:45.657833+00:00"
content_hash: "sha256:8967752cdd53236af83108f9a01d244821bdaec884d8f6d49b1899345a52c482"
---

# Stop Arguing With Engineers - Design Better Systems

You've been in that meeting. The one where you *know* the right answer, you explain it perfectly, and everyone nods... then does something else anyway. The arguments never stick and you're back where you started.


Stop trying to change minds. Change the system instead.


Engineers are like any other person. They respond to incentives over arguments. The fastest path from A to B is to make B really annoying.


This is the art of *well-placed friction* .


## What is friction?


Friction is anything that makes a behavior harder like how mandatory PR reviews prevent deploying straight to prod. The bad behavior is now impossible. What's the next best option? Going through PR - friction in action.


Other examples include:


- **Long deploy processes** prevent quick iterations thus encouraging mega releases
- **"no agenda, no meeting" policies** prevent aimless, unprepared meetings thus encouraging canceling or structured meetings
- **Password rotations every 30 days** prevent remember passwords thus encouraging them being written on sticky notes
- **Demos at the end of every sprint** prevent building in isolation thus encouraging earlier stakeholder feedback


## How to use this to your advantage


### 1. Figure out the outcome you want


Not the behavior - the *outcome* . "Fewer bugs in production" not "write more tests". Start with what success looks like, not what you think causes it.


Example: Fewer bugs in production


### 2. Find what behaviors lead to those outcomes


Work backwards. What behaviors *cause* that outcome. Maybe it's writing more tests. Maybe it's having more time to learn their tools. Don't assume - *observe and test* .


Example: Writing high value tests


### 3. Remove friction from the good behaviors


Make the good behavior effortless. Auto-generate test boilerplate. One-click deploys when checks pass. Templates that pre-fill the boring parts. CI that runs fast. If the right path feels like a chore, people won't take it.


Example: Setup the testing libraries and write example tests


### 4. Add friction to the wrong behaviors


Why aren't people doing the right thing already? Usually because the wrong thing is easier. Skipping tests is faster than writing them. Mega-PRs avoid multiple review cycles. Copying old code is quicker than refactoring. Find the shortcut people are taking.


Example: Require tests with new code


### 5. Make a noisy "break glass" path


Allow your system to be broken in cases where it is horribly unfit. This prevents your system from remediating emergencies. It also hints to management that something serious needs to be fixed.


Example: Allow admin override with email to director


### 6. Test and iterate


Listen to reality. What effect are you actually having? Are you achieving the desired outcome? Remember, the dysfunction was originally created from someone having good intentions and not checking in after.


Example: The process itself becoming fair game in retrospectives


## Why this works


- No ego battles
- Works while you're in meetings
- Scales across teams and orgs
- People *discover* the right answer themselves


## Pitfalls


- You optimize for the wrong outcome
- Adding friction everywhere
- You treat people like they're dumb
- You make friction for culture
- You forget to remove it when not needed any longer


## Closing


You can't nag people into doing the right thing, but you can artfully use a carrot and stick to do so. Design your systems to do the heavy work for you. Maybe taking a closer look will help you realize why everyone habitually does the wrong thing?
