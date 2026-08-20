---
schema_version: "1.0.0"
document_id: "0969a0e3bf8b9673cf092f326e022c0c7060cf86b94f706eee7d88f5ab905eab"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/aws-reserved-instances-savings"
published_at: "2023-07-09T19:00:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:f6bd4aa6a4b26630debf3872f0fb0d56e9c3422efe8d964d40418e618056cf53"
---

# AWS Reserved Instances - How Much Can You Really Save?

AWS services are available on demand by default. This is where you pay a monthly bill for services used at the demand rate. This setup can quickly become expensive if you are a business that uses a lot of services and deploys several instances.


For this reason, an increasing number of businesses are adopting AWS reserved instances (or RIs).


If you'd like help choosing a cloud cost management solution for AWS,[try Taloflow](https://use.taloflow.ai/signup) for a comparative vendor analysis suited to your exact use case.


## **What Is a Reserved Instance in AWS**


Introduced in 2012 by Amazon Web Services, Amazon EC2 reserved instances were developed to help alleviate the issue of increasingly expensive Amazon EC2 bills.


Officially described as a "billing discount" to the demand rate applied to the use of instances that are on-demand in your account, AWS reserved instances are not actually physical instances.


By committing to a long-term period, of one or three years in length, you'll receive discounted billing. As you commit to paying for all the hours of the full one or three year term, you receive a significantly lowered hourly rate and therefore greater cost savings.


Ideal for predictable and steady usage, reserved instances help companies save a significant spend on their Amazon EC2 costs.


You can find out more about how AWS operates[here](https://aws.amazon.com/what-is-aws/) .


## **AWS reserved instances types**


When it comes to AWS reserved instances types, there are two. These are standard reserved instances (SRIs) and convertible reserved instances (CRIs).


The primary difference between an SRI and CRI is that a CRI can firstly be swapped for another Reserved Instance (e.g. you can convert a m5.2xlarge in us-east-1 to a c5.2xlarge in us-east-2).


Secondly, the savings are slightly lower compared to an SRI because of the added flexibility.


## **Why does Amazon offer reserved instances?**


The simple answer is that, by reserving capacity in advance, businesses provide AWS with more stability and certainty about their long-term compute needs.


In return, customers guarantee payment of resources over a span of one or three years.


AWS Management Console - Purchase Reserved Instances


## **How much can you save?**


When it comes to how much you can save by committing to a long-term reserved instance, this is quite significant. Let's look at a few examples:


### **EC2 Reserved Instances vs. Savings Plans**


#### **EC2 reserved instance pricing**


- By committing to three-year no-upfront Amazon EC2 reserved instances, which is the highest possible discount without needing to pre-pay AWS, you will save an average of 57% on your AWS EC2 spend.
- By committing to a one-year reserved instance, you'll receive a 37% discount, which is a great discount for a one-year commitment.


#### **Usage AI savings calculator**


By using the[usage.ai Savings Calculator](https://www.usage.ai/aws/ricalculator) , you can determine exactly what you could be saving on your cloud costs with vs. without the Usage platform. Here are a few examples of the significant cost savings you can achieve with one m5.4xlarge on-demand instance that would be covered with a reserved instance:


- Annual ***savings*** with 1-year Compute Savings Plan: $1,787.04 (27% discount)
- Annual ***savings*** with 1-year reserved instance: $2,487.84 (37% discount)
- Annual ***savings*** with 3-year Compute Savings Plan: $3,320.04
- Annual ***savings*** with Usage Flex RI (zero commitment): $3,819.36


## **Limitations of AWS Convertible reserved instances**


There are a few limitations of AWS Convertible reserved instances to consider. Let's look at a few of these.


- You'll receive lower discounts for Convertible RIs than Standard RIs. You need to therefore consider that common usage instances, such as M4 medium Linux, are not a great fit for Convertible RIs.
- The pricing for the one-year convertibles is not all that attractive for those larger companies. The same can be said for companies with predictable workloads. In these instances, the three-year RI is a much better option.
- You can take advantage of future price reductions by using Convertible RIs, or you can handle usage changes. You need to be aware that you can't do both.
- Convertible RIs that are unused can unfortunately not be sold in the reserved instance marketplace.


## **How to avoid paying the AWS marketplace fee**


If your capacity planning didn't quite work out the way it should've, Amazon applies a 12% fee on the **upfront** portion whenever you sell an RI. However, if you sell an RI for $0 upfront, you don’t need to pay the 12% fee. That's an important consideration to take note of.


If you bought the RI with $0 upfront, then selling it at $0 upfront means zero loss. On the other hand, if you bought an RI for an up-front amount and then sell it at $0 upfront, then you do in fact take a loss. That’s why Usage AI only buys and sells no-upfront RIs.


## **How to price your RI’s**


Amazon will automatically suggest a price for AWS customers to sell their Amazon reserved instances and then decrease it automatically over time. This is to increase your likelihood of it being sold. There are loads of mathematical whitepapers that describe pricing RI’s on the EC2 RI Marketplace in much greater detail. Here’s one such[mathematical whitepaper](https://ieeexplore.ieee.org/abstract/document/8416358) that describes using an equation to calculate the probability of selling an RI based on instance type, region, and price.


## **Usage AI plug**


What about Usage AI? With the Usage AI plug, a platform that empowers companies to reduce their monthly cloud expenditure, you'll benefit from the following:


- You won't need to worry about figuring out when RI’s need to be sold
- You won't need to worry about pricing your RI’s
- You also won't need to worry about finding a buyer (in fact, Usage is contractually obligated to buy the RI’s back from you)
- You streamline your capacity planning and optimize cloud costs
- You finagle less with the AWS management console


## **What are the term commitments for RI’s?**


As we mentioned earlier, you can purchase a reserved instance for a one-year or three-year commitment.


However, reserved instances don't renew automatically. Once expired, you can still use the EC2 instance without any interruption, but you will then be charged on-demand rates, which gets expensive.


## **What are the three-year upfront RI’s?**


Three-year full-upfront reserve yield the greatest possible discount, with over 60% savings.


The trade-off here though, is that you’re pre-paying three years in advance which many finance companies may not be comfortable with. That’s why Usage only uses no-upfront three-year RI’s to get a very similar discount with $0 in pre-payment to AWS.


Let's take a look at Usage AI's arbitrage and guarantee.


### **Usage AI’s arbitrage**


Usage AI is able to manage the economic risk of buyback by trading reservations between its pool of customers, which is entirely operated by APIs for the AWS EC2 RI Marketplace. Every 24-hours, Usage scans each AWS account to see what needs to be bought and sold in order to maximize savings for all customers. You also benefit from a dashboard experience and reporting focussed on maximizing savings, unlike the AWS management console.


### **Usage AI’s guarantee**


Usage AI is contractually obligated to buy RIs back and therefore assume the financial risk instead of the customer.


## **Savings plans vs RIs**


AWS Management Console - AWS Savings Plans


Compute Savings Plans (CSPs) were launched after reserved instances as a simpler way of saving money with less commitment management and the ease of savings applied in any AWS region. With CSPs, all you need to do is commit to AWS with how much you're going to spend each hour. There is no need to worry about forecasting what instance types you’ll be using, or the AWS region you’ll be using them in.


However, you’ll be trading this flexibility for reduced savings compared to reserved instances.


- If you were to go with a one-year Compute Savings Plan, you’ll get a 27% discount. Similarly, if you can commit to a 3-year Compute Savings Plan, you’ll receive a discount of 49%.


On the other hand, AWS also offers EC2 Instance Savings Plans. With these, you can reduce the cost of the same instance family within a region by up to 72%.


## **Other popular cost-saving strategies**


There are other popular cost-saving strategies, including Spot instances.


### **Spot instances**


If potential downtime and task interruptions are acceptable for some or all of your workloads,[Spot instances](https://www.taloflow.ai/blog/aws-spot-management-xosphere) can help you reduce costs by as much as 90%! With Spot instances, for workloads that can be interrupted, this can sometimes save you more than RI's. (57% vs up to 90%). However, on top of the strong possibility of downtime, spot prices aren’t fixed as they are with RI's.


So, the price can increase or decrease in real-time, which can sometimes become even more expensive than on-demand. That's an important consideration. You can somewhat get around this issue by choosing good fallback instances that can still run your workloads at a lower cost, but that’s a significant time investment you should take into consideration when looking at your cost reduction strategy.


For Spot instances and rightsizing, we would suggest using cast.ai. Their AI engine is able to automatically resize your K8s clusters in real-time to reduce cost and maintain uptime by intelligently choosing backup instances, on top of a number of other features such as automatic patching and disaster recovery.


## **Are you looking to commit to AWS Reserved Instances?**


We hope the above article paints a clearer picture of the benefits and considerations of AWS reserved instances.


There is plenty to consider when it comes to using Amazon Web Services, so make sure that you choose the right service plan for your business.


**Editor's Note:** This post was originally posted on May 20, 2022, and updated on July 9, 2023.
