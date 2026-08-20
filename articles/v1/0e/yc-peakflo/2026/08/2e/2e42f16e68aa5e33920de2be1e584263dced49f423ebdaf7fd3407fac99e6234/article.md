---
schema_version: "1.0.0"
document_id: "2e42f16e68aa5e33920de2be1e584263dced49f423ebdaf7fd3407fac99e6234"
company_key: "yc-peakflo"
company: "Peakflo"
source_id: "yc-peakflo-news-import-4ba227f4ca0c"
canonical_url: "https://peakflo.co/blog/treasury-finops-manual-payment-execution-ap-automation-last-mile"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-07T19:45:08.119073+00:00"
fetched_at: "2026-08-07T19:45:10.039918+00:00"
content_hash: "sha256:c94b533e2b7bd0aba80b92f271e48c9115f496f2b002b05cf388f015fc366b49"
---

# The Last Mile Problem in AP: Why Treasury Teams Still Execute Payments Manually After Invoice Approval

**TL;DR:** Most AP automation implementations solve the intake, coding, and approval problem — but leave the treasury team to manually execute every payment after approval. This “last mile” gap consumes 15-25 hours of treasury time per month for organizations processing 400+ payments across multiple entities. Fully automated payment runs with maker-checker controls, bank API connectivity, and automatic ERP posting eliminate this bottleneck and complete the end-to-end AP cycle without manual intervention.


## What Is the Last Mile Problem in AP, and Why Does It Persist?


Ask a FinOps or treasury team what their typical day looks like, and the answer often sounds like this: invoices are approved by the end of the previous day, but then the treasury team spends the morning reviewing what’s approved, logging into the bank portal, manually preparing payment files, executing transfers, and updating the ERP with payment confirmation. The approval is the easy part. The payment is where the manual work begins.


This is the AP last mile problem: the gap between invoice approval and actual payment execution that most AP automation platforms do not close. The invoice capture is automated. The GL coding is automated. The multi-level approval workflow is automated. But the moment an invoice is approved and a bill is created in Oracle NetSuite, the automation stops — and a treasury team member picks up the work manually.


For organizations processing hundreds of vendor payments per month across multiple entities, this last mile is not a minor inconvenience. It is a meaningful portion of the FinOps team’s weekly capacity — consumed by repetitive, low-judgment tasks like selecting approved invoices, preparing bank upload files, logging into portals, and reconciling payments back into the ERP.


The problem persists because most AP automation vendors treat payment execution as someone else’s problem. They focus on invoice processing and approval routing — areas where document AI and workflow automation deliver visible, demonstrable value — and position the payment step as “handled by your bank.” The result is that organizations that implement AP automation still have a substantial manual gap between approval and payment.


According to[analysis from Ardent Partners on AP performance benchmarks](https://ardentpartners.com/) , only about 28% of organizations report fully automated payment execution as part of their AP process. The remaining 72% still have significant manual steps between invoice approval and payment completion — making the last mile one of the largest remaining opportunities in AP automation.


## How Does Manual Payment Execution Actually Work in Practice?


To understand what needs to be automated, it helps to map exactly what happens in a typical treasury team’s manual payment workflow after invoices are approved.


**Step 1 — Review approved invoices:** The treasury team logs into the AP platform or ERP and reviews the list of approved bills ready for payment. For multi-entity companies, this review spans multiple subsidiaries, each with their own approved invoice queue.


**Step 2 — Select invoices for the payment run:** The team manually selects which approved invoices to include in the current payment batch based on due dates, cash positions, and any pending exception holds. Invoices due within the next seven days are typically prioritized.


**Step 3 — Validate vendor bank account details:** Before preparing the payment file, the team checks that vendor bank details are current — particularly for vendors who have recently changed their accounts or whose details may differ between subsidiaries.


**Step 4 — Prepare the bank payment file:** The team exports a payment file from the ERP or AP platform, formatted for the bank’s upload system. For companies using multiple banks or payment methods across entities, separate files may need to be prepared for each bank.


**Step 5 — Log into the bank portal and upload:** The team logs into the bank’s online portal, uploads the payment file, and reviews the batch summary before submission. For banks that require additional authentication — token-based OTP, dual authorization within the bank system — additional steps are required.


**Step 6 — Execute the payment and confirm:** After the bank processes the file, the team reviews payment confirmations and notes any rejections due to invalid bank details or insufficient funds.


**Step 7 — Update the ERP:** The team manually marks each paid bill as settled in NetSuite and posts the payment journal entries. For multi-entity companies, this update must be performed separately for each subsidiary.


**Step 8 — Send remittance advices:** The team notifies each vendor of their payment, including the invoice references and payment date, typically via email.


This sequence repeats weekly — or more frequently for organizations with urgent payment needs. The total time investment for a 400-payment-per-month organization across 6 entities is typically 15-25 hours per month: skilled FinOps and treasury capacity spent on tasks that add no analytical value.


## What Is the Real Cost of the AP Payment Execution Gap?


The cost of manual payment execution extends beyond the direct labor hours consumed. The table below breaks down the full impact across time cost, financial risk, and strategic opportunity cost.


Cost Category Manual Payment Execution Automated Payment Execution


Treasury time per payment 3-5 minutes per payment Near zero (batch processed)


Monthly labor for 400 payments 20-33 hours 2-4 hours (review and approve only)


ERP reconciliation after payment Manual, 2-5 hours/month Automatic write-back, near zero


Vendor remittance notifications Manual emails, 1-2 hours/month Automated on payment execution


Risk of missed payment due dates High (depends on individual availability) Low (scheduled runs with due-date alerts)


Real-time cash position accuracy Delayed until ERP is manually updated Immediate (auto-post on execution)


Audit trail for each payment decision Partial (email records) Complete digital trail in platform


Beyond the direct labor and risk costs, there is a strategic opportunity cost: every hour a treasury team member spends manually executing payments is an hour not spent on cash flow forecasting, financing strategy, or bank relationship management — the activities where treasury expertise generates measurable business value.


## Why Does AP Automation Stop Short of Payment Execution for Most Companies?


Several structural factors explain why the payment execution gap persists even for organizations that have invested in AP automation.


**Bank connectivity complexity:** Connecting an AP platform directly to a bank’s payment systems requires bank-specific API agreements, security certifications, and technical integrations that vary by bank and by country. For organizations operating across multiple banks in multiple countries — such as companies with entities in both Indonesia and Singapore — establishing these connections with each bank individually is a significant implementation project.


**Regulatory and compliance constraints:** Payment execution is subject to banking regulations, central bank oversight, and AML/KYC requirements that impose additional verification steps. In some markets, dual-authorization requirements (maker-checker) are mandated by regulators for corporate payments above specified thresholds. AP automation vendors that do not specialize in treasury and payment operations often avoid this regulatory complexity by leaving payment execution to the bank’s own portal.


**Organizational separation between AP and treasury:** In many finance organizations, accounts payable and treasury are managed by different teams with different system ownership. The AP team manages the invoice-to-approval workflow; the treasury team manages cash and payments. This organizational boundary often maps to a system boundary — AP automation on one side, bank portals on the other — with no automated bridge between them.


**Fear of automation risk in payment execution:** Finance teams are often more risk-averse about automating payment execution than about automating upstream processes, because payment errors are more financially consequential than coding errors. Manual control points in payment execution feel safer, even when they are operationally inefficient, because they provide a perceived opportunity to catch errors before funds leave the account.


## How Does Automated Payment Execution with Maker-Checker Controls Work?


Fully automated payment execution does not mean removing human oversight from payments. It means restructuring that oversight so that treasury team members review and approve a payment batch rather than manually executing each individual payment.


The maker-checker model preserves the control function while eliminating the repetitive execution tasks. Here is how a structured, automated payment run operates:


**Automated batch preparation (maker function):** The AP platform automatically aggregates all approved invoices due within the payment window, validates vendor bank account details, applies the correct payment method per vendor and entity, and generates a payment batch summary. This preparation happens automatically on the scheduled payment run date without any manual intervention.


**Treasury review and approval (checker function):** The treasury team receives a notification that a payment batch is ready for review. They review the batch summary — total amount, number of payments, any exceptions or flagged items — and approve with a single digital authorization. For payments above defined thresholds, an additional approver tier is triggered automatically.


**Automated bank submission:** After the checker approves the batch, the platform submits the payment file to the bank via API or structured file transfer. Payment confirmations are received from the bank and recorded in the platform.


**Automatic ERP write-back:** On payment execution, the platform automatically posts the payment journal entries in NetSuite, marks the corresponding bills as paid, and updates the cash account ledger — with no manual ERP intervention required.


**Automated remittance notifications:** Vendors receive automated payment confirmation emails with invoice references and payment amounts immediately after execution.


The result is that the treasury team’s role shifts from manual execution (repetitive, low-value) to exception review and approval (high-value oversight) — which is where treasury expertise should be deployed.


## How Peakflo Closes the AP Last Mile for FinOps Teams


[Peakflo’s AP automation platform](https://peakflo.co/accounts-payable) is designed to close the full AP cycle, not just the approval stage. The platform includes structured payment run management that connects invoice approval directly to payment execution with built-in controls.


Approved invoices from any entity flow into the payment run queue automatically. The treasury team reviews a consolidated payment dashboard showing all approved, due, and overdue invoices across entities — in one view, not entity by entity. Payment batches are prepared automatically based on configurable payment windows and due-date rules.


The maker-checker flow is native to the platform. The payment preparer (maker) reviews the batch and submits for approval. The designated approver (checker) receives a notification, reviews the batch summary, and approves with a digital signature. Once approved, the payment file is transmitted to the bank automatically.


Upon payment confirmation,[Peakflo’s NetSuite integration](https://peakflo.co/integrations/netsuite) automatically writes the payment back to the correct subsidiary’s ledger — marking bills as paid, posting journal entries, and updating the cash account balance in real time. The treasury team no longer needs to log into NetSuite to complete the payment cycle.


For organizations that need to understand how this fits into the full procurement-to-payment journey,[the complete guide to procure-to-pay automation](https://peakflo.co/blog/procure-to-pay-automation-guide) provides context on how payment automation connects to the upstream purchasing and approval cycle.


For teams managing cross-border payments,[Peakflo’s multi-currency payment processing capabilities](https://peakflo.co/blog/multi-currency-payment-processing) ensure that currency selection, FX conversion, and local payment method routing are handled within the same automated payment run — without requiring separate bank portal sessions for each currency.


## How Does the Last Mile Gap Affect Cash Flow Visibility?


The last mile problem has a secondary consequence beyond labor costs: it creates a real-time cash visibility gap. When payment execution and ERP posting are manual and delayed, the ERP’s cash balances do not reflect the true current position. The AP team sees approved bills that appear unpaid. The treasury team knows payments have been initiated but the bank confirmation has not yet been posted. The CFO reviewing the cash position report sees numbers that are several days stale.


For multi-entity companies managing cash across multiple subsidiaries and currencies, this visibility gap compounds. Each entity’s ERP may show a different lag between payment execution and posting, making consolidated cash reporting across the group unreliable.


Automated payment execution with immediate ERP write-back eliminates the lag. The moment a payment executes, the ERP ledger reflects the updated cash position. Consolidated cash dashboards across entities are accurate in real time, enabling treasury to make liquidity decisions based on current data rather than approximations.


The[relationship between AP automation and cash flow forecasting accuracy](https://peakflo.co/blog/automated-payment-processing-cash-flow-predictions) is direct: the faster and more completely payments are posted, the more accurate cash flow visibility becomes — which is why payment execution automation and cash management are increasingly treated as a unified capability rather than separate functions.


## What Should Finance Teams Look for in a Payment Execution Automation Platform?


When evaluating whether an AP automation platform truly closes the last mile, the following capabilities distinguish full payment execution automation from partial solutions that stop at invoice approval.


Capability Partial Solution (Stops at Approval) Full Payment Automation


Invoice capture and OCR Yes Yes


GL coding and approval workflows Yes Yes


Scheduled payment run preparation No Yes


Pre-payment bank account validation No Yes


Direct bank API connectivity No Yes


Maker-checker payment authorization No Yes


Multi-entity, multi-bank support Limited Yes


Automatic ERP write-back on payment No Yes


Automated vendor remittance notifications No Yes


Real-time cash position update No Yes


Organizations evaluating AP automation platforms should explicitly ask vendors: where does your automation end? If the answer is “at invoice approval” or “we generate a payment file that you upload to your bank,” the platform does not address the last mile problem.


## Our Verdict: Is Full Payment Execution Automation Worth Implementing?


After examining the labor costs, risk profile, and strategic implications of the AP last mile gap, the answer is unambiguous for high-volume finance teams.


**Implement automated payment execution immediately if:**


- Treasury team members spend more than 5 hours per month on manual payment preparation and execution
- Payment execution requires logging into one or more bank portals rather than executing from a centralized AP platform
- Post-payment ERP reconciliation is a manual, recurring task
- Cash position reports are delayed because payment postings take days to complete
- Multiple entities create separate payment execution cycles that compound the labor burden


**Lower priority if:**


- Payment volume is under 50 transactions per month with a single bank and entity
- Bank portal supports direct payment execution from the AP platform via file upload with minimal manual steps
- Treasury team capacity is not a constraint and payment automation is not currently planned


**Our recommendation:** For any organization where invoice approval is already automated but payment execution remains manual, closing the last mile is the highest-ROI next investment in the AP function. The labor savings are immediate and measurable, the risk reduction from pre-payment validation is significant, and the real-time cash visibility improvement supports better treasury decision-making across the business.


## Conclusion: Approval Is Not the End of AP — Payment Execution Is


A fully automated AP function does not end at invoice approval. It ends when the vendor receives their funds, the bank confirms the transaction, and the ERP ledger reflects the updated cash position — all without a treasury team member manually touching a bank portal or updating a spreadsheet.


For finance teams that have invested in AP automation but still find their treasury colleagues consuming 15-25 hours per month on manual payment tasks, the solution is clear: close the last mile by extending automation from the approved invoice all the way through payment execution, bank confirmation, and ERP write-back.


**Next steps for treasury and FinOps teams:**


1. Map exactly where manual work enters your payment process after invoice approval — how many steps, how much time, which team members
2. Define your payment run frequency, maker-checker approval structure, and payment threshold policies
3. Evaluate AP automation platforms that include native payment execution, bank API connectivity, and automatic ERP posting — not just approval workflow management


---


**Ready to close the last mile in your AP cycle?**[Request a demo](https://peakflo.co/request-demo) to see how Peakflo automates payment execution, maker-checker approvals, and ERP write-back in a single connected workflow.


---


## Frequently Asked Questions


### What is the last mile problem in accounts payable?


The AP last mile problem refers to the gap between invoice approval and actual payment execution. Most AP automation platforms automate invoice capture, coding, and approval, but stop short of executing the payment. Treasury teams must then manually select approved invoices, prepare payment files, log into bank portals, and submit payments — a set of manual steps that adds hours of bottleneck time to an otherwise automated process.


### Why do treasury teams still manually execute payments after invoice approval?


Most AP automation platforms focus on invoice processing and approval routing but do not include direct bank connectivity for payment execution. This leaves a structural gap: invoices are approved in the AP platform, a bill is created in the ERP, but the actual payment must be initiated through the bank’s online portal by a treasury team member who must also manually prepare files and update the ERP after payment.


### How much time does manual payment execution add to the AP process?


For an organization processing 400 vendor payments per month across multiple entities, manual payment execution typically adds 15-25 hours per month in treasury team time, covering reviewing approved invoices, validating bank details, preparing payment files, executing bank transfers, confirming payment receipts, and reconciling payments in the ERP.


### What is a payment run in AP automation?


A payment run is a batched group of approved vendor payments prepared, validated, and executed together rather than individually. Automated payment runs pull all invoices due within a payment window, validate destination bank accounts, generate a payment file, and trigger execution — typically on a scheduled frequency — without requiring manual selection or file preparation by the treasury team.


### What is a maker-checker payment approval?


A maker-checker payment approval is a dual-control process in which one person prepares or creates a payment batch (the maker) and a second authorized person reviews and approves it before execution (the checker). This two-person rule prevents unilateral payment authorization and is a standard treasury fraud control. In automated AP platforms, the maker-checker flow is built into the payment run workflow with complete digital audit trails.


### How does AP automation connect to bank payment systems?


AP automation platforms connect to banks through direct bank API integration, file-based payment uploads (ISO 20022 or SWIFT file formats), or local payment rails like GIRO, RTGS, or ACH. Advanced platforms maintain direct connections to multiple banks and payment networks, allowing the treasury team to execute payments across banks without logging into individual portals.


### What is the difference between invoice approval automation and full AP automation?


Invoice approval automation addresses the intake, coding, and approval workflow stages. Full AP automation extends through payment execution, bank reconciliation, and ERP posting. Organizations that automate only the approval stage still require significant manual treasury work to complete payments — which is often the most time-consuming step in the AP function.


### What happens when payment execution is disconnected from the ERP?


When payment execution is disconnected from the ERP, the treasury team must manually update the ERP after each payment to mark bills as paid and post journal entries. This creates a reconciliation lag where the ERP reflects an inaccurate cash position until the manual update is complete. For multi-entity companies, this delay is multiplied across each subsidiary, making real-time group-wide cash visibility impossible.


### How do scheduled payment runs reduce treasury team workload?


Scheduled payment runs batch all invoices due within a defined window and execute them automatically after maker-checker approval. Instead of processing each payment individually throughout the week, the treasury team reviews and approves one consolidated batch. This eliminates repetitive task switching, reduces missed payment risk, and frees treasury capacity for cash flow forecasting and bank relationship management.


### What controls should be in place for automated payment execution?


Controls for automated payment execution should include: maker-checker dual authorization for every payment batch, payment amount thresholds triggering additional approver review, pre-payment bank account validation, a complete digital audit trail of every payment decision, exception reporting for payments that fail validation, and automated remittance notifications to vendors confirming each payment.


### How does automated payment execution reduce late vendor payment risk?


Automated payment execution reduces late payments by removing the dependency on a treasury team member’s availability. In manual processes, a payment can miss its due date if the responsible person is unavailable. Scheduled automated runs execute on a fixed cadence regardless of individual availability, and due-date alerts flag invoices approaching their payment deadline before they become overdue.
