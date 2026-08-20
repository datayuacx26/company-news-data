---
schema_version: "1.0.0"
document_id: "722b52665f9524dec5280775e5f0dba34e97a592e83257c111c0be8319bb2ff2"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/may-2024-update"
published_at: "2024-12-11T13:42:41+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:dd585c24d99cac2122ba51a794dd2829a24e3fe4b1ae3462afb24a931df2acd2"
---

# Our May 2024 Update

# Our May 2024 Update


### Declassifying our old Friends and Family updates


[Daniel Farrell](https://substack.com/@danielfarrell)


and[Kiet Ho](https://substack.com/@kietho)


Dec 11, 2024


Hey there, this is Daniel at Onlook!


If you’re receiving this update, it’s because either myself or Kiet Ho wanted you to know about what’s going on with Onlook.


As a reminder,


[Onlook](https://onlook.dev/) is building a browser extension that turns any product team into an engineering team. We make it easy for anyone to edit web products directly in production, and then effortlessly push their changes to GitHub without having to write any code themselves.


## TLDR


-


Kiet’s visiting Daniel in NYC


-


We have our first paying user


-


Soft launch to Reddit and our new website


-


New feature: Generative AI Styles


## Announcements


### Kiet’s visiting NYC!


Kiet is in town to co-work with Daniel and eat some absolutely delicious NY cuisine. We kicked things off with a slice of Margherita + Burrata thin-crust pizza from L’industrie, and a Gentile Gelato walk through Washington Square Park. We’ll be road mapping the next quarter, redesigning our product onboarding, and enjoying the NY summer vibes. Kiet’s already become a hardcore Citi-biker – “It reminds me of Vietnam”, he said.


### First paying customer + Champion-led growth


We got our first paid subscriptions by Joe from


[SMB.co](https://smb.co/) . He’s using Onlook as a product owner to communicate improvements to his product with engineers. We weren’t expecting to experiment with pricing just yet, but it’s great to have validation from a customer.


Erika from


[iAlign](https://www.ialign.work/) has been an active user of our tool since March. This month she onboarded the product with her Product Manager and QA Engineer, it’s great to see that the product (even with its quirks) can organically expand within teams.


### Soft public launch + new website


Throughout May, we’ve built enough conviction with early users to start experimenting with making the product publicly available. We started with updating the


[website](https://onlook.dev/) with a link directly to the extension, no longer requiring a user to go through a founder onboarding call. This is an experiment to get more people into the product and to understand what unique use cases emerge from just having people try out our extension.


### Experimenting with Product Led Growth channels


Kiet made a


[post about Onlook](https://www.reddit.com/r/sveltejs/comments/1cndnxb/built_my_entire_business_on_svelte/) in the r/Svelte Reddit. It picked up a ton of interest from the community and grew to the top post of the week, then the 2nd top post of the month. While it did drive over 1,000 people to the extension and over 400 visits to our site, it only converted about 30 installs. This is an engineering subreddit, we’ll be targeting more product oriented subreddit such as r/design and /productmanagement in the future.


### …while experimenting with Sales-Led Growth.


Aside from three users manually adding their GitHub on their own, we started a sales motion with some friendly teams to experiment with a more sales-centric approach. We’re chatting with four early-stage teams to get them set up with our codebase. We ultimately will have to choose the right growth motion for our product, and part of the learning here was to see if we can get a quicker path to activation (“the Ah-Ha!” moment) for users.


### New market insights


Through our conversations with customers and other builders in the space, we gathered some more validating intel about our approach:


-


Builder.io (a competitor in the space) takes a full-time employee to set up and maintain and only applies to specifically built sections of a page. Onlook’s approach is much more robust and increases the surface area of how people can contribute to a codebase.


-


Other adjacent competitive leaders in the design space explicitly said they were not interested in competing in the product / building space, or they said our approach was novel and something they weren’t going to build. We recognize it sounds pretty naive to say they won’t try to ever compete, but we’re encouraged that our capabilities are putting us into new territory in the design / product building space. At the very least, we’re forming our technical moat for this approach.


## Product


(Kiet’s favorite section to write)


### Onlook, brought to you by Onlook


We built Onlook to solve our own problem, and as part of solving our own problem, Daniel took the lead on building out the “Welcome” page on the Onlook dashboard using Onlook.


You might be thinking “So what? Daniel built a website?” Well… let us explain why this is a big deal.


Our dashboard is built with Svelte and TailwindCSS, a niche framework and structure that enables us to move fast but takes a bit to get used to. Using Onlook, Daniel was able to ignore all of this setup and just design directly in the Welcome page, then send his changes of the actual interface as a pull request to Kiet.


We made a point to restrict Kiet from writing any code in the page besides the initial setup and any smaller features (like using our design tokens) that we don’t support yet. Right now, the split of work was 90% Daniel and 10% Kiet, as Kiet stepped in to fill some gaps in our editing experience. We’re working on bringing this down to 99 / 1%, then of course, 100% Daniel.


### Generative AI within the tool – Chat with your interface


Our mission is to turn any product team into an engineering team – This means continuing to obsess over how we can make the process of building a product easier for less technical users. With that, we’re thrilled to introduce “Generate styles”, allowing users to make complex changes in simple, plain-English commands.


Even with the best design tools, there are complex actions such as setting breakpoints for designing on mobile vs. desktop, ensuring sites are accessible, creating shadows, and more that all take significant time and skill to accomplish. The chat interface will allow us to offer those capabilities without having to build interfaces for them.


The generated style experience is just the first step in introducing more advanced functionality such as interactions (“When a user hovers over this, make this pop up”) and eventually features (“When a user clicks on this button, make this happen”). And of course, the changes you make using the editor will be writable to a codebase.


What’s most exciting about this medium is that we can use components from the codebase in the page itself, helping equip designers with their styles and design systems right out of the box. The reliability of knowing that what you’re building is usable with a real production codebase will be an immense unlock for designers.


### Introducing AI’s boring cousin: Compiler


Code generation is a balancing act between powerful features and safeguards to make them reliable. Giving a Large Language Model (LLM) full unstructured capabilities to write code makes for a compelling demo, but when that code also gets shipped into production to thousands of users, it needs to be iron-clad.


At Onlook, we make the code generated by the AI consistent by processing the output with a powerful language-agnostic compiler. This month, we made further improvements to predictability in the output, lowering the failure surface and making the system more fault tolerant. This resulted in a substantial decrease in “failed” builds.


In even simpler terms – when we first used AI, if it “hallucinates” 1 single bad token, it can make the result unusable. We built a robust compiler that severely limits the scope of failure, giving AI extremely narrow and explicit instructions. What that means is a way more reliable code output that we can use with more types of codebases.


Here’s a simplified overview of the approach. Without compilers, we’re feeding the LLM raw texts and praying 🙏. With the compiler, we give it just what it needs, then do post processing on the output before writing it back to code. Errors are contained within the attribute and can be isolated, tested and retried. Any code changes that can be made deterministically are isolated and covered.


We are also doing few-shot prompting, function calling, chaining, etc, but that’s a different blog post.


Subscribe for free to receive new posts and updates on how we’re building Onlook


## Challenges


### Onboarding


Users have been (understandably) confused by our onboarding. An ideal onboarding experience would get users to directly push code as quickly as possible, but it’s tough to know what to do after the extension is installed. The Welcome page has helped with conversion, but there are a lot of fixes we could make to the experience to avoid self-serve users from dropping off. A big focus of the next month is getting people to the “ah ha” moment much quicker with onboarding.


### Attaching a codebase is still complex


While our users can onboard to the extension and make changes to their apps, users are confused when trying to connect to their GitHub for that last step of publishing changes. We’re scoping ways to do a 1-click setup with Onlook on their codebase without cloning the repo or going through an engineer at all. Many designers already have GitHub accounts (but haven’t written any code!) so the ability for a designer to advocate for the tool, install it, and use it to write changes to the codebase will be a huge boost to their flow.


## Fundraising


### Interviewed with YCombinator


We interviewed with YCombinator a week(!) after we applied. We like to think that meant we were towards the top of the stack. Our panel was with


[Garry Tan](https://www.ycombinator.com/people/garry-tan) (CEO of YC) and


[Aaron Epstein](https://www.ycombinator.com/people/aaron-epstein) (Creative Market), both designers who are very very familiar with the space, and very intense interviewers. While they ultimately passed, expressing concern in how difficult it is to get buy-in from both engineers and designers, we’re grateful for the opportunity. Our onboarding improvements are top-of-mind for addressing this concern, and we’re already getting buy-in from smaller teams on bringing Onlook into their product process.


### An update on our Pre-Seed


Last month we kicked off the raise. We’ve received two term sheets and are waiting on some last steps for two others. There are a handful of other conversations still in progress, and we’re grateful for the support and enthusiasm from many of you who we took calls with for both advice and potential funding. It’s been an insane experience to meet some of the smartest people in the venture and startup world, and while for some of you, we may not have had the chance to partner at this stage, we’re excited to build relationships with you over the years to come.


### Other notes


-


As part of technical due diligence, we onboarded Angular as a framework. It only took a weekend thanks to the general nature of our tech. While we don’t plan to continue official support for Angular, we proved that Onlook truly can onboard to frameworks with little effort.


-


█████


**REDACTED MAJOR COMPETITOR** █████’s team reached out and we had a great discussion about our differentiation in the space.


## Next Month


-


We’ll be focused on improving our onboarding so that organic users who find our website and extension will retain better.


-


We’ll be finalizing our key partners for the fundraise.


## Thanks & Asks


### Thank you to...


-


Thanks to Rowan Hume, Jonathan Alder, Eric Drymer, Brandon Bourn, for introductions to investors and beyond.


-


Thanks to Rajiv Ayyangar (CEO, Product Hunt) for help in preparing for our YC Interview.


-


Thanks to Cintrifuse for the immense support, both helping with onboarding marketing help, bridging introductions, and more.


### Asks


-


We’re looking to connect with Product Managers and Product Designers at early-stage companies. If you know of anyone that fits the sub-100 employee count company profile, is building in B2B SaaS on the web, and is open to trying out new tools, we’d love any introductions. Feel free to share our sales deck – Onlook for Teams for more information.


-


Subscribe to our Substack Newsletter to learn more about how we’re building Onlook.


-


Know any incredible full-stack engineers who may be interested in joining the team? Let us know! Feel free to forward the job description to them or have them reach out to Daniel.


Best,


Daniel & Kiet


Cofounders of Onlook


---


Check out our other declassified updates:


[Our April update](https://onlook.substack.com/p/our-april-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho) and[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell)


·


September 4, 2024


[Read full story](https://onlook.substack.com/p/our-april-update)


[Our March Update](https://onlook.substack.com/p/march-2024-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


May 7, 2024


[Read full story](https://onlook.substack.com/p/march-2024-update)


[Our February Update](https://onlook.substack.com/p/february-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell)


·


April 17, 2024


[Read full story](https://onlook.substack.com/p/february-2024-update)
