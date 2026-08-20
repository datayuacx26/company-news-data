---
schema_version: "1.0.0"
document_id: "4cf7f938411a206bbd443ded85fd61d697229bd96d30ea09027955415455b1c0"
company_key: "yc-grey"
company: "Grey"
source_id: "yc-grey-news-import-fd6b968aa507"
canonical_url: "https://grey.co/blog/who-pays-wire-transfer-fees"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-21T22:07:43.040277+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:ffbb469f309433d89598616da95c860389779e2809b103028351fb248533b8b5"
---

# Who pays Wire transfer fees: Sender or Recipient?

Wire transfers are among the oldest and most reliable methods for sending money, especially internationally. The sender initiates the transfer in their currency. The payment instruction goes through the SWIFT system. The money moves from one bank to another until it arrives at its destination. However, in reality, wire transfers are not so transparent. There are various undisclosed fees for both the sender and receiver, and even between the intermediary banks.


The sender typically pays the wire transfer fee at their own bank, but the recipient's bank can also charge an incoming wire fee, usually $10 to $25. For international wire transfers, correspondent banks in the middle of the transfer chain can deduct their own fees, reducing the amount the recipient receives. The sender can choose who pays via SHA, OUR, or BEN instructions.


Understanding who is responsible for each fee, and how to control this, matters whether you are sending money to a supplier abroad or receiving payment from an international client.


## Who pays wire transfer fees?


The simple answer is that the sender pays the outgoing fee charged by their bank to initiate the transfer. This fee differs across banks, countries, and currencies. US commercial banks usually charge $15 to $45 for initiating an international transfer, while UK banks charge £15 to £30.


However, this is not the only fee involved in wire transfers. The bank receiving the wire transfer also charges an incoming wire transfer fee when the funds arrive. This is typically $10 to $25 in the US. And for international wire transfers specifically, there are correspondent or intermediary banks that can deduct their handling fees from the amount sent.


So, who ends up paying the wire transfer fees? The more practical answer is that it depends on which charge code the sender uses when initiating the transfer. The three options are SHA, OUR, and BEN, and they divide the fee responsibility differently. We will explain this better later in this article. However, most banks default to SHA unless the sender specifies otherwise.


Bank wire charges, in short, can come from three sources: the sending bank, the receiving bank, and any correspondent banks in between. The charge code determines which of those the sender covers and which the recipient absorbs.


## Does the sending bank or the recipient bank charge the fee?


Both can charge, and in most international wire transfers, both the sending and receiving banks will do.


### **Sending bank fee**


The outgoing wire fee is charged by the sender's bank. This fee covers processing and transmitting the payment instruction via the SWIFT network. The sender pays this fee and it is deducted from their account separately from the transfer amount. So, it does not reduce the amount sent. They just have to have enough in their balance to cover the fee.


Typical outgoing wire fees at major US banks:


- Bank of America: $45 for international wires in foreign currency
- Chase: $40 for international wires online
- Wells Fargo $25 for digital wires and $40 for branch wires
- Citibank: $35 for most international wires


### **Receiving bank fee**


The recipient’s bank charges an incoming wire fee on international transfers when the money lands. This fee is deducted from the amount transferred before the money even fully reaches its destination. In essence, the recipient eventually gets less than what was sent without any heads-up.


Typical incoming international wire fees at major US banks:


- Bank of America: $15 per incoming international wire
- Chase: $15 per incoming international wire
- Wells Fargo: $15 per incoming wire
- Citibank: $15 per incoming international wire


**Who pays what in practice:** The sender always pays the outgoing fee. The recipient typically bears the incoming fee because it is deducted from the arriving funds. The SHA charge code, which is the default at most banks, splits additional correspondent bank fees between the parties depending on the routing path.


## Wire transfer fee: sender vs receiver


Understanding which fee each party bears depends on the transfer type and the selected charge code.


Fee type Who bears it When charged Typical amount


Outgoing wire fee (sending bank) Sender When transfer is initiated $15 to $45 (US banks)


Incoming wire fee (receiving bank) Recipient Deducted from the amount sent when it lands $10 to $25 (US banks)


Correspondent bank fees (SHA) Both (split arbitrarily) Deducted in transit from the amount sent $10 to $25 per correspondent bank


Correspondent bank fees (OUR) Sender Charged upfront or separately by the sending bank Variable; bank-dependent


Correspondent bank fees (BEN) Recipient Deducted in transit from the transfer amount $10 to $25 per correspondent bank


**How to know the fees in advance:** Sender banks usually list their outgoing wire fees in their fees. Same for the incoming fee at the recipient’s bank. The dilemma is usually with the correspondent bank fees. The exact amount is generally unknown until the transfer is completed, because it is determined by the banks the money goes through.


**Learn about how**[telegraphic transfers work](https://grey.co/blog/telegraphic-transfer) **.**


## International wire transfers: who pays which fees?


Domestic wire transfers occur within the same country, so they are generally straightforward. Most of the time, the outgoing fee from the sender and the receiving bank's incoming fee are the only fees applied. The transfer moves directly between them with no intermediary.


International wire transfers are more complex and involve a longer chain. The SWIFT network does not move money directly from the sender’s bank to the recipient’s because they are in different countries. Instead, it sends the payment instructions through correspondent or intermediary banks. These banks are large financial institutions that have bank accounts at other banks in multiple countries and act as intermediaries.


Here is what the process often looks like:


1. Sender's bank (Country A) initiates the transfer
2. Correspondent Bank 1 (major US bank) routes to Country B's network
3. Correspondent Bank 2 (regional bank in Country B) routes to the receiving bank
4. Receiving bank credits the recipient's account


At step 2 and step 3, each correspondent bank deducts a handling fee of $10 to $25 from the transfer amount. By the time the money reaches the recipient, a $1,000 transfer may have been reduced to $950 to $960 after two correspondent bank deductions and the receiving bank's incoming wire fee.


Let’s try an example. Say the amount sent is $1,000


- **Sending bank outgoing fee:** $35 (paid by sender separately)
- **Correspondent Bank 1 deduction:** $15 (deducted in transit)
- **Correspondent Bank 2 deduction:** $20 (deducted in transit)
- **Receiving bank incoming fee:** $15 (deducted on arrival)
- **Amount credited to recipient:** $950
- **Total fees involved:** $85


The sender paid $35 plus the amount sent. The recipient received $50 less than expected. Neither party was informed of the correspondent bank deductions in advance. This is one of the most common sources of confusion and dispute in international wire transfers, and it is the reason the charge code matters so much.


## SHA, OUR, BEN: what these wire transfer instructions mean


SWIFT, the messaging network that routes international wire transfers, uses standardised charge codes to determine who is responsible for correspondent bank fees in transit. These codes are formally defined in SWIFT messaging standards and are selected by the sender when initiating the transfer.


### **SHA (Shared)**


The SHA instruction means that the sender pays the fees charged by their own bank, and the recipient pays any fees charged by correspondent banks and the receiving bank. These fees are deducted from the transfer amount while it is in transit.


SHA is the default at most banks. It means the recipient receives less than the amount sent whenever correspondent banks are involved.


### **OUR**


The OUR instruction means the sender covers all fees, including those charged by correspondent banks in the routing chain. In theory, the recipient receives the full amount. In practice, the result varies because the sending bank cannot always guarantee what correspondent banks will charge. Some banks that offer OUR simply add a flat fee (often $25 to $35) to the transaction to cover anticipated correspondent costs.


OUR is the instruction to use if you want the recipient to receive the full transferred amount. Confirm with your bank how they handle OUR before relying on it for a specific amount.


### **BEN (Beneficiary)**


The BEN instruction means the recipient pays all fees, including the sending bank's fee, which is deducted from the transfer amount before the recipient receives it. This is rarely in the sender's interest and is seldom used.


**Which code to request:** If you are sending money and want the recipient to receive a specific amount, use OUR and confirm with your bank how it will apply. If you are the recipient expecting a specific amount, ask the sender to initiate the wire with OUR.


## How to avoid hidden wire transfer fees


Several approaches reduce or eliminate the fees that reduce what the recipient receives.


- **Specify OUR if the recipient must receive a specific amount:** For business payments, supplier invoices, or tuition fees with an exact due amount, OUR protects the recipient from shortfalls. Confirm with your bank how they apply OUR and whether they charge an additional fee for it.
- **Ask for a fee disclosure before initiating:** Your bank is required to disclose fees before processing an international wire. Ask explicitly for the outgoing fee, whether correspondent bank fees are covered, and what the recipient's bank typically charges. You will not always get a precise answer on correspondent fees, but asking the question establishes it.
- **Consider fintech alternatives for the transfer:** For many international payment corridors, fintech platforms route money through local payment networks rather than SWIFT, avoiding correspondent banks entirely. A payment sent via ACH in the US, Faster Payments in the UK, or SEPA in Europe moves as a domestic transfer with no correspondent bank chain and no mid-transit deductions.


Grey processes international transfers through local payment rails where possible. When a UK client pays a recipient's Grey USD account via ACH, the transfer moves entirely within the US banking network. There are no correspondent bank deductions because there are no correspondent banks in the chain. You can[send money internationally with Grey](https://grey.co/money-transfer) with full visibility on fees before you confirm.


## Frequently asked questions


### **Can the sender pay all wire transfer fees?**


Yes, by selecting the OUR charge code when initiating the wire. OUR instructs correspondent banks to route the sender's payment obligation rather than deducting from the transfer amount. However, the application of OUR varies by bank, and some correspondent banks may still deduct fees despite the instruction. Confirm with your sending bank how they handle OUR and whether there is an additional charge for selecting it.


### **What is an incoming wire transfer fee?**


The recipient's bank charges an incoming wire transfer fee when a wire payment arrives. It is typically deducted from the transfer amount rather than charged separately to the recipient's account, which means the recipient receives less than what was sent. US banks typically charge $10 to $25 per incoming international wire. Some banks waive this fee for premium account holders.


### **Why does my recipient get less than what I sent?**


The most common reasons are: the recipient's bank deducted an incoming wire fee from the transfer amount on arrival; one or more correspondent banks in the routing chain deducted handling fees in transit; or the exchange rate applied differed from the rate you saw when initiating the transfer. Selecting the OUR charge code and confirming the routing path with your bank before sending reduces but does not always eliminate this issue.


### **What is OUR in a wire transfer?**


OUR is a SWIFT charge code that instructs the transfer to be processed such that the sender bears all fees, including those charged by correspondent banks in the routing chain. It is the opposite of SHA, which splits fees between sender and recipient by deducting them from the transfer amount. Banks may charge an additional fee for OUR transfers, and the guarantee that the full amount arrives is not absolute because not all correspondent banks honour the OUR instruction in every scenario.


### **Do all banks charge incoming wire fees?**


No. Some banks waive incoming wire fees for premium account tiers, high-balance customers, or as part of a business account package. Online banks and some credit unions do not charge incoming wire fees. If you regularly receive international wires and your bank charges an incoming fee, it is worth asking whether your account type qualifies for a waiver or whether another account tier eliminates the charge.


### **Does Grey deduct fees from the transfer amount?**


Grey does not route transfers via SWIFT for payments that can be processed through local networks such as ACH, Faster Payments, and SEPA. For those transfers, there are no correspondent banks and therefore no mid-transit deductions. Grey charges a deposit fee of 0.8% of the amount received (minimum $2, maximum $10), plus a currency conversion fee where applicable. These fees are disclosed before the transfer is confirmed.


[Send money internationally with Grey](https://grey.co/money-transfer) with transparent pricing and no correspondent bank deductions on supported corridors.
