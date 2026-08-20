---
schema_version: "1.0.0"
document_id: "4c65da43c5b47f92aa90de74a8fd24c960fc996a1a6daffd5f11f3fcc2bef0a8"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/chart-component"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:c6944f9277458d2c8371c5f7927198c8954202e04cdd5e60261b5ad18fd495f5"
---

# New Chart Component

Numbers buried in a paragraph are easy to skim past. A trend line is hard to ignore.


Data is easier to understand when it's visual. That's why we've added a new Chart component to the email editor, available in both[Broadcasts](https://resend.com/features/broadcasts) and[Templates](https://resend.com/features/templates) .


You can now embed bar, line, and area charts directly in your emails.


## How it works


Insert a chart using the slash command menu (` /chart` ) or from the component toolbar. Once added, configure it from the inspector sidebar:


- **Chart type** : choose between bar, line, and area charts
- **Title** : add a label (also serves as the image alt text for accessibility)
- **Data** : enter values using a spreadsheet-style table or paste raw JSON
- **Series** : rename series and customize colors for each data set


As you add data, the editor generates the chart as an image that gets embedded in your email. While the chart will auto-generate on a schedule, you can click **Render chart** to update the image immediately.


## Theming


You can choose a theme for your chart from the inspector sidebar.


## Data sources


You can enter data using a spreadsheet-style table or paste raw JSON.


```text
[        {          "date"  :     "Feb '26"  ,          "downloads"  :     8942400        }  ,        {          "date"  :     "Mar '26"  ,          "downloads"  :     16492000        }  ,        {          "date"  :     "Apr '26"  ,          "downloads"  :     16036000        }     ]
```


## Built for the realities of email


Every email client runs its own rendering engine, so it's unwise to rely on anything beyond basic HTML.


The editor renders your data as images to ensure it looks the same in every email client. What you see in the editor is exactly what your subscribers see, whether they open in Gmail, Outlook, Apple Mail, or anywhere else.


## What's next


We want to continue improving the component based on user feedback. If you have suggestions or run into any issues, reach out to us on[support](https://resend.com/support) .
