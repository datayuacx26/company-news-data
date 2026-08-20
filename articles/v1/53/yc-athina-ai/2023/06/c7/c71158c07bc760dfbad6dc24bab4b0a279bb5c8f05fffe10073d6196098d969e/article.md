---
schema_version: "1.0.0"
document_id: "c71158c07bc760dfbad6dc24bab4b0a279bb5c8f05fffe10073d6196098d969e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/temporal-data-meets-llm----explainable-financial-time-series-forecasting"
published_at: "2023-06-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:f055f78ca7a47c9a7d11bf8727eb4e2691b18cf4034ed959a090d631f044d821"
---

# Temporal Data Meets LLM -- Explainable Financial Time Series Forecasting

Do not index


Original Paper


[https://arxiv.org/abs/2306.11025](https://arxiv.org/abs/2306.11025)


Blog URL


[blog.athina.ai /tempora...ecasting](https://blog.athina.ai/temporal-data-meets-llm----explainable-financial-time-series-forecasting)


**Original Paper:**[https://arxiv.org/abs/2306.11025](https://arxiv.org/abs/2306.11025)


**By:**[Xinli Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20X) ,[Zheng Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20Z) ,[Yuan Ling](https://arxiv.org/search/cs?searchtype=author&query=Ling%2C%20Y) ,[Shujing Dong](https://arxiv.org/search/cs?searchtype=author&query=Dong%2C%20S) ,[Zongyi Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Z) ,[Yanbin Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20Y)


**Abstract:**


> This paper presents a novel study on harnessing Large Language Models' (LLMs) outstanding knowledge and reasoning abilities for explainable financial time series forecasting. The application of machine learning models to financial time series comes with several challenges, including the difficulty in cross-sequence reasoning and inference, the hurdle of incorporating multi-modal signals from historical news, financial knowledge graphs, etc., and the issue of interpreting and explaining the model results. In this paper, we focus on NASDAQ-100 stocks, making use of publicly accessible historical stock price data, company metadata, and historical economic/financial news. We conduct experiments to illustrate the potential of LLMs in offering a unified solution to the aforementioned challenges. Our experiments include trying zero-shot/few-shot inference with GPT-4 and instruction-based fine-tuning with a public LLM model Open LLaMA. We demonstrate our approach outperforms a few baselines, including the widely applied classic ARMA-GARCH model and a gradient-boosting tree model. Through the performance comparison results and a few examples, we find LLMs can make a well-thought decision by reasoning over information from both textual news and price time series and extracting insights, leveraging cross-sequence information, and utilizing the inherent knowledge embedded within the LLM. Additionally, we show that a publicly available LLM such as Open-LLaMA, after fine-tuning, can comprehend the instruction to generate explainable forecasts and achieve reasonable performance, albeit relatively inferior in comparison to GPT-4.


---


###


Summary Notes


####


Enhancing Financial Forecasts with Large Language Models (LLMs)


The financial world is constantly on the lookout for more accurate and understandable forecasting methods.


With the introduction of machine learning (ML) and artificial intelligence (AI), the prediction of market trends has seen significant improvements. However, the complexity of financial time series data often challenges traditional forecasting methods.


This is where Large Language Models (LLMs) like GPT-4 and Open LLaMA come into play, promising better accuracy and clarity in predictions.


This post focuses on how LLMs can revolutionize forecasting for NASDAQ-100 stocks.


####


Traditional Methods vs. LLMs


In the past, financial forecasts depended on statistical models (ARMA-GARCH) and ML techniques (decision trees, neural networks). Despite their effectiveness, these methods struggle with the dynamic nature of financial data. LLMs, however, excel in understanding complex data relationships and generating human-friendly outputs.


####


How LLMs Work in Financial Forecasting


LLMs are applied to financial forecasting through:


- **Zero/Few-Shot Learning** : Using models like GPT-4 to predict market trends with minimal training data.


- **Fine-Tuning Open LLaMA** : Adapting this model to improve forecast accuracy.


- **Chain-of-Thoughts (COT) Technique** : Enhancing predictions with step-by-step reasoning.


####


Experiment Results


Our experiments compared LLMs to traditional models using NASDAQ-100 stock data, focusing on weekly/monthly returns. We measured performance using precision metrics and mean squared error (MSE).


Key findings include:


- LLM methods, especially GPT-4 with COT, outperformed traditional models in accuracy and MSE.


- Fine-tuned Open LLaMA showed competitive but sometimes extreme predictions.


These results highlight LLMs' superiority in managing complex financial data.


####


Conclusion and Implications


Incorporating LLMs into financial forecasting marks a significant advancement by improving prediction accuracy and transparency.


This research paves the way for further studies, including exploring different stock indices and data types, and using larger models for fine-tuning. LLMs offer a valuable tool for financial analysts, promising more accurate and understandable forecasts.


####


Tips for AI Engineers


AI engineers in the finance industry can benefit from these insights:


- **Explore LLMs** : Start using LLMs like GPT-4 for financial forecasting. Their ability to process complex data and provide clear predictions can significantly impact your work.


- **Experiment with COT** : Using the Chain-of-Thought technique with LLMs can boost model performance. Trying different reasoning paths may improve accuracy.


- **Fine-Tune Strategically** : While pre-trained models are a good starting point, customizing them with your specific data is crucial. This tailoring can greatly enhance model performance.


####


Further Reading


The research paper offers detailed methodologies, experiments, and results, including prompt examples for GPT-4 and performance comparisons between LLMs and traditional models.


The references section is an excellent resource for those interested in both traditional financial forecasting and the latest in AI and ML research.


This exploration into using LLMs for financial time series forecasting sets a new standard for accuracy and explainability in the field, showcasing their potential to revolutionize financial predictions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
