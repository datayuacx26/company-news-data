---
schema_version: "1.0.0"
document_id: "e9b9c36aca35ade231f62c5870f4418b983c50ff330ae1fdddf4425165e52235"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/july-2024-update"
published_at: "2025-04-23T19:44:55+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:6a56c9fdd3efe0982ae27afdc056d18eb6ff7ae5be9870be01c5e568a0a27f08"
---

# Our July 2024 Update

# Our July 2024 Update


### Declassifying our old Friends and Family updates


[Daniel Farrell](https://substack.com/@danielfarrell)


and[Kiet Ho](https://substack.com/@kietho)


Apr 23, 2025


Hey there, this is Daniel at Onlook!


If you’re receiving this update, it’s because either myself or Kiet Ho wanted you to know about what’s going on with Onlook.


As a reminder, Onlook is building an open-source product studio (formerly an extension) that turns any product team into an engineering team. We make it easy for anyone to visually design and edit websites and apps without writing any code themselves.


## TLDR


-


We pivoted from the Chrome Extension to Onlook Studio, our open-source desktop app


-


Onlook Studio became #1 on HackerNews at launch, and earned 1.4k+ GitHub stars since


-


We now target developers rather than designers and plan to launch on Product Hunt next month.


-


We gained multiple significant code contributions, 60 forks, and 200+ clones of the project on GitHub


-


New features include a detailed layer list, responsive editing, undo/redo, multi-select, and an onboarding screen.


## Announcements


### Onlook Studio is our new desktop app product, and our primary focus


One of the key points of feedback we received was that there was skepticism for recruiting developers to use the tool in their workflows. Designers would obviously like to have more control over how things get implemented, but today we need developers to be bought-into the tool and to trust the code output.


With that, we built a local-first visual editor that ties 1 to 1 with existing React codebases. This is tackling the design to dev handoff from a different angle, making it easy for developers to be able to build UIs today without much setup at all.


While there is still a lot of work to do to get the Studio experience up to par with other design tools, it already feels much punchier and more reactive than the Chrome Extension. For more details on how we’ve been working to get to a baseline design tool, see some of our updates in the Product section.


### Open-sourcing


We decided to open-source Onlook Studio for a couple of reasons:


1.


For any devtool that interacts with a codebase, it’s a good way to get developer trust and buy-in as they can inspect the code.


1.


Our users are also our contributors, opening issues, requesting features and sometimes contributing the features themselves.


1.


Being open-source helps with distribution as we provide inherent value to developers who are hoping to learn, converting them to users in the process.


### Launch on Hacker News


Kiet packaged Onlook up and posted it on Hacker News, and it rose to the top of HN. The post got 300+ upvotes, 90+ comments, and drove a ton of traffic to the site and GitHub repo. The launch helped us earn over 1.4k+ stars on GitHub and generated over 50,000 visits to the main assets for this launch. Please


[drop a star on the repo](https://github.com/onlook-dev/onlook) if you have a GitHub account!


### We’re pulling the Onlook Chrome Extension from the store


Riding off of the news from our launch, some people mistakenly arrived at the chrome extension, and it caused a bit of confusion. It’s difficult enough to focus on one product, let alone two, so we decided to take it down. The future of the tool may look a bit different, with a closer connection to Onlook Studio, but for now we’re focusing our efforts, as we’re still just a team of 2.


“These aren’t the droids you’re looking for” - Kenobi


## Product


### Layers


You can now see the full list of elements rendered on the page. To make it easy to understand, you’ll see they’re also named and indicated with icons for the type of layer they are. We’re super excited to use layers as the foundation for so many other great features!


### Responsive editing window (Shoutout to @


[nicklammo](https://github.com/nicklammo) for contributing!)


You can now resize your window with handles on the right, bottom, and lower-right corner of the window. This means you can develop and test how things look on different screen-sizes in real time!


### Undo / Redo


Now you can CMD+Z your edits, just like you’d expect with an editor.


Special thanks to @


[bkrmendy](https://github.com/bkrmendy) for building out the edit history and linking up the feature to the UI.


### Multi-select


When you select one component, we want you to be able to select all components so you have a better sense of how your change will affect other instances. This makes it much easier to communicate how things are connected in your workspace.


### Action mode


At the top of the app, you’ll see two modes — Design and Action. In Action mode, you can interact with elements without worrying that you’ll accidently change a property or (eventually) accidently move it.


### Welcome to Onlook


A new onboarding screen greets users with helpful links and an email sign-up for updates. Thank you to everyone who subscribed so far!


## Challenges


### New form factor and audience means a new approach


Our existing waitlist of 1k+ was mostly designers, and while there were some technical people in the mix, we’ll need to adjust our GTM to go down more developer-focused channels. We’re now back to doing a lot of discovery calls, adjusting our roadmap for this new core customer.


### We have very little visibility into the day-to-day usage of the product


The Hacker News launch was a great way to get a lot of signal, but in an effort to put it out into the world and make it as open-source-friendly as possible, we didn’t include many analytics. We just added light analytics last weekend but are planning on capturing more granular data. We’re still finding the balance between intrusiveness and visibility, which is especially important for an open-source project.


## Next Month


### Product Hunt Launch


The Hacker News launch was immensely helpful in generating inbound interest to the platform, so we’re hoping to replicate that with a launch on Product Hunt. If you are on Product Hunt, please


[sign up to be notified](https://www.producthunt.com/products/onlook-2) when we do launch! We’d very much appreciate your support.


### Continue to generate organic traffic while manually recruiting new partners


Almost all of our users still come from external channels, so we’ll continue to spread the word, but we will also look to recruit some early partners to bring Onlook Studio into their development workflows.


### Component detection & insert components


With an updated customer profile, we’re reversing our roadmap. Features closer to development are moved to the top, to make developers more efficient, while more basic design features like inserting divs and primitives move down the line.


### Better metrics & visibility


We’ll be getting more analytics to better understand the retention of users, as well as monitor the usage of the tool at a more granular level.


## Thanks & Asks


-


Thanks to Mark Percival at 1984.vc for not only making a


[contribution](https://github.com/onlook-dev/onlook/pull/38) to Onlook but also helping us brainstorm our new dev-first Go-To-Market strategy. Also, thanks to Ellen Chisa at Boldstart, Peter Zakin at Upfront Ventures, and Daniel Chesley at Work-Bench for ideating on Go-To-Market with us after the product pivot.


-


Thanks to Rowan Hume for continuing to introduce us to great investors.


-


Thank you to Cristian Cabrera for continuing to support and offer guidance.


-


We’re looking to land some initial partners to develop our product alongside us. Ideally these are agencies who are building React apps for their clients, or startups that are building their product on React. If you have any in mind, please reply to this email to let us know who would be good to chat with!


Best,


Daniel & Kiet


Cofounders of Onlook


---


Subscribe for free to read more about how we’re building Onlook.


---


Check out our other declassified updates to read more about our progress from the first year of building Onlook:


[Our June Update](https://onlook.substack.com/p/june-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell) and[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


January 31, 2025


[Read full story](https://onlook.substack.com/p/june-2024-update)


[Our May 2024 Update](https://onlook.substack.com/p/may-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell) and[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


December 11, 2024


[Read full story](https://onlook.substack.com/p/may-2024-update)


[Our April 2024 Update](https://onlook.substack.com/p/our-april-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho) and[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell)


·


September 4, 2024


[Read full story](https://onlook.substack.com/p/our-april-update)


[Our March 2024 Update](https://onlook.substack.com/p/march-2024-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


May 7, 2024


[Read full story](https://onlook.substack.com/p/march-2024-update)
