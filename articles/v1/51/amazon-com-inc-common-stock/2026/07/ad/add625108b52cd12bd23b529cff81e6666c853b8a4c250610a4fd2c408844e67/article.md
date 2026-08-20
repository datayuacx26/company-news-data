---
schema_version: "1.0.0"
document_id: "add625108b52cd12bd23b529cff81e6666c853b8a4c250610a4fd2c408844e67"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-s3-event-notifications-system-generated-tags/"
published_at: "2026-07-16T18:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:0e39accf9cb07dbab64b5edf4e936f90096a6c463eb202adf3f17990f9142126"
---

# Amazon S3 Event Notifications now include system-generated tags

Amazon S3 Event Notifications now include system-generated tags in events delivered to all destinations including Amazon EventBridge, Amazon SQS, Amazon SNS, and AWS Lambda. System-generated tags are metadata labels attached to your bucket by AWS services. You can use these tags to filter events from thousands of buckets with a single EventBridge rule, instead of listing each bucket name individually.


To get started, enable S3 Event Notifications on your general purpose buckets through the AWS Management Console, AWS SDK, or AWS CLI. If AWS services like AWS CloudFormation have already applied system-generated tags to your buckets, S3 automatically includes them in new event notifications. System-generated tags in S3 Event Notifications are available at no additional cost in all AWS Regions and require no changes to existing configurations. To learn more, visit the[S3 Event Notifications documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/notification-content-structure.html) .
