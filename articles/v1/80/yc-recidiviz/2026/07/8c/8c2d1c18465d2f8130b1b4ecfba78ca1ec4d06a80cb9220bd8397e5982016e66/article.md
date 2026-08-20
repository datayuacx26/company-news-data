---
schema_version: "1.0.0"
document_id: "8c2d1c18465d2f8130b1b4ecfba78ca1ec4d06a80cb9220bd8397e5982016e66"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/part-iii-tips-for-procuring-and-adopting-criminal-justice-technology"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:8971137443988a60101744acff41d7b974064ffc6db38e3b054378c48724fc85"
---

# Part III: Tips for procuring and adopting criminal justice technology

*This is the third part in a four-part series Recidiviz has written to document the use of technology in the criminal justice ecosystem and share specific recommendations for practitioners in tech procurement, integration, and deployment (see*[Part I](https://medium.com/recidiviz/part-i-a-mapping-of-tech-in-the-criminal-justice-system-bce3c5f37fd7) *,*[Part II](https://medium.com/recidiviz/part-ii-why-good-data-and-feedback-loops-in-criminal-justice-are-hard-to-come-by-b07e7e2ee3d7) *). As always, we welcome thoughts and feedback.*


The criminal justice system is[awash with new tech tools](https://www.recidiviz.org/updates/part-i-a-mapping-of-tech-in-the-criminal-justice-system) and[inherently a difficult place to establish data-driven feedback loops](https://www.recidiviz.org/updates/part-ii-why-good-data-and-feedback-loops-in-criminal-justice-are-hard-to-come-by) . Practitioners looking to use technology must wade through a dizzying array of options to determine what to use, when to upgrade, and how to implement changes and track impact. Drawing on both what we’ve learned from building Recidiviz and successful practices from the private sector and government, we created this guide to help criminal justice practitioners procure and deploy technology effectively.


# 1. Start with the problem and the people, not the solution


Before looking at technology solutions, we should clearly articulate a) the need we’re trying to address, b) the people who would use the solution, and c) how it could fit into their workflow.


For example, imagine our goal is to increase the efficiency of re-entry case managers to reduce deferred parole hearings. It might be tempting to jump straight to looking at checklist or project management software, or designing something in-house. But this skips to the solution, before we really understand the problem. If the main issue holding back re-entry readiness is that vocational and rehabilitation is often required for parole, but only available in certain facilities, there are a lot of options to explore to fix the issue, and many don’t require new tools at all — e.g., expanding programming availability across facilities might help more than a to-do list tool would have. Another common issue is understanding the problem, but not how our approach would fit into existing workflows: we might rush to procure a great to-do list app, only to find that case managers only have time to check their to-do’s during lunch time, when away from their computer, and the app we procured isn’t built for smartphones. Knowing the right place to intersect with their workflow might have helped us find the right solution.


One area to be especially wary is with shiny new technology, like[blockchain](https://www.ibm.com/topics/what-is-blockchain) or[deep learning](https://www.ibm.com/cloud/learn/deep-learning) , where exciting buzzwords can obscure that the tool doesn’t actually address the problem at hand. Thinking about the problem, and the user, rather than starting the solution can help weed out costly missteps and lead to more adoption within the agency. Without adoption, shininess doesn’t matter much. As an added bonus, this process will also lead to a better RFP that treats user-experience as a first-order priority.


### Checklist


- Answer five key questions up-front:
◦ **Who is the key user of this tool?**
◦ **What is the need, as they would articulate it?** (It’s really easy to build tools that we think people need, but if it’s not what they think they need, you’re not going to see much uptake)
◦ **What’s the core problem they’re trying to solve?** (If the user says they need a to-do list tool — why? What’s the real stumbling block for the organization that they’re trying to fix with it? Try to separate what they want from what they’re trying to solve.)
◦ **In what types of environments will the tool be used?** (Consider the types of platforms, e.g. mobile or desktop, that might be appropriate)
◦ **What would wild success look like with this tool** (or, to put it in RFP language: what is the “desired impact”)? Is there more than one way to get there?
- Interview users to prioritize pain points and figure out where a new tool would be most effective in their workflow
- Talk to peer agencies to see if they’ve had similar needs and how well different solutions have worked for them. (Trust numbers above anecdotes — did program matching go up, and by how much? Do they have usage metrics to show how often staff use the tool?)
- Articulate product requirements clearly and specifically in the RFP
- Whenever possible, ask vendors for a trial period *prior* to signing a contract to validate that the tool addresses these requirements


# 2. Think about the long-term


Custom-built solutions may seem like a good way to meet a specific or immediate need, but they can lead to negative consequences in the long term. They also make it less likely that the new system will ever be able to communicate with other parts of the justice system. When products and data are integrated *across* agencies, rather than in individual silos, we can look at improvements and impact more holistically.


We should ask questions like: Does the new bodycam software output videos in a format that is well-supported by the police department’s database, or with other video software? Can other organizations, e.g. correctional facilities, easily read or access information from the probation office’s new record management system?


Choosing a custom solution also makes it more difficult to switch vendors if product requirements change down the line, leading to lock-in. When only one vendor in the world knows how your software works, you’ll only ever be able to work with that one vendor. Before committing to any tool, and especially a highly custom one, the following considerations may help to improve long-term efficacy:


### Checklist


- Determine how interoperable the inputs and outputs of the tool are with other systems, both inside and outside the agency
- Ensure adequate training for personnel to incorporate new tools into their workflow
- Understand who owns the data collected or generated by the tool, and ensure it’s the agency
- Draft shorter-term contracts that make incremental and measurable improvements to the system, rather than one big overhaul that will be difficult to change as needs change. Assume the work will take twice as long as promised.
- Incorporate maintenance and update requirements (especially for security updates) into the RFP
- Incorporate documentation requirements into the RFP (getting monthly exports to your data warehouse isn’t helpful if your IT staff don’t have a data dictionary from the vendor to help your team understand them)


# 3. Choose vendors that can explain their tools in plain language


It is often a red flag if a tech vendor can’t produce a clear answer to how a tool works or how an algorithm weighs specific factors in a predictive decision. This is unfortunately still common — many companies have[historically resisted disclosure](https://cdt.org/wp-content/uploads/2017/08/2017-07-31-Trade-Secret-Algorithms-as-Barriers-to-Social-Justice.pdf) , calling the workings of their tools protected trade secrets.


Take the example of probabilistic genotyping software, which is used by investigators to predict the probability that a genetic sample came from a specific individual. Practitioners should ask: In what types of cases will the software likely fail? What kind of mistakes does it make (e.g., can it fail in both directions — saying the samples came from the same person in cases where they didn’t, *and* other times saying they didn’t come from the same person when they actually did)? What are the consequences of these mistakes (e.g. leading to punitive outcomes or opportunity denial)? How often do mistakes happen? If the tool’s output is challenged, do I (or the vendor) have enough information to determine and explain what went wrong?


### Checklist


- Ask vendors to describe how their tools work in a language that is understandable and not overly technical. (Your agency may need to provide a similar explanation to the public someday.)
- Bring the most technical member of your team along to ask all the questions they can think of, then have them write up a report internally explaining how the system works
- Always assess machine learning tools by how explainable their outputs are
- Ensure that staff who will be using the tool have a solid understanding of its limitations


# 4. Analyze for impact on underrepresented and historically marginalized people


Technology has the potential to make the criminal justice system more fair and efficient. However,[a growing body of research](https://www.brookings.edu/research/understanding-risk-assessment-instruments-in-criminal-justice/) has shown that certain types of technology can be discriminatory. Luckily, we can assess this up-front and use what we learn to inform procurement decision-making.


All tools, especially those that use machine learning, should be assessed for performance (i.e. accuracy) across protected characteristics (i.e. race, gender, age, sexual orientation, and socioeconomic status). A good example is the[set of analyses](https://www.nist.gov/news-events/news/2019/12/nist-study-evaluates-effects-race-age-sex-face-recognition-software) the[National Institute of Standards and Technology](https://www.nist.gov/) conducted for facial recognition software, evaluating 189 algorithms from 99 different developers for bias across race, age, and sex. It’s a good sign if a tool has already been evaluated by another agency or academic group that has no financial ties and a history of objectivity.


For tools that benefit justice-involved individuals, bias can come in the form of denial of opportunity — for example, socioeconomically disadvantaged groups who don’t own smartphones or live in areas with reliable cellular service won’t be able to reap the benefits of court reminder apps. In those cases, we need to think of additional ways to reach these audiences. For example, this might include mail or postcard campaigns, e-mail as a backup to text messaging, or using contacts with parole and probation officers.


### Checklist


- Research the different risks associated with the specific technology being procured
- Ask vendors to provide information about their tool’s performance across different protected characteristics such as race, gender, age, sexual orientation, and socioeconomic status
- Ask vendors to explain any disparities and how they will be mitigated
- Check for unbiased, third-party analysis of the tools’s impact on protected classes. If there is none, check with other agencies who use the tool for any internal assessments or analyses they’ve tried to run.
- Monitor the impact of the tool across these groups after deployment


# 5. Create a data-driven rollout plan


Before launch, we can develop a data-driven rollout plan to reduce the risk of failure, track impact, and inform future iterations. Rollouts can be tricky in the criminal justice context — traditional tech experimentation frameworks, like A/B testing, can become ethically questionable when people’s lives, freedoms, and futures are at stake.


At Recidiviz, we use pre-determined “backstop” metrics during our launches to determine when changes should be rolled back (essentially the opposite of success metrics). For example, if a new feature for our[line staff tools](https://www.recidiviz.org/corrections) were to disproportionately lead to fewer positive outcomes for Black individuals on supervision, our metrics would show that as it started to happen, giving us the chance to stop the feature roll-out and make changes before trying again.


### Checklist


- Align on a set of specific metrics to track before launch, including both indicators of success and criteria for rollback; this doesn’t need to be a heavyweight process and it’ll pay off when you know whether the rollout worked. These metrics should be measurable and quantifiable, not just anecdotal.
- Success metrics should cover both adoption (are staff using the new tool?) in addition to impact (is it solving the problem we procured it to solve?). “Every police department has some piece of tech on their shelves that they don’t use anymore,” is something we’ve heard over and over again while talking to departments across the country.
- Start with a small set of initial users and features
- Proactively check in with users to understand reasons for drop-off and points of frustration


# 6. Weigh the benefits against the costs


Ultimately, every tool will have benefits and drawbacks. Surveillance technologies that use facial recognition can help law enforcement quickly wade through footage and identify potential suspects, but can also violate the privacy and security of those being monitored. If these costs can’t be mitigated, or if the tangible benefits can’t be realized, then it may be time to[consider another type of tool, or none at all](https://www.latimes.com/local/lanow/la-me-laser-lapd-crime-data-program-20190412-story.html) .


### Checklist


- Systematically track successes and failures attributable to the deployment of a new tool
- Check how the system is performing compared to what was in place before
- Regularly check in with stakeholders and users to determine if funding for the tool should continue


# We want your feedback


This is an evolving piece. We’d love to hear your feedback and suggestions on the Playbook, especially if you have experience working in this space. Reach out to us at[this link](https://www.recidiviz.org/contact) .


*We would like to thank*[Clementine Jacoby](https://www.linkedin.com/in/clementine-jacoby-50578483/) *,*[Samantha Harvell](https://www.linkedin.com/in/samantha-harvell-359b9814/) *, and*[Lauren Haynes](https://www.linkedin.com/in/lauren-haynes/) *for their helpful comments and contributions.*


*Recidiviz is proud to be a*[GLG Social Impact Fellow](https://glginsights.com/glg-social-impact/) *. We would like to thank GLG for, among other things, connecting us to some of the experts who provided valuable insights showcased in this post.*


‍
