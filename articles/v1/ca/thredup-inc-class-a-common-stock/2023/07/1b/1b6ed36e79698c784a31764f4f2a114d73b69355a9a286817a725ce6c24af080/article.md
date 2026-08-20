---
schema_version: "1.0.0"
document_id: "1b6ed36e79698c784a31764f4f2a114d73b69355a9a286817a725ce6c24af080"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/how-aws-is-helping-thredup-revolutionize-the-resale-model-for-brands-amazon-web-services-54159b507967"
published_at: "2023-07-21T23:44:51+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:8c74a9db7c3190a12e78a7bf82ee9c41f553c78cc4b55972afea5b4f12136a6b"
---

# How AWS is helping thredUP revolutionize the resale model for brands | Amazon Web Services

# How AWS is helping thredUP revolutionize the resale model for brands | Amazon Web Services


[David Grumm](https://medium.com/@dgrumm?source=post_page---byline--54159b507967---------------------------------------)


7 min read


·


Jun 6, 2023


--


*Unleashing the Power of Resale: 💡 I‘m thrilled to share this thought-provoking blog post written by AWS’ Madeline Steiner* *about how thredUP and AWS work together to help inspire a new generation of consumers to think secondhand first! It’s a powerful testament to the potential resale has in transforming the way we shop and addressing the fashion industry’s wastefulness. Kudos to*[Ann Starodaj](https://www.linkedin.com/in/annstarodaj/) ,[Anton Naumenko](https://www.linkedin.com/in/antonnaumenko/) *and the rest of our Resale-as-a-Service (RaaS) team for leveraging AWS to support thredUP’s RaaS offering, leading the charge towards a more sustainable and conscious future. Let’s unite in making fashion eco-friendly and accessible to all.* ♻️👗 #SustainableFashion #ResaleAsAService #AWS


**Originally**[published](https://aws.amazon.com/blogs/industries/how-aws-is-helping-thredup-revolutionize-the-resale-model-for-brands/) **by Madeline Steiner | on 06 JUN 2023 | in**[Amazon EC2](https://aws.amazon.com/blogs/industries/category/compute/amazon-ec2/) **,**[Amazon QuickSight](https://aws.amazon.com/blogs/industries/category/analytics/amazon-quicksight/) **,**[Amazon RDS](https://aws.amazon.com/blogs/industries/category/database/amazon-rds/) **,**[Amazon SageMaker](https://aws.amazon.com/blogs/industries/category/artificial-intelligence/sagemaker/) **,**[Amazon Simple Storage Service (S3)](https://aws.amazon.com/blogs/industries/category/storage/amazon-simple-storage-services-s3/) **,**[Auto Scaling](https://aws.amazon.com/blogs/industries/category/compute/auto-scaling/) **,**[AWS Cost Explorer](https://aws.amazon.com/blogs/industries/category/aws-cloud-financial-management/aws-cost-explorer/) **,**[Industries](https://aws.amazon.com/blogs/industries/category/industries/) **,**[Retail](https://aws.amazon.com/blogs/industries/category/industries/retail/)


Like global landfills, the fashion industry waste problem is growing by the *second* . Retailers are struggling to address an enormous (and pressing) concern: what happens to their products after point-of-sale and what are the environmental implications?


In the United States, companies spend an estimated $50 billion on product returns. These returned goods are responsible for massive landfill waste and[27 million tons of carbon dioxide emissions annually.](https://www.fastcompany.com/90756025/product-returns-are-wasteful-for-companies-and-the-planet-heres-how-to-change-that) This is part of what’s called a linear economy, where we take materials from the Earth, make products from them and eventually throw them away as waste. For example,[research](https://ellenmacarthurfoundation.org/circular-examples/keeping-clothing-in-use-to-save-us-money-and-reduce-waste-thredup) shows that clothes in the US “are only worn for around a quarter of the global average and some garments are only worn between seven and ten times.”


After little wear, “these huge volumes of clothes are landfilled or incinerated each year.” This wastes not just the materials, but also the energy, water, nutrients, land, and other resources used to produce the textiles and garments.


On the flip side of this is what’s called a circular economy. According to the[Ellen MacArthur Foundation](https://ellenmacarthurfoundation.org/topics/circular-economy-introduction/overview) , the circular economy is based on three principles driven by design:


- eliminate waste and pollution,
- circulate products and materials (at their highest value), and
- regenerate nature.


Some examples of the circular economy in retail include resale, repairing, reusing, remanufacturing, recycling, rental, subscription, and more.


With growing support of this model and the concept of resale, more retailers are discovering the benefits of sustainably driven design and production. Whether retailers are driven by customer demands, reputation risk, or they’re just trying to get ahead of looming regulation, resale is a positive path forward for retailers to achieve their sustainability goals. Some added benefits of resale include:


- acquiring new, eco-conscious customers or consumers that can access a brand at a discounted rate,
- controlling the resale experience for their brand and
- driving additional sales.


If resale is so great for businesses, why isn’t every retailer embracing it? Unfortunately, building an in-house resale channel from scratch is complicated and expensive. Not all companies have the resources for complex initiatives like reverse logistics, authentication, and data collection, preventing them from making resale implementation a reality.


Fortunately for retailers, this is where[thredUP](https://www.thredup.com/about) comes in.


## Reimagining resale


thredUP is one of the largest online resale platforms that is transforming resale by making it easy to buy and sell secondhand clothing. Since its inception in 2009, thredUP has leveraged technology and data to build a thriving marketplace that connects buyers and sellers of gently used apparel, shoes, and accessories.


Now, thredUP is taking things a step further, offering[Resale-as-a-Service](https://www.raas.thredup.com/) (RaaS) for some of the world’s leading brands and retailers that want to provide their customers with a sustainable, eco-friendly, and cost-effective way to shop. According to The Recommerce 100, a comprehensive review of branded resale programs, there are 139 brands with resale shops, a 3.4x growth from 2021 to 2022, with 260,000 total resale shop listings. If all 260,000 resale shop listings in The Recommerce 100 sold, it would be the equivalent of 29,000 trees planted, 400 homes powered annually, and $11.4 million estimated total revenue.


Press enter or click to view image in full size


*Brands’ adoption of Resale showing 3.4x YTD growth between 2021 to 2022*


In its[2023 Resale Report](https://www.thredup.com/resale/static/2022-resaleReport-full-92a77020598ceca50f43227326100cc2.pdf) , thredUP reported that 86 percent of retail execs say their customers are already participating in resale. With 58 percent of retail executives saying offering resale is becoming table stakes for retailers, it’s safe to say resale is grabbing the attention of higher-ups in the retail industry. That number is only set to increase. In the U.S., the secondhand market is expected to nearly double by 2027, to $70 billion, while the global secondhand market is predicted to grow to $350 billion by 2027.


## Built for brands, powered by AWS


Powering its RaaS offering, Amazon Web Services (AWS) is helping thredUP revolutionize the resale business model for brands. Let’s look at the key features and benefits of thredUP’s RaaS offering and how AWS is helping brands deliver a seamless resale experience to thredUP’s customers.


From its start as a secondhand marketplace in 2009, thredUP selected AWS as its cloud provider due to scalability, cost-efficiency, security, reliability, and access to modern advanced technologies. AWS services like[Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/pm/ec2/?trk=36c6da98-7b20-48fa-8225-4784bced9843&sc_channel=ps&ef_id=CjwKCAjw67ajBhAVEiwA2g_jELcxfQ37zUgsUPdfYPa7XSRR9WAj4thR7gKJch5lZkoVzEFbgUxIkRoC2pUQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21467723097970%21e%21%21g%21%21amazon+ec2%2111198711716%21118263955828) ,[Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/pm/rds/?trk=83add82a-8e52-4837-bc73-c323da62d78c&sc_channel=ps&ef_id=CjwKCAjw67ajBhAVEiwA2g_jEC5-8Cfrhb52UD1zKAEyIjIxERmzeFfgs4odHZ9RhsXTO733M8z1XxoC2toQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21610000101549%21e%21%21g%21%21amazon+rds%2111086562318%21108339552603) , and[Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/pm/serv-s3/?trk=fecf68c9-3874-4ae2-a7ed-72b6d19c8034&sc_channel=ps&ef_id=CjwKCAjw67ajBhAVEiwA2g_jED5vpvenLhh7hykYMu_teNUMLypAtqZKtXMhwQIsZRUWW5UyXL5W8BoCKz8QAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21536452728638%21e%21%21g%21%21amazon+s3%2111204620052%21112938567994) form the foundation of thredUP.com’s infrastructure.


## Inventory Management


thredUP’s RaaS uses[Amazon SageMaker](https://aws.amazon.com/pm/sagemaker/?trk=8987dd52-6f33-407a-b89b-a7ba025c913c&sc_channel=ps&ef_id=CjwKCAjw67ajBhAVEiwA2g_jEFtOUkk042SWqG9mphj1FRq6LK9ltYaBJXS9yfEGOvupcPxG6Roz3hoCteoQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21532502995192%21e%21%21g%21%21aws+sagemaker%2111206038603%21113600690367) to manage and optimize inventory mix, ensuring that brands have the right products at the right time. thredUP has collected secondhand apparel sales data across 55,000 brands for longer than a decade. thredUP unlocks the power of that data to the benefit of resale buyers and sellers by making better decisions on pricing, inventory mix, and merchandising. Nine years ago, a thredUP engineer was able to programmatically provide probability that a given item would sell in the next 30 days using[AWS Artificial Intelligence and AWS Machine Learning (AI/ML)](https://aws.amazon.com/machine-learning/ai-services/) services. thredUP was able to implement this model in a month without the need for data scientists or ML engineers.


## Pricing Optimization


Using machine learning algorithms to automatically price products based on market demand, thredUP’s RaaS enables brands to maximize their profits while offering competitive prices to customers. thredUP handles millions of used products and reprices hundreds of thousands of items daily. On any given day, these new product arrivals are added, and millions of emails and push notifications are sent, all using[Amazon Managed Streaming for Apache Kafka (Amazon MSK).](https://aws.amazon.com/msk/?trk=56601b48-df3f-4cb4-9ef7-9f52efa1d0b8&sc_channel=ps&ef_id=CjwKCAjw67ajBhAVEiwA2g_jELReUFZ5BZO6ejx5hJvkbMyMzhag1cICMgHgQT3YjPf8maBnBiFLKhoClQQQAvD_BwE%3AG%3As&s_kwcid=AL%214422%213%21629393324629%21%21%21g%21%21%2116080176129%21133788113198) With this much activity on different platforms and RaaS resale sites, thredUP greatly relies on Amazon MSK to help things run smoothly. Repricing in event driven architecture, Amazon MSK is also foundational to cross-list secondhand products on multiple resale websites and reprice as many as 100,000 items in one hour.


## Analytics and Insights


thredUP’s RaaS employs[Amazon QuickSight](https://aws.amazon.com/quicksight/) to supply brands with near real-time analytics and insights into their resale performance, enabling them to make data-driven decisions and optimize their operations. Amazon QuickSight dashboards provide usage-based pricing and gives thredUP the ability to provision access to brands programmatically and embed the dashboards and reports into web applications.


## Security


thredUP’s RaaS clients require a high level of security and data protection from thredUP, and AWS is able to deliver on this with a wide range of robust security features, such as firewalls, encryption, and identity and access management. AWS has certifications with various industry standards, such as HIPAA, PCI DSS, and SOC 2, which helps thredUP provide brands with confidence that their RaaS services meet the necessary security requirements and are independently audited and certified by recognized industry organizations. Having a prominent level of compliance certification speeds up the sale process and vendor onboarding process significantly.


## Scale


thredUP can scale its infrastructure and resources up or down based on demand using[AWS Auto Scaling](https://aws.amazon.com/autoscaling/) . Just like with typical ecommerce, sales are critical for resale. Sales generate revenue, attract and retain customers, build a strong brand, gain market share, and enable growth.


## Cost Efficiency


thredUP is able to optimize costs with flexible usage-based pricing models for the resources they need, only when they need them.[AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) helps ensure efficiency for thredUP and the brands they work with. As a specific example, thredUP recently migrated from a self-managed Kubernetes cluster to[Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) /[Amazon Elastic Container Registry (Amazon ECR)](https://aws.amazon.com/ecr/) because custom configuration became too complex to maintain internally and caused unplanned downtimes during upgrades. After the migration, thredUP was able to keep the infrastructure team small, supporting 80+ Kubernetes deployments and 20+ tools. The time spent on patching decreased by 80 percent, downtime related to unsuccessful patching was eliminated, security posture by outsourcing security hardening improved, and CIS Kubernetes Benchmarking was enabled. thredUP also enjoyed instance cost reduction of around 20 percent by switching to Graviton instances.


While consumers do care about the planet, most can’t seem to shake the habit of wanting more clothes more frequently thanks to a history of fast fashion.[thredUP](https://www.thredup.com/resale/static/2022-resaleReport-full-92a77020598ceca50f43227326100cc2.pdf) believes secondhand is a way for consumers to satisfy constant newness while being mindful of their environmental impact. In fact, in thredUP’s[2023 Resale Report](https://www.thredup.com/resale/static/2022-resaleReport-full-92a77020598ceca50f43227326100cc2.pdf) , 64 percent of Gen Z and Millennials say they look for an item secondhand before purchasing it new.


By leveraging the power of AWS, thredUP is helping brands tap into the fast-growing resale market and provide their customers with a sustainable, affordable, and convenient shopping experience. With thredUP’s RaaS, brands can easily integrate resale into their existing business models, reduce their environmental impact, and drive customer loyalty and engagement. As the demand for sustainable and ethical fashion continues to grow, thredUP’s RaaS is poised to become a game-changer for the retail industry.


Interested in how AWS tools and technologies can help revolutionize your business? Learn more about[AWS for retail](https://aws.amazon.com/retail/) or contact an[AWS Representative.](https://pages.awscloud.com/retailContactUs)


## *Originally published at*[https://aws.amazon.com](https://aws.amazon.com/blogs/industries/how-aws-is-helping-thredup-revolutionize-the-resale-model-for-brands/) *on June 6, 2023.*
