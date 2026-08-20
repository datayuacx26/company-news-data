---
schema_version: "1.0.0"
document_id: "26aa27e136938f8ba7002bf4c7e57e1c6bac35d2c37f0bf6db358ac7fd377863"
company_key: "yc-palenca"
company: "Palenca"
source_id: "yc-palenca-rss-c9f8371bccc6"
canonical_url: "https://blog.palenca.com/brazil-the-frontier-of-open-finance-in-latin-america/"
published_at: "2023-11-17T16:57:29+00:00"
first_seen_at: "2026-07-25T18:13:53.501952+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:16ea599a62682364ca1a0e6d164b9801438fcecd515ccbafedf022517a3f644a"
---

# Brazil, the Frontier of Open Finance in Latin America

How we got here and where (we think) we're heading


## The origin story:


David and Goliath by Dan Craig


In 2012 Brazilian entrepreneur Thiago Alvarez and his American co-founder Ben Gleason created Guiabolso. The idea was bold and innovative for the time: banks were starting to create online portals and apps where users could login and access their bank account, so they allowed users to share the data from these accounts by giving Guiabolso their usernames and passwords. With this data, Guiabolso would be able to give the user a neat PFM (Personal Financial Management) tool to monitor their savings and expenses across all the bank accounts they wanted to connect. Besides the PFM tool, users would also be able to access a marketplace for curated credit products based on the data they had shared. The incentive for the end user to share their banking information was very clear: *use **your data** to give better information and get access to tailored products* .


Guiabolso went on to raise USD $66m from investors like Kaszek and QED. The most important part of the story, however, came in 2016 when Bradesco, one of the largest banks in Brazil, sued Guiabolso. Bradesco argued that the access to the customers’ credentials violated the contract entered by the customer and the bank; in other words, the data belonged to Bradesco and not the customer. The Ministry of Finance joined the court proceedings as an interested party, issuing an opinion saying that *the bank account information belongs to the customer and not to the bank* , and required the Administrative Council for Economic Defense (CADE in portuguese) to open an investigation for abuse of dominant power. At the end, both parties came to an agreement which set a few rules:


- Bradesco had to develop secure API’s so Guiabolso could access the information of customers who gave their consent.
- Users had to give an explicit consent when sharing their data that allowed Bradesco to give Guiabolso access to the user’s information.
- The consent had a validity of 12 months (last week the Central Bank changed it to forever), but the user could always revoke the consent.


Does this seem familiar? It should. Guiabolso was ultimately acquired by PicPay in 2021, but they set the precedent for what today we all know as Open Finance. The most important idea being that the account information belongs to the user and not the institution.


## Where are we now:


The Guiabolso vs Bradesco case laid the groundwork for the regulation the Brazilian Central Bank set for Open Finance (which in turn is the regulation the Mexican Central Bank attempted to copy). As of this writing, the Brazilian Central Bank is in the process of implementing the fourth of four phases for it’s rollout of Open Finance, here is a very brief overview of each phase:


- **Phase 1:** Participating financial institutions (banks) facilitated the transfer of data amongst themselves. This means banks created the API’s so they can share their data to other banks and they can get the data from the others.
- **Phase 2:** Users can share their data from any bank they want to with other banks, this is exactly what Guiabolso was doing, only a regulated and “official” version. You can see how it works in PicPay, which is the company that acquired Guiabolso, it is likely that they used the Guiabolso app to build this. First Users connect their accounts, then they can get a PFM with the aggregated data from all their accounts:


From the 2022 let's open MegareportFrom the 2022 let's open Megareport


- **Phase 3:** Launching products on top of Open Banking: besides sharing data, new products like payments can be built through the same API’s. In this case a user can use the same process of connecting their bank account to an app like Guiabolso, for approving payments directly from their bank account. For example, this is how it looks in Mercado Pago.


From the 2022 let's open Megareport


- **Phase 4:** Sharing data for products other than bank accounts: in the same way that users can share the data of their savings and checking accounts, they will be able to share data for investments, insurance and FX.


The Brazilian Central Bank has been incredibly consistent with the implementation of each phase and we can expect the completion of the last phase very quickly. The Mexican Central Bank has been incredibly consistent in its incompetence and we can expect their rollout and implementation to go the same way as CoDi (if you don’t know what this is don’t worry, no one else knows).


All of this was explained quite well by the folks at a16z in last year’s piece “[Brazil’s Surprising Fintech Winds](https://a16z.com/brazils-surprising-fintech-tailwind/?ref=blog.palenca.com) ”, except that the companies they mentioned, mainly Belvo and Datanomik, are not quite the regulated Open Finance they talk about. These companies, like Palenca, are in the *unregulated* Open Finance space, they make the same connections and have the same legal fundamentals as Guiabolso for operating. They have followed the same steps as the regulated Open Finance, they allow users to connect their banking data with third parties; for example, when an SME wants a loan with Mercado Libre, the company can connect their bank accounts with Belvo to show their cash positions and movements. Now, they are also rolling out payment products, where users can connect their bank accounts with third party apps to make payments, in addition to sharing their data. For this, companies like Klavi and Belvo have acquired the license to be an *Iniciador de Pagamentos* in Brazil (and an *Institución de Fondos de Pagos Electrónicos* in Mexico).


**Open Finance players focused on banking integrations:**


For Brazil, it can be quite confusing why companies would be focused on doing something that essentially became a commodity for banks, but there are three reasons why these companies are playing an important role:


1. First, only regulated banks with licenses can implement the regulated Open Finance, so if a non-bank fintech, lender or other type of company wants to use this data for underwriting, they need one of these players.
2. Second, focus. While Open Finance is something that all banks must have, it is one of tens of other tech products that they are subsequently launching for their apps: payments, digital cards, easy onboarding experiences, etc. The likelihood that Open Finance is a priority for them is low, because it’s not something they will make money on for every transaction, like a payment or swiping their credit card. On the other hand, these companies are *exclusively* focused on making these integrations work, and it is the *only* way they are making money, so the quality of their integrations and the data they are getting is likely to be significantly better than the ones the banks are using.
3. This leads to the third and most important point, the *incentive* of the user to connect their bank account. My coworker, Luiz, has an excellent phrase about Open Finance: “ *É um jogo de incentivos* ” (it’s a game of incentives). When an SME or a person is applying for a loan within an app that uses Belvo or Klavi, they are asked to share their banking information as part of the application, therefore the user understands that the data in his bank account will be used to determine his capacity to pay back the loan. If a person is already applying for a loan, he already has a strong incentive to share this data. If you look at the previous example from PicPay, you’ll see that a user needs to go to a specific part of their app to willingly share their data with PicPay. See the difference? It’s the incentives, and this is the biggest misconception about Open Finance. A user needs to know the benefit to share their data, if this is not made abundantly clear the moment they are asked to share their data, it is unlikely they will share it.


## The Frontier - where are we headed:


The story of where we are and how we got here should be quite clear, and now (I hope) you have a pretty clear picture of how Open Finance has been implemented in Brazil and where startups are playing a part. Very odd that the guy from Palenca has not mentioned Palenca, but this is where I’m going, the frontier of Open Finance.


For this, I want to go back to the core principle of Open Finance that came from the Guiabolso/Bradesco case: *use **your data** to give better information and get access to tailored products* . The question to get to the frontier of Open Finance is very simple: which data is next? For us the answer came by accident. Palenca started as a lending company for Gig Workers in Mexico. We thought with banking data we’d be able to underwrite easily and better than banks, but that wasn’t quite the case: users either didn’t connect their accounts or there simply wasn't any data available, they had many bank accounts and they weren’t connecting their main accounts. This is why we decided that we had to connect **directly to the income source:** the Gig Economy apps. The connection was done in the same way as Guiabolso did for banks, with the user’s credentials we got the data. Quickly we found the answer for what data was next: income and employment data. Palenca was born pivoting from lending to just making the API to get the data for all the other lenders. This is where the vision got much broader. Income and employment data can be found in many different sources: gig economy apps (for gig workers), government systems, private payroll systems, catalog sales apps, and many others.


This is how a worker will select their employment type.


If users can already connect their bank accounts (where you can find the income data), what’s the point of making this data shareable for users? Most applications for financial services already require users to submit proof of employment and proof of income, many of which are still manual, in Brazil sending a PDF of your *holerite* (paystub) is quite common. Banking integrations *can* provide this data, you’ll logically have a debit in your account each month for the amount of your income coming from your employer. Now the frictions here can be seen from two perspectives: from the user *sharing* the data and from the institution *using* the data.


- **For the user** , sharing your bank account data is quite straightforward when you are applying for a financial service, you will prove your income and if you have enough money to pay back. However, when you connect your bank account, that’s not the only data point you are sharing, you’re sharing your entire bank account data! Have one or two unsavory subscriptions? That will show up. Shopaholic? That will show up. These data points are not necessarily a problem, and not necessarily something the lending institution will use *against* the user, but as the person sharing your data, it makes you think twice about how much you are willing to share.
- **For the institution** this can be an issue as well, imagine having to be the person filtering all this information just to get to the data point you need to use, or going back X amount of months until you find that the user started getting paid for that job. Companies (especially the unregulated ones) are doing a great job at filtering this data and turning it directly into credit scores but you can see how there are some frictions in sharing and using this data.


We’re not actually getting this kind of information. The sources we are connecting to only contain Income and Employment data. For the user, there’s less friction in sharing an account that only contains information of where you work and how much you’re earning, especially if you’re already applying for a loan and know that you will be required to provide this information. Additionally it’s much easier to connect an account than to download a PDF and send it to the lender. For the lenders, it’s a more straightforward data point that can easily be plugged into their underwriting models. Take a look:


Example of a formal worker in Brazil, once they connect their account.


We're not the first or the only ones in this thesis, but at Palenca, we think employment and income data is the next frontier of Open Finance. We want to make it possible for any worker in Latin America to be able to share where they are working and how much they are earning when they need to share this information in a safe and reliable way.


## Useful Resources:


- By far the best writer on Open Finance in Brazil, is Gabriel Pereira. His[blog Let's Open](https://www.letsopen.com.br/?ref=blog.palenca.com) is amazing.
- [Let's Open 2022 Megareport](https://www.letsopen.com.br/p/open-finance-mega-report-2022?ref=blog.palenca.com)
- [PicPay blog](https://meajuda.picpay.com/hc/pt-br/articles/4407870332691-Quais-s%C3%A3o-as-fases-do-Open-Finance-?ref=blog.palenca.com) on the four phases of Open Finance.
- [a16z's post](https://a16z.com/brazils-surprising-fintech-tailwind/?ref=blog.palenca.com) on fintech in Brazil from last year.
- If you want to understand the business model of these startups (API First), Packy McCormick's Not Boring has a[very good piece](https://www.notboring.co/p/apis-all-the-way-down?ref=blog.palenca.com) .
