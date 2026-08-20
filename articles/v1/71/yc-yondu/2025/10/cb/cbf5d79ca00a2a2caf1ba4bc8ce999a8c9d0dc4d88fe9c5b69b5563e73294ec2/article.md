---
schema_version: "1.0.0"
document_id: "cbf5d79ca00a2a2caf1ba4bc8ce999a8c9d0dc4d88fe9c5b69b5563e73294ec2"
company_key: "yc-yondu"
company: "Yondu"
source_id: "yc-yondu-news-import-cb001de43134"
canonical_url: "https://www.yondu.ai/blog-posts/a-peek-behind-the-curtain-of-humanoid-manufacturing"
published_at: "2025-10-27T00:00:00+00:00"
first_seen_at: "2026-07-26T06:13:50.682599+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:09bfe29a8056955016e0d09b89e105c31649a877d52e93f30bbd04e1eb6ceecf"
---

# A Peek Behind the Curtain of Humanoid Manufacturing

##### **Introduction**


We went on a two-day trip across five cities in China to get a high-level glimpse of the country's robotics industry in the world’s largest manufacturing powerhouse. The first day was entirely spent in Beijing. On the second day, we visited several cities in the Yangtze River Delta, a major economic and technological hub in Eastern China that contributes over 25% of China's GDP despite covering only a small fraction of its total area. Specifically, we visited Changzhou, Suzhou, Huzhou, and Shanghai. We did not tour any facilities in Shanghai; it was simply our departure city. However, the view of the skyline across the Huangpu River left us captivated and helped us forget the fatigue from two days of continuous travel.


In this blog, we are sharing our reflections on what we experienced. Since our trip was short and we only visited a limited number of robotics facilities, these observations represent approximations based on the data points we gathered.


##### **Location of Facilities**


###### Most of the companies we met have offices distributed across multiple cities in China. The most common hubs were Beijing, Suzhou, Changzhou, Hangzhou, Huzhou, Shanghai, and Shenzhen. Broadly, these locations fall into three key regions.


##### **Beijing:**


Nearly every company has at least one office in Beijing. These offices are primarily used for research and development, software development, AI model development, design, and sales and marketing.


As the capital, Beijing offers easy access to potential customers, which explains why many companies base their business and marketing teams there. It is also home to China's top universities, such as Tsinghua and Peking, making it easier to attract strong technical talent for high-tech innovation.


After Beijing's air quality crisis years ago, the government relocated most heavy industries out of the city. Combined with rising land costs, this pushed companies to place their manufacturing facilities elsewhere.


Interestingly, one of the Beijing facilities we visited, Company B's office, was located in a repurposed steel factory, a symbolic nod to the city’s industrial past.


##### **Yangtze River Delta Region:**


The Yangtze River Delta (YRD), where the Yangtze River meets the East China Sea, is the heart of China's advanced manufacturing. Excellent infrastructure and strong government support have made it the preferred base for robotics production.


Our first stop was Company B’s manufacturing plant in Changzhou, a well-known manufacturing hub for both Chinese and international firms. Changzhou also hosts the Changzhou–Israel Innovation Park, a joint initiative between the Israeli and Chinese governments that fosters high-tech collaboration.


Next, we visited Company A's plant in Suzhou. The Suzhou Industrial Park, co-developed with Singapore, has been a key driver of high-tech manufacturing in the region. Beyond its industrial importance, Suzhou's greenery and lakes make it one of China’s most scenic industrial cities.


After Suzhou, we toured Company B's factory in Huzhou, located in Zhejiang Province. Senior management told us they recently relocated from Nanjing due to better government support offered by Zhejiang.


Zhejiang has become a rising hub for high-tech companies, producing giants like Alibaba, Unitree, and High Flyer Quant (the parent company of DeepSeek). Much of this success comes from top talent at Zhejiang University and early-stage support from the provincial government.


##### **Guangdong-Hong Kong-Macao Greater Bay Area:**


Many companies we visited also maintain offices in this region, though we did not have time to visit it on this trip. Sorry, Canton folks, we will definitely see you next time.


While most robotics companies have offices in Shenzhen, their main manufacturing plants remain in the Yangtze River Delta. Manufacturing space there is generally cheaper, and local governments in the Delta provide stronger support than those in Shenzhen.


##### ‍ **Other Potential Locations for Further Expansion:**


According to Company B, they plan to expand data collection operations to Guizhou, a more remote province where labor is cheaper. There, they aim to run large-scale teleoperated robot data collection centers.


##### **Manufacturing Plants:**


We toured several humanoid robotics companies’ manufacturing plants and were surprised by how manual the processes were. Almost every assembly step was done by hand, likely because these platforms are still evolving rapidly.


Production lines, not demand, appear to be the bottleneck, while demand continues to grow steadily. We also noticed a lack of robust quality control and mechanical testing, especially for lifting systems and mobile bases.


##### **State of the Technology:**


The ecosystem is tightly interconnected. Only a few companies actually produce their own motors, while most humanoid robotics firms assemble components made by nearby suppliers. For instance, Company B manufactures its own actuators but still uses off-the-shelf AMRs for its wheeled bases.


Many humanoid robots we saw felt closer to prototypes or R&D units than finished products. Even arm manufacturing is often outsourced, with smaller firms relying on established players like Company B for subassemblies.


Hardware lead times are long, typically one to two months before shipping, plus an additional two to three weeks if you are ordering from abroad. For now, being based in China provides a clear advantage through faster access to the latest hardware.


##### **Data Collection Facilities:**


We learned of at least two major facilities in China dedicated to large-scale humanoid data collection. Each houses over one hundred teleoperated robots gathering data for different customer specifications.


There is also an informal alliance between several companies, including Company B, BrainCo Max Insight, Oymotion, and Iotech, that share components and jointly collect and exchange data.


The Chinese government has heavily invested in these efforts, purchasing large quantities of humanoid robots as part of multi-year national plans to advance embodied and physical AI platforms.


Inside the facilities, most operators are college interns with minimal technical backgrounds who perform repetitive teleoperation tasks.


These data collection programs have drawn attention from global players such as Meta, NVIDIA, and other robot foundation model developers. Company A, for example, has been collecting data for PI across diverse real-world environments, not just in data centers.


##### **Data Collection platforms:**


Several robotics companies, including Company A, Company B, and other specialized startups, are building comprehensive data platforms for collection, quality assurance, and labeling. Their pipelines include both edge and cloud-level checks to ensure quality.


However, much of the quality assurance process remains manual. Human reviewers still inspect each recorded episode to verify data quality, sync cameras, and detect dropped frames.


Currently, labeling primarily consists of assigning text tags to subtasks for vision-language-action datasets.


##### **Standard vs Customized Solutions:**


Because the humanoid robotics industry is still young, hardware design varies widely between companies, although designs are rapidly improving. Typically, smaller startups focus on custom solutions, but we were surprised to see even large manufacturers offering customizations for small orders.


For example, motor manufacturers often tailor arm designs per customer request. One potential bottleneck to innovation, however, is that design houses cannot easily replicate successful designs for multiple clients without intentionally introducing small differences, likely to protect intellectual property or contractual exclusivity.


##### **Robotics Industry Market:**


Industrial robotics remains the dominant sector, given its structured environments and predictable tasks that can be preprogrammed.


However, with progress in data-driven robot control, companies are expanding into less structured domains such as barista robots for coffee shops, warehouse automation, retail service robots, and even massage robots.


Household robotics remains a longer-term goal. The sheer variability of home environments makes it a much harder problem to solve.


##### **Final Remarks:**


Robots are poised to play an essential role in the future. On one hand, many countries face aging populations; on the other, global wealth continues to rise. As societies become wealthier, people increasingly seek fulfillment beyond manual labor. To maintain current living standards without a large labor force engaged in physical work, robotics appears to be the only viable path forward.


The field remains in its infancy, and developing reliable hardware for the physical world is inherently difficult. Real-world physics adds layers of complexity that even the best software struggles to model.


Every major physical industry, from cars to airplanes to mobile phones, took decades to mature. Robotics will likely take even longer, given its combination of complex hardware and software. To reach that future, we need persistence, long-term conviction in the economic and societal value of robotics, and a commitment to learning from every iteration rather than chasing hype.
