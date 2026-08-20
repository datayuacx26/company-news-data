---
schema_version: "1.0.0"
document_id: "39e345a155a8a51cd303b9f59138640704da5a4715abfd56251ba9c6572e7e3f"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/hyatt-turns-to-bitrise-on-aws-to-deliver-superior-guest-experiences-built-on-trust"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:5aca83a71300d708682097949e864bdc27a4a482c53d3070a168366aa37c7c6b"
---

# Q&A: Hyatt turns to Bitrise on AWS to deliver superior guest experiences built on trust

> If you haven't read it yet, the[full Hyatt case study](https://bitrise.io/customer-stories/hyatt) is the place to start: 89% faster builds, 78% faster test execution, and two to two and a half days a month reclaimed from pipeline firefighting. The Q&A below goes deeper.


Since opening its first motel in Los Angeles in 1957, Hyatt has grown to 1,350 hotels across 78 countries, all while staying true to its mission: to care for people so they can be their best. That promise extends to team members and guests alike, shaping how Hyatt continually elevates the guest journey. Today, much of that journey centers on its World of Hyatt app, its core digital offering supporting guests before, during, and after their stay.


Delivering these experiences requires uncompromising digital security and a scalable foundation for growth. And so, when Hyatt set out to modernize its mobile CI/CD, it turned to Bitrise on AWS, a solution that not only meets its rigorous security standards, but strengthens guest trust, and helps Hyatt differentiate its mobile offering in today’s highly competitive hospitality market.


We spoke with Hyatt’s iOS Architect, Dimitar Milinski, about why they turned to Bitrise on AWS to transform mobile CI/CD, how it’s helping them deliver guest experiences built on trust, and what’s next for the hospitality leader.


## \[Q1\] **What is Hyatt’s core focus right now and how does your mobile strategy fit into that?**


**A:** Our top focus is always on delivering a seamless, personalized experience for guests across all channels, that includes web, mobile, and on property. Our goal is to make the guest experience smooth and memorable from start to finish, from booking to checkout and beyond.


Mobile is a huge part of that. The fact the app lives on your phone and is always there when you need it, makes it a powerful channel to connect with our guests. We see the most use on property, so it’s become a key way to engage on a more personal level and build loyalty.


## **\[Q2\] How important is security at Hyatt and in particular for your mobile offering?**


**A:** Security is huge for us. It goes beyond meeting regulations, it’s about protecting our guests' trust, which is the most important thing for us. It was the primary driver in moving away from our legacy mobile setup. We wanted to find a more secure and trusted solution that could meet our future needs. **‍**


## **\[Q3\] What security challenges were you facing with your old setup?**


**A:** Our biggest issue was having two very different build setups for iOS and Android. Android builds ran on Jenkins, while iOS required a more complex Azure setup since it had to run on Mac. As our repo was on-prem and Azure was not, we couldn’t pull code directly. Instead, we used Jenkins to push the repo to Azure after every commit.


From a security standpoint, this was far from ideal, sending our code outside our on-prem environment defeated the purpose of hosting it internally in the first place. On top of all that, keeping Jenkins and Azure in sync was a constant challenge. The whole process was overly complex and risky. And so when I joined Hyatt, my first priority was to find a better solution.


## **\[Q4\] Were there any other challenges you were dealing with?**


**A:** Our builds were painfully slow, mainly due to the fact Azure was running on Intel hardware. On a good day a build could take 30 minutes, and on a bad day it could be more like 90. It varied wildly depending on how busy Azure was. For example, weekends might be fine but then weekdays might be really slow. The worst part was we had no control over the process or ability to make it better.


Test runs were also inconsistent. About 20% of the time they would just fail for no reason. And when a test already takes over an hour and then fails, having to start all over again was hugely frustrating. It also meant we couldn’t block pull requests on failed tests because it would create too much friction. As a result, there was always the risk of things getting into the codebase and breaking tests or adding warnings, which was not ideal.


## **\[Q5\] Were there things you wanted to do but couldn't because of your setup?**


**A:** As our tests took so long to run that we were reluctant to add more, especially snapshot tests. As they generally take longer than unit tests and our runs were already 60 minutes, it just didn’t feel feasible. As a result, the stability of our app was always at risk, as good snapshot coverage generally means fewer bugs.


Delivering builds to QA was also painful: if one was needed at 5 p.m., it could take an hour to run, and if it failed, you might be stuck rerunning until 7:30. We just couldn’t face adding more tests knowing it would slow things down even further. It didn’t feel like a good trade-off.


## **\[Q6\] What impact did those challenges have on the mobile team?**


**A:** Builds took too long, tests were unreliable, and releasing to QA could mean late nights when something failed last-minute. There was friction at every step which brought frustration and left developers with little confidence in the process. We all wanted to find a better solution.


## **\[Q7\] What were your non-negotiable requirements when looking for a new mobile CI/CD solution?**


**A:** We had a long list of non-negotiables, so finding the right solution wasn’t easy. For starters, we wanted a mobile-focused platform that offered a consistent setup for iOS and Android to speed up builds and tests. Also, we wanted a solution that could run on dedicated hardware in our Amazon Web Services environment so it would be low maintenance for our teams to manage.


## **\[Q8\] What made you choose Bitrise on AWS?**


**A:** Bitrise on AWS was the only solution that met all our requirements without compromise. We looked at GitHub Actions, GitLab Runners, CircleCI, and a few others, but they all came with trade-offs we just couldn’t make.


Bitrise on AWS ticked every box, it was the perfect fit for our needs.


## **\[Q9\] What was the onboarding process like?**


**A:** It took about three days to get up and running, which was impressive given our unique set of requirements. It was great having the Bitrise team on hand throughout, they were incredibly helpful and supportive.


Our in-house AWS experts supported us with the initial setup, but since then, it has been so straightforward to run and maintain. Anyone on the team can use it confidently without deep AWS knowledge which is great.


## **\[Q10\] Since switching to Bitrise on AWS, what benefits have you seen?**


**A:** We are realizing so many benefits with Bitrise on AWS. To start with, builds that used to take up to 90 minutes now finish in about 10. Testing has gone from around 45 minutes to 10—and the best part is they are consistent and reliable. It’s allowed us to expand into snapshot testing: we’ve added 150 so far and are aiming for 1,000 this year. And of course more tests means we are getting fewer warnings and ultimately less bugs slipping through, which helps everyone.


We’ve also built in a lot of automation, which is really helping to cut down manual work, and improve accuracy across the build process. For example, when a build finishes, it automatically posts the build number to Jira. And every build now pulls in the latest feature flags from LaunchDarkly which is really helpful.


## **\[Q11\] What impact has it had on other teams in the organization?**


**A:** Our Quality Assurance teams have noticed how much faster builds are, which is great. They’re no longer stuck logging tickets, releases are going out on time, and as test coverage expands, we will see quality continue to improve.


From a developer perspective, it’s really boosted confidence in our mobile CI. Now when a Bitrise build passes, it means something to our developers and they trust the result. Previously a pass or fail didn’t count for much.


## **\[Q12\] What’s been the real benefit from your personal experience?**


**A:** As the architect on the project, I can focus more on architecture work instead of figuring out why tests are broken or staying up late to get a build out. Being able to take a step back and focus on that higher-level work is great for me but I think it ultimately benefits the whole team too.


Previously, we could spend up to half a day a week dealing with unplanned pipeline issues. You never knew what was coming next or what errors might slip through that could derail your progress. It was so unpredictable. With Bitrise on AWS there are no surprises so we don’t have to worry about that anymore. We have more control over the process and can plan ahead much more effectively.


## **\[Q13\] What’s next on the horizon for your mobile team now Bitrise is in place?**


**A:** We’re modernizing our codebase and expanding the World of Hyatt app to bring guests more loyalty-focused experiences. Bitrise on AWS lets us tap into advanced iOS features like widgets and app clips. Of course, more functionality means more build targets, so having a stable CI/CD solution really makes all the difference in what we can now accomplish.


## **\[Q14\] How would you sum up the partnership with Bitrise?**


**A:** It’s been an absolute pleasure working with the team. Now with Bitrise on AWS we’re able to deliver higher-quality experiences to our guests, faster, while having the confidence to expand what we offer through the World of Hyatt app.


We’re excited about what’s next in our partnership and we’re looking forward to taking our World of Hyatt app to new levels to strengthen our guest experience and ultimately competitive advantage.


Read the[full Hyatt case study here](https://bitrise.io/customer-stories/hyatt) . 💙
