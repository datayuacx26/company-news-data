---
schema_version: "1.0.0"
document_id: "294ca0ad1cce1f6292f82dbd5adea031164b31009bae4825090ef5659f03bb5e"
company_key: "yc-invert"
company: "Invert"
source_id: "yc-invert-news-import-968576ef11e8"
canonical_url: "https://invertbio.com/blog/connecting-shake-flask-to-final-product-with-lineage-views-in-invert"
published_at: "2025-11-11T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:27.297188+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:d08e1a7b79e0b32f1c61775f1fabb630a4bc74b02134bc1a57d8a39bc5bac102"
---

# Connecting Shake Flask to Final Product with Lineage Views in Invert

Every molecule of a product matters in bioprocess. Questions such as “where are we losing over 15% of our product?” or “what is the cumulative yield between harvest and purification?” should be both simple and immediate to answer—but the reality is often far messier.


While products move through successive steps, the data produced at each step does not necessarily follow along. Data from every unit operation is often siloed and disconnected, especially between upstream and downstream process development. Connecting data between fragmented systems is manual and time-consuming. Analytical results might be stored on an ELN or LIMS, while time series data might be siloed in system-specific software.


Exporting, aggregating, cleaning, and combining this data to make sense of it could take days or even weeks—and there would be no guarantee that relevant insights would be accessible to different teams. Without insight into previous experimental results and conditions, many points of potential process optimization are left untouched, and anomalies could escape detection until CQAs are significantly impacted.


## Link process parameters to outcomes


Invert’s lineage view connects products across every unit operation and material transfer throughout the entire process. It acts as a family tree for your product, tracing its origins back through purification, fermentation, and inoculation. Instead of manually tracking down the source of each data point, lineages automatically show material streams as they pass through each step.


Lineage view showing material streams through centrifugation, fitration, and chromatography


Beyond tracing the genealogy of a given product, lineage view also automatically calculates and displays:


- **Product yields and recovery rates** at each process step
- **Material losses** and where they occur
- **Mass balance closures** across unit operations


Run summary view showing lineage, KPIs, and Invert-Assist generated observations and notes about expected mass balances.


## Flagging anomalies and potential waste


Mass closures allow you to reconcile the amounts of starting material with the material recovered. These values are crucial for process consistency and understanding, as well as for regulatory compliance. However, manual calculations are tedious and error-prone. They might involve multiple unit conversions or tracking separate material streams, such as pooling supernatant into a single column, or splitting harvests into multiple purification runs.


With Invert’s lineage view, all these calculations are done automatically, ensuring that anomalies are easily flagged. For example, a mass balance closure of only 85% after a purification step would highlight unaccounted loss or documented waste and could be addressed immediately, instead of only after batch review weeks later.


## Finding hidden losses


One of the most powerful applications of lineage tracking is loss analysis. When overall process yields are lower than expected, pinpointing where and why the problem is occurring is can be a challenge. Scouring the data from different unit operations for irregularities might involve weeks of checking calculations, normalizing data, and troubleshooting results.


With Invert’s lineage view, you can simply view the entire cascade of events to identify the areas of unusual product loss. Being able to narrow down the potential source of loss to chromatography, for example, could lead you to check if the column was overloaded, or if sample pH needs to be adjusted—a much more directed course of action compared to troubleshooting every unit operation step by step.


## Strengthening regulatory efficiency


Beyond operational efficiency, lineage tracking strengthens regulatory compliance:


- **Complete traceability** : Demonstrate chain of custody from raw materials to final product
- **Data integrity** : Automated calculations eliminate transcription errors
- **Deviation investigation** : Assess impact when something goes wrong
- **Process validation** : Build robust mass balance data across multiple batches
- **Tech transfer** : Clear process understanding facilitates site-to-site transfer


## Get a full picture of your process


Invert’s lineage view connects processes from shake flask to final product, calculating yields, losses, and mass closures across upstream and downstream process. It gives scientists the insights to troubleshoot efficiently, optimize for the highest impacts, and trace deviations back to their source.


By supporting process understanding, operational rigor, and regulatory compliance, Invert’s lineage gives you the full picture—the experimental context, visibility, and traceability necessary to improve processes with confidence.


‍
