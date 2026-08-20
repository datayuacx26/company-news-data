---
schema_version: "1.0.0"
document_id: "7cdcea146b07fec430b5e9dbabd61c78459246be4e5a2397379f88a673f37e38"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/shared-responsibility-model-how-we-approach-security-in-guidewire-cloud-part-2"
published_at: "2022-04-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:8f5c17068c0f20d15bbacd87f9172c808286c644d7594406eed153300db527e6"
---

# Shared Responsibility Model: How We Approach Security in Guidewire Cloud – Part 2

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Shared Responsibility Model: How We Approach Security in Guidewire Cloud – Part 2


*This blog post is a two-part blog post about how we approach security. In[Part One](https://www.guidewire.com/resources/blog/developers/how-we-approach-security-in-guidewire-cloud-part-1) , we discussed what we do at Guidewire to build trust with our customers, how we approach security as a company in the products we build, and how we demonstrate our commitment with compliance and third-parties. In Part Two, we’ll talk about the “Last Mile of Security” and the responsibility that Guidewire and our customers share in investing in and promoting security best practices.*
**In related news:** On April 6, 2022,[Guidewire announced](https://www.guidewire.com/about/press-center/press-releases/20220406/guidewire-co-founds-it-isac-critical-saas-special-interest-group) that the company is a co-founding member of the Critical SaaS Special Interest Group (CSaaS SIG), part of the IT-ISAC. The new group aims to enhance the collective defense of its members’ customers and build increased security resilience in the SaaS industry as a whole. Joining Guidewire are fellow founding members:[MongoDB](https://www.mongodb.com/) ,[Okta](https://www.okta.com/) ,[Oracle SaaS](https://www.oracle.com/index.html) ,[ServiceNow](https://www.servicenow.com/) ,[Workday](https://www.workday.com/) , and[Zscaler](https://www.zscaler.com/) .


## Guidewire Customers and the Last Mile of Security


Imagine going on a trip. If you’re one person planning a trip, you can easily plan where to go, how long it will take, what you need to bring, and how long you’re going to be there.


There are many details, but it’s not incredibly complicated with just one person. However, when you invite more people to join you, it gets exponentially harder to plan and execute your trip.


That’s a good illustration of the complexities of security in SaaS (Software as a Service) application development. When we say “the Last Mile,” we’re referring to the point in our journey when we deliver our products to our customers and what happens from that point on. The rest of the trip is marked by how we work together with our customers to uphold strong security practices for mutual benefit and protection. And that brings us to the shared responsibility model.


## Shared Responsibility Model Examples in SaaS


Almost every SaaS company has a Shared Responsibility Model. If you’ve been interested in the security environment, you have likely seen models like this before. We have examples below from Amazon, Microsoft, Google, and others.


At Guidewire, we wanted to create our own Shared Responsibility Model, so we explored these different models, talked to some of our customers, looked at existing engagements, and pulled it all together into one model that works for our customers and us.


A Shared Responsibility Model is used to help each customer know where to invest in security in their companies, and where to build additional skills and competencies as time goes on. It explains the value of what we bring to you as a SaaS company, but it’s not a[RACI matrix](https://www.cio.com/article/287088/project-management-how-to-design-a-successful-raci-project-plan.html) .


## Guidewire’s Shared Responsibility Model


For Guidewire’s Shared Responsibility Model, we broke down eight different parent categories of security and determined what Guidewire is responsible for (blue), what our customers are responsible for (white), and what responsibilities we share (half blue and half white).


Now, as an example here, using this story of how we help Self-Managed clients move to the Guidewire Cloud Platform, you can see how we have a lot more of shared responsibilities (half blue and half white) — up in the Guidewire Cloud Platform. That’s pretty awesome.


But to help break it down even further, we dug into a collection of subcategories for each category to provide more clarity.


So now you can see there’s an even more apparent, more precise distinction of responsibilities between Self-Managed and the Guidewire Cloud Platform: who owns what within each subcategory.


## Guidewire Cloud Security Standards


So now that we know who owns what, what if you don’t know where to start? What if you don’t know how to do the things you’re responsible for?


We spent a lot of time thinking about this, and we came up with a[collection of cloud standards](https://developer.guidewire.com/guidewire-cloud-standards-for-developers/) . Within our Documentation site, you can take a deep dive into our latest, most up-to-date[Guidewire Cloud Standards](https://docs.guidewire.com/cloud/standards/latest/Overview/overview.html?_gl=1*1hj19rv*_gcl_au*MTUwMTA3Nzg0NC4xNzI1MDQ1MDUz*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMjMwNTUyOC43OS4xLjE3MzIzMDY5ODcuNjAuMC4w) by release (login required to view).


Within those standards, we explain different aspects and ways to adhere to what will keep you more compliant so that you’re in line with fast cadence updates as we move through each one of the different ski releases.


As a security example, we have different standards looking at payment card information (PCI) compliance and how to ensure that your code doesn’t contain PCI, or even personally-identifying information (PII).


## Gosu Secure Coding Standard


I also want to show you a unique standard we’ve included and how it came to be. It is IS-SEC-1289 which is the[Gosu Secure Coding Standard](https://docs.guidewire.com/cloud/standards/latest/Security/Standard-IS-SEC-1289-Gosu-Secure-Coding-Standard.html?_gl=1*nepiwl*_gcl_au*MTUwMTA3Nzg0NC4xNzI1MDQ1MDUz*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMjMwNTUyOC43OS4xLjE3MzIzMDcwMDkuMzguMC4w) (login required to view). You’ll see that this document contains several rules which break the standard into smaller digestible chunks.


[Gosu is Guidewire’s custom language](https://www.guidewire.com/developers/developer-tools-and-guides/gosu-programming-language) on top of the Java programming language. Since Gosu is derived from Java, it enables us to leverage existing java security guidance. Guidewire’s InfoSec team hired a luminary from the security realm,[Robert Seacord](http://robertseacord.com/wp/) , to help us wade through any potential Gosu-related security vulnerabilities, along with the[OWASP](https://owasp.org/) top 10 related issues and other variants that would be unique to Gosu or Java. So that’s how we came up with 18 different rules within that standard.


## Moving Towards Static Application Security Testing


We’ve also been thinking about making these rules more consumable so that you can actually use them. Because it’s great that we have all those standards, but frankly, reading through each one takes a lot of time, effort, and understanding.


[Static Application Security Testing (SAST)](https://www.synopsys.com/glossary/what-is-sast.html) is a technology similar to static code analysis, which many program languages have, but focuses on security. It allows you to look across the code itself and look for specific abnormalities and things that are wrong inside the source code.


[Inside IntelliJ](https://www.jetbrains.com/idea/) itself, we have a parser that will do some of that for you. Of course, there are well-known programs out there that will do this for other programming languages, but we have our programming language of Gosu, and there are no existing third-party vendors that target Gosu.


So we’re in the process of taking each one of those rules, pulling them into IntelliJ, and adding them as an extension to the Gosu Studio itself. That way, you can run these and see these abnormalities when they come up inside your code. So that project is not complete, but it is in process.


## Using Checksums in Code Verification


With every Guidewire Cloud ski release, we publish checksums for that release on our Documentation site, like PolicyCenter’s checksums for the Dobson release (login required to view). So as you download our product, this allows you to verify it across the checksum to ensure that there’s been no malicious code injected into that package that’s coming out.


These checksums are available right now, right this second. We’re also planning to apply that same technology across the customer’s client code as they build things and have these checksums available. It’s not available now, but it is in our path to go forward.


## No Finish Line for Security


For Guidewire, our approach to security will never be done. There’s no finish line. And that’s the way we intend to approach it because we know that the security environment around us continues to evolve.


And it’s essential to remember that we’re on this journey together.


We have a vision for the future, but we also want the feedback that all of you bring to us. We’re in the same car, and we want to make sure we’re moving forward together.


*The above post is a transcript of a session titled “Security in Guidewire Cloud, presented by James Dolph and Mark Sayewich at Connections 2021. The transcript has been condensed and edited for easier reading.*


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
