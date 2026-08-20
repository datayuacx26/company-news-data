---
schema_version: "1.0.0"
document_id: "40c77fd4748d4b813aae85072cca1968f0e64db83ca7f1fe821d8acb4008cbda"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/rouge-metric"
published_at: "2024-06-01T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:5d509e4c60fa89530a4ab6a3b7a2c25db9d1798f02ea7658c50866dd038576b7"
---

# Evaluating Machine Summarization with ROUGE

## Abstract


*This post examines the ROUGE framework for evaluating machine summarization, highlighting its recall-oriented approach compared to precision-based metrics like BLEU. It breaks down key variations such as ROUGE-N, ROUGE-L, and ROUGE-S, explaining how each measures overlap and sequence preservation through detailed equations and examples. The guide concludes with strategic recommendations on choosing the right ROUGE metric for different types of content, from technical documents to creative writing.*


---


## Introduction


In our previous[post](https://deconvoluteai.com/blog/bleu-score) , we explored the BLEU (Bilingual Evaluation Understudy) metric \[1\], which is primarily designed for machine translation. BLEU focuses on n-gram precision and penalizes deviations from reference translations, which can lead to unfair penalization for legitimate paraphrasing and the use of synonyms.


When it comes to evaluating summarizations, a different approach is needed. Unlike translation, where there's often a single correct output, summarization can have multiple valid outputs. A good summary might use synonyms or rephrase content in various ways. This is why ROUGE (Recall-Oriented Understudy for Gisting Evaluation) \[2\] was developed. It is specifically designed for evaluating summaries by measuring the overlap between the generated summary and reference summaries, essentially focussing on recall (see my[post](https://deconvoluteai.com/blog/blog/evaluating-nlp-models) for precision and recall). By focusing on recall, ROUGE captures how much of the reference summary's content is reflected in the generated summary, allowing flexibility in phrasing and word choice. This approach acknowledges the multiplicity of correct summaries and ensures a more comprehensive evaluation.


ROUGE accommodates multiple reference summaries by performing pairwise calculations of scores between the generated summary and each reference summary. The highest score among these pairs is often selected to represent the quality of the summary. Additionally, ROUGE incorporates methods like averaging and Jackknifing to ensure robust and reliable evaluation. Interestingly, ROUGE also performs well for translation tasks, as noted in various research papers, but its primary strength lies in handling the unique challenges posed by summarization.


There are different versions of ROUGE, each capturing different aspects of summary quality. In the following sections, we will first explore the intuition behind each variation before formalizing them. We end the post with a recommendation on when to use which version of ROUGE.


## Variations of ROUGE


In this section, we will explore the different variations of ROUGE, explaining the idea behind each one and providing the corresponding equations. These variations include ROUGE-N, ROUGE-L, ROUGE-W, and ROUGE-S.


### ROUGE-N: Overlap of n-grams


Imagine you want to check how much of the original text is captured in the summary by comparing sequences of words. This is the essence of ROUGE-N, which measures the overlap of n-grams between the generated summary and reference summaries.


The ROUGE-N score is calculated as follows:


ROUGE-N=∑S∈{Reference Summaries}∑gramn∈SCountmatch(gramn)∑S∈{Reference Summaries}∑gramn∈SCount(gramn)\\text{ROUGE-N} = \\frac{\\sum_{S \\in \\{\\text{Reference Summaries}\\}} \\sum_{gram_{n} \\in S} \\text{Count}_{\\text{match}}(gram_{n})}{\\sum_{S \\in \\{\\text{Reference Summaries}\\}} \\sum_{gram_{n} \\in S} \\text{Count}(gram_{n})}


ROUGE-N


=


∑


S


∈


{


Reference Summaries


}


​


∑


g


r


a


m


n


​


∈


S


​


Count


(


g


r


a


m


n


​


)


∑


S


∈


{


Reference Summaries


}


​


∑


g


r


a


m


n


​


∈


S


​


Count


match


​


(


g


r


a


m


n


​


)


​


Here, nn


n


is the length of the n-gram gramngram_{n}


g


r


a


m


n


​


, and Countmatch(gramn)\\text{Count}_{\\text{match}}(gram_{n})


Count


match


​


(


g


r


a


m


n


​


)


is the maximum number of n-grams co-occurring in the candidate summary and the set of reference summaries. Count(gramn)\\text{Count}(gram_{n})


Count


(


g


r


a


m


n


​


)


is the total number of n-grams in the reference summaries.


ROUGE-N focuses on recall, meaning the denominator in the formula is the total number of n-grams in the reference summaries. This is in contrast with BLEU, which is precision-based. As more reference summaries are added, the denominator increases, expanding the space of possible good summaries. For more information about precision and recall, see my[post](https://deconvoluteai.com/blog/evaluating-nlp-models) on evaluations.


By summing over all reference summaries in the numerator, ROUGE-N gives more weight to n-grams that appear in multiple references. This favors candidate summaries containing n-grams shared by more references, aligning with the goal of finding a summary that closely matches the consensus among reference summaries.


### Handling Multiple References


When using multiple reference summaries, the computation of ROUGE-N involves pairwise comparisons between the candidate summary and each reference summary. The highest pairwise ROUGE-N score is selected as the final score:


ROUGE-Nmulti=arg max⁡iROUGE-N(ri,s)\\text{ROUGE-N}_{\\text{multi}} = \\argmax_i \\text{ROUGE-N}(r_i, s)


ROUGE-N


multi


​


=


i


arg


max


​


ROUGE-N


(


r


i


​


,


s


)


Here, rir_{i}


r


i


​


is an individual reference summary, and ss


s


is the candidate summary.


To enhance robustness, a Jackknifing procedure is applied. Given MM


M


reference summaries, the best score is computed over MM


M


sets of M−1M−1


M


−


1


references. The final ROUGE-N score is the average of the MM


M


scores using different sets of M−1M−1


M


−


1


references. This procedure allows for an estimation of average human performance and ensures the reliability of the ROUGE score computations.


### ROUGE-L: Longest common subsequence


Let's look at an example to understand a weakness of ROUGE-N and how ROUGE-L addresses it.


- Reference: police killed the gunman
- Candidate 1: police kill the gunman
- Candidate 2: the gunman kill police


As we can see, Candidate 1 has the same meaning as the reference, and Candidate 2 has a different meaning. This difference, however, is not reflected in ROUGE-N scores, such as ROUGE-2 (bi-grams), where both candidates achieve the same score.


The issue here is that ROUGE-N does not capture the sentence-level structure. To address this, ROUGE-L measures the longest common subsequence (LCS) between the generated summary and reference summaries.


#### Longest Common Subsequence


The LCS of two sequences is the longest sequence that can be derived from both by deleting some elements without changing the order of the remaining elements. For example, for the sequences "police killed the gunman" and "police kill the gunman", the LCS is "police the gunman".


The intuition behind using LCS is that a sentence is a sequence of words. The longer the LCS between two sentences, the more similar they are. This makes LCS more flexible than n-grams, as it does not require consecutive matches but still maintains the order of words.


#### Calculation of ROUGE-L


Using LCS, we can calculate ROUGE-L as the[F score](https://deconvoluteai.com/blog/evaluating-nlp-models) on the sentence level. As a reminder, the F score combines precision and recall. The precision here is calculated like this


PLCS=LCS(S,R)Length(S)P_{\\text{LCS}} = \\frac{\\text{LCS}(S,R)}{\\text{Length(S)}}


P


LCS


​


=


Length(S)


LCS


(


S


,


R


)


​


where LCS(S,R)LCS(S,R)


L


CS


(


S


,


R


)


is the length of the longest common subsequence between the candidate summary SS


S


and the reference summary RR


R


, and Length(S)Length(S)


L


e


n


g


t


h


(


S


)


is the length of the candidate summary. The recall is calculated like this


RLCS=LCS(S,R)Length(R)R_{\\text{LCS}} = \\frac{\\text{LCS}(S,R)}{\\text{Length(R)}}


R


LCS


​


=


Length(R)


LCS


(


S


,


R


)


​


where Length(R)Length(R)


L


e


n


g


t


h


(


R


)


is the length of the reference summary.


Using these two, ROUGE-L is calculated as


ROUGE-L=FLCS=(1+β2)⋅PLCS⋅RLCSPLCS+β2⋅RLCS\\text{ROUGE-L} = F_{\\text{LCS}} = \\frac{(1 + \\beta^2) \\cdot \\text{P}_{\\text{LCS}} \\cdot \\text{R}_{\\text{LCS}}}{\\text{P}_{\\text{LCS}} + \\beta^2 \\cdot \\text{R}_{\\text{LCS}}}


ROUGE-L


=


F


LCS


​


=


P


LCS


​


+


β


2


⋅


R


LCS


​


(


1


+


β


2


)


⋅


P


LCS


​


⋅


R


LCS


​


​


Here, ββ


β


is typically set to 11


1


, making LCSLCS


L


CS


the harmonic mean of precision and recall.


ROUGE-L effectively captures the sentence-level structure by considering the longest matching sequence of words in order. However, it has some limitations. It only counts the main in-sequence words, which can penalize shorter summaries and alternative sequences that are equally valid but not the longest.


### ROUGE-W: Weighted version of ROUGE-L


We have seen how ROUGE-L captures the sentence-level order of words using the Longest Common Subsequence (LCS). We can illustrate a weakness of LCS with the following example:


- Candiate: \[A B C D E F G\]
- Reference 1: \[A B C D H I K\]
- Reference 2: \[A H B K C I D\]


Here, both references achieve the same ROUGE-L score, as both contain the LCS A,B,C,DA, B, C, D


A


,


B


,


C


,


D


. The problem is that LCS does not differentiate between different spatial relations of the matching words. In other words, it doesn't account for the contiguousness of the matching sequences.


To improve upon the basic LCS method, ROUGE-W introduces a weighted Longest Common Subsequence (WLCS). The idea is to remember the length of consecutive matches encountered so far. This method assigns higher weights to longer contiguous matches.


#### Weighted Longest Common Subsequence (WLCS):


In WLCS, a parameter kk


k


is used to indicate the length of the current consecutive matches ending at positions ii


i


in the candidate summary and jj


j


in the reference summary. Longer consecutive matches are rewarded with a higher weight, while jumps and gaps are penalized.


The full implementation uses dynamic programming, making the exact equations complex. However, a simplified version of the formula without penalty terms for jumps and gaps can be expressed as:


ROUGE-Wapprox=∑(i,j)∈WLCSk(i,j)Length of Reference\\text{ROUGE-W}_{\\text{approx}} = \\frac{\\sum_{(i,j) \\in \\text{WLCS}} k(i,j)}{\\text{Length of Reference}}


ROUGE-W


approx


​


=


Length of Reference


∑


(


i


,


j


)


∈


WLCS


​


k


(


i


,


j


)


​


Here, k(i,j)k(i,j)


k


(


i


,


j


)


is the length of the current contiguous matches ending at positions ii


i


and jj


j


. The numerator sums up the lengths of the contiguous matching sequences in the WLCS. The denominator is the length of the reference summary, normalizing the score.


#### Advantages of ROUGE-W


ROUGE-W, as a weighted variant of ROUGE-L, addresses the limitation of LCS by giving higher scores to longer contiguous matches. This emphasizes the importance of maintaining longer sequences of matches, which better reflects the quality of the generated summary in preserving the order and coherence of the reference summary.


### ROUGE-S: Skip bi-gram Co-Occurrence Statistics


ROUGE-S extends the concepts of ROUGE-N and ROUGE-W by considering not just consecutive n-grams but also skip-bigrams. This allows for a more flexible comparison that captures non-contiguous relationships between words in the text.


#### Skip-Bigrams


A skip-bigram is any pair of words in their sentence order, allowing for gaps. This flexibility can capture relationships between words that are not adjacent but still maintain their order.


In the reference "police killed the gunman", the skip-bigrams are "police killed", "police the", "police gunman", "killed the", "killed gunman", and "the gunman".


ROUGE-S measures the overlap of skip-bigrams between the generated summary and reference summaries. By considering skip-bigrams, ROUGE-S can capture the sentence structure more flexibly than consecutive n-grams.


ROUGE-S=∑S∈{Reference Summaries}∑skip-bigram∈SCountmatch(skip-bigram)∑S∈{Reference Summaries}∑skip-bigram∈SCount(skip-bigram)\\text{ROUGE-S} = \\frac{\\sum_{S \\in \\{\\text{Reference Summaries}\\}} \\sum_{\\text{skip-bigram} \\in S} \\text{Count}_{\\text{match}}(\\text{skip-bigram})}{\\sum_{S \\in \\{\\text{Reference Summaries}\\}} \\sum_{\\text{skip-bigram} \\in S} \\text{Count}(\\text{skip-bigram})}


ROUGE-S


=


∑


S


∈


{


Reference Summaries


}


​


∑


skip-bigram


∈


S


​


Count


(


skip-bigram


)


∑


S


∈


{


Reference Summaries


}


​


∑


skip-bigram


∈


S


​


Count


match


​


(


skip-bigram


)


​


Consider the earlier example sentences:


- Reference: police killed the gunman
- Candidate 1: police kill the gunman
- Candidate 2: the gunman kill police


For Candidate 1, the skip-bigrams that match the reference are "police the", "police gunman", and "the gunman". For Candidate 2, only the skip-bigram "the gunman" matches, making Candidate 1 a better summary. This makes sense, as Candidate 2 has a different meaning than Candidate 1 and the reference.


Note how, when considering for example unigrams, both candidates would receive the same score. ROUGE-S captures nuances better by considering all pairs of words in sequence, regardless of their adjacency. This helps to better evaluate the quality of summaries, especially when the word order significantly impacts the meaning.


#### Advantages of ROUGE-S


ROUGE-S offers greater flexibility than ROUGE-N and ROUGE-L by allowing for gaps between words. This makes it particularly useful for capturing the essence of the summary when the exact phrasing might differ but the overall order and relationship between words are preserved.


## Choosing the Right ROUGE Metric


Selecting the appropriate ROUGE metric depends on your summarization task's specific requirements. Each metric provides unique insights into different aspects of summary quality, so understanding when to use each one is crucial.


For technical and terminology-heavy content, it's important to maintain essential terms and phrases. For flexible and creative content, focus more on preserving overall meaning.


When key terms and phrases from the reference need to be present in the summary, use ROUGE-N. This is particularly useful for technical documents, reports, or any content where specific terminology is crucial. ROUGE-1 (unigrams) captures individual keywords, while ROUGE-2 (bigrams) and higher-order n-grams evaluate the preservation of short phrases.


If maintaining the order and coherence of information is important, use ROUGE-L. This metric is beneficial for tasks requiring summaries that maintain the logical flow and sentence structure of the original text, such as legal documents, scientific articles, or narrative content.


For tasks where flexibility in word order is acceptable but capturing related terms in proximity is still important, use ROUGE-S. This metric is useful for creative tasks, such as summarizing fiction, dialogue, or content where the meaning can be preserved even if the word order changes. ROUGE-SU, which combines skip-bigrams with unigrams, provides a more nuanced evaluation for such flexible summarization tasks.


For a well-rounded evaluation, use a combination of ROUGE-N (e.g., ROUGE-1, ROUGE-2), ROUGE-L, and ROUGE-S. This approach is ideal for general summarization tasks where balancing key terms, sentence structure, and word order flexibility is important. It provides a comprehensive view of summary quality by covering multiple content dimensions.


## References


\[1\] Kishore Papineni, Salim Roukos, Todd Ward, and Wei-Jing Zhu. 2002. *BLEU: a method for automatic evaluation of machine translation.* In Proceedings of the 40th Annual Meeting on Association for Computational Linguistics (ACL '02). Association for Computational Linguistics, USA, 311–318.[https://doi.org/10.3115/1073083.1073135](https://doi.org/10.3115/1073083.1073135)


\[2\] Chin-Yew Lin. 2004. *ROUGE: A Package for Automatic Evaluation of Summaries.* In Text Summarization Branches Out, pages 74–81, Barcelona, Spain. Association for Computational Linguistics.[https://aclanthology.org/W04-1013.pdf](https://aclanthology.org/W04-1013.pdf)
