---
schema_version: "1.0.0"
document_id: "ad041779021ca7ab5e63cc79d3922aae196424902aaa9c46eecadbbaf1488145"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/the-business-impact-of-synthetic-identity-fraud-and-why-prevention-requires-a-lifecycle"
published_at: "2026-06-08T17:21:32+00:00"
first_seen_at: "2026-07-20T23:21:50.456876+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:2290fe5b39c1ac73efa81a92d7e7d39bc98e026867510cb7913df5f2b94af626"
---

# The business impact of synthetic identity fraud and why prevention requires a lifecycle approach

Synthetic identity fraud challenges banks, fintechs and other institutions at the financial, operational, and strategic level. It causes losses that are real but often invisible, buried in credit line loss metrics that obscure the fraud behind them. The fraud controls that organizations believe are sufficiently protecting them are often insufficient to meet the nature of this threat, and the prevention strategies that actually work require a total rethink of how identity is managed throughout customer relationships, rather than just at onboarding.


## Financial impact


It is difficult to estimate the true financial impact of synthetic identity fraud, because it often goes undetected and is misclassified as other types of losses. The scale of the problem, however, is not in doubt. In research conducted by the Federal Reserve in conjunction with the Consumer Financial Protection Bureau (CFPB), synthetic identity fraud was cited as the fastest growing financial crime in the United States.


CFPB research estimated the total losses at billions of dollars annually, and Datos Insights added precision to this estimate, reporting that U.S. unsecured credit synthetic identity fraud losses reached approximately US$2.94 billion in 2025, up from US$1.80 billion in 2020—a roughly 16% compound annual growth rate driven by baseline market forces, with additional acceleration expected as AI tools lower barriers to entry for criminal networks. By 2026, losses are projected to reach US$3.12 billion. The expansion of digital lending and ease with which these fraud operations can be enhanced by and managed with AI-powered tools is rapidly driving their expansion.


The direct losses from synthetic identity fraud come in the form of charge-offs, which might be from credit cards, personal loans, auto loans, or other lines of credit that are rapidly used up to maximum utilization before the synthetic identity disappears. With no victim other than the financial institution, there’s often no fraud report to reclassify the losses and they’re booked as standard credit charge-offs. Effectively, when synthetic identities go undetected, financial institutions are underestimating their fraud exposure and overestimating the quality of their credit portfolios. This gap has significant implications for provisioning, capital allocation, and regulatory reporting.


Indirect losses compound the impact. When synthetic identity fraud is suspected, investigation and recovery efforts requires substantial time from analysts, and the forensic work needed to reconstruct a synthetic identity’s history is labor-intensive. Institutions may also see compliance costs associated with regulators’ expectations, due to enhanced scrutiny of how institutions are classifying and reporting these losses. A systemic risk also exists, which is portfolio risk concentration: if synthetic fraud becomes concentrated in particular products or origination channels where fraud rings have found success establishing accounts, the associated exposure when the ring executes a coordinated bust-out across hundreds of accounts simultaneously can generate losses that appear sudden and unexpected but were accumulating for months.


## Why synthetic fraud is often misclassfied


Traditionally, fraud classification systems are largely dependent on a victim’s fraud report, which then kicks off the investigation. But, synthetic identity fraud is engineered specifically so that there is no victim other than the financial institution. Rarely, if ever, will it generate signal from anyone outside the institution.


If a real person’s identity is stolen and used to open fraudulent accounts, that person will eventually notice. They’ll find the accounts on their credit report, or they might start receiving statements or calls from a collection agency – and that’s when they file a report with the credit bureaus and begin the process of submitting fraud claims to the financial institutions involved. Through the investigation to clear the real person’s name, the institution will gain visibility into the nature of the compromise that allowed the accounts to be opened. The institution can also recover some losses through their fraud reserves.


With synthetic fraud, there’s no signal from a victim to kick off the investigation. The identity that busted out never really existed, so it has no specific anchor in the real world. If the real person whose Social Security number was used discovers it at all, they usually find it much later, during an unrelated credit inquiry. Their own file will show no adverse activities because the synthetic identity had its own file with a different name at a different address. This means that the financial institution never discovers that these charge-offs were actually fraud rather than a credit loss.


Organizations may thus believe their exposure to synthetic fraud is low, when instead they lack the visibility to see the extent of the fraud at their institution. The Federal Reserve and CFPB have both highlighted this as a structural problem; institutions that aren’t actively investigating their charge-off portfolios for signs of synthetic fraud are almost certain to be underreporting this type of loss and thus misallocating reserves between credit risk and fraud prevention. The spillover effect – that these institutions are operating with a flawed model of their portfolio quality – is highly concerning to regulators.


## Why point solutions fail


The first reaction many institutions have to an emerging vulnerability is to add more controls at the most visible point of contact. If onboarding fraud is increasing, they strengthen identity checks at onboarding, or if document fraud is rising, they add an additional layer of document verification. This approach works for some types of fraud but fails against synthetic identity fraud, because it isn’t concentrated at a single point in the customer lifecycle and can defeat individual controls.


Point solutions will catch some attempts at synthetic identity fraud. Identity verification at onboarding may catch the synthetic identities that lack a credit history and have flawed fabricated documents. But, a synthetic identity that has been cultivated for twelve to eighteen months already, with a real credit file and plausible documents may pass verification.


Credit bureau checks are also less useful in these cases because they have recorded what financial institutions report to them. A synthetic identity that has been carefully managed with on-time payments and low credit utilization will look creditworthy in the bureaus’ reports. They address the consistency of the credit file, not the legitimacy of the identity.


Many institutions also rely on rules-based fraud detection systems, which are inherently reactive. They search for fraud patterns that are indicative of previous schemes. Static rules, however, can’t keep up the pace with adversaries that adapt – like the sophisticated fraud rings that manage portfolios of synthetic identities, constantly adapting their techniques and using AI tools to test behaviors against detection logic at scale before they interact with real institutions.


Siloed internal teams also contribute to challenges in detection. When fraud and credit risk teams operate with separate data, alerts, and escalation paths, it compounds the problem and makes it so that no one has a complete view of the full lifecycle of a synthetic identity’s behavior.


Datos Insights’ report confirms the urgency of moving beyond point detection: with over 40% of financial institutions already observing evidence of increased attack rates directly attributable to generative AI, and a similar proportion of fraud executives indicating that they believe fraudsters are already as sophisticated as, or more sophisticated than their institutions, the solution providers defending against them are due for an upgrade.


[Download the new Datos report: The Synthetic Identity Crisis: Detection, Prevention, and the AI Arms Race](https://www.miteksystems.com/resource-library/white-papers/the-synthetic-identity-crisis-detection-prevention-and-the-ai-arms-race)


## What effective prevention requires


Effective prevention techniques for synthetic identity fraud require institutions to implement full lifecycle visibility. This means starting early, monitoring continuously, and integrating signals from the full lifecycle. Synthetic fraud leaves some evidence at every stage, but it’s often missed by traditional controls.


The foundation of effective prevention is lifecycle visibility, maintaining a continuous view of the signals that are associated with each identity starting with first contact (application inquiry, pre-qualification request, even an initial web visit) through the onboarding process, account management, and whatever events occur before the account’s eventual termination. This doesn’t mean adding customer friction at every touchpoint; instead, it means assessing risk signals continuously and transparently in the background, and flagging accounts for review if behaviors consistent with synthetic fraud are detected. Legitimate customers will rarely, if ever, be flagged.


The investment case for these types of lifecycle-capable controls is gaining even more traction: for three consecutive years, investment in application fraud controls has ranked at or near the top of fraud-prevention funding priorities among North American fraud executives. This reflects the growing recognition at the institutional level that incremental improvements to existing controls are no longer sufficient.


Continuous monitoring looks for indicators after the identity has passed onboarding, knowing that these behaviors develop over time. For example, the behavioral shift that precedes a bust-out often includes escalating cash-advance requests, simultaneous balance transfer activities across multiple cards, and rapid increases in spending patterns that are detectable weeks before the account becomes delinquent. Institutions that are actively monitoring for these activity patterns can freeze accounts and limit losses before the full drawdown is completed – making the potential difference between early detection and late or no detection tens of thousands of dollars of prevented losses per identity.


Legitimate customers vastly outnumber synthetic identities, so continuous monitoring adds minimal friction for real customers by design, understanding that institutions are as highly focused on the customer experience as they are on fraud prevention. From document analysis and passive liveness detection at onboarding to signal detection, behavioral analytics, and risk-based review triggers throughout the rest of the customer lifecycle, these controls are designed to stay invisible to real customers even as they monitor for patterns associated with synthetic identity fraud.


## What leaders should prioritize next


The first and most important step is the completion of an honest and comprehensive assessment of the level of visibility your organization currently has to its exposure to past and current synthetic identity fraud. This assessment often uncovers meaningful gaps, for example, revealing that charge-off portfolios are never analyzed for synthetic fraud indicators, gaps in onboarding controls where document authenticity is verified but not identity consistency, or data silos that limit fraud and credit risk teams from conducting comprehensive analyses.


Industry data reinforces the need for honest self-assessment. In the Datos report, 84% of fraud executives identify synthetic identities as a high or moderate threat to their institutions’ application processes. Many still lack the visibility to accurately measure their own exposure, with their synthetic fraud losses being chronically misclassified as standard credit charge-offs, rather than fraud.


This breaking down of silos is one of the highest-leverage structural changes organizations can pursue. Synthetic fraud losses appear in credit risk’s books – but they originated as fraud. By enabling these teams to share their data and processes, visibility across the full lifecycle of the synthetic identity is unlocked, as is the opportunity to intervene at multiple points before the full loss is absorbed.


As you consider updates to your identity verification infrastructure, evaluate onboarding performance as well as lifecycle coverage. Effective onboarding solutions provide strong signals at that stage, but must be integrated with continuous monitoring, network analysis, and risk-scoring to maintain effective protection against synthetic identities that pass onboarding. When a robust onboarding check isn’t connected to any controls downstream, it becomes a gate that sophisticated fraud rings can eventually discover ways to pass.


Finally, assess vendors’ expertise in defense broadly as well as against this specific type of fraud. Synthetic identity fraud is a highly specialized domain, and defense requires elements of document forensics, biometric verification, behavioral analytics, and network intelligence to create a layered solution. By partnering with vendors that understand the full threat landscape and are actively developing countermeasures that address the AI-driven techniques fraud rings are now deploying, your organization will be better positioned to stay ahead of a moving threat from a sophisticated adversary. The institutions that make investments now in lifestyle-capable, AI-aware detection solutions will have significantly lower losses and decreased regulatory exposure versus those that wait for the next wave of bust-outs to reveal a gap they failed to close.


[Download the Datos Synthetic Identity Fraud Report](https://www.miteksystems.com/resource-library/white-papers/the-synthetic-identity-crisis-detection-prevention-and-the-ai-arms-race)
