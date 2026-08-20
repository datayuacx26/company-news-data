---
schema_version: "1.0.0"
document_id: "fdf511ccf5a7164eba7744292eac0f2db0983a5e791e7e10488e5eda813a8618"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/research/universal-2"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:3a4baab676bfe83015adb0b8bbd6463e848c63686a1a451bac592cd3ffb92ee7"
---

# Universal-2-TF

Our previous Speech-to-Text model[Universal-1](https://www.assemblyai.com/research/universal-1) , trained on 12.5 million hours of audio, is a best-in-class model, achieving remarkable robustness and accuracy across several industry-critical languages. Universal-2 builds upon the foundation laid by Universal-1, pushing the boundaries of Speech-to-Text further by improving accuracy both overall and in the areas critical to real-world use cases - proper nouns, formatting, and alphanumerics.


Fig. 1 - Comparison of error rates for Universal-2 vs Universal-1 across overall performance (Standard ASR) and four last-mile areas, each measured by the appropriate metric


###### Solving the "last mile" issues in Speech-to-Text


Speech-to-Text models have steadily improved for several decades, approaching human level performance in terms of raw word error rate (WER)[\[source\]](https://www.economist.com/technology-quarterly/2017/01/05/speech-recognition) . However, univariate WER analyses of Speech-to-Text systems fail to capture the nuances of how a desirable Speech-to-Text system should behave, leaving much of the story untold. There are a range of "last mile" issues that modern Speech-to-Text systems exhibit, like poor formatting and proper noun accuracy, which can severely degrade transcript quality but are masked by these univariate WER analyses.


Universal-2 was designed to address these last mile issues, bridging the gap between the raw accuracy of modern Speech-to-Text systems and the final result of transcripts that look and "feel" good - with accurate proper noun spelling, proper formatting and punctuation, and the like which comprise desirable transcripts:


1. **Qualitatively preferred:** People prefer transcripts from Universal-2 more than **70% of the time** compared to those of Universal-1
2. **Faithful proper nouns:** Universal-2 faithfully transcribes proper nouns, demonstrating up to a **24% improvement** over Universal-1 in proper noun accuracy and achieving state of the art results
3. **Accurate formatting and casing:** Universal-2 creates highly readable transcripts, demonstrating up to a **14.8% improvement** over Universal-1 in formatting accuracy and achieving state of the art results in truecasing and inverse text normalization accuracy
4. **Precise timestamps:** Universal-2 accurately places timestamps, with the vast majority of timestamps accurate to within **200ms of tolerance** , maintaining parity with Universal-1 and outperforming Whisper Large-v3 by 65.6% relative.
5. **Best-in-class accuracy:** Universal-2 maintains the leading position in standard ASR accuracy established by Universal-1, outperforming Universal-1 by **3%** , and exceeding the next-best system we tested—across both open-source models and Speech-to-Text providers—by **15%** .


## Building Universal-2


Our Speech-to-Text pipeline consists of an Automatic Speech Recognition (ASR) model and a Text Formatting module, both of which Universal-2 improved over Universal-1 to achieve the advancements highlighted above. We thoroughly examined every aspect of our pipeline and made significant improvements to several key technical elements, including data processing, tokenization, and model architecture, and have integrated these enhancements as outlined below.


### ASR


#### Model Architecture


Universal-2's architecture maintains a similar configuration and capacity to its predecessor, Universal-1. Our models are based on an ASR decoder architecture called the Recurrent Neural Network Transducer (RNN-T), which offers advantages in scalability, robustness against hallucinations, and timestamp accuracy—key factors for real-world usage at scale. We use a 600M parameter Conformer RNN-T model. While the prevailing trend, especially in the natural language processing domain, leans toward scaling model size, our extensive investigation has shown diminishing returns from scaling both the encoder and decoder beyond certain thresholds. In fact, doubling the model's capacity resulted in less than a 5% relative improvement in performance, while inference time more than doubled, indicating a poor trade-off between capacity and efficiency. Therefore, we have chosen to retain a modest model capacity.


#### Tokenization


Tokenization is the process of converting text, or a sequence of words, into smaller units called "tokens". ASR models learn to predict tokens rather than text by training on tokenized transcripts, so the design of the tokenization process has a significant impact on the performance of an ASR model, in some cases even limiting its abilities.


Following the release of Universal-1, we conducted an extensive evaluation of the model across a wide range of Speech-to-Text use cases. One area of particular interest to our customers is the accurate recognition of consecutive digits, or more broadly, alphanumeric strings, which is critical to properly transcribing phone numbers, license plates, and customer IDs. While Universal-1 performed well overall, we observed a degradation in recognition accuracy when handling consecutively repeating digits. This degradation primarily manifested as the deletion of repeated digits. For example, the following utterance:


one two two three three three


was often transcribed as:


one two three three


After investigation, this issue was identified as one intrinsic to RNN-T's architecture itself, that being RNN-T has a limited ability to produce any consecutive identical tokens. Indeed, previous studies \[1,2\] have reported that RNN-T exhibits a strong inductive bias against predicting identical tokens in sequence. We validated this by comparing RNN-T to a Connectionist Temporal Classification (CTC) decoder, using the same encoder structure from Universal-1 and keeping all other variables constant. The CTC model achieved perfect accuracy in recognizing repeated identical digits in synthetic utterances while RNN-T struggled to recognize them, verifying our hypothesis that Universal-1's RNN-T decoder was responsible for this relative weakness in predicting consecutive digits.


To address this shortcoming of the RNN-T architecture while maintaining the benefits mentioned in Model Architecture[\[link\]](https://www.assemblyai.com/research/universal-2#asr-model-architecture) , Universal-2 utilizes a special <repeat_token> in its tokenization scheme. This token is inserted between repeated tokens in the target sequences of the training data. Since each digit is represented as a separate word-piece in the model’s vocabulary, the example digit sequence would be represented as follows:


one two <repeat_token> two three <repeat_token> three <repeat_token> three


With this change, the RNN-T model no longer needs to predict the same token multiple times in a row, allowing it to recognize consecutive repetitions accurately without deletions and therefore overcome this shortcoming of the RNN-T architecture. During inference, the <repeat_token> is simply removed from the final ASR output, resulting in the correct sequence:


one two two three three three


As demonstrated in the performance analysis section below, this tokenization scheme improves the model's accuracy by up to 90% relative on synthetic datasets designed to simulate the target use case. Additionally, we show that this solution generalizes well beyond digits, successfully handling the case of consecutively repeating words as well.


#### Training Data


In ASR, the quantity and quality of training data play a central role in determining the accuracy and robustness of the resulting ASR models. Upon auditing our Universal-1 training data, we found that the quality of the reference transcripts was inconsistent across training samples. In our prior model development, this inconsistency forced us to sample training examples carefully to ensure that low-quality samples would not negatively impact the resultant model's quality, which prevented us from using the entire training dataset to its fullest extent. In Universal-2, we further improved our training data processing pipeline while also increasing the size of the supervised training set.


For Universal-2, we doubled the size of the supervised training dataset from 150,000 hours to 300,000 hours. In addition to the verification-based filtering scheme described in[\[3\]](https://www.assemblyai.com/research/universal-2#references) , we have introduced additional steps to further clean and filter our training data. One finding from our audit was that some reference transcript errors stemmed from the inconsistent use of linguistic artifacts (for example, confusion between primes and apostrophes) and punctuation errors. This resulted in unfavorable side effects in our text normalization process, which is applied to derive the reference transcripts for ASR model training. We have established various heuristics to detect and correct errors in both normalized and formatted transcripts. While Universal-2's ASR model is trained to predict normalized text, formatted transcripts provide valuable signals to identify issues in the reference transcripts.


In the actual model training, we first pre-trained an RNN-T encoder using 12.5 million hours of diverse, multilingual audio. After the pre-training, the encoder was combined with a randomly initialized decoder, and the entire model was fine-tuned using a combination of the supervised dataset described above and a pseudo-labeled dataset, similar to the approach used in Universal-1.


The expansion of supervised training data, combined with enhanced data cleaning and filtering processes, enabled us to train the Universal-2 model effectively on all training samples with more iterations, resulting in continuous performance improvements. This extended training was especially effective in boosting the model's accuracy in recognizing proper nouns, as demonstrated in the performance analysis section.


#### Text Formatting


As mentioned previously, our Speech-to-Text service features a modular architecture, composed of the ASR model and a Text Formatting module. The ASR model transcribes spoken words into verbal form, which helps reduce the model's vocabulary size (i.e., the number of tokens used). This reduction facilitates ASR model training and can enhance transcription accuracy. However, the raw verbal transcripts lack punctuation and are entirely lowercased. Additionally, all words are spelled out, which may be unsuitable for certain types of content, such as dates and currencies. This unformatted output can be less readable for humans and challenging for downstream tasks that rely on properly formatted text.


To address these issues, our Text Formatting module converts raw transcripts into well-formatted text by applying Punctuation Restoration, Truecasing, and Inverse Text Normalization (ITN). This ensures that the final output is both readable and suitable for various applications. For example, the Text Formatting module transforms the following raw output:


*“are you happy that assembly a i will release universal two on october thirtieth at three p m e t”*


into a properly formatted written form:


*“Are you happy that AssemblyAI will release Universal-2 on October 30th at 3pm ET?”*


Universal-2 has introduced a major upgrade in Text Formatting compared to Universal-1, resulting in significant improvements in Truecasing and ITN. Universal-1 relied on a combination of rule-based and statistical approaches, which limited its ability to leverage linguistic context for Text Formatting. In contrast, Universal-2’s Text Formatting employs a fully neural approach, making it entirely trainable, context-aware, and easier to maintain. This new method has led to noticeable advancements in Truecasing and ITN, resulting in transcripts that are more natural and readable, all while maintaining computational costs comparable to Universal-1.


#### Model Architecture


The following diagram illustrates the Universal-2 Text Formatting architecture, which was designed with three key objectives:


1. Token-based Truecasing: Universal-1 used a character-based model for Truecasing, which was prone to hallucination errors and required higher computational costs. By switching to token-based modeling, Universal-2 addresses these drawbacks, resulting in more accurate Truecasing with reduced computational demands.
2. Seq2Seq Modeling for ITN: Universal-1 relied on a rule-based ITN approach, which was cumbersome to maintain and challenging to extend. This approach also struggled to leverage linguistic context, limiting ITN accuracy. Universal-2 overcomes these limitations by utilizing a seq2seq model that can better capture contextual information.
3. Robustness and Efficiency: Fully neural approaches often face challenges in edge cases that weren’t adequately observed during training, and they can also be more computationally intensive. To make Universal-2’s Text Formatting model robust and efficient at scale, we’ve designed it to maintain a similar computational cost to Universal-1, with enhanced robustness even in challenging cases.


The Universal-2 Text Formatting architecture consists of two models:


- Multi-objective tagging model: This model consists of a shared Transformer encoder followed by three separate classification heads to perform specific tasks: post-punctuation prediction, token-level truecasing dealing with all-caps, all-lowercase, word capitalization as well as mixed-case words identification, and textual span detection for ITN processing.
- Text span conversion model: This seq2seq model utilizes a Transformer encoder-decoder architecture. It is applied to normalized mixed-case and ITN text spans identified by the multi-objective tagging model, generating corresponding formatted counterparts.


Splitting the Text Formatting process into these two stages provides two practical benefits:


- The computationally intensive seq2seq model is applied only to text spans likely to contain words requiring ITN or mixed-casing, which helps reduce overall computational costs.
- By focusing solely on ITN and truecasing for mixed-cased words, the seq2seq model is much less likely to produce hallucination errors.


The two models are trained independently, enabling iterative improvements for each component. This modular design also simplifies maintenance and upgrades for specific parts of the pipeline as needed.


Overall, Universal-2’s new Text Formatting architecture eliminates brittle, rule-based components and reduces error-prone approaches, leading to more accurate and adaptable casing and ITN predictions. In addition, the enhanced maintainability resulting from the adoption of a fully neural approach will support rapid development iterations over time.


#### Training Data


Universal-2’s Text Formatting module has been trained using a mix of open-source and in-house data, focusing on English texts. Despite some overlap, the training datasets for the multi-objective and seq2seq models are distinct, allowing for specialized training of each component of the Text Formatting pipeline.


Our data preparation process involved careful gathering and cleaning of various open-source and in-house datasets to produce high quality training data. In particular, we applied cleaning functions to remove artifacts, unwanted symbols and punctuation marks. We have observed that this data preparation process has contributed to the robustness and accuracy of our Text Formatting models.


Multi-objective Tagging Training Data: The multi-objective model was trained primarily on datasets from spoken domains. This choice was made to minimize domain shift during inference, as the model will be processing spoken transcripts in real-world applications which may have different punctuation styles compared to written text.


Our multi-objective tagging model has been trained for 100k steps with a batch size of 256 over 23 million training samples, totalling approximately 5.2 billion words of which 72% are publicly available data, while 28% have been sourced in-house. Each training sequence averages 230 words.


Together they provide a rich foundation for the multi-objective model to learn the nuances of punctuation, casing, and ITN spans in natural spoken text.


Text Span Conversion Training Data: In contrast to the multi-objective tagging model, the seq2seq model requires training data with a wide variety of mixed-case words and linguistic entities that need inverse normalization. Since punctuation is not the main focus here, we did not limit the training data to spoken language sources; instead, we included written text rich in formatting and casing.


Despite the large amount of data gathered, a comprehensive analysis of the available samples pointed out the imbalance and sparseness of specific linguistic entities (e.g., credit card numbers, email addresses, URLs, etc.), which caused sub-optimal learning of the ITN task. To overcome this limitation, we generated targeted synthetic textual samples using multiple open-source LLMs.


Moreover, we observed that the seq2seq model is very sensitive to low-quality training data; we applied stricter filters to the seq2seq data to obtain much cleaner training examples. This further filtering accounts for the difference in the number of samples for the datasets shared between the seq2seq and the multi-objective models.


The seq2seq model was trained for more than 500k steps using a batch size of 512 on heterogeneous data. The training dataset contains 8.9 billion words with 62% of the data sourced in-house, 22% synthetic, while 16% are publicly available data. Each training sequence averages 157 words.


## Performance Analysis


### Results Overview


The bar chart below compares Universal-2, Universal-1, and other ASR systems including both commercial providers and open-source models. All evaluations were performed on the latest models available as of October 2024. When properly evaluating a speech recognition model, it is imperative that the model is not tested on data it was trained on, as this would constitute data leakage and lead to artificially inflated performance metrics. Note that while AssemblyAI models have not been trained on the test sets used in our evaluations, we cannot guarantee this to be the case for the other ASR systems evaluated given that a portion of our test sets was publicly-sourced.


Here is a top-level overview of the results of our tests:


- Universal-2 consistently outperforms Universal-1 in transcribing speech across diverse test sets, including:


- Standard ASR test set, which consists of various testing domains such as telephony, podcasts, and noisy audio.
- Proper noun test set, which is used to measure accuracy in recognizing human names, entity names, places, etc.
- Alphanumerics test set, which includes samples containing many alphanumeric strings (e.g., PS5, iPhone 15).
- Accented speech test set, covering French, Spanish, British, Indian, and African American accented English.
- ASR + ITN + Truecasing, for measuring formatting errors in addition to ASR errors.


- The superiority of Universal-2 over Universal-1 is particularly evident in recognizing proper nouns and alphanumerics, as well as in handling ITN and Truecasing.
- Universal-2 outperforms all other ASR providers overall, ranking first in standard ASR, proper noun recognition, and ITN and Truecasing handling, while placing second in alphanumerics recognition and accented speech recognition.


Fig. 2: This bar chart compares Universal-2, Universal-1, commercial ASR providers, and Whisper large-v3 across several categories. Word Error Rate (WER) is used for Standard ASR, Alphanumerics, and Accented Speech. Proper Noun Error Rate (PNER) is used for Proper Nouns. The performance of “ASR + ITN + Truecasing” is evaluated using Unpunctuated WER (U-WER). To improve readability, bars are truncated at 25% to minimize the impact of outliers.


These results highlight Universal-2's versatile utility and its relevance in practical application scenarios. In the sections that follow, we provide a more detailed analysis of these results.


### ASR Accuracy


Table 1 shows a breakdown of the Standard ASR evaluation results, outlined above, across individual constituent test sets. Universal-2 outperformed all other ASR systems for 7 out of the 10 test sets. On average, Universal-2 achieves a relative WER reduction of 3% compared to Universal-1 and surpasses the nearest external model by 15% relative. On our internal test sets, which consist of more practically relevant audio samples and ensure no data leakage across the systems tested, Universal-2 consistently outperformed all other external systems.


Table 1 - WER for general English ASR test sets, obtained by Universal-2, in comparison to Universal-1 and other open-source and commercial ASR systems.


Table 2 analyzes the proper noun recognition results, where we provide WERs computed over proper nouns, referred to as PNWER, in addition to PNER. Unlike PNER, which is based on the Jaro-Winkler error rate and thus accounts for spelling proximity, the PNWER is based simply on word-level errors. This dataset was curated by sampling audio clips with a high frequency of proper nouns and subsequently transcribing them with a human transcription service for ground-truth labels.


Universal-2 demonstrates significant enhancement compared to its predecessor in proper noun recognition accuracy, with 24% and 20% relative gains with respect to PNER and PNWER, respectively. The results also show its superior effectiveness compared to other Speech-to-Text systems.


Table 2 - Performance on proper noun test set, obtained by Universal-2, in comparison to Universal-1 and other open-source and commercial ASR systems.


Furthermore, we analyzed the benefits of using the <repeat_token> in ASR tokenization. This token was introduced to address RNN-T's limited ability to handle repeated tokens as mentioned previously[\[link\]](https://www.assemblyai.com/research/universal-2#asr-tokenization) . Therefore, for this evaluation, three synthetic test sets were created, each featuring different repetition patterns:


- **Up to 3 Repeating Digits:** A dataset containing digit sequences (1-10), with a 0.5 probability of digit repetition and a maximum of three consecutive repetitions.
- **Up to 10 Repeating Digits:** Similar to the first set but with up to ten consecutive digit repetitions.
- **Up to 3 Repeating Words:** A dataset of random word sequences, also with a 0.5 probability of repetition and a maximum of three consecutive repetitions.


Table 3: WER on synthetic datasets with consecutively repeating digits/words obtained by Universal-2, in comparison to Universal-1.


Table 3 shows the experimental results. By employing the new repetition modeling strategy using the <repeat_token> during tokenization, Universal-2 achieved substantial improvements across all test sets, reducing WER by 90%, 76%, and 53% relative to the baseline for the "up to 3 repeating digits," "up to 10 repeating digits," and "up to 3 repeating words" datasets, respectively. These results demonstrate that our improved tokenization scheme effectively addresses RNN-T's limitations in repeated token modeling, improving both digit and word recognition.


All these analysis results establish Universal-2 as the leading ASR system for recognizing both general and important words in practically relevant scenarios.


### Text Formatting Accuracy


The table below reports the Unpunctuated WER (U-WER) for various datasets across the models we tested. U-WER measures the accuracy of ASR, ITN, and Truecasing, allowing us to assess the improvements to ITN and Truecasing highlighted above. For the sake of completeness, we also report Formatted WER (F-WER), which is similar to U-WER except it additionally measures punctuation accuracy. Note that F-WER tends to fluctuate more than U-WER, given that correct punctuation is not always uniquely determined.


Table 4 - F-WER (Formatted WER) and U-WER (Unpunctuated WER) on Universal-2, Universal 1 and competitors


Regarding U-WER, Universal-2 shows significant improvements over its predecessor and outperforms all other ASR systems across all test sets, confirming its advantage in end-to-end Speech-to-Text performance. In terms of F-WER, it also ranks first in average performance, showcasing its competitiveness, although F-WER scores tend to fluctuate for the reason mentioned above.


To better illustrate the improvements provided by Universal-2's Text Formatting architecture, we conducted isolated evaluations focusing solely on Text Formatting elements—i.e., Punctuation, Truecasing, and ITN—without accounting for ASR errors. Since we do not have access to the Text Formatting components of other Speech-to-Text systems, we focused on comparing Universal-2 and Universal-1. In these evaluations, normalized ground-truth text was fed into the Text Formatting modules of both Universal-2 and Universal-1, and their outputs were assessed using various metrics, each targeting different formatting attributes.


Table 5: Isolated text formatting evaluations, without including ASR errors


Table 5 presents the comparison results of the two Universal Text Formatting models. Across all test sets, ITN accuracy, as measured by I-WER, was consistently improved by Universal-2, with gains of up to 36 percentage points. Similarly, Universal-2 also showed substantial improvements in Truecasing performance, particularly in predicting capitalized and mixed-case words, as demonstrated by the improvements in CAPITAL F1 and M-WER scores. Punctuation restoration accuracy was comparable between Universal-2 and Universal-1. These results demonstrate that the all-neural architecture employed in Universal-2 achieved its design goal of providing significant improvements in Text Formatting.


### Human Preference Test (end-to-end)


Objective metrics like WER do not necessarily capture the[subtle qualities](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/#beyond-wer) that humans value in speech transcripts. To assess Universal-2's ability to capture these qualities, we collaborated with two external vendors to additionally conduct a blind human evaluation using 175 audio samples. We generated formatted transcripts with both Universal-2 and Universal-1 and presented the transcript pairs alongside the audio to human judges. As is typical in blind preference evaluations, the identities of the models were masked during the assessment. Judges were asked to choose between a preference for one model or a “neutral” rating, indicating no preference for either model on a given audio file. Every test sample was seen by every judge.


Figure 3 shows the result of the human evaluation, indicating that our new generation model, Universal-2, was preferred over Universal-1 for 72.9% of the test samples, and preferred 73.8% of the time when there was a preference. This result demonstrates a significant qualitative improvement from Universal-1 to Universal-2, further advancing the state of the art in speech transcription. Notably, the human evaluation for Universal-2 reflects an even greater quality improvement compared to the jump Universal-1 achieved over its predecessor,[Conformer-2](https://www.assemblyai.com/blog/conformer-2/) , where Universal-1 was preferred 60% of the time.


Fig. 3 - Results of the side-by-side human preference test between Universal-2 and Universal-1. Only samples where at least two-thirds of the judges agreed on their ratings were included., which accounted for 93% of all test samples.


### Timestamp Accuracy


As a leading Speech-to-Text service provider, we must ensure that any improvements do not degrade other aspects of performance when building and releasing a new model. Word-level timestamp prediction is a crucial feature utilized by our customers in various applications and downstream tasks, so when testing Universal-2 we sought to ensure that it preserved Universal-1's timestamp estimation accuracy.


Figure 4 demonstrates that Universal-2’s word timestamp prediction accuracy is on par with that of Universal-1, maintaining superior precision compared to Whisper,[as previously reported](https://www.assemblyai.com/research/universal-1) . The figure illustrates word-level timestamp prediction accuracy as a function of the estimation error tolerance threshold. For each value on the x-axis, the corresponding y-axis value indicates the percentage of words whose estimated timestamp falls within this threshold compared to the reference timestamp. A curve that approaches the upper-left corner represents a model with more accurate timestamp estimation. For details on the dataset and how timestamp accuracy is measured, refer to[our Universal-1 blog post](https://www.assemblyai.com/research/universal-1) .


Fig. 4: Word-level timestamp estimation accuracy as a function of estimation error tolerance.


### Inference Efficiency


While the ASR model architecture has not been substantially changed, the Text Formatting model received a major upgrade, which led to both performance and inference efficiency improvements. To better understand the benefits of the new architecture, we ran an analysis of the Text Formatting model’s latency in isolation from the rest of the system.


The execution time analysis was performed on production-like infrastructure using the same inference code and compute resources. To simulate realistic scenarios, we ran the tests in two conditions, short-form and long-form texts. In both cases, we used 120 texts. While the short texts contained an average of 416 words, long texts were composed of 5,477 words on average. Hence, the total numbers of words processed in the two conditions were 50k and 657k, respectively. This allowed us to completely fill up both batch size and sequence length, accounting for heavy load scenarios.


The texts were run through the models three times and the average execution time was measured for each model. After each run, the cache was cleared, and GPU memory was freed.


Fig 5: Inference efficiency analysis of the Text Formatting model. Scores indicate execution times of the two models in different conditions.


Figure 5 shows the results of the inference efficiency analysis, comparing the Universal-2’s Text Formatting model against its predecessor. It is clear that, in both conditions, the new Text Formatting model outperforms the old one, achieving up to 24.2% faster inference speeds. The increased efficiency can be attributed to two major changes:


- The legacy character-level Truecasing model, previously used to predict mixed-case words, has been replaced by a token-level tagging model, resulting in shorter sequence lengths to handle;
- ITN and mixed-case words prediction is now performed by a seq2seq model that operates only on targeted textual spans. This has eliminated unnecessary computation.


In conclusion, this analysis demonstrates that Universal-2's Text Formatting module improved accuracy while reducing computation time, successfully achieving its design goals.


## Available Today in our API


The easiest way to try Universal-2 is through our Playground, where you can upload a file or enter a YouTube link to see a transcript in just a few clicks.


You can also try out our API directly for free. Sign up for a free API token, and head over to our docs or Welcome Colab to be up and running in just a few minutes.


## Evaluation Datasets


###### ASR Datasets


- Common Voice V5.1: We used the English subset of the V5.1 dataset from the[official website](https://commonvoice.mozilla.org/en/datasets) .
- CORAAL: We used the version 2021.07 dataset from[official sources](http://lingtools.uoregon.edu/coraal/) and segmented according to the[FairSpeech project](https://github.com/stanford-policylab/asr-disparities/blob/master/input/CORAAL_transcripts.csv) .
- TED-LIUM 3: We used 11 TED talks, following the[Whisper](https://github.com/openai/whisper/tree/main/data) ’s[TED-LIUM 3](https://www.openslr.org/51/) long-form partition.
- LibriSpeech: We used the test-clean and test-other splits from the[LibriSpeech ASR corpus](https://www.openslr.org/12) .
- Earnings-21: We used the corpus of earnings calls from[the speech-datasets repository](https://github.com/revdotcom/speech-datasets/tree/202206/earnings21) from the 202206 version.
- Meanwhile: We followed the dataset creation procedure from[Whisper](https://github.com/openai/whisper/tree/main/data) and downloaded the 64 segments from YouTube.
- Podcast: We used a 18.2 hour human-labeled dataset of podcasts from a mix of public and internal sources.
- Broadcast: We used a 7.5 hour human-labeled private dataset of news broadcasts from a mix of public and internal sources.
- Telephony: We used a 8.2 hour human-labeled private dataset of telephone conversations from a mix of public and internal sources.
- Noisy: We used a 7.2 hour human-labeled private dataset of noisy real world audio from a mix of public and internal sources.
- Accented speech: We used datasets with French, Spanish, British, Indian, and African American accented English.


- French: a 3 hour human-labeled private dataset with short-form speech.
- Spanish: a 3 hour human-labeled private dataset with short-form speech.
- British: a 4 hour human-labeled private dataset with short-form speech.
- Indian: a 5 hour human-labeled private dataset with short-form speech.
- African American: a 15 hour sample from a public CORAAL dataset downloaded from[official sources](http://lingtools.uoregon.edu/coraal/) , containing long-form speech.


- Proper nouns: We used a 10 hour dataset created by sampling audio clips with a high frequency of proper nouns and subsequently transcribing them with a human transcription service.
- Alphanumerics: We used a 10 hour dataset created by sampling audio clips rich in alphanumeric content and subsequently transcribing them with a human transcription service.


###### Text Formatting Datasets


- SummScreen: We used the test split of the SummScreen text only abstractive screenplay summarization dataset from the official[GitHub repository](https://github.com/mingdachen/SummScreen) .
- DialogSum: We used the test split of the DialogSum text only dialogue summarization dataset from the official[GitHub repository](https://github.com/cylnlp/dialogsum) .
- EuroParl: We used the English test split of the Europarl text only dataset from the[official website](https://www.statmt.org/europarl/) . Europarl is a collection corpus from the proceedings of the European Parliament.
- MeetingBank: We used the test split of the MeetingBank text only dataset from the[official website](https://meetingbank.github.io/dataset/) .
- AMI: We used the AMI dataset downloaded from the[official website](https://groups.inf.ed.ac.uk/ami/corpus/) . We made use only of the transcripts for evaluating text formatting models.
- Rev16: We followed the dataset creation procedure from[Whisper](https://github.com/openai/whisper/blob/main/data/README.md#rev16) .
- SPGI: We used the English Test Set of SPGI from the[official website](https://datasets.kensho.com/datasets/spgispeech) .
- Proper Noun: We used a 10 hour human-labeled private dataset of proper noun rich real-world audio from a mix of public and internal sources.


###### Timestamp Datasets


- We used a combination of our internal ASR evaluation datasets (e.g. podcast, broadcast, telephony, etc) and a high fidelity forced alignment algorithm to create word-level reference timestamps.


## Evaluation Metrics


For an overview of the philosophy and methodology of properly evaluating Speech-to-Text systems, please reference our blog post:[How to evaluate Speech Recognition models](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/) .


###### ASR Metrics


- WER (Word Error Rate): A commonly used metric for assessing ASR performance, measuring the degree of discrepancy between ASR outputs and ground-truth transcripts.
- Proper Noun Error Rate (PNER): A metric calculating the Jaro-Winkler distance between ASR outputs and ground-truth transcripts, focusing on proper nouns extracted from both texts.[\[link\]](https://www.assemblyai.com/blog/how-to-evaluate-speech-recognition-models/#proper-noun-evaluation)


###### Text Formatting Metrics


- F-WER (Formatted Word Error Rate): The word error rate calculated directly on formatted texts as they are produced by the full end-to-end ASR service. This metric accounts for all the errors propagated by all tasks of an e2e system (i.e. ASR, Punctuation restoration, Truecasing, ITN).
- U-WER (Unpunctuated Word Error Rate): The word error rate computed over formatted outputs from which punctuation marks are deleted. This metric focuses on Truecasing and ITN directly, besides ASR mistakes.
- I-WER (ITN Word Error Rate): This metric accounts solely for Inverse Text Normalization errors. First, we remove all punctuation marks and lowercase both ground truth and predicted text. Then we locate the words which should be inverse normalized by aligning formatted ground truth with normalized ground truth texts. Next formatted ground truths are aligned with predicted texts, without any punctuation and casing. We only consider alignment of words at the indices where an inverse normalizable word was detected during the first alignment. After this, we simply calculate word error rate on words at these indices by totaling the number of substitutions, insertions and deletions, which is divided by the total number of words that should be inverse normalized.
- M-WER (Mixed-case Word Error Rate): Word Error Rate calculated only on the words that are mixed-case in the ground-truth transcripts.
- PER (Punctuation Error Rate). The word error rate computed over punctuation marks only, based on the proposal in \[4\].
- CSER (Casing Error Rate): Character Error Rate computed without considering punctuation marks.
- <LABEL> F1: The token-level F1 scores computed for each label predicted by the post-punctuation and casing heads of the multi-objective model, where LABEL represents one of the following categories: PERIOD, COMMA, QUESTION, LOWER, ALLCAPS, CAPITAL. .


###### Timestamp Metrics


- Word Timestamp Estimation Accuracy: Measures the percentage of predicted word-level timestamps (start and end of word) that fall within certain millisecond thresholds of actual ground truth timestamps labeled using a mix of forced alignment and manual heuristics.


## Credits


###### Core Research


Luka Chkhetiani (lead), Andrea Vanzo (lead), Yash Khare, Taufiquzzaman Peyash, Ilya Sklyar


###### Research Contributors


Michael Liang, Rami Botros, Ruben Bousbib


###### Research Data


Ahmed Etefy


###### Benchmarking


Pegah Ghahremani, Gabriel Oexle, Jaime Lorenzo Trueba


###### Research Infrastructure


William Pipsico Ferreira


###### Production Engineering


Ben Gotthold, Soheyl Bahadoori, Mimi Chiang, Aleksandar Mitov, Enver Fakhan


###### Technical Leadership


Takuya Yoshioka, Travis Kupsche


###### Technical Writing


Ryan O’Connor


###### Quality Assurance


Rajpreet Thethy (lead), Sergio Ramirez Martin


## References


##### References


[\[1\] Rnn-Transducer with Stateless Prediction Network.](https://ieeexplore.ieee.org/document/9054419)


[‍](https://ieeexplore.ieee.org/document/9054419)[\[2\] Efficient Sequence Transduction by Jointly Predicting Tokens and Durations.](https://arxiv.org/abs/2304.06795)


[‍](https://arxiv.org/abs/2304.06795)[\[3\] Anatomy of Industrial Scale Multilingual ASR.](https://arxiv.org/abs/2404.09841)


[‍](https://arxiv.org/abs/2404.09841)[\[4\] Meister, A., Novikov, M., Karpov, N., Bakhturina, E., Lavrukhin, V., & Ginsburg, B. (2023, December). LibriSpeech-PC: Benchmark for Evaluation of Punctuation and Capitalization Capabilities of end-to-end ASR Models. In 2023 IEEE Automatic Speech Recognition and Understanding Workshop (ASRU) (pp. 1-7). IEEE.](https://arxiv.org/pdf/2310.02943)
