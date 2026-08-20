---
schema_version: "1.0.0"
document_id: "ed73003760c27ab7a69afec23975a91db9d5db02b64e7b2db03aa202abb9ba72"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-security-agent-mfa/"
published_at: "2026-08-06T16:54:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:4313a347ace177881844795771780a329ef366f02f4d5dae769c5c434e072818"
---

# AWS Security Agent now supports email-based MFA for penetration testing

AWS Security Agent (now part of[AWS Continuum](https://aws.amazon.com/continuum?refid=77f1fbea-4ad5-4c1d-b16e-e5634ca5086e) ) now enables penetration testing of applications that use email-based multi-factor authentication (MFA) as part of their login flow. Previously, applications requiring one-time codes or verification links sent by email were out of scope for automated pentesting because the agent had no mechanism to intercept those messages. This launch expands coverage for penetration testing customers whose target applications rely on email-based authentication.


To use this feature, AWS Security Agent generates a unique forwarding address per credential, allowing you to route your application's MFA emails directly to the agent using a forwarding rule in your existing email provider. During a pentest, the agent automatically reads the forwarded message and submits the code or link to complete authentication — no email account credentials are stored, preserving a strong privacy posture. This capability complements existing TOTP support, giving customers a unified solution for testing applications across multiple MFA methods.


This feature is available in all AWS Regions where AWS Security Agent is supported.


To learn more, visit the[AWS Security Agent product page](https://aws.amazon.com/security-agent/?trk=77f1fbea-4ad5-4c1d-b16e-e5634ca5086e&sc_channel=ps&ef_id=CjwKCAjw4dDTBhAqEiwAkHYmSuBZ_V1zxJ29F9CUMF-dw-2pVhe8YC2_92NoHZwMhMknYw6nqMHbUhoCvjwQAvD_BwE&gads_camp=23532472972&gads_ag=200881312961&gads_ad=812204788708&gads_kw=aws%20security%20agent&gads_matchtype=e&gads_network=g&gads_device=c&gads_geo=9193254&gad_campaignid=23532472972&gbraid=0AAAAADjHtp8JMRwV7qBFDei1P9Woz23HE&gclid=CjwKCAjw4dDTBhAqEiwAkHYmSuBZ_V1zxJ29F9CUMF-dw-2pVhe8YC2_92NoHZwMhMknYw6nqMHbUhoCvjwQAvD_BwE) and the[AWS Security Agent User Guide](https://docs.aws.amazon.com/securityagent/latest/userguide/what-is.html?trk=77f1fbea-4ad5-4c1d-b16e-e5634ca5086e&sc_channel=ps) .
