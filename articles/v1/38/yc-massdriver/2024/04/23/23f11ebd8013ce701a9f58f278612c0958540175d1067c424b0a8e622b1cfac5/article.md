---
schema_version: "1.0.0"
document_id: "23f11ebd8013ce701a9f58f278612c0958540175d1067c424b0a8e622b1cfac5"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/terraform-test-considered-harmful"
published_at: "2024-04-20T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:0678de180936932e3a7eec0eb3c9c6ff3b6826173f7f821bb7e6655f15e699ed"
---

# Terraform Test Considered Harmful

I love Test-Driven Development (TDD), but Terraform's (and OpenTofu's) new test functionality is a step in the wrong direction. If widely adopted, it will set infrastructure teams back and give them a false sense of security. Let me explain why.


### The Importance of Behavior Over Attributes


When it comes to testing infrastructure, the only thing that truly matters is the behavior of that infrastructure. Testing that you’ve set a variable is a nonsense test and shows that you don’t trust your tools. This is akin to testing an ORM extensively in your application instead of focusing on your domain’s logic—but even more pedantic.


### The Futility of Testing Variables and Locals


Testing mutations of variables via locals is pointless. Locals are private logic to the module, and every test just duplicates your work. In fact, you’re tripling your efforts because someone will eventually validate those same rules in policy code, leading to a low-value maintenance burden.


Both of these scenarios are better served by stronger input validation or policy tooling.


### Misguided Attribute Testing


Testing the resultant attributes of an applied plan is the most egregious waste of time and leads to a false sense of security. You can only truly test infrastructure by observing that it adheres to the intended behavior in production.


For example, the format of a database’s URL doesn’t matter. What matters is that it’s not publicly routable. You can’t assert this with` terraform test` ; you need something like[Terratest](https://terratest.gruntwork.io/) that can attempt to make a PSQL connection, see the failure, and then connect through a bastion or VPN to a subnet with access to validate the actual behavior you care about.


### Weak Assertions


Consider testing whether encryption is turned on in RDS. This is another example of a false sense of security. You can’t actually test the encryption; you don’t have access to the disks. You have to trust that Amazon respects your encryption settings.


“Well, I want to make sure it’s set to true.” Okay, but I bet you didn’t test that it could be set to false. If you didn’t, you haven’t even properly tested the variables and locals you distrust.


In scenarios like encryption, you should just hard-code it. Not every attribute needs to be configurable by your module caller. You should be encoding best practices and expertise around a use case into your modules, not building a useless abstraction that merely proxies variables to resources. Developer experience can start as low level as the modules you share with your team.


### Real-World Testing for Real-World Systems


Terraform’s new test functionality might seem like a step forward, but it’s actually a step back. It focuses too much on superficial checks and not enough on the real-world behaviors that truly matter.


For teams serious about validating the behavior of their production systems, I recommend using tools like[Terratest](https://terratest.gruntwork.io/) . These tools allow you to write real tests that verify the actual behavior of your infrastructure in a live environment. This approach ensures that your infrastructure works as intended and meets your security, performance, and reliability standards.


By focusing on behavior rather than attributes and properties, you can achieve a more robust, reliable, and secure infrastructure that truly supports your business needs.


‍
