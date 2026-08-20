---
schema_version: "1.0.0"
document_id: "fb3fcb89411ba0c48d24f08944e305aa74b315410f0e6f175ac651d65e69620a"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/making-the-best-first-impression-using-machine-learning-to-optimize-photo-selection-0767e82e4fe8"
published_at: "2025-10-30T18:00:02+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:7a7316bcb636fea032e5257513ae3bec41e2104b7cd7c2c6418e3fead2ffc3a3"
---

# Making the best first impression: Using machine learning to optimize photo selection

# Making the best first impression: Using machine learning to optimize photo selection


[Tripadvisor](https://medium.com/@tripadvisor-tech?source=post_page---byline--0767e82e4fe8---------------------------------------)


8 min read


·


Oct 30, 2025


--


Press enter or click to view image in full size


*How we drive conversion with the AI-powered Primary Photo Service, which selects the most engaging primary photo to showcase a Tripadvisor listing.*


*By Myeongjin (Jen) Lee Ph.D., Mario Cardoso, and Ryan Gu*


The first photo a traveler sees when looking for a hotel, restaurant, or experience can make or break a booking decision. As a key partner to business owners and tour operators across the globe, we want to ensure that when multiple images are equally relevant, the one shown first helps them put their best image forward, while still surfacing authentic traveler perspectives. Our model doesn’t suppress unflattering or critical images; they remain fully visible in the gallery and can rank highly based on quality and relevance. When multiple images are equally relevant (e.g., multiple bedroom shots), we prioritize the one that is most visually engaging as the primary image.


We recently launched our **Primary Photo Service** , a machine learning system that automatically selects the most compelling primary photos — the first photo that represents a hotel, restaurant, or attraction on Tripadvisor — to enable faster visual decision-making for our partners.


This end-to-end solution combines computer vision, pairwise learning, and large-scale infrastructure to scale high-quality photo selection.While simple on its surface, it processes around **12,000 read requests per second during peak hours** . And since launch, we’ve seen significant increases in click-through rates and bookings, all without adding a layer of complexity to the lives of our partners.


Press enter or click to view image in full size


Before/After of a thumbnail picture for a top ranked global chain hotel in a major EU city


## Why primary photos matter


For any online platform, across categories, photos are business drivers. According to a 2022 report from[Journal of Business Research](https://www.sciencedirect.com/science/article/abs/pii/S0148296322007627) , high-quality, visually rich images can significantly increase user engagement across digital platforms. Images are a central component of traveler engagement at Tripadvisor, with about 350M images published to locations and an average year-over-year growth in image uploads of about 33% since 2010.


When travelers browse our site, the primary photo is often their first impression of a property. A gorgeous exterior shot of a hotel with just the right light can bring in clicks. A generic, dark photo of a basic room will keep people scrolling. As one of our travelers stated plainly in the title of their[review](https://www.tripadvisor.com/ShowUserReviews-g187791-d7604185-r828921624-HT6_Hotel-Rome_Lazio.html) , “\[I\] booked because of the location, photos, and reviews.”


But acting on that knowledge historically has been difficult. Manual curation of photos, amongst the millions of images uploaded by owners, operators, and travelers alike, isn’t a feasible, scalable solution. And while our partners have a deluge of images to choose from, they vary greatly in perspective and quality.


## Our methodology


### **Using AI to find patterns in attraction**


To find a solution, we leaned into a key insight: “attractiveness” may be subjective, but patterns exist. There are commonalities to be found in popular photos across categories and cultures: the right lighting, the perfect perspective, the exposition of unique features. We set out to develop an approach that combines two key metrics — visual appeal and relevance — to identify photos that are both eye-catching and accurately representative of what travelers want to see when they’re in discovery-mode.


We built a set of core machine learning models and heuristics:


- **The Attractiveness Score Model** , which assesses the visual appeal of each photo
- **The Primary Photo Selection Logic** , which combines attractiveness scores with business rules to effectively pick the best thumbnail for each item.


### **The Attractiveness Score Model**


Attractiveness is inherently subjective, depending on technical quality, relevance to the specific context, and aesthetic quality. We had to lean into computer vision: training an AI model to look at and evaluate images the same multifaceted way we do as people.


Instead of defining beauty in absolute terms, we used a[“pairwise” learning approach](https://towardsdatascience.com/ranking-basics-pointwise-pairwise-listwise-cd5318f86e1b/) . Instead of asking, “Is this photo attractive?” we asked, “Which of these two photos is more attractive?” This relative comparison proved far more reliable and consistent since it focuses on the relative preferences between images, rather than deciding on arbitrary absolute metrics which can lead to inconsistent results.


Our model defines a high-attractiveness image as a high-quality photo that encourages travelers to click and explore the property further. These scores are used to sort through potentially thousands of images for any given location, weighted by heuristics, to identify the best candidates for the primary photo.


For any given location, let **a** denote all the attractiveness scores of its uploaded images, **w** the weights produced for each image by business rules / guardrails, and **k** the number of desired primary photo suggestions. The primary photo (PP) selection logic is given as follows:


Mathematical definition of primary photo selection.


### Gathering Data


We created a large-scale dataset of pairwise comparisons for each line of business (i.e., hotels, restaurants, attractions). We experimented with multiple approaches to generate labels, including annotations and LLM-generated, to discover that LLM-generated annotations allowed us to process hundreds of thousands more samples, much faster, and at a fraction of the cost.


### Model Architecture


Inspired by the[RankNet](https://icml.cc/Conferences/2015/wp-content/uploads/2015/06/icml_ranking.pdf) architecture, we leveraged a Siamese neural network structure — essentially two identical AI models that process image pairs simultaneously — to train the LLM. Each model produces an “attractiveness score” for its image, and the system learns to predict which photo users would prefer. At scale, these pairwise preferences approximate a global ranking of photo appeal.


Figure 2. Model training overview. Each sample is composed of a pair of images, whose embeddings are fed into a Siamese model to produce attractiveness scores. These scores are combined with an LLM-generated ground-truth label to calculate the loss.


## The Primary Photo Selection Logic


### Layering business logic on top of aesthetic preferences


The most attractive photo isn’t always the right choice. A gorgeous bathroom shot might score high on visual appeal but poorly represent a hotel’s key features. To allow for this additional flexibility, a final score is calculated by combining the attractiveness scores with weights derived from heuristics. We layered business rules on top of our attractiveness scores to further control the quality of outputs across our verticals:


- **Hotels** : We suppress certain scenes (bathrooms, gyms) that don’t drive bookings
- **Attractions** : We add explicit relevance weights ensuring photos accurately represent the experience
- **Restaurants** : We prioritize images that showcase signature dishes or ambiance


This hybrid approach — AI-driven attractiveness combined with business intelligence — strikes the optimal balance between algorithmic precision and practical control.


### Evaluation methodology


We evaluated the quality of our primary photo suggestions using multiple methods to ensure a robust understanding of how the solution performs.


- **Model Metrics:** We measure and observe model binary performance metrics across different runs. These metrics include accuracy and ROC AUC, whose main purpose is to provide a holistic view of the model’s performance across the entire list of images.
- **LLM-as-a-judge:** We leveraged an LLM-as-a-judge to collect binary preferences across a representative dataset of original vs suggested primary photos.
- **Quality Assurance (QA) Testing:** We also conducted rounds of QA testing with an internal team using a smaller curated dataset of original vs suggested primary photos across a representative sample of locations. Both the QA metrics and the LLM-as-a-judge metrics focused exclusively on measuring the quality of primary photo suggestions.


## Engineering work & infrastructure


### Bridging Data and Services


As computer vision began exploring ways to integrate their primary photo suggestions into the existing media ecosystem at Tripadvisor, we realized our existing infrastructure was intended for service-to-service communication. This worked well for our engineering teams, but it introduced a challenge for data-centric teams who don’t typically have their own microservices. Their work happens in data pipelines and platforms like Snowflake which don’t have the ability to call our APIs directly. To bridge this gap, we built an S3 Ingestor to make it easier for data-centric teams to call media endpoints.


### **Designing our S3 Ingestor**


Clients like computer vision are responsible for exporting their results into properly formatted CSVs and uploading them to a Media-owned S3 bucket. Similar to a microservice, the ingestor has a pseudo-API that clients need to follow.


1. The CSV data should mimic a typical endpoint call and must include all of the endpoint’s required fields as a column
2. The upload path in the S3 bucket also must be formatted like: /computervision/setPrimaryPhoto/filename.csv


Once the CSV file lands in the S3 bucket, it’s completely hands-off for the clients. The media team takes over from there and the S3 Ingestor handles reading the raw CSV data, transforming it into service-ready calls, and acts as a microservice caller to ingest the data into the Primary Photo Service.


Press enter or click to view image in full size


This clear ownership divide allows data-centric teams to stay focused on their models, products, and workflows while the media team manages the complexity of service-to-service communication and business logic enforcement.


The S3 Ingestor has proven to be a powerful tool that empowers data-centric teams to move faster and drive impact, without the hassle of managing their own microservices or any custom integrations. Since we designed with flexibility in mind, we’re already using it for **rotating images** , and **setting media metadata** .


## What’s next


While this system focuses on primary photo selection, the underlying attractiveness scores open doors to broader applications. We’re now exploring how to enhance entire photo galleries, creating richer visual experiences throughout the traveler journey.


We’re also looking at incorporating additional signals to recommend primary photos using known information and user-generated content to recommend photos that highlight relevant and unique aspects of the place. With this, an owner could highlight a hotel’s unique swimming pool that’s popular in reviews, or a signature dish of a restaurant. There may also be ways to contextualize and personalize the primary photos, allowing for more relevant images that match each traveler’s interests


By continuing to teach machines to recognize visual appeal while respecting business context, we’ve created a scalable solution that helps our partners find new customers, provides more relevant information to users, and helps make everyone a better traveler.
