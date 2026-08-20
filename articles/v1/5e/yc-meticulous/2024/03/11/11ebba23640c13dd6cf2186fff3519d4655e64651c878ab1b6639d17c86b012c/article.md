---
schema_version: "1.0.0"
document_id: "11ebba23640c13dd6cf2186fff3519d4655e64651c878ab1b6639d17c86b012c"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/power-meticulous-case-study"
published_at: "2024-03-26T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:2615bfe78bc32c9e467b7166b50212c51296a204a0da8b31017e2d7968a8085b"
---

# Power <> Meticulous case study

## What does your company do?


[Power](https://www.withpower.com/) connects patients with clinical trials. We're making it easier for patients who have a specific condition to find leading medical researchers. At the same time we help researchers who are conducting these clinical trials find patients to bring their treatments to market.


## How did you hear about Meticulous?


One of our co-founders heard about it through an investment group. At the time I think you had just got your seed round.


We were looking into different frontend testing systems and then he said, “Hey take a look at Meticulous!”. We looked at the site and it seemed like it was too good to be true, and so we reached out.


## What were Power looking for in a front-end testing solution?


I’ve worked on frontend tests before, both unit testing and puppeteer-style tests. Those are always a lot of work to write - it would often take longer to write the test than the change itself. They are also brittle, where sometimes it’s hard to know if a failure is intentional or not.


We wanted to do better than that. We wanted to have a system where our team didn't have to spend a lot of time writing or maintaining tests.


## How did Meticulous match up to your expectations?


It far exceeded anything I expected.


Meticulous does a great job of covering our front end testing. We trust it fully, like 100%. We definitely get coverage on the specific surface areas that we want, such as guaranteeing our signup flow works.


However, what blows us away is it catches bugs that we wouldn’t have even thought about. We wouldn’t have even written tests for these classes of bugs. But because of the way it works, by recording user flows, it catches a class of errors we didn’t even think were possible to catch with a framework.


## Do you have any metrics or KPIs or anything that show how meticulous has been doing?


We essentially don’t have UI regressions anymore.


I know you had a short outage in December and then we had a bunch of regressions suddenly appear, and we were all like “this is weird, what’s going on?” and it turned out it was an outage.


I know you’ve fixed those issues and implemented better alerting to catch these. The support was great. But it actually kind of showed us how much Meticulous was catching for us.


The other metric would be in terms of time saved.


One way to think about this is for every 1 hour of frontend dev time I'd expect to spend 15 mins writing traditional frontend tests, and maybe 10% of the time would introduce a regression that would take 30 mins to fix.


So across the team of 6, if we spent 40 hours per week building frontend, that's 10 hours saved on not building frontend testing, and 2 hours saved not fixing regressions. I'd say the team probably spends 1 hour per week cumulative using the meticulous system.


So maybe Meticulous saves 10 hours for every 40 hours of frontend engineering time or 25%.


## What have you been most impressed with?


How responsive the team has been.


The team has been great to work with, and flexible. For example, we have a lot of sensitive data we’re dealing with and need to treat carefully. The team worked with us to make sure the setup was secure.


The other aspect is that with traditional testing, there’s all sorts of upkeep. Like this test is flaky, or this test is broken, or we need to update our chrome version.


What’s amazing about Meticulous is there’s no upkeep.


It’s kind of funny, you ask me how meticulous is going and the answer honestly is I didn’t even think about it this week. It just works nicely in the background and catches bugs when there are issues.


## What plans do you have to use Meticulous in future?


This is kind of out there. We send a lot of emails. We found a way to render emails in webpages so that Meticulous could test these as well. Not sure if that’s on your roadmap, but better support for emails.


We trust this system and so we want to get as much of our UI surface into it.


## You signed a 2-year contract, what made Power take that decision?


We think of it as a core part of our testing infrastructure now and we’re happy to invest in the pieces which we know are foundational to our engineering org.


## When you open a meticulous diff page, how long does it take you?


90% of the time I spend less than 20 seconds looking at it. Usually it’s pretty obvious to me what’s going on.


## Anything else we should know?


One other angle that has been exciting for us, and I think you’re going to love this: Being a software developer at Power is better because of Meticulous. We have better developer satisfaction!


We spend way more time building stuff and not worrying about some classes of UI test implementation.


We talk a lot about developer experience at Power. Meticulous is a fantastic example where it solves a lot of problems which developers don’t like, and solves this in a really elegant way. So I think we’re proud of it when we onboard new team members and say, ”Hey, you don’t have to write frontend tests anymore, this is all handled for you!”


PS.[Power is hiring](https://trypower.notion.site/Careers-at-Power-f885aa53d8d14a288dd154f13b644e4a) !
