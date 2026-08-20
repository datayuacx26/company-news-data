---
schema_version: "1.0.0"
document_id: "6f06b68938e2db44949db4c7e2829ac0295502f48ff38c719f079a398419ec39"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/using-policycenter-cloud-api-form-patterns"
published_at: "2024-01-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:5431538c3252ecbd676343b159e3a462750c44076079716c5ce1f62df3bb8db4"
---

# Using PolicyCenter Cloud API Form Patterns

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Using PolicyCenter Cloud API Form Patterns


## Form Patterns Overview


Form patterns are not new to PolicyCenter, so you might already be familiar with them. However, the ability to access them from Cloud API is new, as of the Innsbruck release. So if you already know all about form patterns in the user interface (UI) and only want the technical overview, feel free to skip ahead to the next section.


For those of you who are still here, I’ll explain what form patterns are. When someone takes out an insurance policy, there is a lot of paperwork involved. Sometimes it’s electronic, sometimes it’s actual paper, but either way, there’s usually a lot of it. That paperwork consists of the forms detailing the terms of the policy and all the legal stuff that goes with it. Guidewire does not store those forms. All of those forms are in a document management system somewhere that is not Guidewire.


We do, however, have to be able to identify those forms. That’s where form patterns come in. A form pattern provides the information that is required to identify which forms go with each policy.


Forms are assigned to policies automatically when a job is created; you can’t explicitly associate a form with a policy. Where and when the form is associated depends on the information supplied in the form pattern. For example, a form can be associated with a policy when a job is bound or quoted, and can be restricted to one or more specified job types such as submission or renewal.


Form patterns can also include information about jurisdictions (such as a specific state or region) and policy start and end dates.


Policies matching the criteria specified in a form pattern will automatically be assigned the form the pattern represents.


## Form Patterns Endpoint


It actually takes quite a bit of information to identify policy forms and determine which policies they should be applied to. In the UI, there are six tabs’ worth of information to do this.


With Cloud API, all this information can be accessed through two endpoints: form-patterns and lookups.


**form-patterns endpoints**


- GET /admin/v1/form-patterns
- GET /admin/v1/form-patterns/{policyFormPatternId}
- POST /admin/v1/form-patterns
- PATCH /admin/v1/form-patterns/{policyFormPatternId}
- DELETE /admin/v1/form-patterns/{policyFormPatternId}


**lookups endpoints**


- GET /admin/v1/form-patterns/{policyFormPatternId}/lookups
- GET /admin/v1/form-patterns/{policyFormPatternId}/lookups/{lookupId}
- POST /admin/v1/policy-form-patterns/{policyFormPatternId}/lookups
- PATCH /admin/v1/form-patterns/{policyFormPatternId}/lookups/{lookupId}
- DELETE /admin/v1/form-patterns/{policyFormPatternId}/lookups/{lookupId}


The form-patterns endpoints provide access to all of the information relating to form patterns except jurisdiction availability. The lookups endpoints provide access to all information related to the jurisdiction availability. Here are some examples.


This is a sample response from a GET operation on the form-patterns endpoint:


{
...
"attributes": {
"code": "FormIL17",
"edition": "00 00",
"endorsementNumbered": true,
"formNumber": "PF 01",
"genericFormInference": {
"code": "GenericAlwaysAddedForm",
"displayName": "No additional criteria"
},
"id": "pc:100",
"inferenceTime": {
"code": "quote",
"name": "Quote Time"
},
"jobTypes": \[
{
"code": "Renewal",
"name": "Renewal"
},
{
"code": "Submission",
"name": "Submission"
}
\],
"name": "Common Policy Conditions",
"priority": 2,
"products": \[
{
"displayName": "Personal Auto",
"id": "PersonalAuto"
}
\],
"reissueOnChange": false,
"removalEndorsement": false
},
...
}


Some of the information you can determine from this response includes:


- The Form Code is FormIL17.
- The Form Number is PF 01.
- The Name of the form is Common Policy Conditions.
- The form applies only to the Personal Auto product.
- The form will be applied to appropriate policies when they are quoted.
- The form will be applied to a policy on job submission or renewal.


In addition to general form information and under what circumstances to apply the form to a policy, a form pattern also needs to specify which policies to apply the form to based on certain availability information. This is found with the lookups endpoint. Here is a sample response from a GET operation on the lookups endpoint:


{
...
"attributes": {
"availability": {
"code": "Available",
"name": "Available"
},
"id": "pc:150",
"jurisdiction": {
"code": "CA",
"name": "California"
},
"startEffectiveDate": "2024-01-19T08:00:00.000Z",
"uwCompanyCode": {
"code": "3111_33333",
"name": "Acme High Hazard Insurance"
}
},
...
}


In this example, the form is currently Available to be applied to policies, but only to policies that are in the state of California, become effective starting January 19, 2024, and are underwritten by the Acme High Hazard Insurance company. The only required fields are availability and startEffectiveDate, which means you can leave jurisdiction blank (null). In that case the form will be automatically applied to all policies as of the startEffectiveDate.


You can also retrieve a list of forms that have been applied to a job or policy, using these endpoints:


- GET /job/v1/jobs/{jobId}/forms
- GET /policy/v1/policies/{policyId}/forms


As I mentioned earlier, you can’t explicitly assign a form to a policy, but once the form has been automatically assigned you can use these endpoints to view the form information.


## Learn More


To get more details and see complete examples using the form patterns endpoints, refer to the[PolicyCenter Cloud API documentation](https://docs.guidewire.com/cloud/pc/202310/cloudapibf/cloudAPI/topics/124-PCsupport/00-form-patterns/c_form-patterns.html?_gl=1*aua61s*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjM2MzI1MC4xNjMuMS4xNzM2MzYzNDc3LjYwLjAuMA..) .


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
