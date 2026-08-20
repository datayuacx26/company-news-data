---
schema_version: "1.0.0"
document_id: "2d5a6a709a2939f7f26bb7d098b3aad99228f0db2bfe101b405c4b1c8a325f44"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-for-data-science"
published_at: "2026-06-06T12:48:09+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:cacf913a39d5433bc0c5c828a1b5a3330d418b889158bd6f4881054c20910078"
---

# Claude Code for Data Scientists: Python, Pandas, and ML Without the Boilerplate

## Building a Feature Engineering Pipeline


Feature engineering is where data scientists spend disproportionate time. Claude Code turns it into conversation.


**Prompt pattern:**


> "For this churn prediction model, engineer these features from the transactions DataFrame:
>
>
> - recency: days since last transaction
> - frequency: transaction count in last 90 days
> - monetary: total spend in last 90 days
> - avg_order_value: monetary / frequency
> - days_as_customer: days between first and last transaction Handle division by zero for avg_order_value."


Claude Code writes the Pandas code, runs it against your data, handles the edge cases, and returns a function you can reuse.


## Training and Evaluating Models


Claude Code can run a full model training loop:


```text
> Train a gradient boosted classifier on this dataset.
> Target column: 'churned'.
> Features: everything except 'customer_id' and 'churned'.
> Use 5-fold cross-validation.
> Report accuracy, precision, recall, and F1.
> Show me feature importances.


```


It writes the sklearn code, runs it, fixes import errors or shape mismatches, and returns the evaluation results. What used to be 30 minutes of boilerplate is 5 minutes of conversation.


## Debugging Notebook Errors


Jupyter notebook errors are uniquely annoying: stack traces that reference kernel internals, cell execution order issues, variable scope problems.


**Pattern:**


1. Copy the error from the notebook
2. Open Claude Code:` > This error occurred in my notebook:`
3. Paste the error and the relevant cell content
4. Claude Code diagnoses it ("The issue is that` df_merged` was created in a cell that ran before` df_left` was modified — re-run from cell 3")


The advantage over Stack Overflow: Claude Code can see your entire notebook if you want it to (` claude .` in the project directory gives it full context). It does not need to guess at your variable names and data shapes.


## Data Visualization


Claude Code generates visualization code and can iterate on it conversationally:


> "Plot the monthly revenue trend with a 3-month rolling average. Use Matplotlib. Make it clean — no grid lines, light grey bars for monthly, dark blue line for rolling average. Title: 'Monthly Revenue with 3-Month Rolling Average'."


It writes the Matplotlib code, runs it, and you see the output. "Actually, add a secondary y-axis for customer count." One more message; Claude Code adds it.


## The CLAUDE.md Setup for Data Science


A good CLAUDE.md for a data science project:


```text
# Data Science Project: [Project Name]


## Data
-   Raw data: data/raw/ (never modify)
-   Processed data: data/processed/
-   Models: models/
-   Reports: reports/


## Environment
-   Python 3.11, conda env: ds-project
-   Key packages: pandas, sklearn, matplotlib, seaborn, xgboost


## Conventions
-   All DataFrames use snake_case
-   Feature engineering functions in src/features.py
-   Model training scripts in src/models/
-   Notebooks are for EDA only — production code goes in src/


## Analysis conventions
-   Always check data shape before and after merges
-   Log null counts before and after cleaning
-   Use consistent random seeds (42)
```


With this context, Claude Code writes code that follows your project conventions from the first message.


## Build This Into Your Development Workflow


Building a data application alongside your analysis? Add Blink as your infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a data analytics dashboard that reads from the database, shows a revenue trend chart, and lets analysts filter by date range and region. Host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Claude Code runs in your terminal with access to your full compute. Large datasets that exceed what fits in the model's context window are handled through sampling, chunked processing, or file-based operations. Tell Claude Code the size of your dataset and it adjusts its approach accordingly.


Claude Code works from the terminal and edits .py files natively. For notebooks, the most effective pattern is: use notebooks for exploration and visualization, move production code to .py files where Claude Code can edit them directly. Claude Code can also read notebook .ipynb files.


Claude Code has strong coverage of pandas, numpy, scikit-learn, matplotlib, seaborn, xgboost, lightgbm, TensorFlow, PyTorch, and Hugging Face. It is aware of the 2026 versions and current best practices for each. If you encounter an edge case with a newer API, tell it the library version and it adjusts.


Copilot suggests the next line or block as you type — good for autocomplete. Claude Code takes on full tasks autonomously: "clean this dataset, train this model, debug this pipeline." They serve different workflows. Many data scientists use both: Copilot for interactive coding, Claude Code for autonomous tasks.


Be careful here. Use anonymized or sampled data for development. For production access, ensure your data governance policies allow this and configure Claude Code's file system access appropriately. Claude Code's permission mode allows you to control exactly what it can read and modify.
