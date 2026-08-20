---
schema_version: "1.0.0"
document_id: "bfe5e9453cc2af1cb2e3da0d73a3d7e3e2f086538c2c276fb1bd7a6ab40dff62"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/utilities/potential-solution-protecting-customer-privacy-when-sharing-smart-meter-data"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:7fd2063fd2e8cc725e8299c531c5ca224717838eef4c26c07ace7fc25b04c61d"
---

# A potential solution for protecting customer privacy when sharing smart meter data

[By 2020, the European Union (EU) aims to replace 80% of household electricity meters with smart meters](https://energy.ec.europa.eu/index_en) . This is because smart meters are widely regarded as a key to reducing both energy consumption and emission levels. Along with smart meter technology, however, comes some challenges in protecting consumer’s data privacy. This blog describes these privacy concerns as well as a potential solution to address them.


**The risk of leaking smart meter readings**


With the help of machine learning and advanced data analytics, several load-monitoring techniques can deduce a customer’s activity patterns just from their electricity metering data. Imagine a situation where a third party gets hold of your metering data and, using data mining algorithms, and can detect your presence or absence and other household activities. Wouldn’t that be devastating?


While the simplest solution to avoid this type of risk lies in not sharing your metering data with anyone, including your utility provider, which is easier said than done. The utility provider requires your power consumption data for billing, data aggregation and for estimating other physical elements critical to the grid operation that may not be measurable using sensors.


This motivates us to solve the following business challenge:


**How can metering data be shared securely for estimation purposes, while preserving a customer’s privacy?**


**Cryptography to the rescue**


In my[master thesis](http://resolver.tudelft.nl/uuid:16c3bece-e4ee-46fa-aaa3-6cdfa749d700) work in collaboration with the[Delft Center for Systems and Control (DCSC)](https://www.tudelft.nl/en/3me/departments/delft-center-for-systems-and-control/) and cybersecurity community at Delft University of Technology, I developed the open source algorithm *Obfuscate(.)* as a potential solution to this challenge. *Obfuscate(.)* is based on cryptographic masking where a raw plaintext dataset is transformed into a masked dataset through randomization. Computations are then performed on the masked data, and the results are later unmasked through de-randomization using a private key.


To get a basic understanding of how *Obfuscate(.)* works, imagine you hold two secret numbers (say 2 and 5). However, you have to share this data with a third party to calculate the sum. Rather than sharing the two numbers as plaintext, an algorithm generates a random key and masks it to randomized data (say 198.8 and 501.2). Next, the third party calculates the sum to be 700, and later de-randomizes the answer using the private key (100 in this case). As a result, it is practically impossible for an adversary to determine the actual numbers since there are infinite numbers of pairs that sum to 700. Then why not let users do the computation by themselves and share only the results? While this might work for the simple example mentioned above, the general consumer would need very large computational capabilities to estimate parameters in a complex power grid.


The essence of *Obfuscate(.)* lies in randomizing the data to prevent leakage of user’s metering data while still being able to accurately estimate the necessary parameters from this randomized dataset. What makes *Obfuscate(.)* stand out from a business perspective is that it is proven to be computationally efficient and practically scalable with every increase in the number of smart meters.


Currently, there are close to 200 million smart meters used in Europe, accounting for 72% of consumers. This number is expected to increase steadily in the coming years, which makes *Obfuscate(.)* increasingly viable as consumers would collectively save the total higher computation costs.


For more information on how *Obfuscate(.)* works, I invite you to[read my journal paper](https://link.springer.com/article/10.1186/s42162-019-0078-y) , which was presented for the proceedings of Energy Informatics held September 26-27 this year in Salzburg, Austria .


Of course, you can also contact me if you are interested in knowing more about the potential business case for *Obfuscate(.)* in the utilities sector.


*Note: This blog is based on my*[master thesis](http://resolver.tudelft.nl/uuid:16c3bece-e4ee-46fa-aaa3-6cdfa749d700) *work done in collaboration with the Delft Center for Systems and Control (DCSC) and Cyber Security community at Delft University of Technology, The Netherlands.*
