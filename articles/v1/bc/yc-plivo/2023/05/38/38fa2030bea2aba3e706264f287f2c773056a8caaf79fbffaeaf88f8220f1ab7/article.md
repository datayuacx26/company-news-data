---
schema_version: "1.0.0"
document_id: "38fa2030bea2aba3e706264f287f2c773056a8caaf79fbffaeaf88f8220f1ab7"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/toll-free-verification/"
published_at: "2023-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:9b2f1add9287ff62c4c60c5470dc84031e9b14272cfbd5e13c575bc57380ae75"
---

# Latest Verification Requirements for Toll-Free Messaging in North America

The carriers responsible for toll-free text messaging in North America mandate verification of toll-free numbers before you can use a toll-free number to send application-to-person (A2P) messages. This change aligns toll-free messaging with[10DLC](https://plivo-webflow.webflow.io/sms/10dlc) and[short codes](https://plivo-webflow.webflow.io/sms/shortcode) , which already have business verification measures in place. The intent is to ensure that message senders on all services are reviewed and that businesses that send messages are documented and verified.


The verification process involves the carrier responsible for verification reviewing the submitted information and either approving or rejecting the application. The process can take up to two weeks.


Irrespective of the volumes of application-to-peer (a2p) messages you intend to send on toll-free numbers, you must verify each toll-free number.


[Plivo](https://www.plivo.com/) **recommends that you begin sending messages only after a toll-free number has been verified.**


## How to get toll-free numbers verified


1. You can rent messaging-enabled toll-free numbers on the console.
2. Once you rent a messaging-enabled toll-free number, you have two ways to submit details for verification:


- Via Plivo’s[toll-free verification API](https://www.plivo.com/docs/messaging/api/tf-verification/)


- We recommend this method if your business frequently requires toll-free number provisioning.
- The API submits the verification request directly to the carrier with no manual review by Plivo, reducing the turnaround time.
- As soon as a toll-free number is verified, you will get a call-back and can initiate A2P messaging traffic on-the-go.


- Via the[Plivo Console](https://console.plivo.com/sms/10dlc/tollfree_verification/)


- We recommend this method if your business has a one-time or occasional need for toll-free number provisioning.
- The Plivo Console also directly submits your request to the carrier with no manual review by Plivo team, ensuring fast turnaround.
- For bulk toll-free number verification (more than 10 numbers), please fill out this[sheet](https://www.plivo.com/assets/dist/Plivo_Bulk_TFN_Verification_Form.xlsx) and[create a ticket](https://support.plivo.com/hc/en-us/requests/new?ticket_form_id=360000156292) to share it with the Plivo support team. We will submit your request to the carrier for review.


3. You can follow the verification status for any toll-free number on the console by visiting Phone Numbers >[Active](https://console.plivo.com/active-phone-numbers/) . By default numbers are classified as “unverified”. Once a number is submitted for review, it’s marked “pending verification”.
4. When the carrier notifies us of the successful completion of the verification request, we change the status to “verified”.
5. If you had submitted the request via the toll-free verification API, you will also get a call back with the terminal status of your verification request – “rejected” or “verified”.
6. If the carrier rejects the verification request, we change the status to “unverified”. You should get this update as a callback if the request was submitted via Plivo toll-free number verification API. In case a verification request is rejected and the request was submitted manually, you will get an email from Plivo Support team.


Verified numbers are subject to the least amount of filtering — but that doesn’t mean customers can use them in unapproved ways. All messaging must still comply with[best practices for A2P messaging](https://www.plivo.com/docs/sms/concepts/us-messaging-best-practices/) and must not be used for any use case that is[not allowed by carriers](https://www.plivo.com/docs/sms/concepts/us-messaging-best-practices#other-prohibited-content) in the US or Canada.


Once verified, a number does not need to be re-verified for the same use case in the future. If you want to use any given toll-free number for a different or an additional use case, you should submit a new verification request. Failure to do so can be considered noncompliance by the carriers and may result in your messaging services being suspended.
