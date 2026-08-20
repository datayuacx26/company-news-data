---
schema_version: "1.0.0"
document_id: "fd6667c0eba678769746bd00e61fc3088fd2f68d46fe13264fca0dc606fcb807"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/taimi-text-classification-case-study/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:42e19e716f84195474092452efc65f0809db5653a3257f44df48e27f674ca4ca"
---

# Taimi Cuts Content Moderation Time by 3x and Slashes Costs 10x Using Nyckel

[Taimi](https://taimi.com/) is an LGBTQ+ dating app, with over 16 million users on the platform. With a community as large as Taimi’s, content moderation is critical to ensure a positive user experience.


"\[Nyckel\] is significantly cheaper than manual moderation…9 or 10 times cheaper than using a \[moderation\] company in Europe or the US."


Vladislav Yavorskyi


Moderation Manager, Taimi


Taimi uses Nyckel text classification to moderate content across its platforms 3x reduction in content moderation time 4x increase in auto-moderation coverage 10x cost reduction compared to manual curation 96% accuracy for auto-moderation


## About Taimi


Taimi is a dating app for lesbian, gay, bi, trans, queer, and others who don’t identify as[cisgender and heterosexual](https://www.nyckel.com/pretrained-classifiers/gender-detector/) . The company built the app around the concept of dating fluidity, and users choose it because it offers them a queer-centric bio, authentic connections, a place to be themselves, and a strong commitment to harassment prevention.


## The Challenge


Taimi’s moderation team was up against a lot: they had thousands of images and pieces of text per day to moderate. At the same time, they needed to optimize their moderation tools.


Taimi needed a content moderation solution that was faster and higher quality than keyword-based moderation and would take into account the context of the content within the platform. The team recognized they needed machine learning (ML) to support their complex moderation needs.


## The Solution


Taimi’s Moderation Manager, Vladislav Yavorskyi, discovered Nyckel’s[content curation solutions](https://www.nyckel.com/custom-content-moderation-api) via an internet search for ML and AI solutions. Vladislav was immediately pleased with the simplicity and usability of Nyckel’s interface, and the ready-to-use free plan meant that Vladislav’s team could get started right away.


"The user interface is really ergonomic and user-friendly."


Vladislav Yavorskyi


Moderation Manager, Taimi


Taimi’s moderation isn’t as simple as classifying content as acceptable or unacceptable. The team uses more than 10 categories during the moderation process, including contextual considerations. For example, the team weighs whether the content contains external links or social media.


Taimi’s first approach with Nyckel was to use a single model across half a dozen moderation categories. This was successful, but Vladislav’s team needed the model to auto-moderate more content, and with greater accuracy. They decided to train individual models for each of their categories. The next step was to train the first of these stripped-down models.


"Training this model took less than two minutes and you can really trust it."


Vladislav Yavorskyi


Moderation Manager, Taimi


The first of the new models that the team tested were able to moderate 45% of Taimi’s content with 95% accuracy. And with every training cycle, the numbers improved. The team moved forward with implementing these models across each category.


## The Results


Today, most of Taimi’s content is moderated automatically. The moderation time — the time from when the content goes live to when the content has been moderated — has been reduced to a few seconds. And to monitor the model’s continued performance as user behavior changes, the moderation team checks a subsample of auto-moderated content.


Using Nyckel’s[Text Classification API](https://www.nyckel.com/blog/text-classification/) , Taimi now automates 60% of all content moderation with an accuracy of 96%. Average moderation time is down 3x and auto-moderation coverage is up 4x. Apart from continuing to improve the current model, Vladislav plans to apply Nyckel’s machine learning to the problem of scammers and spammers in the future.


---


Interested in exploring how our custom content moderation solutions could support your business?[Sign up to give Nyckel a try for free](https://login.nyckel.com/) , or[learn more about our content moderation solutions](https://www.nyckel.com/custom-content-moderation-api) .


For more reading on content moderation, check out these articles:


- [The Ever-Growing Landscape of Content Moderation AI Platforms](https://www.nyckel.com/blog/the-ever-growing-landscape-of-content-moderation-ai-platforms/)
- [Content Moderation using SurgeAI’s Toxicity Dataset](https://www.nyckel.com/blog/content-moderation-using-surgehq-toxicity-dataset/)
- [What Features Does Your Manual Content Moderation Tool Need?](https://www.nyckel.com/blog/what-features-does-your-manual-content-moderation-tool-need/)
