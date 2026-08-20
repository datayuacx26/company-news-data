---
schema_version: "1.0.0"
document_id: "ed6056fb6015205989d5682bdf4440c930f831d00c5de5508e93c9b35681c46c"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-55baf9c2c2ac"
canonical_url: "https://www.microsoft.com/en-us/research/blog/aurora-1-5-extending-open-foundation-models-for-weather-and-earth-system-applications/"
published_at: "2026-07-09T16:46:22+00:00"
first_seen_at: "2026-07-20T04:34:28.884326+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:7b37d7984ab3694c689b9b0115bc22cb18403a203719f4e41e75ec36307bf1e3"
---

# Aurora 1.5: Extending open foundation models for weather and Earth-system applications

## At a glance


- Aurora 1.5 is a major extension of Microsoft’s Aurora Earth System foundation model that adds 22 more weather variables relevant to energy, agriculture, transport, and climate risk, along with hourly temporal resolution and probabilistic ensemble forecasting.
- Released as open source on GitHub with model checkpoints on Hugging Face, Aurora 1.5 enables researchers and developers to use, evaluate, and build on the model.
- Aurora 1.5 connects open research to Microsoft Weather services, linking the model with data, infrastructure, managed access, and operational use for weather and Earth-system applications.


Aurora 1.5 is a major update to the open Aurora Earth-system foundation model, adding 22 new weather variables for a broader view of atmospheric conditions, hourly forecasts, and probabilistic ensemble forecasting. Developed by Microsoft Weather as an extension of the original model from Microsoft Research AI for Science, Aurora 1.5 shows how frontier research can move into broader use: open for researchers and developers to evaluate and extend, and designed to support customers where additional data, infrastructure, and operational assurance is needed. As climate and weather-related risks continue to affect communities, infrastructure, and economies worldwide, advances in Earth-system forecasting can help improve preparedness and decision-making.


## What is Aurora?


Aurora is a foundation model for the Earth system developed by Microsoft Research AI for Science, first introduced in 2024 and[published in Nature (opens in new tab)](https://www.nature.com/articles/s41586-025-09005-y) in 2025. It showed that a single model could be adapted to medium-range weather, ocean waves, atmospheric chemistry, and emerging climate applications, including high-resolution weather forecasting through fine-tuning. Its growing use has reinforced the value of an open, collaborative model that is easier to adapt, evaluate, and put to use.


This[next phase of Aurora (opens in new tab)](https://www.bing.com/ck/a?!&&p=f9c93e7b19f62b3737c7c3282badddf1233badf5058fb7d7861ef84db05d08e0JmltdHM9MTc4MjQzMjAwMA&ptn=3&ver=2&hsh=4&fclid=24451b10-f799-6468-1027-0c47f6ba6571&psq=microsoft+aurora+ai+weather+2024&u=a1aHR0cHM6Ly9ibG9ncy5taWNyb3NvZnQuY29tL29uLXRoZS1pc3N1ZXMvMjAyNS8xMS8xMy90aGUtbmV4dC1waGFzZS1vZi1hdXJvcmEtb3Blbi1hbmQtY29sbGFib3JhdGl2ZS1haS1mb3Itd2VhdGhlci1hbmQtY2xpbWF0ZS1mb3JlY2FzdGluZy8) builds on that foundation by making the model openly available for the global community to adapt, extend, and build on.


## What is new in Aurora 1.5?


Aurora 1.5 advances the broader effort to make open weather foundation models practical and scalable for organizations that rely on atmospheric and Earth-system intelligence. Alongside new variables and higher temporal resolution, Aurora 1.5 adds one of the most requested capabilities from users: ensemble forecasting. Because forecasts are sensitive to initial conditions and model uncertainty, ensembles run multiple simulations to show the range and likelihood of possible outcomes. Aurora 1.5 builds on Microsoft Research’s scientific foundation with new product engineering, cloud infrastructure, managed access, and decision-support capabilities. Together, these advances make Aurora 1.5 a valuable enterprise-grade weather solution for organizations.


Figure 1: Illustration of the capabilities of Aurora 1.5 ensemble for predicting new impactful parameters such as total cloud cover and solar radiation. Ensemble mean and standard deviation are shown **.**


The breadth update adds 22 new variables to Aurora’s original 4, including representative surface, pressure-level, wind, temperature, humidity, precipitation, and radiation fields. That broader coverage makes the model more relevant for sectors that depend on integrated Earth-system signals, from energy and agriculture to transport and resilience planning.


The update to hourly temporal resolution enables fine-grained detail for precision operational guidance, such as the onset of precipitation, trade decisions, or a landfalling tropical cyclone.


> *“Aurora 1.5 is a meaningful step toward making weather foundation models more open, useful, and practical. By releasing the model openly, we give researchers, developers, and organizations a clearer path to evaluate it, adapt it, and understand where it can help. Microsoft Weather’s role is to connect that open research foundation with the data, infrastructure, and applied workflows required by enterprises to use weather intelligence responsibly and with confidence.”*
>
>
> **Sridhar Iyer, Corporate Vice President, Microsoft AI**


PODCAST SERIES


## The AI Revolution in Medicine, Revisited


Join Microsoft’s Peter Lee on a journey to discover how AI is impacting healthcare and what it means for the future of medicine.


[Listen now](https://www.microsoft.com/en-us/research/story/the-ai-revolution-in-medicine-revisited/)


Opens in a new tab


## Ensemble Forecasting in Aurora 1.5 Unlocks More Confident Decisions in the Face of Weather Uncertainty


The ensemble version of Aurora 1.5 introduces stochastic perturbations to represent model uncertainty, allowing the generation of multiple forecast members to estimate the spread of possible futures. For a multitude of applications including power systems, transport, agriculture, extreme-weather planning, and climate risk, the model distribution matters as much as the best estimate.


This ensemble capability was developed through multi-stage fine-tuning on top of the original Aurora model. After expanding the variable set and adding hourly temporal resolution, the team introduced controlled perturbations into the model’s latent conditioning pathway and optimized the ensemble for probabilistic forecast quality. A final round of auto-regressive fine-tuning on ECMWF High Resolution (HRES) analysis data from 2018 to 2023 improved rollout behavior and stability.


Figure 2. Comparing Aurora 1.5’s probabilistic forecasts with the ECMWF ensemble forecast. The shading shows relative probabilistic forecast error, using ECMWF ENS as the baseline: blue areas indicate where Aurora 1.5 performs better, and red areas indicate where it performs worse. Across upper-air geopotential, temperature, and humidity, together with five surface variables, Aurora 1.5 outperforms ECMWF ENS on 88.9% of the evaluated variable-and-lead-time targets.


Aurora’s ensemble approach summarizes uncertainty across multiple model runs. Its probabilistic forecasts outperform those of the state-of-the-art ECWMF dynamical ensemble on 88.9% of evaluated targets (Figure 1). In evaluations on all 2024–2025 tropical cyclones, Aurora 1.5 substantially reduced track errors, including roughly one-third lower track error when comparing the ensemble median to the original Aurora. An example for the devastating Hurricane Helene shows how Aurora 1.5’s skill translates to high-impact weather applications.


Figure 3. Hurricane Helene ensemble forecast from Aurora 1.5, showing multiple plausible storm tracks starting at 0 UTC on September 24, 2024. The probabilistic ensemble forecast envelops the verified track, effectively capturing uncertainty in the storm’s progression. Figure 4. Aurora 1.5 reduces track error relative to the original model across lead times. Ensemble mean and median tracks are used for diagnostics, with the median showing the strongest gains, reaching roughly one-third lower error by day 5. Results reflect track position only.


## Beyond weather: Aurora as an Earth-system foundation


Beyond medium-range weather applications, Terradot – part of the Microsoft Climate Innovation Fund portfolio—is working with the[AI for Good Lab (opens in new tab)](https://iclr.cc/virtual/2026/10014507) and the Microsoft Research Accelerator on[TerraNova, using Aurora-derived weather representations (opens in new tab)](https://iclr.cc/virtual/2026/10014507) to estimate and optimize carbon dioxide removal from enhanced rock weathering under real field conditions. Sasankh Munukutla, Co-Founder of Terradot, highlights *, “By building on Aurora, we’re significantly advancing our R&D timelines and accelerating our path towards gigaton-scale carbon removal.”* This work shows how Earth-system foundation models can support climate mitigation and public-interest science beyond forecasting, including settings where rigorous evaluation and responsible deployment matter.


Aurora is also being explored with partners such as the UK Met Office, exploring how foundation models can work alongside established physics-based systems to tackle problems from weather to climate time scales. The aim is faster, more flexible forecasts that support decision-making without replacing the science behind trusted prediction.


> *“Microsoft’s Aurora model is an exciting and promising tool, enabling Met Office scientists to bring their data and expertise to help solve climate problems and provide new kinds of climate information. Met Office and Microsoft scientists and engineers are working together every day to translate lessons from AI weather prediction into the climate information space, sharing expertise in data science and climate science. Aurora is a great platform for learning how to translate these tools for use in climate projection to make the AI climate models of the future.”*
>
>
> — Doug McNeall, Science lead for Data-Driven Climate Modelling, Met Office Hadley Centre


## Connecting open models to operational use


Microsoft connects open research, product engineering, responsible deployment, and partner ecosystems so that models can move from scientific advance to evaluated operational use. As an example, Aurora began in Microsoft Research AI for Science and is now being built on for operational use by Microsoft Weather, with AI for Good helping to evaluate public-interest applications. The platform path brings[Aurora into Microsoft Foundry and Planetary Computer Pro](https://www.microsoft.com/en/customers/story/26785-bkw-fmb-energie-ag-foundry-models) , alongside Agent skills and Azure services that connect models with geospatial data, scalable infrastructure, and applied workflows.[BKW provides an early proof point](https://www.microsoft.com/en/customers/story/26785-bkw-fmb-energie-ag-foundry-models) : the company is using Aurora 1.5 alongside existing operational Microsoft Weather models to support energy operations where weather-dependent generation, infrastructure planning, and environmental data need to come together.


> *“This collaboration demonstrates how advanced AI capabilities and robust cloud infrastructure can be applied to one of the most strategic domains — energy, where weather plays a fundamental role. In a time of accelerated transformation, it supports our ambition to operate increasingly renewable-based systems, where generation is inherently weather-dependent, and to better anticipate and manage this variability with greater confidence and precision.”*
>
>
> Farhat Quiñones Yamshid, Lead, AI and Technology, BKW


## From open research to broader impact


Aurora’s open-source availability is intended to help researchers, agencies, companies, and civil society evaluate, apply, and extend the model. Microsoft Weather is building on that open foundation to deliver easier access to Aurora forecasts through managed services, integrations, and responsible deployment paths for organizations that depend on weather and Earth-system intelligence.


Foundation models should complement—not replace—physics-based models and domain expertise. The opportunity is to use them responsibly, with careful evaluation and transparency, and to invite researchers, agencies, companies, and public-interest partners to test where Aurora and related Microsoft Weather capabilities can improve forecasting, planning, and climate resilience in their own settings.


## About Microsoft Weather


Microsoft Weather is the AI-based forecasting team behind weather experiences across Windows, Bing, Copilot, Edge, and MSN, reaching more than a billion devices across 180 countries. The team has been applying AI to operational weather forecasting for more than seven years and has built a proven track record of delivering high-quality forecasts at global scale. Microsoft Weather has won multiple forecasting competitions and was ranked the world’s most accurate global forecast provider by an independent third party for three consecutive years from 2022 to 2024. Building on today’s Aurora 1.5 announcement, the team plans to extend this work in the coming months with additional fit-for-purpose AI weather models designed for enterprise scenarios where forecast quality, speed, uncertainty, and operational decision support matter most.


If you are interested in exploring Aurora and Microsoft Weather solutions for commercial or organizational applications, please contact us atAIWeatherClimate@microsoft.com


[Aurora 1.5 on Microsoft Foundry](https://ai.azure.com/catalog/models/Aurora-1.5)


[Aurora 1.5 on GitHub](https://github.com/microsoft/aurora)


[Aurora 1.5 paper](https://www.microsoft.com/en-us/research/publication/aurora-1-5-fine-tuning-a-foundation-model-for-medium-range-ensemble-weather-prediction/)


[Agent Skills for adapting Aurora to new applications](https://github.com/microsoft/vibe-kit/tree/main/skills/msresearch-aurora)


Opens in a new tab
