---
schema_version: "1.0.0"
document_id: "350c5013b2f0bbac1e4167067fc45e11b1bed38fa376d7363de44ec29d802a34"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/your-cart-has-a-story-heres-how-we-learned-to-read-it-10ba9188f534"
published_at: "2026-06-01T10:18:57+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:5e8c9e3e8485e42b4b722a802d30faa1b0f5cd0d1f7b9d73aaf687671f9a7b64"
---

# Your Cart Has a Story. Here’s How We Learned to Read It.

At Zepto, the cart is more than a list of items. Behind every cart is an intent.


Someone adding mozzarella, pasta, and basil isn’t randomly browsing; they’re making dinner. Someone with diapers, wipes, and formula is stocking up for their baby. Someone with chips, cola, and nachos is probably planning a night in. These patterns are obvious to a human observer. The challenge is teaching a system to see them too, and act on them in real time.


The purpose of recommendations at Zepto isn’t just to show popular products. It’s to identify what a user is trying to accomplish as early as possible in their session and surface the items that help them get there faster. Instead of waiting for users to search for every ingredient individually, we want to proactively complete the picture.


The cart contextual model is our solution to this problem.


### The Core Idea


Every day, thousands of carts are created across the Zepto platform. Over time, strong patterns emerge. A large fraction of carts cluster around recognizable intents: the weekly grocery refill, the protein-focused cart, the party snack run, the quick breakfast replenishment. The more frequently a pattern appears, the stronger and more reliable the intent signal becomes.


Our cart contextual model is trained on these historical cart patterns. When a user adds their first item, the model starts forming a hypothesis about what they’re trying to accomplish. As more items land in the cart, it refines that hypothesis and adjusts recommendations accordingly. The system is never waiting for the user to tell it what they need; it’s inferring.


The technical mechanism we are using for this is **Masked Language Modeling (MLM)** , borrowed from the NLP world and adapted for shopping carts. Instead of predicting missing words in a sentence, the model learns to predict missing products in a cart. The analogy is surprisingly clean: a cart is a sentence, and each product is a word. Understanding the “grammar” of a cart means understanding what belongs together and why.


The model backbone is a **Transformer-based encoder architecture** , which excels at capturing relationships across a full sequence using self-attention. This lets the model understand not just which products are present, but how they relate to each other within the context of the full cart.


### Building the Model


High level model architecture


### Extracting the Right Signals


We start by aggregating the historical order data. Raw transaction records are enriched with product attributes including brand and category hierarchy (Category, Sub-category, Product Type). Beyond product identity, we extract two additional layers of context:


**Temporal signals** : The day of the week and hour of the order. Shopping behavior has clear time-of-day and day-of-week patterns. A Monday morning cart looks different from a Saturday evening one.


**Geographical signals :** The city of the order, as regional preferences in India are significant. What sells in Bengaluru on a weekday evening is meaningfully different from what sells in Delhi.


Combining product, time, and location gives each cart a richer contextual fingerprint.


### Structuring the Vocabulary


Neural networks operate on numbers, not text or category names. The first structural step is building a vocabulary: a mapping from each categorical entity to a unique integer.


We build vocabularies for products (read more:[https://blog.zeptonow.com/how-we-built-high-precision-low-latency-semantic-search-in-production-75a6c61dee25](https://blog.zeptonow.com/how-we-built-high-precision-low-latency-semantic-search-in-production-75a6c61dee25) ), brand, category and subcategory, city, day of week, and hour of day.


We also reserve two special tokens, \[PAD\] to normalize sequences of varying lengths, and \[UNK\] to handle rare or unseen values at inference time.


### Creating Cart Sequences


Products within each order are grouped by order ID to form cart sets. Each cart becomes a short “sentence” of products.


To keep learning focused, we retain carts with 2 to 10 items. Shorter carts provide little context; extremely long ones add noise and computational overhead without proportional signal.


The data is split deterministically using a hash of the order ID, with approximately 99% used for training and 1% held out as a validation set. This ensures reproducibility and clean separation between training and evaluation.


### Warm-Starting with Pre-trained Embeddings


Rather than training from scratch, we initialize a large embedding matrix covering the full product vocabulary and populate it wherever possible with pretrained product embeddings from a previously trained model. Remaining entries are initialized randomly. This warm start means the model enters training with meaningful product representations, which accelerates convergence and improves quality on long-tail items.


### Masking: Turning Carts into Learning Signals


The masking step is where the model actually learns. The idea is simple: hide a subset of products in a cart and train the model to predict them using the remaining items as context. Each training step creates a question-and-answer pair.


### Weighted Masking


In standard MLM, tokens are masked uniformly. We don’t do that.


Quick commerce catalogs are skewed. Several staples (e.g. eggs, milk, bananas) account for higher order frequency, while niche products appear infrequently. If we masked uniformly, the model would spend most of its capacity predicting popular items and barely learn anything useful about the long tail.


Instead, we apply **inverse-frequency masking** :


- Rare products are masked more often, forcing the model to learn harder, more nuanced relationships
- Very common products are masked less often, preventing the model from overfitting on easy predictions


This balances learning signal across both everyday staples and niche products.


We also enforce a hard safety rule: every cart must have at least one masked item. If the probabilistic draw produces zero masks, we force-mask one valid product. A training step with nothing to predict is a wasted step.


### What the Model Sees vs. What It Gets Graded On


Let’s walk through a concrete example. Suppose a user’s cart contains five products:


> *original: \[10, 25, 42, 7, 99\]*


The masking decision selects positions 3 and 5. The model’s input becomes:


> *input_ids: \[10, 25, \[MASK\], 7, \[MASK\]\]*


The corresponding label array is:


> *labels: \[-100, -100, 42, -100, 99\]*


The -100 value tells PyTorch’s CrossEntropyLoss to ignore that position entirely. Loss is computed only at the masked positions.


Press enter or click to view image in full size


The model is penalized only for failing to predict 42 at position 3 and 99 at position 5. Everything else is invisible to the loss function. This design ensures training is efficient and focused entirely on prediction, not memorization.


### Model Architecture


### The Embedding Layer


For each product in the cart, we build a rich representation by combining several embeddings:


- A trainable product embedding (updated during training)
- A frozen auxiliary embedding from the pretrained model (for stability)
- Contextual embeddings for brand, category hierarchy , city, day, and hour


This ensures each product representation captures not just its identity but where and when it appears.


### Projection to a Unified Dimension


All per-product embeddings are concatenated into a single long vector and projected into a fixed 512-dimensional representation. This projection step compresses all product and context information into a format the Transformer can process uniformly and efficiently.


### The Transformer


The 12-layer Transformer encoder processes the full cart simultaneously using self-attention. Rather than scanning items one at a time, it learns relationships across the entire cart in parallel.


A concrete example: if a cart contains milk and cereal and a third product is masked, the model learns that this combination strongly hints at a breakfast context, and raises the probability of related items accordingly. With 12 layers, the model can capture complex, non-linear relationships across products, categories, time, and geography.


### The MLM Head


The final layer projects the Transformer’s output back to the full product vocabulary. For every masked position, the model produces a probability score over every product in the catalog. During training, loss is computed only at masked positions. At inference time, the top-scoring candidates become recommendation inputs.


### What the Model Ultimately Learns


Over thousands of carts and millions of training steps, the model internalizes intent patterns at scale:


- Pizza base + mozzarella → tomato sauce probability increases
- Shampoo + conditioner → hair serum likely to follow
- Diapers + wipes → baby lotion likelihood rises
- Oats + banana → protein powder surfaces as a natural complement


The key distinction from a simple co-occurrence model is that the Transformer captures non-linear, multi-item relationships. It doesn’t just learn that A and B appear together; it learns that A, B, and the context of a Monday morning in Bengaluru collectively point toward C.


This is what contextual recommendation actually means: not reacting to a single query, but reading the latent intent inside a partial basket and completing the picture.


Side-by-side comparison of recommendations


### Impact


We ran an A/B test to measure the effect of cart contextual recommendations against the existing baseline. The results are as follows:


- **Add To Cart Rate : +23.4%**
- **Average Order Value: +70p**
- **Gross Profit Per Order : +10p**


The ‘You Might Also Like’ surface, which exposes recommendations directly within the cart experience, saw the strongest lift. This makes intuitive sense: when a user is actively building a cart, a well-timed, contextually relevant suggestion is far more actionable than one served on a cold homepage. The model’s ability to infer intent early in the session is what drives this, getting the right recommendation in front of the user before they’ve had to think to search for it.


### What’s Next


The cart contextual model is the foundation layer for recommendations at Zepto. The next step is moving from cohort-level intent understanding to individual-level personalization, layering user history, behavioral signals, and session context on top of the cart signal. We’ll cover that in a follow-up post.


---


[Your Cart Has a Story. Here’s How We Learned to Read It.](https://blog.zepto.com/your-cart-has-a-story-heres-how-we-learned-to-read-it-10ba9188f534) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
