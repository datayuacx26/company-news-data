---
schema_version: "1.0.0"
document_id: "0a628eff4e95373dff1a263be5e34b3e5b760693508ea7034271ad7130d812cc"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/the-economics-of-operating-a-ci-cloud"
published_at: "2025-05-13T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:59e6dab8faea62af4c095944d3cfa3f059265259ffda70fc7d428476903a7061"
---

# How the economics of multitenancy work

In the early days of Blacksmith, back when we were just a scrappy[YC startup](https://www.blacksmith.sh/blog/going-through-yc-winter-24) building a serverless cloud platform for CI workloads, we ran simulations to model our margins. We figured that with enough customers, the math would work out, and we crossed our fingers — but we didn’t have any real-world data to back up our predictions.


About six months after we launched, I came across[a blog post](https://brooker.co.za/blog/2023/03/23/economics.html) by Marc Brooker on the economics of multitenant systems. It captured what we were trying to do much more elegantly than the half-formed ideas in our heads. This post was heavily inspired by Marc’s, and reading it was a real moment of, *"Oh! someone else has thought about this, and it makes sense."* ‍


Now, we’re running thousands of jobs every minute and millions every month, and it’s been exciting to actually see this play out at scale and watch the math work in real life. Yet, people still often ask in disbelief how we actually make money from our setup — are we just burning sweet VC dollars with no return for them in sight? So, for the non-believers and anyone who is just a little bit curious, let’s take a peek behind the curtain and dive into how the economics of multitenancy work — using ourselves as a case study.


## CI isn’t like production (and that matters)


Unlike production workloads, CI workloads tend to be very spiky. Below, we’ve plotted CPU utilization for one of our customers over a 24-hour window. It spikes when someone pushes code, then chills out in between.


This customer runs 35 jobs on 16 vCPU machines for every git push, meaning they need over 500 vCPUs every time they run CI. See where the chart flatlines like a patient on the George Clooney classic “ER”? Since their team is split across the US and EU, there’s an 8-hour stretch in the middle with zero usage. And when a few engineers push at once — say, five people — they suddenly need 2,500+ vCPUs instantly. And all these CI jobs are short-lived. Most CI jobs finish fast (relatively speaking), somewhere between 5 and 40 minutes. All of these chaotic characteristics of CI workloads might sound like a nightmare, but it’s actually a perfect fit for the serverless model we’ve built our platform around, and most importantly, for our customers.


## Why our serverless CI model works


Think about it from the customer's side: If you need 2,500 vCPUs at peak, it would be crazy to buy and manage all that hardware yourself — especially when it would sit idle most of the time. But with Blacksmith’s serverless CI cloud, you get to borrow from our pool and only pay for what you use. Spiky, bursty, chaotic? No problem.


What’s more, CI traffic is highly predictable. Developers are pushing code during work hours, not at 2AM or during holiday weekends. Unlike some production workloads, our fleet is not bracing for Black Friday-style traffic surges — and that shapes a lot of how we built it.


## Our fleet: what it looks like and what matters most


We have a fleet of hundreds of bare-metal gaming CPUs that we’re virtualizing over — when a customer needs to run a CI job, we spin up a microVM using[Firecracker](https://firecracker-microvm.github.io/) , and once the job’s complete — poof! It’s gone.


Each of our machines has 32 vCPUs, and across the whole kit and kaboodle, we manage tens of thousands of vCPUs across our region us-west and eu-central. We pin customers to one region for consistency and workflow support.


Currently, we lease these machines for a fixed period. Soon, we’ll be racking them up in a datacenter. Regardless, these machines are a fixed cost — whether we have customers or not, we’re still paying for them. So, the name of the CI cost optimization game is utilization. If they’re barely used, our margins are low; if they’re used enough, we make that cash money. The metric we track the most is average fleet utilization — and that’s where customer chaos becomes our secret weapon.


## A bit of chaos is bad. A lot of chaos? Chef's kiss


Let’s say we only had one customer — the one from earlier who needs 2,500 vCPUs at peak. We’d need around 80 machines to handle that load, but for most of the day, those machines would just sit there, twiddling their thumbs. We’d be bleeding money.


Now, add a second customer in the same time zone. Their CI jobs don’t peak at the exact same time, so instead of needing 160 machines (80 + 80), we might only need 110 to cover both. As we add more customers, the effect compounds. We add even more customers, and all the random bursts of activity start to blend together.


Over time, CI jobs start behaving like a Poisson process — random, short bursts spread out across time. From a distance, what once looked like sharp spikes from individual customers smooths into a predictable pattern. The more customers we serve, the less intense each individual spike appears. In short: the more chaotic it gets, the better it is for our business. And when it’s better for business, it’s better for customers, too — because as our fleet gets busier, the cost to serve each job goes down. That lets us keep prices low while still running a sustainable business.


The beauty of this setup is that every new customer actually makes the system better for everyone. Like when you have a dinner party and say, “ *the more the merrier* ” and actually mean it. More customers = more randomness = smoother overall operation. Multitenant systems work better with more users: utilization goes up, and our costs to serve go down. That means that growing chaos on our fleet only improves cost savings and efficiencies. You win. We win. In fact, even our fleet running hot is a good thing.


## *A fleet running hot means more money*


Since we have to pay for a fixed fleet of machines no matter what, our gross margins depend almost entirely on how busy our machines are.


Basically, our revenue scales with the average utilization of the fleet. There’s a direct link between utilization and gross margins, and it’s not linear.


- At 10% utilization, we’re already hitting around 35% gross margins.
- At 20% utilization, margins jump to about 70%.
- At 35% utilization, we’re flirting with 85%+ gross margins.


Modest improvements in utilization result in massive improvements in profitability. Once utilization is high, the next major lever to keep improving margins is driving down the cost of acquiring machines — and for that time of day plays a surprisingly big role.


## Who’s pushing code when?


During the weekends, our fleet only sees about 1/5th our typical usage. But weekdays? That’s when the party really starts.


Most of our customers are based in the US, with a decent chunk in Europe, and a slowly growing portion in Asia. As seen in the chart below, utilization stays low during the first 8 hours of the day. Here’s the breakdown (in UTC time):


- Early hours = crickets (our Asian customer base is still small).
- Midday = a bump from Europe.
- Late Afternoon = the US wakes up, and our fleet is flying.


The biggest spikes we see come when Europe’s finishing the day, and when the East Coast and West Coast are both working at the same time. Customers outside the US use our fleet during low-traffic hours — essentially free utilization. That boosts margins without us needing more machines. That’s CI cost optimization at its finest. Add it to the board, another win. We only really need to expand our fleet to keep up with growth in the US. And just like time zones shape customers’ daily usage and how we think about our fleet, geography shapes where we build and scale our fleet.


## Region economics


We originally started with a single region in eu-central, but over time, we realized that we needed a second region in the US. This was driven by customer requests since Docker pushes to a container registry in the US is even faster when your runner is in the US. Plus, a few customers preferred keeping their code inside the US for compliance reasons. At first, the US region had just one big customer, so margins and utilization were meh. But as more folks have joined, our numbers are looking better and better as utilization improves.


We’re still working on optimally load balancing our regions, but this post is already too long so that’s a story for another day. If you made it this far, thanks for reading. Still burning a few VC dollars, but hey — margins are looking good thanks to the power of multitenancy. If you’d like to help improve them even more,[try out Blacksmith](https://app.blacksmith.sh/) .
