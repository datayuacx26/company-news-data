---
schema_version: "1.0.0"
document_id: "152ce990e12b501468b1a580a7ae9eeae6d4a8ed213f437565acd320cd3ce6c2"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-rss-e53e7c01950b"
canonical_url: "https://tech.spscommerce.com/2023/01/23/giving-back-to.html"
published_at: "2023-01-23T16:24:09+00:00"
first_seen_at: "2026-07-20T23:19:01.810458+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:9e6289dc071bb7e919c2bf2aef70253bcdd70a7f228737d8520fe29681220527"
---

# Giving back to the AWS community

Open source has become the de facto way to build software not only in tech, but across diverse industries. As companies use open source code to build their own commercial products and services, they also see the strategic value of contributing back to those projects. Since at least 2017 SPS has seen the benefits of contributing back to the open source community in ways large and small. Recently with the acceptance of[Add ClientMetadata to SRP auth flow #90](https://github.com/aws/aws-sdk-net-extensions-cognito/pull/90) to the[aws-sdk-net-extensions-cognito](https://github.com/aws/aws-sdk-net-extensions-cognito) repository another step has been made to contribute back to the community and share additional features out to all consumers of this extension.


The .NET extension for Cognito supports developers using the C# programming language who also use the[AWS Cognito](https://aws.amazon.com/cognito/) service to easily make calls to the API. The repository puts it best:


> [Amazon.Extensions.CognitoAuthentication](https://www.nuget.org/packages/Amazon.Extensions.CognitoAuthentication/) simplifies the authentication process of[Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) for .NET developers.
>
>
> It allows you to use various authentication methods for Amazon Cognito User Pools with only a few short method calls, and makes the process intuitive.


One aspect of calling Cognito which wasn’t covered in the extension was the ability to provide Client Metadata to a custom token generation lambda function. Because the lambda token generation function needs to have a very short runtime it is inadvisable to reach out to a database in order to add custom scopes or other information. Additionally, some information may be sensitive or not stored in a database and can or should only be passed by an API call. For this and other reasons the Cognito API provides a Client Metadata object as part of the[Initiate Auth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) request.


For some software used at SPS, information which was available in the API needed to be passed to Initiate Auth and consumed in the[pre-token generation lambda](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-token-generation.html) to be sent to the end user. A pull request was opened and accepted, so now as of[Amazon.Extensions.CognitoAuthentication 2.2.4](https://www.nuget.org/packages/Amazon.Extensions.CognitoAuthentication/2.2.4) it is possible to pass “ClientMetadata” along.


We hope that this contribution will be of great assistance to the community of C# Cognito users and enable more users of the Cognito service to take advantage of customizing their Cognito tokens!


[-Jonathan DeMarks, Principal Cloud Engineer](https://www.linkedin.com/in/jonathan-demarks-02a64694/)


[SPS Technology](https://tech.spscommerce.com/)[@spstech](https://micro.blog/spstech)
