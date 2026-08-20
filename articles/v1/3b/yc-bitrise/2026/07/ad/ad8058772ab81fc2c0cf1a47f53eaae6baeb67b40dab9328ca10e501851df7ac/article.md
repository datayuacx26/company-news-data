---
schema_version: "1.0.0"
document_id: "ad8058772ab81fc2c0cf1a47f53eaae6baeb67b40dab9328ca10e501851df7ac"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/bitrise-achieves-the-aws-devops-competency-for-continuous-integration-and-continuous-delivery"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T22:21:27.993259+00:00"
fetched_at: "2026-07-31T22:21:28.592641+00:00"
content_hash: "sha256:56e2bf0b4faaa835c1134d5a9a1b5689e1385c45309118dcd9131318d7492c75"
---

# Bitrise achieves the AWS DevOps Competency for Continuous Integration and Continuous Delivery

Bitrise is one of the few global AWS technology (software) partners who build on top of the Amazon EC2 Mac compute offering. We are proud to announce we have received recognition of our solution quality and expertise from AWS for our mobile-first CI/CD product,[Bitrise on AWS](https://bitrise.io/platform/build-platform/bitrise-on-aws) .


## What the AWS DevOps Competency recognizes


The AWS DevOps Competency is a validation, not a badge you can buy. AWS grants it to AWS Partner Network (APN) members who have demonstrated technical proficiency and proven customer success in delivering DevOps solutions on AWS. The offering is reviewed against AWS best practices by AWS DevOps experts before anything is awarded.


Within that competency, the Continuous Integration & Continuous Delivery category is specific, recognizing solutions that let teams rapidly and reliably build, test, and release their applications while improving quality and reducing time to market. That is the exact problem Bitrise was built to solve, while deploying on top of the Amazon EC2 Mac compute capacity.


Achieving the AWS DevOps Competency differentiates Bitrise as an AWS Partner Network (APN) member with proven customer success in CI/CD on AWS. Bitrise has demonstrated expertise in helping mobile teams ship faster on infrastructure they control, while following AWS best practices for architecture and security.


Was the audit a lot of work? Yes. Going through it made us tighten a few things we'd been meaning to tighten anyway, which is usually how these processes pay off.


## Why this matters for mobile teams building on AWS


Most CI platforms treat mobile as an afterthought bolted onto a generic Linux pipeline. For teams shipping iOS and Android, that is a broken model, because the hard parts of mobile delivery live exactly where generic tooling is thinnest: hosted macOS runners, code signing, Xcode version management, and device-shaped test environments.


Bitrise on AWS takes a different route. Bitrise CI runners deploy directly into the customer's own AWS VPC. The build environment sits on dedicated Amazon EC2 Mac hardware, unreachable from the internet, with Linux and macOS EC2 AMI images that come preloaded with mobile tooling that gets updated with every new tool version. Teams get the developer experience the mobile community expects from Bitrise, on infrastructure that meets enterprise security and data residency requirements. We further help you optimize your build environment through our macOS virtualization built into the AMIs, allowing you to run two isolated VMs on one EC2 Mac host.


For a regulated business, this security model is mission-critical. If your source code management repositories are hosted also in your Amazon VPC, then source code stays inside your company's security boundary.


If you are interested in reading about how a major hospitality organization optimized their mobile app delivery with Bitrise on AWS, check out the[Hyatt customer story](https://bitrise.io/customer-stories/hyatt) .


## About the AWS Competency Program


AWS is enabling scalable, flexible, and cost-effective solutions from startups to global enterprises. To support the seamless integration and deployment of these solutions, AWS established the AWS Competency Program to help customers identify AWS Partners with deep industry experience and expertise. Competency validation gives customers confidence that a solution has been reviewed by AWS and has a track record of real customer success behind it.


For mobile engineering teams weighing up how to run CI/CD on AWS without giving up control of their code, that confidence is the point. If you want to see what the security model and the CI runners look like in practice, the[Bitrise on AWS overview](https://bitrise.io/platform/build-platform/bitrise-on-aws) walks through this, you can read our joint article[with AWS on the APN blog](https://aws.amazon.com/blogs/apn/accelerating-ios-builds-with-bitrise-ci-cd-on-amazon-ec2-mac/) , and access the detailed[technical documentation](https://docs.bitrise.io/en/bitrise-platform/infrastructure/bitrise-on-aws--cloud-controller/bitrise-on-aws-overview) .


After a successful Proof of Concept deployment, the solution can be purchased through[the AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-it4x6xnwsi3hi) based on private offer.
