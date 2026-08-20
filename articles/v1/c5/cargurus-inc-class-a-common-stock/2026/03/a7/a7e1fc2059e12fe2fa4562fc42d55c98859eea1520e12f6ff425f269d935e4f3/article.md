---
schema_version: "1.0.0"
document_id: "a7e1fc2059e12fe2fa4562fc42d55c98859eea1520e12f6ff425f269d935e4f3"
company_key: "cargurus-inc-class-a-common-stock"
company: "CarGurus Inc."
source_id: "cargurus-inc-class-a-common-stock-rss-ea326c83890f"
canonical_url: "https://cargurus.dev/2026/03/06/genstats-standardizing-experimentation-analysis-at-scale/"
published_at: "2026-03-06T17:58:39+00:00"
first_seen_at: "2026-07-20T03:33:13.583792+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:ac95f9f631e5431afc2309d23bfe8cb7bb74454ef7f2772c9f3bb8770cd091b7"
---

# genStats: Standardizing Experimentation Analysis at Scale

Authors: Mark Gordon and Naman Rathi


### **Problem**


CarGurus runs hundreds of A/B tests annually to improve every aspect of our marketplace. Because the car-buying journey is complex and touches every part of our platform, we validate product changes through qualitative insights (consumer interviews) and quantitative experimentation (A/B tests).


Over the past year, our experimentation workflow reached its breaking point, revealing critical limitations in three key areas:


- **Limits of BI tools:** Most BI tools are excellent for displaying and visualizing metrics, but not for analyzing them. It took us thousands of lines of code to implement basic stats tests (t-tests, confidence intervals, power calculations, etc). For any other deeper analysis, we had to rely on other tools like Jupyter Notebooks or Excel sheets.
- **The Cross-Domain Silo:** As our marketplace grew, experiments began to span multiple product domains (e.g., Search influencing Financing). Because metrics and data pipelines are organized by domain, analyzing these experiments required combining outputs from multiple SQL queries and metric definitions. This fragmentation slowed experimentation velocity and increased overhead. Analysts would spend significant time coordinating across teams, tracking dependencies, and sourcing metrics, rather than focusing on test analysis.
- **Tool Fragmentation:** With basics in our BI tool, deep analysis in Jupyter notebooks, and detailed write-ups in Jira, we lacked a single source of truth. Context was scattered, methods drifted, and revisiting old experiments became a forensic exercise.


We needed a system built for **analysis** , not just visualization—a consistent, reproducible workflow that let analysts spend time interpreting results instead of stitching together tools.


### **Solution**


genStats is an internal framework orchestrated in Python that standardizes A/B test analysis across CarGurus. It automates notebook generation, centralizes statistical logic, and packages domain-specific metrics into reusable “metric families,” addressing the limitations listed above. A Metric Family is a collection of standardized queries. Analysts contribute the queries for the metrics they specifically own. This approach **codifies domain expertise** , turning individual knowledge into a shared tool.


Now, an analyst can use trusted metrics from other teams without having to learn a completely new domain. This ensures that every experiment—regardless of who runs it—is consistent, reproducible, and high-quality.


Analysts start by using a simple CLI command to generate a new analysis notebook. They input core test metadata (ticket ID, name, dates, metric family, key dimensions). genStats automatically assembles a ready-to-run .ipynb notebook with all relevant SQL queries and standardized statistical calculations. This minimizes manual intervention, allowing analysts to immediately execute the notebook, review results, and focus on interpretation, not setup.


### **Infrastructure**


To support this new workflow, we built an experimentation framework designed to automate and standardize every step of the analysis process—from environment setup to statistical testing to report generation.


- **Managed, Reproducible Python Environment:** Powered by Poetry and Makefiles, every analyst installs the exact same dependencies (pandas, numpy, scipy, statsmodels, connectors) with a single command. This eliminates “works on my machine” issues and dependency drift.
- **Abstracted Data Access:** Analysts use a simple helper function for a secure data warehouse connection, abstracting away DSNs and secrets. This ensures a reliable, consistent data interface without custom setup.
- **Modular Notebook Construction:** Analysis is decomposed into version-controlled, reusable “cells” (SQL snippets, Python functions, markdown). Product areas (Search, VDP, Financing, Checkout) own their metric definitions and SQL within these cells. genStats dynamically assembles notebooks based on the selected metric family.
- **Advantages of Modularity:** Analysts automatically inherit correct cross-domain SQL/logic without needing to know underlying schemas. Methodological improvements or bug fixes update all future notebooks instantly.
- **Centralized Statistical Core:** All statistical calculations (z-tests, confidence intervals, MDE, power, guardrail checks) live in unified functions within the package. This ensures the methodology is consistent, versioned, reviewable, and easy to evolve.
- **Automated Report Generation:** Analysts can export their exploration notebooks to clean, code-hidden HTML or Markdown reports with a single command. This provides PMs with a polished, standardized artifact while analysts keep the full notebook for deeper dives.


### **Impact**


genStats standardized the analysis workflow, delivering three key results:


- **Speed:** Generating a cross-domain notebook now takes minutes instead of hours, removing the need to rebuild SQL for every test.
- **Consistency:** Centralized statistical logic eliminates variation in lift and significance calculations.
- **Reproducibility:** All code and reports are version-controlled, meaning any analysis can be audited or run by analysts (even without relevant domain knowledge).


This impact was especially visible during analyst onboarding. A recently hired Product Data Analyst summarized the change clearly:


*“genStats was absolutely helpful and made my work center more on actually analyzing the outputs rather than getting everything formatted correctly. At both of my previous jobs, there was always talk of finding a way to standardize the analytics process like this, but this is the first time I’ve seen it done successfully in a truly user-friendly way (and in a way that can be used seamlessly across product areas).”*


genStats reduced operational overhead, improved throughput, and gave analysts a consistent, scalable framework for experiment analysis across all product surfaces.


### Share this:


- [Share on X (Opens in new window) X](https://cargurus.dev/2026/03/06/genstats-standardizing-experimentation-analysis-at-scale/?share=twitter)
- [Share on Facebook (Opens in new window) Facebook](https://cargurus.dev/2026/03/06/genstats-standardizing-experimentation-analysis-at-scale/?share=facebook)
-


### Like this:


Like


Loading…
