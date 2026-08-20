---
schema_version: "1.0.0"
document_id: "0ab2504809d088453c7347d931c4257dd7ebe6b740f6afe8c40158433347d541"
company_key: "yc-qfex"
company: "QFEX"
source_id: "yc-qfex-rss-5ea6110e160b"
canonical_url: "https://research.qfex.com/p/from-linear-alphas-to-conditional"
published_at: "2026-06-15T10:11:29+00:00"
first_seen_at: "2026-07-25T20:12:49.931294+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:3157086689375b6362da613177c009e4f7b361f3cba1eb8e18f4c35f8e96c35e"
---

# From Linear Alphas to Conditional Alpha: Decision Trees, Random Forests, and the Models Behind Modern HFT

# From Linear Alphas to Conditional Alpha: Decision Trees, Random Forests, and the Models Behind Modern HFT


[Annanay Kapila](https://substack.com/@annanaykapila)


Jun 15, 2026


After modeling order-book alphas for long enough, one eventually notices that they are rarely unconditional.


An imbalance signal that works beautifully in a wide-spread market can become completely obsolete once the spread narrows. Similarly, a micro-price skew that predicts the next few hundred milliseconds during volatile periods may become noise in quieter conditions.


Thanks for reading QFEX Research! Subscribe for free to receive new posts and support my work.


Yet the first generation of prediction models assume exactly the opposite.


In the last few pieces we’ve walked through linear regression, regularization, and bagging - the foundational toolkit used to turn raw market signals into something tradable. Those models start from a simple assumption: every alpha contributes to the future price through a fixed coefficient.


\\(b_1x_1+b_2x_2+b_3x_3 \\)


This approach is intuitive and surprisingly effective, but it has a limitation that becomes impossible to ignore in real markets.


It cannot express interactions.


The model has no way to say that imbalance only matters when the spread is wide. It cannot learn that volatility changes the usefulness of micro-price skew. Every relationship is assumed to be fixed.


You could try adding interaction manually, but the number of combinations quickly explodes. Alternatively, instead of guessing the rules ourselves, we can let the data discover them.


This paradigm shift takes us from linear models to decision trees.


## Engineering Conditional Alpha


If I want to predict prices, I need to create new, conditional alpha from old alphas.


Doing this manually quickly leads to too many possibilities. So instead of writing increasingly complicated linear equations, we move to a decision tree.


Suppose we have a large training dataset containing the values of our alphas - imbalance, microprice skew, spread, short-term realized volatility, and others - together with the future price observed after each snapshot.


The objective is to find a clean partition of that dataset. We want a binary question that separates observations into groups where future returns behave more consistently.


The questions might look like:


-


spread > 1 tick?


-


imbalance < 0.5 bps?


-


realized volatility > threshold?


Every possible split divides the data into two child nodes. The tree then asks a simple question: can these two groups be predicted more accurately than the original group?


Imagine taking every observation and sorting it into two buckets. In one bucket, future returns tend to be positive. In the other, they tend to be negative. The cleaner that separation becomes, the more useful the split.


There are many ways to formalize this, but a popular approach is that of ‘Gradient Boosted Trees’. When trying to minimize squared loss (like we did in linear regression), we define the similarity of a set of points as:


\\(\\frac{ \\sum (y-\\text{mean}(y))^2 } {\\text{num_samples}} \\)


Then, for each possible split, the tree calculates the gain:


\\(gain ={similarity(parent\\,node)} -\[{similarity(left\\,child)}+similarity(right\\,child)\]\\)


We then split the point by picking the best alpha and x such that “alpha < x?” is the split that maximizes gain. Once that first split is chosen, the same procedure is repeated on the two partitioned datasets. Then repeated again. And again.


The result is a hierarchy of conditional rules:


```text
spread > 1 tick?
yes -> volatility high?
yes -> expected return +0.32 bps
no  -> expected return +0.08 bps
no  -> imbalance > threshold?
yes -> expected return +0.11 bps
no  -> expected return -0.03 bps


```


At the end of the process, each leaf node makes a simple prediction:


\\(\\text{predicted_future_return}=\\text{mean}(y) \\)


for the samples that ended up inside that node


So the tree is not inventing a new raw signal. It is engineering a conditional alpha from existing alphas. Instead of saying “imbalance predicts future price,” the model can learn something closer to:


“If spread is wide, and volatility is high, then imbalance predicts future price.”


That is the conceptual jump. Linear regression combines alphas with fixed weights. A decision tree creates conditional rules that decide when those alphas matter.


## The Practical Problems


Opening the door to conditional alpha creates a new set of problems.


When should the tree stop creating new splits? How do we prevent it from fitting noise? And why are the model’s predictions now discrete and bounded?


These questions are unavoidable because decision trees are incredibly flexible. Left unconstrained, a tree can continue splitting until it memorizes the training set.


The model appears brilliant in-sample while becoming useless out-of-sample. The challenge shifts from discovering structure to distinguishing genuine structure from randomness.


We’ll revisit those issues later, but for now it’s enough to recognize that a decision tree solves one problem while creating several new ones.


The most glaring issue is the fragility of a single tree.


## Why One Tree Is Never Enough


Small data changes can alter the earliest splits. Those early splits determine everything that follows. As a result, tiny changes in the dataset can produce a completely different tree.


The model becomes unstable. When we encountered instability in linear models, we addressed it using bagging. The same idea works here.


## Random Forests: Stabilizing Conditional Alpha


Instead of training one model, we train many, create multiple bootstrap samples from the original dataset, then fit a separate tree to each sample. Then, finally, we average the predictions, just like before.


```text
data
├─ bootstrap #1 -> tree T₁ ┐
├─ bootstrap #2 -> tree T₂ ├─> pred_future_price
├─ bootstrap #3 -> tree T₃ ┘
└─ ... -> tree T_B
```


This collection of trees is known as a random forest.


Each individual tree may be unstable. But averaging many trees produces a more robust prediction. The forest becomes far more stable than any single tree.


At first glance, this looks like a complete solution. But there’s something slightly unsatisfying about it.


Each tree discovers useful structure. Then the next tree starts from scratch. The information learned by the previous tree is never reused.


That observation leads directly to one of the most important ideas in modern machine learning: boosting.


## Boosting: The Production Weapon


Instead of treating every tree as an independent model, we let each new tree focuses only on what the current model is still getting wrong.


Start with the simplest possible prediction:


\\(Pred_0= \\text{mean}(y) \\)


Then make the Nth decision tree on the data:


\\( y-\\text{pred}_{n-1} \\)


and update:


\\(\\text{pred}_{n-1} + \\eta T_n \\)


This technique of creating many sequential trees, each correcting the last, is known as boosting.


Conceptually, it’s a very different philosophy from Random Forests. Random Forests reduce instability by averaging many independent trees. Boosting builds models sequentially, with each new tree correcting what the current model is still getting wrong.


Popular implementations include LightGBM and XGBoost.


This is much closer to the type of prediction engine that actually sits inside a modern HFT stack.


## Conclusion


The progression from linear regression to decision trees to boosted ensembles demonstrates a sequence of increasingly powerful ways to extract conditional alpha from market data.


Linear models assume relationships are fixed. Decision trees allow those relationships to depend on market state. Random Forests stabilize those conditional relationships by averaging many trees. Boosted trees go a step further, sequentially correcting errors until the model becomes far more expressive than any individual tree could ever be.


Viewed through that lens, decision trees are not really about trees at all; they’re about discovering conditional alpha.


And boosting is simply the next step in making that conditional alpha useful enough for production.
