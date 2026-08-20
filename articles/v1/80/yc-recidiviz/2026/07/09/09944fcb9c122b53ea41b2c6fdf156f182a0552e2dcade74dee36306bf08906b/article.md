---
schema_version: "1.0.0"
document_id: "09944fcb9c122b53ea41b2c6fdf156f182a0552e2dcade74dee36306bf08906b"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/part-i-a-mapping-of-tech-in-the-criminal-justice-system"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:82a5e011da34ebee84e836905d2a778d9ae67cab8a8a36d9eec5de4443731937"
---

# Part I: A mapping of tech in the criminal justice system

*This is the first part in a four-part series Recidiviz has written to summarize some of the biggest issues we see with the use of technology in the criminal justice space and to create a “playbook” with specific recommendations that practitioners can utilize in tech procurement, integration, and deployment. As always, we welcome thoughts and feedback.*


In 2008, the Los Angeles Police Department (LAPD) began working on a project that was full of promise — to predict crime using data and analytics. Various agencies at the Department of Justice jumped on board, as did academics at UCLA. Within a few years, the resulting software, PredPol (now Geolitica) became a leading vendor of predictive policing technology in the U.S.


But the gleam of this new technology soon faded. Several studies proved that the use of historical data in these systems may have led to discriminatory policing and reinforced racial inequalities. In 2019, LAPD’s[own internal audit](https://www.latimes.com/local/lanow/la-me-lapd-precision-policing-data-20190703-story.html) concluded that there was insufficient data to determine if PredPol actually achieved its purpose — to reduce crime. By 2020, the LAPD[had stopped using PredPol entirely](https://www.latimes.com/california/story/2020-04-21/lapd-ends-predictive-policing-program) — and the Fourth Circuit Court of Appeals had[delivered a blow to the constitutionality of these technologies](https://epic.org/2020/07/federal-appeals-court-sounds-a.html) altogether.


Despite the blow, enthusiasm continues to grow for incorporating technology into the criminal justice ecosystem. And rightfully so, as new technology has helped to[decrease failure to appear rates in court](https://uptrust.co/impact/) ,[automate criminal record clearance](https://www.codeforamerica.org/programs/criminal-legal-system/clear-my-record/) , and improve[policy decisions through modeling](https://global-uploads.webflow.com/5e7ff048d75a9b3c5df52463/5ffa6f714ad6fa2f7afeceb4_IL_3strikes_010621.pdf) . But at the end of the day, tech is a hammer — helpful when used in the right way, good at creating a mess when it’s not.


The majority of the Recidiviz team comes from tech: we are a team of data scientists, software engineers, product managers, and designers. And while we’ve seen tech as a game-changer, many have also seen the downside. In this post, we’ll explore where things can go wrong in criminal justice work when technology is inaccurate, biased, or used improperly. We’ll cover why it pays to be cautious in applying new tools to old problems, and how to take the community, the justice-involved, and the agencies that serve both into consideration when looking at the evolving tech landscape.


# Tech pitfalls in the justice system


Technology is increasingly used at every stage of the justice system: to detect gunshots; to dispatch squad cars to far-flung neighborhoods; to decide whether defendants will be eligible for community service or diversion programs; and to monitor the location of parole and probationers to make sure they don’t miss curfew. These decisions have the potential to improve the efficacy of the justice system, but can also cause harm to already-vulnerable communities.


In the following table, we highlight some real-world examples of how these technologies have led to negative consequences:


- **Historical bias:** Bias is reinforced when past or current data is used to inform decisions for the future. For example, facial recognition systems are often trained on datasets that don’t adequately represent women or darker skin tones, making[misidentifications more likely for people in these groups](http://gendershades.org/overview.html) . These inaccuracies, according to the ACLU, can lead to “missed flights, lengthy interrogations, watch list placements, tense police encounters, false arrests, or worse.”
- **Lack of transparency and explainability:** Criminal justice tech can deny defendants the ability to challenge results from technical systems like risk assessments, which are often used to determine bail amounts and influence sentences. These tools must be tested and held accountable broadly.[Companies often cite trade secret or IP defenses](https://epic.org/issues/ai/ai-in-the-criminal-justice-system/) to avoid publishing comprehensive reports about the inner workings of their tools, e.g., the specific weight they give to each factor in making a decision.
- **Privacy risks:** When any type of personal data is collected or stored, risks loom especially large in the criminal justice context where sensitive information can be used to incriminate or surveil. According to a[study conducted by the Bureau of Justice Statistics](https://bjs.ojp.gov/content/pub/pdf/pauchi.pdf) , nearly 1 in 8 Americans feel that they have been victims of an improper invasion of privacy by a law enforcement agency. While criminal proceedings are public information, a lack of controls around personal data of individuals in the system have often led to extortion (and resulted in[litigation against agencies](https://theappeal.org/pennsylvania-county-owes-67-million-after-man-finds-arrest-records-on-mugshots-com/) ) and[fraud](https://www.themarshallproject.org/2018/05/14/the-man-with-the-stolen-name) .
- **Financial hardship:** Usage of technology in the justice system is not free. When costs can’t be absorbed by the state budget, they sometimes are passed along to the individuals in the system — an approach that can[seriously undermine](https://www.brennancenter.org/our-work/research-reports/steep-costs-criminal-justice-fees-and-fines#footnote1_4iq2an0) the potential for successful re-entry into the community. Costs in Florida, for example, can[top $1,000 for a misdemeanor and $5,000 for a felony](https://www.vera.org/downloads/publications/the-high-price-of-using-justice-fines-and-fees-florida.pdf) , excluding any additional fees incurred for use of services like phone calls or electronic ankle monitors. Costs like these prevent people who’ve completed their sentence from rebuilding their lives.


These challenges can become uniquely troubling in the context of criminal justice. In the consumer context, even the most egregious privacy-abusing app can be uninstalled by its users; by contrast, the criminal justice system is compulsory for the people under its care. What’s more, those of us who help select or deploy new criminal justice technology rarely have direct experience with the justice system ourselves, due to CJIS regulations on who can work with this kind of data — making it less likely that the tech ecosystem working in this area can fully empathize with the people it impacts.


In the table below, we highlight some of the ways these risks intersect with new uses of technology in the criminal justice system. It’s not exhaustive, and most risks compound (e.g., predictive policing systems can be biased due to their training data, which directly reinforces historical bias and indirectly undermines the accuracy of the tools).


‍


# Some key takeaways


**It’s apparent from the chart above that not all harms are equal** — an unnecessary arrest is not the same as a leaked email address. When evaluating new technology, the following considerations can be helpful in putting the pitfalls in context.


- **Frequency:** How often will the tool be used? How often does it make a mistake?
- **Severity** : What are the consequences of a tool being wrong? Are they different for false positives vs. false negatives? Can the tool lead to unnecessary arrest or incarceration, or other loss of freedoms? What safeguards are in place, and what recourse does an individual have if the tool is wrong?
- **Impacted audience:** Does the tool have the potential to adversely affect people who aren’t in the justice system, e.g., predictive policing technologies? The broader community has different constitutional protections from those who are incarcerated. Additionally, creating unnecessary first-time interactions with the system may carry more risk due to the criminogenic effect.
- **Active harm versus opportunity denial:** Can the tool cause active harm toward an individual or group? Or does it deny benefits to certain populations, such as people who don’t own smartphones?


**Another consideration is the negative impact on criminal justice agencies themselves.** Agencies have faced diminished public trust, lawsuits, and significant new[compliance regimes](https://www.nola.gov/nopd/nopd-consent-decree/) in response to the adoption of tools that unintentionally create biased outcomes or lack of transparency. When agencies can’t explain what led to a certain result, trust in both the tool and the process can erode. Clear documentation and processes for data access are crucial — a good example is the recent push for[free and timely release of body camera footage](https://www.calpublicagencylaboremploymentblog.com/public-safety-issues/governor-signs-sb-1421-and-ab-748-dramatically-increasing-public-access-to-peace-officer-personnel-records-2/) in California, which gained enough public support to pass as law in 2019.


**Finally,** **it’s easy for technology to create reservoirs where biases can propagate.** Tools that rely on historical data are used in every step of the criminal justice system, from predictive policing and surveillance enabled by facial recognition, to sentencing and release-date calculations.


Carefully weighing community harms against community benefits can help agencies to decide when technology is the right tool for the job, and when it’s just adding risk. When technology does make sense to help solve a problem, evaluating potential edge cases and establishing safeguards to protect against these risks can be a small investment that saves lives, time, and resources for the agency and the community in the long-run.


# What can we do?


We believe there is a useful, even crucial, role for certain types of technologies to play in the criminal justice system. But indiscriminate use of technology can also cause harm to people and weaken public trust in the system. This isn’t new — polygraph machines, social media-based[surveillance](https://www.engadget.com/lapd-ban-third-party-facial-recognition-clearview-ai-112526446.html) , and other ill-fitting technology have come and gone before. What is new is the increased enthusiasm around information technology, and the very real limitations of it for some applications.


Over the next few blog posts, we’ll dig into a few of the structural issues that have made technology hit-and-miss in this domain, scrutinize the types of artificial intelligence used across different criminal justice technologies, and synthesize a “playbook” that we hope will be helpful to practitioners navigating this increasingly complex space.


*We would like to thank Clementine Jacoby, Samantha Harvell, Mayuka Sarukkai, and Jason Tashea for their helpful comments and contributions.*


*Recidiviz is proud to be a GLG Social Impact Fellow. We would like to thank GLG for, among other things, connecting us to some of the experts who provided valuable insights showcased in this post.*


‍
