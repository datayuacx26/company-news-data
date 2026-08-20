---
schema_version: "1.0.0"
document_id: "c43be000e0ab6bee312201544682cb077f2757ef9ea62a4135856f6a71fc1d43"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/face-search-duplicate-account-detection/"
published_at: "2026-08-04T14:51:01.160+00:00"
first_seen_at: "2026-08-04T18:58:29.118711+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:637d5848bbbf6c3ef7705741f54702364cf8405d9f2cee9727919e8135406b22"
---

# Face Search 1:N: Finding Every Account One Person Controls

[Back to blog](https://didit.me/blog/) Blog · August 4, 2026


# Face Search 1:N: Finding Every Account One Person Controls


One API call searches a face across every verified user you have and returns each matching account with your own identifier attached. Free with Didit verification — the primitive that turns an account list into an actor map.


By Didit


·


August 4, 2026 ·


Updated Aug 4, 2026


An operator running forty accounts on your platform has forty email addresses, probably forty payment instruments, and possibly forty devices. What they do not have is forty faces.


If any part of your access flow captures a selfie, you are already holding the one identifier that is genuinely expensive to multiply. Face Search 1:N is the call that uses it — one request, one face, and back comes every account in your own system that the same person verified.


It is **free** with Didit identity verification, it returns in under two seconds, and it runs automatically during liveness inside a verification session.


## **Key takeaways**


- ` POST /v3/face-search/` searches a face against **the faces your own application has enrolled** — sessions run with` save_api_request=true` — not a shared global index.
- Matches come back with **your own` vendor_data`** on each, so results map straight onto your account IDs.
- Two modes:` most_similar` for deduplication and returning users,` blocklisted_or_approved` for blocklist screening.
- ` status` is` "Declined"` **only** on a blocklist match. Duplicates return` "Approved"` with a` DUPLICATED_FACE` warning — informational by design, because the dedupe policy is yours.
- The response is a **singular**` face_search` object, not an array. This trips people up.
- **Free** with Didit verification. Sub-two-second response. Runs automatically during liveness.


## **What 1:N means, and why it is the right tool**


A 1:1 face match answers "is this the person in this document?" That is a verification question, and it is what runs during onboarding.


A 1:N search answers a different question: "of all the people I have already verified, is this one of them?" One image goes in, and every match in your index comes out.


For coordinated account abuse, the second question is the one that matters. Anthropic's report on distillation campaigns described attribution built from relational signals — shared payment methods, coordinated timing, shared infrastructure. A biometric 1:N search is the same class of signal, sourced from the most expensive identifier an operator has to multiply.


The index is **yours** . Face Search runs against faces your own application enrolled through prior verifications — sessions with` save_api_request=true` , or Passive Liveness with` save_api_request=true` . It is not a search across other Didit customers' users. If you have not enrolled faces, there is nothing to search.


## **The API**


### **Request**


```text
curl -X POST 'https://verification.didit.me/v3/face-search/' \
-H 'x-api-key: YOUR_API_KEY' \
-F 'user_image=@./selfie.jpg' \
-F 'search_type=most_similar' \
-F 'save_api_request=true' \
-F 'vendor_data=acct_8842'
```


` multipart/form-data` , authenticated with` x-api-key` .


**Required:**` user_image` — jpg, jpeg, png, tiff or webp, maximum 5 MB. PDFs are not accepted. The image must contain at least one detectable face; when several are present, the largest bounding box wins.


**Optional:**


- ` search_type` —` most_similar` (default) for deduplication and returning-user detection, or` blocklisted_or_approved` for blocklist screening.
- ` save_api_request` — enrol this image into your index.
- ` vendor_data` — your own identifier for the subject.


### **Response**


The response carries a **singular**` face_search` object. Most Didit features return plural arrays, so this is the one shape worth reading carefully before you write the parser.


```text
{
"request_id": "...",
"face_search": {
"status": "Approved",
"total_matches": 12,
"matches": [
{
"session_id": "...",
"session_number": 4471,
"similarity_percentage": 97.4,
"vendor_data": "acct_3310",
"verification_date": "2026-06-02T09:14:00Z",
"user_details": { },
"match_image_url": "...",
"status": "Approved",
"is_blocklisted": false
}
],
"user_image": { "entities": [] },
"warnings": []
}
}
```


The fields that carry the investigation:


- **` total_matches`** — how many accounts share this face.
- **` vendor_data`** on each match — your identifier, so a match list is immediately an account list.
- **` similarity_percentage`** — the strength of each individual match.
- **` verification_date`** — the timeline. Twelve accounts verified across eleven months reads differently from twelve verified in one afternoon.
- **` is_blocklisted`** — whether this match is already on your blocklist.
- **` session_id`** — the pivot into everything else that session captured, including its device and network warnings.


### **Status semantics**


This is the most important behavior in the whole endpoint:


> ` status` is` "Declined"` **only** when at least one blocklist match is found. Pure duplicate matches return` "Approved"` .


A duplicate is not a decline. It is information. Didit deliberately refuses to make the dedupe decision for you, because duplicates have legitimate explanations and only you know your product's rules.


### **Warnings**


Warning Meaning


` FACE_IN_BLOCKLIST` Definitive blocklist match — reject


` POSSIBLE_FACE_IN_BLOCKLIST` Borderline match below the hard threshold — route to manual review


` DUPLICATED_FACE` This face is already verified under different` vendor_data`


` POSSIBLE_DUPLICATED_FACE` Borderline duplicate


` MULTIPLE_FACES_DETECTED` More than one face in the submitted image


### **Failure modes**


- **HTTP 400** — no face detected in` user_image` . Ask for a retake.
- **HTTP 403** — out of credits.
- **` status: "Declined"` with` FACE_IN_BLOCKLIST`** — definitive hit. Reject.
- **` POSSIBLE_FACE_IN_BLOCKLIST`** — below the hard threshold. Manual review.
- **` DUPLICATED_FACE`** — already verified under different` vendor_data` . Merge, block or allow according to your policy.


## **The automatic path**


You often do not need to call the endpoint at all. Face Search runs **automatically during liveness** inside a verification session:


- Facial biometrics are compared against all previously verified users.
- Potential duplicate accounts are identified by facial similarity.
- Matches are flagged according to your configured similarity thresholds.
- Faces are checked against your blocklist, and a blocklist match automatically declines the verification.


So for any tier where you already run full verification, duplicate detection is included at no extra cost and no extra call. The standalone endpoint is for the cases the session flow does not cover — investigating an account after the fact, screening an image you obtained another way, or switching` search_type` to run a blocklist-focused search over an image you already hold.


## **Turning matches into an actor map**


The practical workflow, starting from one suspect account:


1. **Search the face.**` total_matches: 12` — twelve accounts, one person.
2. **Read the` vendor_data` .** Twelve of your own account IDs, no join required.
3. **Read the timeline.** Cluster the` verification_date` values. Accounts created in bursts are operationally different from accounts created over years.
4. **Pivot on` session_id` .** Pull each session's device and network warnings. Faces that share` DUPLICATED_DEVICE_FINGERPRINT` tighten the cluster; accounts on unrelated devices may be a different arrangement.
5. **Expand.** Devices and IP ranges surfaced in step 4 will pull in accounts that the face search missed — because a different person completed those checks.
6. **Decide once, enforce across the identifiers.** If the cluster is confirmed abuse, post the confirmed` reference_session_id` to each entry-type blocklist you care about — face, device, IP, email, phone, document. It is one call per list, and each call auto-extracts the right value from that session, so nothing is retyped by hand.


Six steps, one starting point, and no prompt inspection anywhere. The traffic layer tells you *something is wrong with this account* . This tells you *how many accounts that actually is* .


Worth stating plainly: **a 1:N face search does not prevent model extraction and does not detect it.** Face Search has no visibility into your API traffic. It resolves accounts to people, which is what lets you act on an alert across a whole cluster instead of one row. Model-level output controls and semantic traffic detection are separate layers, and they remain the model provider's responsibility.


## **Use cases**


**AI API platforms** resolving a behavioral alert into the full account set an operator controls.


**Free-tier and credit abuse** — one person, many trial accounts, is the same detection problem with lower stakes.


**Marketplaces and gig platforms** catching banned sellers, drivers or couriers re-registering.


**iGaming** enforcing single-account rules and self-exclusion, where a returning excluded player is a regulatory failure, not just an abuse case.


**Financial services** identifying synthetic-identity rings where one real face is spread across many fabricated identities.


## **Frequently asked questions**


**Is my face index shared with other Didit customers?**


No. Face Search runs against the index your own application built through your own verifications. It is not a cross-customer search.


**What controls whether a face enters the index?**


` save_api_request=true` on a verification session or a Passive Liveness call. You decide what is enrolled and you control retention, consistent with your own privacy notice and legal basis for processing biometric data.


**What similarity threshold should I use?**


Be aware of where tuning applies. On the **standalone endpoint** , the similarity bands that separate confirmed hits (` FACE_IN_BLOCKLIST` ,` DUPLICATED_FACE` ) from possible hits (` POSSIBLE_FACE_IN_BLOCKLIST` ,` POSSIBLE_DUPLICATED_FACE` ) are **fixed internally** — per-application threshold tuning applies to the workflow liveness check, not to` POST /v3/face-search/` . So on the standalone path, read` similarity_percentage` per match and apply your own bar in application logic, and treat the` POSSIBLE_*` warnings as your review queue rather than your decline queue.


**How fast is it at scale?**


Sub-two-second response.


**Can I search a face that never went through Didit verification?**


Yes. Any` user_image` in an accepted format works. If no face is detected the call returns HTTP 400.


**Is it really free?**


Yes — Face Search 1:N is free with Didit identity verification. There is no per-search charge. You are paying for the verifications that build the index, at $0.33 for the full bundle, with the first 500 each month free.


**What if the same person legitimately has two accounts?**


Then` DUPLICATED_FACE` is exactly the informational signal it is designed to be — which is why it does not decline. Merge them, allow them, or ask the user, according to your product's rules.


## **Ready to get started?**


Face Search is available on every Didit account, with no separate product to buy.


- **Read the docs** —[Face Search 1:N overview](https://docs.didit.me/core-technology/face-search/overview) and the[Lists API](https://docs.didit.me/management-api/lists/overview) for face blocklists.
- **See the product** —[User Verification](https://didit.me/products/user-verification) .
- **Check the pricing** —[Face Search 1:N is free](https://didit.me/pricing) ; the verification bundle that builds the index is $0.33.
- **Start free** —[business.didit.me](https://business.didit.me/) , 500 KYC verifications a month at no cost.


Keep reading


## Related articles


- [The Hydra Account Problem: Why Distillation Defense Starts With Identity Resolution](https://didit.me/blog/llm-distillation-defense-identity/)
- [Business Verification for AI API Access: Who Actually Controls This Account?](https://didit.me/blog/kyb-ai-api-enterprise-access/)
- [Verified API Access for AI Model Providers: A Risk-Tiered Architecture](https://didit.me/blog/verified-api-access-ai-model-providers/)
- [Biometric Step-Up for AI API Access: Binding Privilege to a Person](https://didit.me/blog/biometric-authentication-ai-api-access/)
- [Hydra Account Networks: How 20,000 Accounts Become One Actor](https://didit.me/blog/hydra-account-networks-ai-api-abuse/)
- [Blocklist Propagation: Making One Confirmed Abuse Case Kill the Whole Network](https://didit.me/blog/ai-api-abuse-blocklist-propagation/)
