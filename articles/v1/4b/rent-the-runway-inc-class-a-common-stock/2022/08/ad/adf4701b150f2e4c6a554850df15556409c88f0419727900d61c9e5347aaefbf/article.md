---
schema_version: "1.0.0"
document_id: "adf4701b150f2e4c6a554850df15556409c88f0419727900d61c9e5347aaefbf"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/how-rtr-moved-to-the-cloud"
published_at: "2022-08-08T13:00:00+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:da90f6e6dc0be517e6f724b4b0855742e409407aecb2384541aee8b42694d3aa"
---

# Putting the Closet in the Cloud, in the Cloud: How RTR Moved to Cloud Services

## Why did we decide to move on to Cloud technologies?


At Rent the Runway, we pioneered a new business model for fashion rental and subscription. At the time, the appropriate off-the-shelf technology did not exist. So we created the technology to enable fashion rentals and reverse logistics to power the business.


Historically, our tech stack ran in a traditionally hosted datacenter with a services architecture running on self-managed virtual machines. Fast forward 11 years, after building and proving out the business, to the beginning of the pandemic. We had to make a lot of long term business impacting decisions. One of those decisions for RTR was to focus on our tech stack and ensure it would continue to effectively power the business into the future. We knew that there would be some product development cycles available during the pandemic for our teams to focus on core technology enhancements. Plus, overall, we were confident in our belief in the business and that our customer base would continue to grow after lockdowns subsided. We wanted to ensure that we were prepared to operate efficiently at scale and be well-positioned to provide continued enhancements to the customer experience, such as around search, discovery and fit initiatives. To that end, we have focused on the key benefits of being in the cloud such as elasticity, enhanced reliability, and better economics.


Our business is seasonal; that is, customers change their behavior at different times of the year. Our broader customer interaction has been somewhat predictable over the long term but also highly variable during the day and week. Gaining elasticity with our infrastructure would help us continue to deliver on our customer promise and maintain sound economics. Traditional hosting didn’t provide the agility we needed for scaling over the long-term, but the ability to quickly spin up new service instances and supporting technology did.


We also understood the benefits of an enhanced reliability posture. Moving to Google Cloud Platform enables us to spread our software assets across a broader array of physical hardware and increase our overall availability. We’re also able to leverage additional hosting regions more economically for both load balancing as well as disaster recovery. Google Cloud was our choice given their leadership position in driving and leveraging cloud technology.


Traditional hosting workloads also burden teams by requiring management of machines and associated administration along with slower processes. In the cloud, we benefit from rapid scaling of instances, which can be added in minutes instead of days or weeks. We also have the ability to utilize managed commodity services instead of running these services ourselves.


Finally, regularly iterating on and modernizing our tech stack enables opportunities outside of the operational objectives. Our employees gain new growth opportunities as they can learn and positively impact the company much more quickly than in the past. We are also able to advance several of our environmental sustainability goals by moving to the cloud, something that is a key priority of our business. The elastic nature of the technology ensures we’re only using the resources required to run our business. Google Cloud has superior efficiency (2x more than a typical data center) and shares our commitments to decarbonization through 100% renewable energy and waste diversion.


## Is this all we worked on?


Of course not! Complicating the cloud migration were some other key drivers for our business. We were preparing for an IPO, which also required considerable focus from nearly every technology team. Additionally, we were still focused on supporting day-to-day operations and growth. We were tasked with delivering features for operations efficiency as well as customer growth, acquisition, and retention.


## How did we accomplish this?


Given the additional priorities, we had to be very thoughtful in our approach. We were laser focused on breaking down the work into pragmatic deliverables. We started with a small focus group of leaders and staff engineers. The decision-making was distributed deeply into the organization with a clear governance structure to maintain consistency and knowledge sharing across the teams.


We set five service and application reliability requirements which were taken on by every team for identification of the required work. Since most teams were not solely focused on Google Cloud migration, they had to work these tasks into their product backlogs. We ensured there was clear ownership for applications and services by individual teams. These teams then understood what was expected of them given our reliability requirements.


One team responsible for transformation was the Infrastructure team. They were focused on the big rocks of the migration effort. We were transforming from a standard virtual machine infrastructure to a kubernetes/container architecture. We designed the new architecture with the focus on meeting our business and technology goals.


The most instrumental decision was to design for rollback. We didn’t assume the migration would be flawless, and knew that we always had to have the ability to come back into our traditional datacenter. We expected to learn and potentially revert post migration for up to 14 days. We practiced rollover and rollback numerous times during off hours and in the end this was the biggest driver of confidence when the decision became a full-on “go.”


Another activity that built confidence was conducting a pre-mortem well in advance of our migration. Instead of waiting until after the migration for a post-mortem (when potentially many things had gone wrong), we conducted a pre-mortem identifying what could go wrong. We leveraged a broad group of knowledgeable people to identify what could go wrong in the new environment and had gone wrong for us in the past. We then prioritized these findings based on the likelihood of each happening and impact if it were to happen. For all the findings rated at medium or higher, we put mitigation plans in place. Interestingly, there was very little coding required to mitigate - most were process and observability related.


## What was our delivery strategy for this project?


Our plan was to migrate into Google Cloud as seamlessly as possible and then transition to new technology. This held true in all instances except for our core infrastructure - in this instance we migrated from VM’s to containers orchestrated by kubernetes and associated support. For this, we converted all of our configuration to infrastructure as code, increasing our deployment automation which reduces the time for developers to get code into production and makes the environment more reliable and provides the foundation for scale and Disaster Recovery automation. Also, all deployments would need to go into our production environment (traditional datacenter) as well as the new Google Cloud environment. In other words, we had to parallel process and build in redundancy to mitigate risk and increase efficiency.


For all other capabilities, we had to maintain core business functionality in Google Cloud and needed to retain observability, diagnostics, and monitoring. We kept the project as simple as possible which wasn’t easy to do. The approach was to transform where required and migrate the rest. We were constantly focused on controlling scope creep to ensure a clear and timely path for success.


Quality was key for us as we needed to fully support the business and customer functionality independent of our hosting location. We leveraged our automation to continually verify the new environment as changes were deployed. We also conducted numerous bug and performance bashes leading up to the switchover. This engaged a broad set of employees, many of whom are also customers and know our site well. They were instrumental in further building confidence in our solution.


## How did we do?


We successfully migrated to the cloud and stayed there with no need to rollback. Overall, the end process was smooth and nearly all associates and customers were unaware that the infrastructure beneath them was swapped out. To us in technology, this is the ultimate victory and sign of excellence.


Along the cloud migration journey, we delivered on our business priorities like the IPO and handling a high rate of customer demand. We also found quick benefit in rapid scaling of service instances to handle demand or other issues that occurred post migration. Our project was delivered successfully and we’ve begun the next phase of adopting new technologies and paradigms in the cloud.


## What happens next?


Now that we’re in the cloud, we have begun migrating our stack to native technologies. We have a real business driver for reserved and automatic scaling of resources. We are mapping our demand activities to increased resources for servicing visitors. Further, at times we can respond to resource needs based on traffic in the moment and at other times we require reserved instances to ensure a positive customer experience.


We plan to continue to evolve our solution and will stay focused on adopting/leveraging new technology that moves our business forward. We believe that integrating key cloud technologies like machine learning, datastores, and new ways to build functionality into our business will provide future benefits. We are running our cloud evolution process similar to the migration process outlined above: leveraging a key set of employees and distributing decision making out to the areas of knowledge and need.


**Cautionary Statement Regarding Forward-Looking Statements**


*Forward-looking statements include all statements that are not historical fact, including statements related to the migration of our technology stack to the cloud, the potential benefits and future uses of cloud technologies for our business, and our ability to successfully evolve our current technologies. Forward-looking statements involve substantial risks and uncertainties that may cause actual results to differ materially from expectations. These risks and uncertainties are more fully described in our filings with the Securities and Exchange Commission, including in the section entitled “Risk Factors” in our Annual Report on Form 10-K for the year ended January 31, 2022, and subsequent reports that we file with the Securities and Exchange Commission. Forward-looking statements represent our beliefs and assumptions only as of the date of this post. We disclaim any obligation to update forward-looking statements, except as required by law.*
