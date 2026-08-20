---
schema_version: "1.0.0"
document_id: "8d062bf54410347159823a861f9a24a1d272b97400e913fb53f293d7f45888d4"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/talk/guts-of-git/"
published_at: "2023-03-15T19:00:00+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:493b30b251cb5c815ca30510ba5af90c74eaad25c10d496a8543b473c259dba5"
---

# Guts Of Git

# Try it yourself:[https://guts-of-git.carson-anderson.com/](https://guts-of-git.carson-anderson.com/)


- Source code:[https://github.com/carsonoid/workshop-guts-of-git](https://github.com/carsonoid/workshop-guts-of-git)


# Summary


This workshop actually started as an internal talk here at Weave. The Weave engineering department has an ongoing commitment to education and that includes regular internal meetups. The driver for this talk was to ensure that all engineers at Weave actually understood one of the technologies they use the most: Git. While we all work hard to better understand our programming languages and other parts of our tech stack. Many developers often forget that source control and Git is one of the most important things they do.


The talk was so well received internally that it was submitted as a talk to[Salt Lake City DevOps Days](https://www.slcdevopsdays.org/) . It was selected by conference leaders but they asked if it could be re-worked into a 2 hour workshop. In order to facilitate an easy and smooth workshop experience, we used Hugo to generate and host the content so attendees could easily follow along.


After being presented at the conference, the workshop was so well received that Carson was asked to present it again at the local[Utah Java Meetup](https://www.meetup.com/utah-java-users-group/events/292476251/) where the workshop content was presented again but in a shorter time frame and as a talk rather than a fully interactive workshop.


# Key Takeaways


- Git is a critical component of most developer’s jobs.
- Almost everything in git is stored as a content-hashed object
- There are two primary ways of updating and combining branches:


- Using` git merge`


- Pro: The history will remain unchanged
- Con: Ugly commits stay in the history forever


- Using` git rebase`


- Pro: You can rewrite and change commits to keep a clean history
- Con: You need to force push your changes since history has been changed


- You should use[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) to add more consistency and value to your messages


# Details


The full source code for the workshop web page can be found at:


- [https://github.com/carsonoid/workshop-guts-of-git](https://github.com/carsonoid/workshop-guts-of-git)
