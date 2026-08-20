---
schema_version: "1.0.0"
document_id: "a74db959d69f8ec90fe23e77907fb434f509af6896bbbb8e037de30394ee7c64"
company_key: "yc-votingworks"
company: "VotingWorks"
source_id: "yc-votingworks-news-import-8770ef899700"
canonical_url: "https://www.voting.works/blog/votingworks-news-1/the-road-to-vvsg-2-0-certification-23"
published_at: "2021-12-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:12:37.331852+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:0e459ed5368ff1a6d9d8aa7ec7cd6c6858d8a3b800be4f3f61def3ae29e64039"
---

# The Road to VVSG 2.0 Certification

*A while back, we asked Twitter if we should take you along on our voting system certification journey, share our experiences publicly and hopefully educate the broader elections community about what this process looks like from the inside. We’re an open-source nonprofit after all, why shouldn’t our certification process be in the open, too? Hearing that some of you would be interested, we are now committed to that goal - and this is the first blog post of that series.*


It’s the end of the year, and time for a little reflection. This year brought a much-anticipated update to the federal standards for voting machines, the Voluntary Voting System Guidelines (VVSG). When it was finally adopted in February of this year, VVSG 2.0 became the first real update to voting technology standards in 15 years; the previous standard was adopted before iPhones existed, if that gives you a reference point. So we thought it was time to check in - 10 months on, where are we?


The first thing to know is that the standard isn’t really ready, yet. The[principles, guidelines, and requirements](https://www.eac.gov/sites/default/files/TestingCertification/Voluntary_Voting_System_Guidelines_Version_2_0.pdf) were adopted by the Election Assistance Commission (EAC), the first and biggest milestone. The EAC also published two iterations of select[test assertions](https://www.eac.gov/sites/default/files/TestingCertification/VVSG%202.0%20Test%20Assertions%20v1.1.pdf) , which is what the Voting System Test Laboratories (VSTLs) actually use to test systems and where the “rubber meets the road,” so to speak. But, even with all that progress, no vendor can start the process to certify to the new standard. Why? Because the test labs are waiting on guidance necessary to update their own processes prior to beginning the accreditation process. This guidance comes in the form of an updated[National Voluntary Laboratory Accreditation Program (NVLAP)](https://www.nist.gov/nvlap) manual and associated documentation from the EAC. Without those updates there’s no accreditation for VSTLs, without accreditation there’s no testing, and without testing no system can be certified. Updates to the manual are scheduled to be completed by the end of 2021, but it’s unclear if the EAC will meet that deadline. The labs we’ve talked to tell us they don’t expect to actually be accredited to test until 2023, two years after VVSG 2.0 was officially adopted.


These delays are part of the reason why most legacy vendors are[not planning to upgrade to 2.0 until 2025](https://unisynvoting.com/security/faq-about-the-vvsg-2-0/) (notice that’s *after* the next presidential election). If it seems a little weird that a bunch of competitors would declare their intention not to compete for the latest certification until an agreed-upon date, you’re right to wonder. There’s a good reason to set aside competitive advantage in this case, however. Currently, legacy vendors who had a system previously certified to VVSG 1.0 can continue making major updates and re-certifying to the old standard, while new vendors (like us) cannot - new systems that have never been certified before are required to certify to VVSG 2.0. Since no one can certify to 2.0 yet, and[most states](https://www.eac.gov/sites/default/files/TestingCertification/State_Requirements_for_Certification09042020.pdf) rely on the EAC Testing & Certification Program or portions thereof for their state certification process, this means that new vendors are locked out of the majority of the market. The EAC, in its understated way, probably says it best: “[This is an issue in terms of barriers to entry.](https://www.eac.gov/voting-equipment/voluntary-voting-system-guidelines) ” Quite.


To the EAC’s credit, they are trying to level the playing field - which brings us to the final reason for the legacy vendors to drag their feet. According to the newly released[VVSG Lifecycle Policy draft](https://www.regulations.gov/document/EAC-2021-0001-0001) , once the first VSTL is certified to test to 2.0, a 24-month countdown begins. At the end of that countdown, legacy systems will no longer be able to make major updates and recertify under the old standard - they, too, will have to upgrade. Meaningful deprecation of the old standard is a major win in terms of actually moving the market forward, but getting that clock started is what it all hinges on. Read more about it in our public comments to the EAC,[here](https://public-assets.voting.works/website/2021/12/2021-12-07-votingworks-vvsg-lifecycle-policy-public-comments.pdf) .


So, what are we doing in the meantime? Lots. We’re making sure our systems meet the requirements, first and foremost - taking care of things like choosing which external coding standard we’re going to adhere to, and building those checks into the linter that validates every line of code we write. (The standard we’ve chosen is the[Google Typescript Style Guide](https://google.github.io/styleguide/tsguide.html) , if you’re curious.) We’re complying with best practices in VVSG 2.0, like making sure we can import/export data in all the[election-related common data formats](https://www.nist.gov/itl/voting/interoperability) . The VVSG is also a camel - a horse built by committee - so there’s a fair bit of back and forth with the EAC to figure out what various requirements actually mean.


> *“When it says a module only has one function, does that mean one purpose or, literally, one callable function in code?”*


(It’s the former, in case you were wondering.) Once that first VSTL is accredited, we plan to be first in line to start testing, so there’s plenty to do even with the delays.


More on that and other topics in our next update - stay tuned!
