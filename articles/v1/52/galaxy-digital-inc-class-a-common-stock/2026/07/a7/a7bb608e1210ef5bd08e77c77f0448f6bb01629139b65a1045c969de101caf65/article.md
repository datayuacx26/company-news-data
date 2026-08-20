---
schema_version: "1.0.0"
document_id: "a7bb608e1210ef5bd08e77c77f0448f6bb01629139b65a1045c969de101caf65"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/sec-peirce-defi-vaults-onchain-lending-securities-law-analysis"
published_at: null
first_seen_at: "2026-07-30T09:12:58.414260+00:00"
fetched_at: "2026-07-30T09:12:59.432705+00:00"
content_hash: "sha256:542cfbf0f8c9463f32beb0926ccedb360abfc99784443cf8b042aec15b984558"
---

# No Headstands Required: Applying Securities Laws to Vaults and Onchain Lending

## Introduction


In a recent[statement](https://www.sec.gov/newsroom/speeches-statements/peirce-statement-crypto-vaults-lending-strategies-072226) on DeFi[vaults](https://morpho.org/blog/vaults-the-future-of-noncustodial-finance/) and onchain lending strategies, Securities and Exchange Commission member Hester Peirce reiterates a[principle](https://www.sec.gov/newsroom/speeches-statements/peirce-statement-tokenized-securities-070925) she has raised before about tokenized securities: moving an activity onchain does not remove it from the scope of U.S. securities laws. “If you do headstands, backflips, and other gymnastics to read the law so that it does not apply to crypto assets and activities that are well within the scope of the federal securities laws, you will have a painful fall,” she warned. This time, the principle applied to vaults and lending strategies.


**Vaults** : Commissioner Peirce describes vaults generically as smart-contract systems that allocate user assets into yield-generating activities (staking, lending, etc.), ranging from fully programmatic/immutable designs to those run at the discretion of a person or group. She cautions that a vault could be an "investment contract" (meeting the “common enterprise” prong of the[Howey test](https://www.law.cornell.edu/wex/howey_test) ) if depositors reasonably expect profit from a deployer's or curator's managerial effort. Vaults holding or allocating into securities could also trigger Investment Company Act (ICA) exposure, with some vaults resembling unit investment trusts (UITs), others management companies, and others separately managed accounts.


**Lending strategies** : Peirce describes onchain lending systems where depositors' assets are lent to borrowers for a fee, managed by parties who set interest rates, choose eligible assets, set loan-to-value (LTV) ratios, and establish liquidation thresholds. In these cases, the regulatory exposure does not turn on whether the underlying assets are securities, she writes. Instead, the loans themselves can resemble "notes" that are securities under the[Reves v. Ernst & Young](https://www.law.cornell.edu/supremecourt/text/507/170) family-resemblance test, depending on the parties' motivation, plan of distribution, and other factors.


She also notes that discretionary management of either structure may separately implicate investment adviser status under the Advisers Act.


What makes this a difficult problem is that it is a layered one. Peirce’s statement includes two distinct, but related, structures and clearing one does not clear the other. A vault may fall outside the securities laws, but that only resolves the top layer; the depositor's capital still lands in an underlying lending market, and the analysis of that market (whether a "note" exists, who the obligor is, how the loan’s parameters are set) is a separate question with its own answer. Compounding this challenge, neither layer is the product of a single actor. A range of entities design, operate and participate in these markets (e.g. deployers, curators, allocators, risk managers, governance bodies, suppliers, and the borrowers on the other side of every loan). The securities-law treatment can differ depending on which participant or layer is under examination and on and how systems are designed. The result is not one question but several, stacked on top of one another.


We are not lawyers, and this report is not legal advice. The following analysis attempts to map the legal tests Peirce’s statement presents onto how these systems (which handle[tens of billions](https://www.galaxy.com/insights/research/crypto-lending-leverage-q1-2026) of dollars) are actually used and designed. It is not a prediction of how any court or regulator will ultimately resolve any particular protocol’s or product type’s status.


## Which Rules Apply to What?


Commissioner Peirce reaches for a different legal test depending on the structure. *Reves* is the bespoke tool for lending (because the instrument in question is a loan), the Investment Company Act is the bespoke tool for vaults (because the instrument resembles a pooled allocation vehicle), and the Advisers Act sits underneath both as a separate, discretion-triggered layer.


## Who Peirce's Statement Could Pertain To


Because the tests above are activity-based, the exposure attaches to whoever performs the relevant function, which could span several distinct roles across the vault and lending stack:


-


Vault deployers: the parties who design and release vault infrastructure.


-


Vault curators: parties selecting or reallocating yield strategies or choosing sub-curators/managers.


-


Curator-aggregators: products or protocols that route funds across multiple third-party curators (meta-vaults).


-


Lending market risk managers/curators: whether a DAO, protocol team, or delegated third party (e.g. risk-curation firms serving protocols). These are the entities setting rates, asset eligibility, LTVs, or liquidation parameters.


-


Investment-adviser-like service providers: third parties offering discretionary allocation advice or portfolio management around these products, even outside the core protocol.


-


Borrowers on lending protocols: the parties who draw down against posted collateral or borrow unsecured; possibly relevant not as regulated managers but as the underlying obligors when analyzing whether a depositor's claim constitutes a "note" under *Reves* .


Based on Peirce’s statement, the tests are functional ones. What matters is the activity performed (e.g. discretionary control over yield/risk parameters), not the label (DAO, foundation, curator, risk manager) attached to the entity performing it.


A caveat on borrowers specifically: unlike the other entities, borrowers are not "targets" of securities regulation in the sense of performing a discretionary or managerial function that could subject them to obligations as an issuer, adviser, or manager. A borrower posting collateral to draw a loan is not issuing a security, offering investment advice, or managing anyone's assets. They appear here only because the borrower-as-obligor is analytically important on the lending side. The distributed set of individual borrower obligations is what a depositor's claim can run against. Identifying those obligors is central to the "is there a note, and whose promise to repay backs it?" analysis under *Reves* discussed below. Borrowers are relevant to characterizing the instrument, not necessarily as regulated actors in their own right. The graphic below highlights the roles of, and relationships between, vaults and markets and their maintainers, and suppliers and borrowers of pool lending protocols.


## What We’re Breaking Down


There are two components of the statement to unpack, each requiring a distinct analysis: 1) lending apps falling under *Reves* and 2) vaults, curators, and lending market maintainers qualifying as investment managers or advisers.


### Onchain Lending Applications and the *Reves v. Ernst & Young* Family Resemblance Test


In her statement, the Commissioner points to *Reves* as the operative test for onchain lending, noting that the loans themselves "can bear the hallmarks of notes that are securities" depending on the parties' motivations, plan of distribution, and other factors. But invoking *Reves* raises a threshold question the statement leaves open. *Reves* presupposes the existence of a note (an instrument evidencing a borrower's obligation to repay a debt). Before asking whether an instrument bears a "family resemblance" to a security, one has to first identify the note itself, which means identifying the obligor who has assumed an independent promise to repay. In some decentralized lending contexts, that predicate is not always a given, and whether it exists at all depends heavily on the protocol's structure.


Under *Reves* , every note is presumed to be a security unless it resembles a recognized category of non-security note (such as a consumer loan or a note secured by a home mortgage). Where a note doesn't fit one of those categories, four factors determine whether it is a security:


-


**Motivation** : whether the seller's purpose is to raise capital for its general business and the buyer's is to earn a return (points toward a security), or whether the note facilitates a commercial or consumer transaction (points away).


-


**Plan of distribution** : whether the instrument is offered broadly to the public for speculation or investment, or is a narrow, privately negotiated arrangement.


-


**Reasonable expectations of the investing public** : how the instrument is marketed and how ordinary buyers would perceive it.


-


**Risk-reducing factors:** whether some other feature, such as collateral or a separate regulatory regime, sufficiently reduces the instrument's risk that applying the securities laws is unnecessary.


**The BlockFi paradigm** \[[1](https://www.sec.gov/files/litigation/admin/2022/33-11029.pdf) \]\[[2](https://www.paulweiss.com/insights/client-memos/blockfi-to-pay-100-million-to-settle-with-sec-and-32-states-over-crypto-lending-business) \]\[[3](https://www.ropesgray.com/en/insights/alerts/2022/02/sec-settles-charges-against-blockfi-lending) \] **.** The security threshold was easy to satisfy in a case like the SEC’s 2022 consent order with BlockFi. Customers transferred their assets to BlockFi, making BlockFi the obligor. BlockFi took those assets onto its own balance sheet, deployed and rehypothecated them for its own business purposes, and contractually promised customers repayment plus a yield. Customers were, in substance, extending credit to BlockFi and relying on BlockFi's own creditworthiness to be repaid. That is the canonical debtor-creditor relationship *Reves* was built to police: an intermediary raising capital for its general business use, funded by instruments marketed to the public on the strength of the returns they were expected to generate. Every one of the four *Reves* factors had a clear referent, because there was a single identifiable issuer whose motivations, distribution, marketing, and creditworthiness could be measured.


**Decentralized lending protocols are structurally different** . Protocols such as Aave and Morpho do not fit that mold perfectly. The *protocol* does not borrow customer assets, does not incur an independent repayment obligation, does not issue debt backed by its own credit, and does not intermediate lending activity flows through its own balance sheet. Instead, it provides a non-custodial, rules-based conduit through which users lend to and borrow from *one another* according to predetermined smart-contract logic. Depositors are not relying on the protocol's willingness or ability to repay. Instead, they hold a claim against a pool of *other users'* overcollateralized borrowing positions, with repayment engineered to come from posted collateral rather than from any intermediary's promise. The lender's principal exposures are collateral risk, market risk (e.g. liquidity and bad debt), and smart-contract risk, not the credit risk of a balance-sheet intermediary.


**This distinction is legally significant, not merely a design difference.** Protocols may make governance decisions about collateral eligibility, risk parameters, and other aspects of protocol design, but those decisions govern how the mechanism operates (more on risk curation in the next section). They do not transform the protocol into an intermediary borrower that has taken on its own obligation to repay suppliers. The borrowers themselves fit the Reves mold no better. A borrower from a pooled lending protocol, like Aave, does not advertise a return, solicit the public, or market that it is raising capital at all; it simply draws liquidity from a standing pool against posted collateral, for its own purposes, and borrows at a cost dictated by an algorithmic and market-driven cost curve. That is the posture of ordinary, self-interested commercial borrowing – closer to the consumer- and commercial-financing transactions *Reves* places on the non-security side of the line than to an instrument offered to the public on the strength of an expected return. So, neither candidate obligor behaves like the capital-raising seller that the motivation and distribution factors are designed to catch. The protocol is not raising money for its own enterprise, and the individual borrower is not offering anything to anyone. The right diagnostic questions make the contrast plain:


-


Who is the issuer?


-


Whose motivations are relevant?


-


Who is raising capital?


-


Whose creditworthiness is the lender evaluating?


-


Whose promise to repay is embodied in the alleged note?


In BlockFi, each question had an obvious answer. In a non-custodial pooled lending protocol, they become harder to answer.


**The rate is a market output, not an offered term.** Reinforcing this structural difference is how the return is determined. The interest rate on a pooled lending market is not a term any party offers or promises to attract deposits; it is a transparent, formula-driven output of a utilization curve that floats continuously with real-time supply and demand for the pool. A depositor earns whatever the curve produces at a given moment rather than a stated yield an intermediary has advertised. This distinguishes the arrangement sharply from the centralized-lender pattern, where the intermediary marketed a fixed or attractive yield to raise capital. That advertised return was the fact that most directly satisfied the *Reves* motivation and public-expectations factors in BlockFi and similar cases. The strength of this point, however, may depend on how fixed the rate model is. Where the curve and its parameters are stable or constrained by[timelocks](https://www.certik.com/blog/Timelock) and narrow governance bounds, the "no party sets or promises the return" characterization can hold; where a curator or governance body can reshape the curve at will, the argument may weaken, because the return then depends on discretionary control over the function that produces it. The proper emphasis is therefore on the source of the return (i.e. algorithmic and market-driven) rather than on its variability; a fluctuating return is still a return, and the defensible claim is not that there is no profit but that no particular party's efforts manufacture or promise it. This argument applies most cleanly to a vault that simply routes deposits into an open market and lets the curve speak for itself; it is weaker for curator-run "earn" products that market a specific rate achieved by blending the organic market rate with a third-party incentive backfill or a curator-absorbed spread, because there a party is, in substance, offering and promising a rate to attract deposits.


The accrual and redemption mechanics of some lending protocols, including Aave, further reinforce this point. Deposits are commingled into a single pool, and a supplier's interest is not tied to any particular borrower's loan or repayment. When you deposit, you receive a token representing your pro rata share of the pool, and a running index tracks the interest the pool has accrued over time; your balance is simply your share multiplied by that index, which rises continuously as interest accrues on outstanding borrowing, not when any particular borrower repays. Critically, when you withdraw, you are paid your principal plus accrued interest out of whatever liquidity is available in the pool at that moment and not by tracking down the specific loans your capital funded. There is no one-to-one match between a supplier and a borrower, so the depositor is not relying on any single obligor's willingness or ability to repay; the yield is a mechanical output of the market, not a sum an intermediary gathers and passes along. The one caveat is that this mechanism design depends on pool-level conditions rather than counterparty credit: realizing your balance requires enough un-borrowed liquidity in the pool to withdraw at that moment, and assumes borrowers remain overcollateralized. So, the depositor's real exposures are pool-level liquidity and bad-debt risk, not the credit risk of an identifiable debtor. Also note that this is not how every lending protocol works.


**The obligor question deserves a more precise answer than "there is no obligor."** In a non-custodial protocol, there is no single, central party standing between depositors and borrowers and pledging its own creditworthiness as BlockFi did — that much supports the argument that the protocol itself is not the *Reves* issuer. The individual borrowing positions underneath the pool sharpen the "no obligor" framing further still, and in a way that cuts in favor of the no-note argument rather than against it. A borrower on Aave has not made an unconditional promise to repay: the position carries no maturity date or scheduled repayment, can be held open indefinitely with no principal or interest payments at all so long as it stays adequately collateralized, and is non-recourse. The protocol's only remedy for an underwater position is automated liquidation of posted collateral, not a claim against the borrower's other assets. That is a materially different animal from a note, which by definition carries an obligation to repay. So, the depositor's claim runs not against a set of individual debt obligations in the traditional sense, but against a pool of collateralized, liquidation-contingent positions with no personal repayment covenant anywhere in the chain. That said, this setup does not eliminate the pooled-interest question entirely: even a claim against a basket of non-recourse, collateral-backed positions is still a claim on an income stream generated by other market participants, which is enough to invite scrutiny under *Howey* 's common-enterprise theory or an asset-backed-securities-style pooled-interest analysis, whether or not any individual leg would qualify as a note.


**Two caveats on the mechanics of the test.** First, courts have generally run the "is there a note?" inquiry together with the family-resemblance analysis rather than treating it as a separate gate, so the threshold-obligor point is best positioned as powerful evidence within the four-factor test rather than a reason to skip it. Second, it is important to be precise about how collateral matters under *Reves* and how it does not. *Reves* asks whether the depositor's claim is a note that is a security; it does not ask whether the collateral backing the loan is a security. Whether the collateral is a stablecoin, ETH, or a tokenized Treasury has no direct bearing on the character of the depositor's instrument. This is a point consistent with the Commissioner's own observation that lending exposure does not turn on whether the underlying assets are securities. What collateral does affect is the fourth *Reves* factor: the presence of overcollateralization is a risk-reducing feature (regardless of the collateral's own securities status) that weighs against treating the instrument as a security, much as the absence of collateral cut against the co-op in *Reves* itself. The takeaway is therefore not that "non-security collateral means *Reves* does not apply," but that overcollateralization is one risk-reducing input into a multi-factor test. This is helpful, but not by itself dispositive.


**Conclusion.** The point is not that decentralized lending as a whole sits categorically outside the securities laws. Protocol design matters, and different architectures will produce different answers. It is that non-custodial, pool-based lending should not be analyzed by rote analogy to centralized lending businesses like BlockFi. They are fundamentally different models, and the *Reves* predicate that was obviously present in the centralized cases is, at minimum, not obviously present here.


### Vaults, Curators, and Market Maintainers as Investment Managers and Advisers


Where the lending analysis asked whether a note exists, this question is one of discretion, and it reaches two kinds of actors: those who curate vaults, and those who set the parameters of the lending markets themselves. For both, the issue is the same: is the party 1) managing an investment, or 2) executing a function that facilitates a user's own decision? This managerial-versus-ministerial line is the axis nearly every active participant in the stack is measured on, whether or not a vault sits on top of the market.


**The governing distinction.** Under *Howey* 's fourth prong, a security exists only where profits are expected from the "efforts of others.” This does not include just any efforts. The relevant efforts are the essential *managerial* ones, the entrepreneurial judgment that determines the success or failure of the enterprise; purely administrative or *ministerial* tasks are expressly excluded. The same conceptual line runs through the Advisers Act, which is concerned with parties exercising discretionary authority over others' assets or providing investment advice for compensation. So, for each participant, the question is not whether they do something (they all obviously do) but whether what they do is an essential managerial effort or ministerial execution. Everything in this section is an application of that single question.


**The anchor precedent.** The most useful footing for the ministerial argument is the Commission's own liquid staking[guidance](https://www.sec.gov/newsroom/speeches-statements/corpfin-certain-liquid-staking-activities-080525) , which concluded that a provider taking custody of deposited assets and selecting a node operator was not engaged in essential managerial effort, because those activities are administrative in nature. The provider does not decide whether, when, or how much of a depositor's assets to stake, and instead acts as an agent facilitating the depositor's own choice. That reasoning is directly portable to some vaults. If selecting a node operator on a depositor's behalf is ministerial, then routing deposited assets into a pre-defined set of lending markets according to fixed parameters is at least arguably ministerial as well. In these cases, the operator is facilitating access, not exercising the entrepreneurial judgment that generates the return.


**The discretion gradient across roles.** The critical point, and the reason "are curators in the crosshairs?" has no single answer, is that these roles are not equally exposed. They sit along a gradient of discretion at different parts of the vault-market stack:


-


**Deployer** : the party that builds and controls the vault contract and establishes the general operating framework but does not direct where assets are deployed or how yield is generated. This is the most clearly ministerial role: building infrastructure is not directing its use.


-


**Curator** : the party that both defines the universe of markets a vault can access and sets the strategy by which user deposits are allocated across them. This is the pivotal role, and it is genuinely two-sided. The ministerial framing rests on two related points. First, the curator does not control the markets it selects and cannot itself generate their yield (this would imply vault curators set the price of borrowing, hold a monopoly on liquidity, and control borrow demand). It is simply assembling a menu of options, not running the enterprise that produces the return. Second, and more powerfully, those markets are typically public and directly accessible: a depositor could supply assets to them without the vault at all. On that view, the curator is offering convenience and packaging rather than access to an opportunity the user could not otherwise reach, and it becomes hard to say the user's profit flows from the curator's essential efforts rather than from a market the user could have entered on their own. This is the heart of the "conduit, not destination" characterization. The managerial framing pushes back: defining an allocation strategy across those markets, and revising it as conditions change, is itself an ongoing exercise of judgment about how pooled user capital is deployed, which is much closer to the entrepreneurial effort the ministerial exclusion is meant not to cover. Where the curator sits on the gradient therefore depends on two variables: how constrained the allocation strategy is (e.g. a fixed, rules-based allocation leans ministerial; an open-ended mandate to reallocate at will leans managerial), and how genuinely accessible the underlying markets are (fully permissionless, independently reachable markets strengthen the conduit argument, while bespoke or gated markets a user could not practically reach alone weaken it, because then the curator is supplying something more than convenience).


-


**Risk curator/parameter-setter** : the party that sets or adjusts the risk parameters of a market (e.g. interest-rate curves, collateral eligibility, loan-to-value ratios, liquidation thresholds, and other asset-level parameters). Of all the roles, this one is the most tempting to treat as categorically managerial since it involves direct judgments about a market's risk parameters. But that treatment does not mean it is strictly managerial in nature in every case. The same managerial-versus-ministerial test applies here as elsewhere, and there is a real case to be made that a suitably constrained parameter-setter falls on the ministerial side. Part of that case rests on what the role is actually for: a risk curator's function is to keep the protocol solvent and safe, not to generate higher returns for depositors. The yield participants earn is a function of the market's utilization curve and borrower demand; the risk curator's parameters exist to keep that mechanism operating safely, not to enhance the return it produces. That distinction matters under Howey, which asks whether profit flows from someone's efforts to generate the return, not from someone's efforts to prevent the whole thing from failing. A function aimed at loss prevention is fundamentally different from one aimed at profit generation, even where both require real judgment. Moreover, much of a risk curator's work is arguably framework-building rather than active management: establishing, up front, the rules by which a market will operate, much as one might design a system and then let it run. Where those parameters are fixed at deployment, or can be adjusted only within narrow pre-set bounds, on a timelock, and with an opportunity for depositors to exit before any change takes effect, the curator begins to look less like someone exercising ongoing discretion over user capital and more like someone administering a predetermined framework. The judgment involved is real, but it may be better understood as the kind of upfront, pre-deposit design effort that the 1996 case[SEC v. Life Partners](https://openjurist.org/87/f3d/536/securities-and-exchange-commission-v-life-partners-incorporated-d) suggests is only minimally related to the returns a depositor later realizes. Those returns still come from the market, not from the curator's continuing intervention. This is not to say the role is comfortably ministerial in every form; an unconstrained curator able to reset parameters unilaterally and instantly is much harder to defend on these terms. But the risk curator is not doomed to managerial treatment by the nature of its function alone. Like every other role on the gradient, where it lands depends on how tightly its discretion is bounded.


**The timing point.** Supporting the ministerial characterization for the earlier roles is the observation that much of the relevant effort occurs *before* a user ever deposits. Building the vault, defining its markets, and setting its initial strategy and parameters are pre-deposit activities; the yield a user later realizes is produced by the market, not by anything the operator does afterward. This tracks the reasoning in[Life Partners](https://openjurist.org/87/f3d/536/securities-and-exchange-commission-v-life-partners-incorporated-d) , where the D.C. Circuit found reliance on the "efforts of others" lacking because the promoter's post-purchase activities were minimally related to the investment's profitability. The argument is strongest where post-deposit activity is genuinely minimal and mechanical; it weakens considerably where the allocation strategy or risk parameters are actively and frequently revised after deposits are taken, because then the operator's ongoing judgment is once again bearing on the user's return. A caveat is that while this rationale remains D.C. Circuit law, the Fifth, Ninth, and Eleventh Circuits have declined to follow insofar as it draws a sharp line against counting pre-purchase managerial effort. It is invoked here for its ministerial/managerial framing rather than as a nationally settled rule.


**Where the argument breaks down.** As with the lending analysis, the honest version of this section has to mark its own limits, because several design realities pull the other way:


-


**The parameter-setting problem** : the claim that a vault's allocation is "automated" is true only at the moment of execution; the automation runs on a strategy and parameters a human chose. Defining the allocation strategy, and setting the interest-rate model, the optimal-utilization point, the LTV, and the liquidation threshold, is discretionary judgment, even if the enforcement of those settings is mechanical. The adversarial reading is direct: you may not set the rate, but you designed and can change the strategy and the functions that produce it. This argument is at its strongest when the strategy and parameters are fixed at deployment or constrained by timelocks, narrow bounds, and user opt-out rights, and at its weakest when a curator or governance body can reshape them unilaterally, quickly, and without recourse for depositors. The ministerial characterization, in short, is *earned* through concrete constraints on discretion — it is not conferred by the mere presence of a smart contract.


-


**Compensation** : fee structure is a central factor here in the same way it is throughout the analysis. A flat fee for providing infrastructure supports the ministerial characterization because it is priced for access, not tied to outcomes. A performance fee, or compensation calculated as a share of the yield generated (shades of the old “[2 and 20](https://ritholtz.com/2016/08/2-20-much/) ” charged by hedge funds), cuts hard the other way. In these cases, the operator's pay is aligned with their performance, which is the economic signature of someone being compensated for managerial effort rather than administrative service. Performance-based compensation is among the fastest ways to convert an otherwise-defensible ministerial posture into one that looks like management.


-


**The Advisers Act is a separate axis** : this is the point most easily missed. Even a party that clears the *Howey* ministerial bar can independently implicate investment-adviser status, because the Advisers Act asks a different question — whether a party is providing investment advice or exercising discretionary authority over assets for compensation. A firm paid a fee to determine and adjust a market's risk parameters, or to run a discretionary allocation strategy across markets, is exercising discretionary judgment over how others' pooled capital is deployed, which is close to the paradigmatic activity the Advisers Act regulates, regardless of how the *Howey* question is resolved. A design can therefore be ministerial enough to avoid being an investment contract while still placing the party setting its strategy or parameters within reach of the Advisers Act. The two analyses do not rise and fall together, and treating a favorable *Howey* answer as dispositive of the adviser question would be **** a mistake.


**A distinct question: the Investment Company Act.** Separate from all of the above, and important not to blur into it, is the Investment Company Act, which does not turn on discretion at all. The ICA question is whether the vault is functionally a pooled vehicle whose character resembles a registered investment company, and it turns substantially on what the vault holds. A vault could be operated in an entirely ministerial fashion and still face ICA exposure purely because it pools user assets and allocates into securities. This is also where the distinction between a unit investment trust and a management company lives: a vault with a fixed, unchanging set of holdings resembles a UIT (a fixed, unmanaged basket), while a vault whose holdings are actively reallocated resembles a management company (which presupposes an ongoing manager). The takeaway is that the discretion analysis and the ICA analysis can point in different directions — a low-discretion vault is a better fit for the UIT characterization, not an escape from the Investment Company Act altogether — and each has to be run on its own terms.


The right way to read Peirce’s statement is not that curators and market maintainers are categorically securities-law actors, nor that they are categorically safe. Exposure runs along a spectrum of discretion: the deployer sits relatively most comfortably on the ministerial side; a risk curator with ongoing, unfettered authority over a market's parameters sits much closer to the managerial side; and in between, the curator’s status may depend on how bounded its market selection and allocation strategy are. Which side any given participant lands on is determined by concrete, observable design choices (e.g. how fixed the strategy and parameters are, how quickly and unilaterally they can be changed, whether users can opt out, whether the markets are independently accessible, and above all how the party is compensated) not by the label the participant happens to wear. And because the Advisers Act and the Investment Company Act ask their own questions, a favorable answer under *Howey* resolves only part of the problem. The layered structure that discussed above reappears here at the level of the individual actor, where clearing one test does not clear the others.


## Conclusion


Taken as a whole, Peirce’s statement is best read not as a verdict but as a map of where the hard questions live. The two structures demand different tests — *Reves* for lending, the Investment Company Act and Advisers Act for vaults — and neither test resolves with a label; each turns on concrete facts about how a system is built and who actually exercises judgment.


On the lending side, we see the strongest ground is that a non-custodial, pooled protocol has no central obligor raising capital or promising a return, though the pooled interest a depositor holds still invites scrutiny under other theories.


On the vault side, we find exposure runs along a gradient of discretion, with the answer depending on how bounded the strategy and parameters are and, above all, how the operator is paid.


The through-line is that design and doctrine are hard to separate here. Details like custody, discretion, parameter mutability, redemption mechanics, and compensation tend to be what move a given activity toward or away from the scope of securities laws. This is why how a system is built is likely to matter as much as what it is called.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
