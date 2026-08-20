---
schema_version: "1.0.0"
document_id: "0e118b44a21ec133fdfff64f77f4f5844c202a0061a57947b4f0f1be8c591aa7"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/identifying-ambiguous-and-low-quality-vehicle-listings-with-huggingface-transformers-64560fe4b39b"
published_at: "2022-05-20T18:27:45+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:32.519238+00:00"
content_hash: "sha256:4b3f8769dd1de54525711fdce6d354b0938fff3cb78055feaf7601226c77d468"
---

# Identifying ambiguous and low quality vehicle listings with HuggingFace transformers

Machine Learning


Data Science


Transformers


NLP


Hugging Face


# Identifying ambiguous and low quality vehicle listings with HuggingFace transformers


[Alexis Carpenter](https://medium.com/@alexisccarpenter?source=post_page---byline--64560fe4b39b---------------------------------------)


10 min read


·


May 20, 2022


--


Press enter or click to view image in full size


My latest car — 2021 Mazda CX-5 Source:[Cars.com](https://www.cars.com/research/mazda-cx_5-2021/)


[Cars.com](https://www.cars.com/) is an automotive marketplace that connects car buyers and sellers. With nearly 25 years under our belt, Cars.com has become synonymous with car shopping and regularly ranks as №1 in our competitive set when it comes to brand awareness. Car dealers and private sellers are able to list their available vehicles with photos and include information about the vehicle’s features, history, pricing and more. Consumers use the marketplace to research, read millions of reviews, shop for cars and connect with sellers to set up test drives and, ultimately, make a purchase. *Note: The terms dealer and seller are used interchangeably throughout the following post.*


Press enter or click to view image in full size


1. Example vehicle listing from Cars.com including vehicle photos, features, and seller provided description.


However, what happens when consumer trust is broken due to a lack of transparency from sellers? For example, one of the primary complaints from car shopping consumers is that the price listed online is not indicative of the price on the lot — a practice that is not unique to Cars.com and largely affects the entire industry. This is due to various reasons, but primarily because the price listed does not include dealer-installed add-ons, dealership or document fees, or added warranties. Whatever the case, sometimes the online price isn’t telling the whole story.


Ambiguous pricing practices not only degrade the Cars.com brand because it reduces the trust that consumers have in our platform, it further unduly penalizes dealers who are being transparent about their pricing practices and who are standing by their online list price. Finally, such ambiguous pricing tactics directly go against Cars.com terms and conditions.


In this article, I will summarize how we, as a Data Science team, approached this problem with a machine learning product we built that utilizes NLP techniques to proactively identify vehicle listings and sellers that may be engaging in ambiguous pricing practices.


## **Background**


When shopping for products online consumers have come to expect that the price listed is what they’ll pay. Hidden fees reflect poorly on the business and cause distrust between buyer and seller. While this is not true of the majority of Cars.com’s listings, there are the rare few that sneak past (or those who disregard) our terms and conditions. Here’s what this might look like on a marketplace: The seller lowers the main listing price of a vehicle and then includes pricing caveats in the seller’s note.A vehicle may be listed for $29,000 but this listed price may exclude a $1000 document fee, which is noted in the seller’s note, bringing the out-the-door price to the consumer to $30,000 (See Figures 1 and 2 for example seller’s note texts).


There are two key sources of information with regards to understanding the context behind pricing: 1) seller’s note and 2) dealer review texts. Seller’s note texts include vehicle or seller information provided to Cars.com **directly from the seller** , whereas dealer review texts provide consumers a space where the consumer can discuss their experience with a particular dealership.


We used various NLP techniques to develop a machine learning product to proactively identify problematic listings. This allows us to then reach out to those specific sellers and request that they make the required changes **to improve consumer trust and transparency on our marketplace** . Prior to this model, Cars.com had to manually flag problem listings or wait until a consumer complaint.


## **Methodology**


Given the wealth of textual information provided in seller’s note and dealer review texts, we focus on these two texts as sources for untrustworthy or ambiguous seller signals. We utilize a multi-label text classification approach where our labels correspond to specific categories of ambiguous pricing (e.g. price does not include additional dealership fees or add ons vs. price includes all rebates or incentives that not everyone will qualify for) to classify texts that may belong to multiple categories or labels simultaneously.


Below is an example of a single seller’s note text and dealer review text and the sentences that would be flagged for ambiguous pricing. The example seller’s note text would be flagged for 1) requiring financing with a specific party (Finance_CashOnly); 2) price excluding taxes and title (Taxes_Registration); 3) price excluding fees and dealer installed add-ons (Fees). The example dealer review text would be flagged for 1) price excluding fees (Fees).


2. Example Seller’s Note and Dealer Review texts with corresponding ambiguous pricing-related sentences flagged.


**Model:** Here we fine-tune a DistilBERT HuggingFace transformer model, which is a distilled version of BERT (Bidirectional Encoder Representations from Transformers) for multi-label text classification on our custom dataset. BERT utilizes a transformer, which is an attention mechanism that learns relations between words (or sub-words) in a particular text. Further, rather than reading the text sequentially (e.g., left to right) like directional models, the transformer encoding reads the entire sequence of words at once. This allows BERT to learn the words based on their surrounding context.


For sentence classification tasks (i.e., where our goal is to categorize a given text into X number of predefined labels), BERT models take a sentence or a sequence of words as the input. This input is segmented into token embeddings, segment embeddings and position embeddings. The combination of such embeddings is the final input to the BERT Encoder.


Press enter or click to view image in full size


3. The sum of these three token, segment and position embeddings acts as the final input to BERT. Token embeddings add a \[CLS\] token at the start and a \[SEP\] token at the end of each sentence and words are subjected to a transformation to word pieces (e.g., playing → play + ##ing). Segment embeddings indicate which sentence a word belongs to and the positional embeddings indicate its position in the input. Figure from[Devlin et al., 2018](https://arxiv.org/pdf/1810.04805.pdf) .


BERT takes each block of this input sequence and passes it through a self-attention mechanism and a feed-forward neural network before being passed to the next Transformer encoder. The attention mechanism essentially decides at each step of the input sequence which other parts of the sequence are important contexts for the given item or word. For example, for our use case the context around the word “marked” affects how we interpret this word’s meaning. That is, the price being marked or marked down should be interpreted differently compared to the *leather seats* being marked. BERT specifically is bidirectional, meaning that it takes into account the prior and subsequent words into account simultaneously. The output vectors here are our word embeddings. We can then use the pooled outputs and feed this into our classifier to get our label predictions.


However, as previously mentioned, we chose to use DistilBERT in order to reduce training and iteration time with minimal impact to model performance. Specifically, distillation is a process by which we can shrink networks to a reasonable size whilst minimizing the drop in performance. With distillation we are training a small student model to essentially mimic a larger teacher model as closely as possible. The idea behind distillation is that once a large neural network has been trained, its full output can be approximated by using a smaller network. As such, distillation results in a smaller model that is faster to train while still retaining the majority of BERT’s language understanding capabilities.


**Price Extraction:** The use case of this model is to flag specific vehicle listings or sellers who are engaging in ambiguous pricing, vehicle damage or bait and switch strategies. Additionally, if available we also aim to extract specific dollar amounts to support price adjustments based on pricing strategies (e.g., +$899 for the online price excludes an $899 dealership fee).


The only input to the model is the text (either seller’s note or dealer review text) which is scored across 14 labels related to ambiguous pricing and vehicle quality. Flagging such ambiguous seller signals in text allows Cars.com to be proactive in contacting sellers and allowing them a window during which they can update their listings to comply with Cars.com terms and conditions.


For repeat offenders (sellers that use ambiguity in their pricing), our review system (powered by[DealerRater](https://www.dealerrater.com/) ) acts as a feedback loop and impacts a seller’s reputation on our marketplace.


## **Challenges faced**


**Data distribution:** Many example texts included mentions of fees **not** being included in the price but very few contained organic mentions of fees being included in the price. Given this, initial attempts highlighted the need for additional data augmentation to our training set as texts highlighting that fees that were included in the online price were incorrectly flagged for ambiguous pricing. For example, we introduced custom negation and topic concatenation rules to ensure that our model is not flagging any text that merely mentions pricing, fees or cosmetics but only those texts that are indicative of ambiguous pricing and low vehicle quality and further to improve performance when multiple signals are present in a given text.


**Identifying the classes and labeled data:** Prior to this model we had a rough definition as to what would constitute ambiguous pricing or a low quality vehicle listing. As an initial step, we created 14 specific labels with corresponding definitions in collaboration with the Fraud and Product teams at Cars.com to identify specific types of ambiguous pricing or vehicle quality issues (e.g., dealer fees not included in price vs. minor cosmetic damage, etc.).


4. Proportion of seller’s note texts flagged across the 14 labels related to ambiguous pricing and vehicle quality.


Our “other_hedging” label was the most frequent given that it included instances where the dealer mentioned that the consumer should call to confirm availability, price or features, whereas labels related to a listing requiring a specific trade-in of a given value or down payment were less common.


## Get Alexis Carpenter’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Explainability:** A key component of our model is its ability to not only flag texts for ambiguous pricing or vehicle quality issues, but also to provide the specific segment of text that resulted in the listing or dealer being flagged. That is, we provide our teams with A) listings and dealers that are flagged for ambiguous pricing or low quality listings and B) the specific text(s) segment that contributed to the text being flagged.


## **Evaluation & Results**


We tracked the F1 score, precision and recall when building and evaluating new versions of the model. These metrics are calculated by running the model against a gold-standard dataset that the model has not seen. After multiple iterations, our model that gave the best F1 score was finalized and our best F1 score was at 0.97 across our 14 labels.


## **Key Takeaways**


In this section, I briefly want to discuss several key takeaways that while not directly related to modeling decisions were integral to the success of the product.


**Crowdsourcing Label Alignment:** Before creating and experimenting with the training set, we needed to understand what labels and definitions we could use to create the training set. Given this we iterated on several sets of labels and definitions and received feedback from our ML as a Service, Product and the Fraud teams. This allowed us to have a single set of labels and corresponding definitions that all teams were aligned on prior to creating the training set.


**Flexibility of the Model:** Further, we tried to build the model with ample flexible parameters. Such flexibility saves significant time when evaluating results and allows for course correction immediately if necessary with less effort. For example, we originally decided to have 14 labels that texts could be scored for. However, later we may want to update these labels and/or definitions based on feedback from other Cars.com teams. Shifting the number of labels and the label names requires changing 2 lines of code, saving a lot of dev efforts and will allow us to rapidly update and/or experiment with the model as the business use case evolves.


**Combine Techniques:** It was important for us to try and combine a couple of different text augmentation techniques together to create our training set. This methodological experimentation provided us with much better results than going with some standard text augmentation techniques like back-translation, etc. Together, typo-aware training methods using TextAttack, custom concatenation rules, and custom negation rules resulted in the best model performance.


**Iterative Approach:** Finally, we aimed to have a model that can be retrained in an iterative fashion, taking feedback from Cars.com teams in order to fine-tune or refine our labels and their corresponding definitions. Thus, we are able to take in mislabeled data and apply our augmentation techniques in order to create an updated training set and retrain our model as needed.


## **Next Steps**


There are several limitations of the current model that we plan to improve upon in the future:


**Hierarchical labels:** We are not currently taking into account relationships among labels and we are ignoring the fact that the generalizing pricing label, for example, is inclusive of fees. A potential future direction would be to utilize a hierarchical approach to label creation and modeling, such that generalized ambiguous pricing, for example, is inclusive of fees, incentives, financing, down payments and undisclosed ambiguous pricing.


**Price adjustment improvement:** Currently we use ad hoc regex rules (e.g., if we extract more than 5 unique price values, this likely indicates a list of options, so we should take the average of the values extracted) to determine how to handle cases where multiple price values are extracted. Future work should focus on a more nuanced approach taking into account the actual sentence structure and the context surrounding the extracted value rather than merely the number of extracted values in a given text.


## Conclusion


In addition to making Cars.com a more trusted and transparent car shopping marketplace, we built a model that is able to:


- Flag texts that likely contain mentions of ambiguous pricing or vehicle damage affecting vehicle quality
- Extract the specific sentence that contributed to that text being flagged for increased explainability
- Extract the price adjustments if the text was flagged for ambiguous pricing in order to provide a more accurate vehicle price for consumers and other downstream Cars.com ML products
- Be updated flexibly and iteratively based on a feedback loop initiated by various Cars.com teams as needed


*Cars.com is currently hiring for several Data Science/ML Engineering positions so please check out our*[careers page](https://www.cars.com/careers/#job-listings) *if you are interested in learning more!*


## References


1. [HuggingFace transformers (DistilBERT)](https://huggingface.co/docs/transformers/model_doc/distilbert)
2. [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/pdf/1810.04805.pdf)
3. [DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter](https://arxiv.org/abs/1910.01108)
4. [Typo-aware training methods](https://arxiv.org/pdf/2111.09618.pdf)
5. [TextAttack augmentation](https://textattack.readthedocs.io/en/latest/apidoc/textattack.augmentation.html)
