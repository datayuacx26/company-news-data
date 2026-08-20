---
schema_version: "1.0.0"
document_id: "0cd8ed1ed58ea8d90892fc29aabd79830228565a56b01206becec53787097d38"
company_key: "yc-keye"
company: "Keye"
source_id: "yc-keye-news-import-36b1eda744e0"
canonical_url: "https://www.keye.co/post/in-pe-99-accuracy-is-100-wrong"
published_at: null
first_seen_at: "2026-07-25T10:42:08.002984+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:34c0019ce8d446952fe5228d74684cc53c5d48f7b846eeb0419015dc98fa5587"
---

# In PE, 99% Accuracy Is 100% Wrong

When we launched[Keye](https://www.keye.co/post/keye-we-built-the-diligence-platform-we-wish-we-had) , we weren’t chasing a trend, we were solving a problem we had lived every day. Private Equity workflows demand speed, precision, and trust in every number, along with total transparency into how that number was derived. There is no “good enough” in this industry; there is only 100% accuracy or a deal at risk. That’s why we built Keye.


Over the past few months, there’s been a wave of tools wrapping LLMs (or OpenAI APIs) to work with Excel, especially for financial modeling. Some of the demos are slick. The marketing is strong. And honestly, it’s validating to see so many teams now tackling the same pain points we identified over 18 months ago. This space *is* desperately in need of better tools.


I am glad there are people joining our group in understanding that Private Equity folks don’t care that much about summarization, but their insights lie in numbers in Excel! We saw this need 18 months ago and began experimenting with LLMs for Excel: building retention curves, running complex models, segmentations, and more. We even got a lot of attention for[it](https://www.linkedin.com/posts/garrytan_keye-yc-f24-empowers-private-equity-investors-activity-7264776446697451521-YB6P?) . But when we tested the results we saw that LLMs worked … to an extent. We quickly ran into roadblocks: outputs weren’t deterministic, calculations could be off, and in private equity, there’s **zero tolerance for errors** , *like ZERO* ***.***


In Private Equity, that’s unacceptable. *One* wrong number can sink a deal. Flashy AI demos might impress on stage, but they fail in diligence.


Naturally, when these new tools came out, I thought *maybe they’ve cracked it* . Maybe we’ve all reached that 100% accuracy and fully assimilated to an investor workflow. So, I tested one.


Not to my surprise…it was still unreliable as eight months ago. A simple ask—segment data by industry—produced a wrong result initially and then the right one. Same file, two different results! Got me back to my September 2024 days!


**RESULT 1:**


**RESULT 2:**


Definitely impressive that all the formulas were included. But, if I had built my Excel like this just for a segment during my banking days, I wouldn’t have lasted long in the job. Again, two different sets of formulae for the same ask, the same data from the same tool!


In Private Equity, the accuracy of the answer matters, but the process to get there matters just as much. Every team experimenting with AI in this space will eventually hit the same roadblock we call the *determinism wall* — where the same input can yield different outputs, and even one wrong number can kill a deal. So, while other entrants in this space are just beginning to discover these challenges, we’ve spent the last 12–14 months building a platform that sidesteps them entirely:


- **Deterministic accuracy** (100% every time)
- **Domain-specific rules** shaped by years in the industry
- **Outputs that meet investor standards** , not just AI demo flash


We actively use AI in many processes, but not for complex calculations where we need nothing less than *100% accuracy* and complete determinism.


It’s exciting to see so many startups following the same path we started on 14 months ago. It validates the need and pushes the space forward. They’ll chart their own learning curves. My only advice: the determinism wall is real. And in our world, even one wrong number can kill a deal.


There are no shortcuts to building a great tool.


‍
