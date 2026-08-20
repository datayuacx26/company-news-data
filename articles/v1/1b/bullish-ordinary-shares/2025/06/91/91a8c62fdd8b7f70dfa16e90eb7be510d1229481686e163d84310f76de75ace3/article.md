---
schema_version: "1.0.0"
document_id: "91a8c62fdd8b7f70dfa16e90eb7be510d1229481686e163d84310f76de75ace3"
company_key: "bullish-ordinary-shares"
company: "Bullish"
source_id: "bullish-ordinary-shares-rss-b348dbb1f0cd"
canonical_url: "https://medium.com/bullish-engineering/time-series-foundation-models-forecasting-crypto-token-prices-7409e9d0db20"
published_at: "2025-06-23T08:26:03+00:00"
first_seen_at: "2026-07-20T04:36:16.773580+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:38797baf4ff5a5731156cf8a7597f9325ca791451d32682d8cbbf29528967636"
---

# AI Time Series Foundation Models: Forecasting crypto token prices

# AI Time Series Foundation Models: Forecasting crypto token prices


[RomainTokyo](https://medium.com/@romain.deborne?source=post_page---byline--7409e9d0db20---------------------------------------)


6 min read


·


Jun 23, 2025


--


The remarkable success of Large Language Models (LLMs) like ChatGPT, powered by the Attention architecture (see[Attention Is All You Need](https://arxiv.org/pdf/1706.03762) *)* , has highlighted the immense potential of[foundation models](https://nixtlaverse.nixtla.io/nixtla/docs/getting-started/glossary.html#foundation-model) to adapt to diverse tasks with minimal retraining. This raises a fascinating question: Can we develop similar foundation models for time series forecasting that rival the capabilities seen in natural language processing?


## From LLMs to Time Series: A New Frontier


While LLMs excel at understanding and generating human language, directly applying them to time series forecasting presents unique challenges. Time series data possesses inherent sequential order, temporal dependencies, and often complex non-linear patterns that traditional forecasting methods have struggled with. These methods also typically require extensive manual tuning and face scalability issues across diverse datasets.[TimeGPT](https://arxiv.org/pdf/2310.03589) aims to address these limitations by exploring whether a “foundation model” approach, analogous to LLMs, can be successfully applied to time series data.


## Introducing TimeGPT: A Foundation Model for Time Series Forecasting


[TimeGPT](https://arxiv.org/pdf/2310.03589) , developed by Azul Garza, Cristian Challu, and Max Mergenthaler-Canseco from[Nixtla](https://www.nixtla.io/) , marks a significant advancement in this field. It functions as a foundational model for time series forecasting, designed to generalize across a wide variety of tasks without needing to be retrained for each specific one.


This capability stems from its massive-scale training and transfer learning approach. TimeGPT was trained on an immense dataset of 100 billion data points from a diverse collection of public datasets. These datasets spanned numerous domains, including economics, healthcare, weather, IoT sensor data, and finance. This extensive and varied training allows the model to learn a rich “vocabulary” of time series patterns, such as multiple seasonalities, cycles of different lengths, evolving trends, noise, and outliers. Through transfer learning, the model gains knowledge from this vast dataset that it can then apply to new, unseen tasks.


TimeGPT’s advanced capabilities enable “zero-shot inference,” meaning it can forecast previously unseen data by utilizing knowledge acquired during its pre-training phase. This eliminates the need for task-specific fine-tuning and allows it to generalize successfully across domains not encountered during its initial training.


## The Architecture of TimeGPT


TimeGPT is meticulously designed to handle time series data with varying frequencies and characteristics, accommodating diverse input dimensions and forecasting horizons. Its core is rooted in the Transformer model, widely recognized for its excellence in sequence-to-sequence tasks. This architecture has been specifically adapted for time series data, comprising three primary components that process information sequentially to analyze and forecast temporal patterns effectively:


**Positional Encoding Layer:** This crucial initial step involves applying positional encoding to the input time series data, often after an initial normalization. This embeds timing information into the data, ensuring the model preserves the sequential order of observations, which is fundamental for understanding temporal dependencies.


**Encoder-Decoder Layer:**


- **Encoder:** The encoded data passes through an encoder consisting of multiple layers. Each layer incorporates self-attention mechanisms, residual connections, and layer normalization. The self-attention layers are particularly effective at capturing both short-term fluctuations and long-term trends in the data.
- **Decoder:** The decoder takes the processed information from the encoder and refines it to generate predictions. This structure is adept at identifying complex patterns and dependencies within the time series data and maintains temporal coherence in the forecasts. Residual connections and layer normalization within each layer stabilize the training process, enabling the model to be deep enough to identify highly complex patterns and dependencies.


**Linear Projection Layer:** The final step involves mapping the decoder’s output to the dimensions of the forecasting window using a linear projection layer. This translates the learned patterns into the final forecasted values.


## Forecasting Crypto Token Prices with TimeGPT


[CoinDesk](https://developers.coindesk.com/) , in partnership with Nixtla, conducted preliminary research to explore whether the TimeGPT model could be fine-tuned to better forecast crypto token prices. This model referred to as **TimeGPT Fine-Tuned** was adapted for crypto market characteristics, resulting in a forecasting solution designed to address the volatility patterns and market behaviors associated with crypto market trading.


The fine-tuning mechanism operates through two primary control parameters: **finetune_steps** and **finetune_depth** . These parameters precisely calibrate both the scope and intensity of the model’s adaptation to new data characteristics.


The model was trained on[CoinDesk Data’s](https://developers.coindesk.com/documentation/data-api/introduction) vast historical price datasets, which include full aggregate and trade-level history for more than 10,000 coins and 300,000 crypto and fiat trading pairs on a daily, hourly, and minute-by-minute basis, dating back to 2010. The research focused on the top contributing tokens to the[CoinDesk 20](https://indices.coindesk.com/coindesk20) index, specifically Bitcoin (BTC), Ethereum (ETH), Cardano (ADA), Ripple (XRP), and Solana (SOL), as well as the[CoinDesk 20](https://www.coindesk.com/price/cd20) index itself. This minute-by-minute data was meticulously gathered over the past four years through December 2024. This substantial dataset, approximately 2.8 GB and housing over 40 million data points, was essential in fine-tuning TimeGPT, with all training conducted efficiently on a single NVIDIA A100 GPU using[Google’s GPU Compute Engine](https://cloud.google.com/compute/docs/gpus) .


## Preliminary Results for BTC


Statistical cross-validation on a 60-minute window was performed, involving dividing time-series data into one-hour segments, with some segments used for testing the models. The performance of TimeGPT Fine-Tuned was evaluated against several other time series forecasting models, including[AutoArima](https://nixtlaverse.nixtla.io/statsforecast/docs/models/autoarima.html) ,[DOTheta](https://nixtlaverse.nixtla.io/statsforecast/docs/models/dynamicoptimizedtheta.html) , TimeGPT (baseline), and[AUTOCES](https://nixtlaverse.nixtla.io/statsforecast/docs/models/autoces.html) .


Press enter or click to view image in full size


Figure 1: Cross-validation with 60-minutes look-ahead time


The accuracy of these models was measured using standard error metrics such as Mean Absolute Error (MAE) and Root Mean Square Error (RMSE). Additionally, how well each model predicts the directional movement of Bitcoin (BTC) price over 60-minute rolling periods was assessed, expressed as a percentage directional accuracy.


- **Direction Errors Comparison:** TimeGPT Fine-Tuned shows a significantly lower percentage of direction errors compared to AutoARIMA, DOTheta, TimeGPT (baseline), and AutoCES. This indicates improved directional accuracy.
- **MAE Comparison:** TimeGPT Fine-Tuned demonstrates a lower Mean Absolute Error compared to TimeGPT (baseline) and AutoCES, and is comparable to AutoARIMA and DOTheta, suggesting better overall accuracy in its predictions.
- **RMSE Comparison:** TimeGPT Fine-Tuned exhibits a lower Root Mean Square Error than TimeGPT (baseline) and AutoCES, and is in a similar range to AutoARIMA and DOTheta, further supporting its improved predictive performance.


These preliminary results suggest that the TimeGPT Fine-Tuned model outperforms the general TimeGPT model and other traditional forecasting models in terms of both directional accuracy and error metrics for Bitcoin (BTC) price forecasting.


Press enter or click to view image in full size


Table 1: Error metrics comparison across models


## Summary


Given the promising results we’ve seen with TimeGPT’s ability to forecast crypto token prices, our next steps are to make the model even more precise and to broaden its coverage across a wider range of crypto tokens. The goal is to develop a comprehensive forecasting solution that can be integrated directly into CoinDesk’s products.


Feeling inspired by the cutting-edge work we’re doing at Bullish? We’re always on the lookout for talented individuals to join our team.[Explore our open roles](https://bullish.com/careers/) and be Bullish on your career. Want to stay up to date with the latest news from across Bullish? Follow us on[LinkedIn](https://www.linkedin.com/company/bebullish) and[X](https://twitter.com/Bullish) .


*Disclaimer: This material, including any material accessed through embedded links (“Information”) is for general informational purposes only and is not intended to constitute investment, tax, accounting, legal or financial advice. This Information is not an offer to buy or sell or a solicitation of an offer or to buy or sell any particular asset or to provide financial services of any kind. You should consider your own personal situation carefully and consult your professional advisors before making any investment decision.*


*Bullish makes no representation as to the accuracy, completeness, timeliness, suitability, or validity of any Information. The products, services, and solutions discussed in the Information are likely to change, so the Information may become outdated, incorrect or incomplete. Bullish is under no obligation to update Information. Bullish will not be liable for any errors or omissions in the Information. Virtual assets and related products are high risk. Access to certain assets or services referred to in the Information may not be available in your jurisdiction or to all types of investors. Information may reference the Bullish Exchange, which is licensed by the Gibraltar Financial Services Commission (DLT license: FSC1038FSA). This Information may not be quoted, deleted, or modified in any way without Bullish’s prior written consent. All rights reserved. Bullish and the Bullish Logo are trademarks of Bullish Global.*
