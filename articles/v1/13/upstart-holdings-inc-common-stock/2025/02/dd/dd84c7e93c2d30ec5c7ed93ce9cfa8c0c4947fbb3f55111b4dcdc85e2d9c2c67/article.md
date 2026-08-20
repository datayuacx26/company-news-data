---
schema_version: "1.0.0"
document_id: "dd84c7e93c2d30ec5c7ed93ce9cfa8c0c4947fbb3f55111b4dcdc85e2d9c2c67"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/effortless-okta-policies-with-terraform-b6574af23d70"
published_at: "2025-02-19T20:47:00+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:601713b04d9e467d01960c4472cb9ce88233f280b5f3e9605e04dafd14952eab"
---

# Effortless Okta Policies with Terraform

Technology


Terraform


Okta


Security


Identity Management


# Effortless Okta Policies with Terraform


[Lukas Indre](https://medium.com/@lukasindre?source=post_page---byline--b6574af23d70---------------------------------------)


7 min read


·


Feb 14, 2025


--


Cybersecurity risks are on the rise — Are you doing everything you can to help your company secure access to its resources? Thankfully, Okta provides a robust suite of tools that empower companies like Upstart to secure their business applications from multiple angles. With Okta, you can enforce access requirements, such as being on a specific network, using a company-managed device, belonging to a particular group, or any combination of these conditions. And that’s just the beginning — Okta offers a wide range of policy options to meet diverse security needs.


However, with great flexibility comes the risk of misconfiguration. The Okta Identity Engine’s policy system is highly customizable but doesn’t come with guardrails. This means it’s possible to configure policy rules in ways that lead to unexpected or non-intuitive behaviors. To address these challenges, I developed a[Terraform module](https://registry.terraform.io/modules/lukasindre/authentication-policy-stack/okta/latest) that abstracts and simplifies Okta’s authentication policies, leveraging the[Okta Terraform provider](https://registry.terraform.io/providers/okta/okta/latest/docs) to streamline and standardize configurations. Detailed below are examples of this module in action.


## Network Zone Abstraction


A common practice in most companies is to ensure that users access company resources through a private network, typically via a VPN. Okta supports this configuration by leveraging groups of gateways or IP addresses, referred to as network zones. In authentication policies, you can require users to route their traffic through these defined network zones. A sample configuration might look like this:


```text
network_connection = "ZONE"  network_includes = ["zone_id_1", "zone_id_2"]
```


This requires defining two arguments: one for the type of network connection and another for a list of network zone IDs.


To simplify this process, the module allows you to use a single key, \`network_zones\`, to handle the otherwise opaque zone IDs for you. All you need to do is ensure the network zone is configured within your Terraform project, and provide its ID as items in the list. For example:


```text
network_zones = [    okta_network_zone.my_zone.id,    okta_network_zone.their_zone.id  ]
```


This approach makes the configuration more intuitive and eliminates the need to reference complex zone IDs manually.


Press enter or click to view image in full size


## Managed Device Abstraction


Managed device access is critical to ensure secure, seamless connectivity for employees while protecting sensitive data. By enforcing device compliance policies, organizations can reduce risk, maintain productivity, and adapt to evolving hybrid work environments.


Let’s say you want to allow only managed iOS devices to access a specific Okta application in your stack. You could attempt to configure this using the Okta Terraform provider like so:


```text
device_is_managed = true  platform_include {    os_type = "IOS"    type = "MOBILE"  }
```


However, there are a few issues with this configuration. One issue, as highlighted in the documentation, is that if you set \`device_is_managed\` to \`true\`, you must also set \`device_is_registered\` to \`true\`. While it’s easy to understand that a managed device must first be registered through Okta Verify, it’s less intuitive why you need to explicitly declare this dependency in the configuration.


To address this, you’d revise the configuration like so:


```text
device_is_registered = true  device_is_managed = true  platform_include {    os_type = "IOS"    type = "MOBILE"  }
```


While this resolves the issue and creates a valid configuration, it introduces another problem: The resulting code is neither succinct nor easy to read. This is where the abstraction provided by my Terraform module shines. With the module, you can achieve the same functionality by simply defining the managed platforms in list format:


```text
managed_devices = ["ios"]
```


This approach is straightforward, concise, and far easier to understand. It abstracts the burden of configuring dynamic blocks for each platform you specify. It minimizes the cognitive load for developers while ensuring that your configuration is both correct and maintainable.


Press enter or click to view image in full size


## Priority Abstraction


Okta allows you to set a priority for policies to determine the order in which rules are evaluated against the authentication context. However, managing priorities can become cumbersome as your authentication policies grow to include 10 or more rules. From a readability standpoint, having 10+ policy rules is challenging, as they won’t all fit on one screen. Now imagine needing to change the order in which these rules are evaluated — You’d have to manually adjust the priority of the rule you want to move and then increment or decrement the priorities of the preceding or succeeding rules accordingly.


This is where the module simplifies the process. It enhances readability and manageability by handling rule priorities for you. Simply declare your policy rules in your desired order, highest priority first, within the \`rules\` list argument. The module will automatically assign priorities based on that order. This approach eliminates the need for manual priority adjustments, making your configuration more intuitive and easier to maintain.


## Default Rule Abstraction


Okta policy framework sets rule priority with integers. The lower the number, the higher the priority. Zero and 99 are the highest and lowest priorities, respectively. When you create an authentication policy rule in Okta, a default rule is automatically attached to the policy at priority 99. This default rule is configured as an “access allow” rule. However, following best practices, it’s recommended to deny all traffic by default and only allow explicitly defined traffic.


To manage this default rule in Terraform, you would typically need to run an import operation (as[outlined in the documentation](https://registry.terraform.io/providers/okta/okta/latest/docs/resources/app_signon_policy_rule) ). Instead of handling this through manual imports, writing automation for the import, or editing the default rule directly in the Okta console, this module simplifies the process by creating its own default rule. The module assigns this rule a priority of 98 — the second lowest — ensuring it sits below all explicitly defined rules. This rule is configured as a “deny any request” rule (an implicit deny), reinforcing secure access by default while allowing your explicitly defined rules to take precedence.


Press enter or click to view image in full size


## MFA Abstraction


Session timeouts in Okta are configured at the authentication policy rule level, which governs access to an application. The[values](https://registry.terraform.io/providers/okta/okta/latest/docs/resources/app_signon_policy_rule#re_authentication_frequency-1) for session timeouts must be set using the[ISO 8601 Period](https://www.digi.com/resources/documentation/digidocs/90001488-13/reference/r_iso_8601_duration_format.htm) format. Although this format is great for computers, it can be less intuitive for humans. For instance, if you want to require multi-factor authentication (MFA) for every sign-in attempt (“step-up authentication”), the re-authentication period must be set to the smallest allowable interval. In ISO 8601, the smallest unit is seconds (\`S\`), and the format requires a time separator (\`T\`) between the date and time components. You prefix these rules with Period (\`P\`) to signify its compliance with the format. Therefore, to configure step-up authentication, you would set the period to \`PT0S\`.


Although this is easy for machines to parse, it’s not particularly human-friendly. To address this, the module introduces a simple step-up authentication Boolean setting, along with human-readable options for session timeouts. Instead of dealing with cryptic expressions, you can configure authentication frequencies in familiar terms, such as:


- daily (\`PT24H\`)
- hourly (\`PT1H\`)


This abstraction makes configurations more accessible and easier to maintain while still aligning with Okta’s requirements.


Press enter or click to view image in full size


## Phishing-Resistant Authenticator Abstraction


Okta allows you to define which categories of authenticators a user is permitted to use in a policy. This functionality is implemented via a \`[constraints](https://registry.terraform.io/providers/okta/okta/latest/docs/resources/app_signon_policy_rule#constraints-1) \` argument. At first glance, this might seem straightforward, as the provider expects \`constraints\` to be a list of strings. However, upon reviewing the[documentation](https://registry.terraform.io/providers/okta/okta/latest/docs/resources/app_signon_policy_rule#constraints-1) , you’ll find that these strings are actually string representations of nested JSON objects. For example:


```text
constraints = [    jsonencode(      {        possession = {          required = true          phishingResistant = "REQUIRED"          userPresence = "REQUIRED"        }      }    )  ]
```


This makes understanding and defining constraints more complex. Also, passing invalid JSON strings can sometimes fail silently, leading to unexpected behavior.


To simplify this process, the module abstracts these JSON strings by allowing you to use Boolean values instead. For example, let’s say you want your users to authenticate only with phishing-resistant authenticators (and who wouldn’t?). Simply set \`phishing_resistant = true\`, and the module will take care of generating and encoding the correct object manifest for you. This not only reduces the risk of errors but also makes your configuration far easier to read and maintain.


Press enter or click to view image in full size


## Conclusion


Managing Okta authentication policies can be complex, especially as your organization scales and your security needs grow. While Okta provides powerful tools to secure access, its flexibility can lead to intricate configurations that are difficult to manage and prone to misconfiguration. This is where the[Terraform module](https://registry.terraform.io/modules/lukasindre/authentication-policy-stack/okta/latest) steps in, offering abstractions that simplify policy management while maintaining robust security practices.


From managing network zones and device policies to automating rule priorities, creating secure default rules, and enabling human-readable configurations for MFA and phishing-resistant authenticators, this module eliminates much of the manual work and complexity involved in using Okta. By translating complicated JSON objects and cryptic formats into intuitive, declarative configurations, it enables teams to focus on building and maintaining secure systems without getting bogged down by the intricacies of policy definitions.


With cybersecurity always a top priority, adopting tools that balance flexibility, security, and usability is more important than ever. This Terraform module helps you leverage Okta’s full potential and also ensures that your authentication policies are easy to understand, maintain, and adapt to your organization’s evolving needs. Effortless Okta policies are no longer just a goal — They’re a reality.
