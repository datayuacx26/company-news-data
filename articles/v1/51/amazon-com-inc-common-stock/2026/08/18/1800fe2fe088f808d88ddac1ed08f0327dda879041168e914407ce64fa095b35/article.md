---
schema_version: "1.0.0"
document_id: "1800fe2fe088f808d88ddac1ed08f0327dda879041168e914407ce64fa095b35"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/AWS-Certificate-Manager-Email-DNS-Switch"
published_at: "2026-08-13T15:00:00+00:00"
first_seen_at: "2026-08-14T00:27:41.812190+00:00"
fetched_at: "2026-08-14T00:27:44.909056+00:00"
content_hash: "sha256:31a3a852728026a21bf3a900073246708027ccea5493d4aa4bd3b4c932cfcbf3"
---

# AWS Certificate Manager supports switching from e-mail to DNS validation

AWS Certificate Manager (ACM) now enables you to change the domain validation method on your existing ACM issued public TLS certificates from e-mail to DNS, without reissuing the certificate or changing its existing Amazon Resource Name (ARN). Due to the[Certification Authority/Browser (CA/B) Forum's](https://cabforum.org/) mandated deprecation of email-based domain validation for publicly trusted certificates, effective March 15, 2028, ACM will phase out its support for email validation throughout 2027. ACM will no longer issue email-validated certificates starting March 31 2027, and stop renewing email-validated certificates on September 30 2027. More details on ACM's deprecation of email validation can be found on the[AWS Security Blog](https://aws.amazon.com/blogs/security/aws-certificate-manager-will-discontinue-email-validation-to-prove-domain-validation-for-certificates) . By switching to DNS validation now, you can transition ahead of that deadline and enable fully automated renewals through DNS validated certificates.


Your certificate ARN remains unchanged after switching from e-mail to DNS validation, so existing ARN references in your CI/CD pipelines, load balancer configurations, and other AWS service integrations continue to work without modification. To switch the validation method, use the ACM console or the[UpdateCertificateOptions](https://docs.aws.amazon.com/acm/latest/APIReference/API_UpdateCertificateOptions.html) API. ACM provides a CNAME record for each domain in the certificate (the same mechanism used when provisioning new certificates with DNS validation) and you have up to 72 hours to add the records to your DNS configuration. You can monitor the validation status of each domain via the console or the[ListCertificateDomainValidations](https://docs.aws.amazon.com/acm/latest/APIReference/API_ListCertificateDomainValidations.html) API. We recommend DNS validation for new certificates, and[HTTP validation](https://docs.aws.amazon.com/acm/latest/userguide/http-validation.html) for Amazon CloudFront distributions..


This feature is available in all AWS Regions where ACM certificates are available. To get started, refer to[Migrating from email to DNS validation](https://docs.aws.amazon.com/acm/latest/userguide/email-to-dns-migration.html) in the *AWS Certificate Manager User Guide.*
