---
schema_version: "1.0.0"
document_id: "cd3d6151dd0ba6c73adddf097f985e28acba20103defdb57bed556a1cb7e93e2"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-dmarc-applies-to-subdomains"
published_at: "2025-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:3ce4f62993ffdc1e6b052c8c529b24259e1bc7a52bfd9124e5608f7ff5a72d7c"
---

# How DMARC Applies to Subdomains

Most advice about DMARC stops at the basics: publish a policy, monitor reports, and eventually enforce. But what happens when emails come from subdomains like` mail.example.com` or` newsletters.example.com` ?


Let's examine how DMARC works with subdomains. Along the way, we’ll answer:


- How does a receiving server determine which DMARC policy to apply?
- When does a subdomain inherit the parent domain’s policy?
- Should you use the` sp` tag to manage subdomain behavior?


This guide assumes you understand the basics of DMARC. If you're not sure on the tags available or the policies` none` ,` quarantine` , or` reject` , start by reading our[guide on DMARC policy modes](https://resend.com/blog/dmarc-policy-modes) .


Before we begin, let's first understand the *organizational domain* (or parent domain), as DMARC depends heavily upon it.


## Understanding the organizational domain


Every domain on the internet has a base domain from which subdomains are derived. DMARC relies on this concept to decide where to look for policy information.


Domain Name Organizational Domain


yourdomain.tld yourdomain.tld


mail.yourdomain.tld yourdomain.tld


send.mail.yourdomain.tld yourdomain.tld


Your base domain is called the **organizational domain** , and it’s not always just the last two segments of a hostname. For example, in the U.K., the entire` yourdomain.co.uk` is considered the organizational domain, not merely` co.uk` .


DMARC validators reference a[public suffix list](https://publicsuffix.org/list/public_suffix_list.dat) to determine the organizational domain. This list defines what counts as a "suffix" (or top-level domain) (like` .com` ,` .co.uk` , etc.) and helps determine where a domain truly starts.


## How receiving servers find a DMARC policy


When a mail server receives a message and wants to apply DMARC, it follows a process called **policy discovery** .


Here's a simplified diagram of the policy discovery process:


Here's what happens behind the scenes:


1. **Extract the domain in the “From” header** (called the` RFC5322.From domain` ).
2. **Look up a DMARC record at that domain.**


- If exactly one valid record is found, use it.
- If multiple records are found, DMARC is skipped.
- If none are found, continue.


3. **Look up a DMARC record on the organizational domain.**


- Again, if one record is found, use it.
- If none or multiple records are found, DMARC is skipped.


4. **Check that the record includes a valid` p` tag.**


- If not, but there’s a valid` rua` (reporting address), the server defaults to` p=none` .
- If there’s no valid` p` and no` rua` , DMARC isn’t applied.


**Intermediate subdomains are skipped**


Importantly, **only two DNS queries** are made at most — one for the exact domain and one for the organizational domain. Intermediate subdomains are skipped entirely.


For example, if the “From” domain is` send.mail.yourdomain.tld` , a policy discovery will check the following domains in this order:


- ` send.mail.yourdomain.tld`
- ` yourdomain.tld`


It will *not check* intermediate subdomains like` mail.yourdomain.tld` , even if those records exist.


## How policies apply to subdomains


The DMARC policy (i.e.,` p=none` ) on your organizational domain cascades to subdomains, but more specific policies can override it.


There are three scenarios to consider:


### 1. Subdomain publishes its own record (most specific policy)


Any subdomain that publishes its own DMARC policy (with a` p=` tag) will override whatever the organizational domain says, including the` sp` value.


```text
yourdomain  .  tld          TXT      "v=DMARC1; p=reject; sp=reject"    mail  .  yourdomain  .  tld     TXT      "v=DMARC1; p=none"
```


In this example,` mail.yourdomain.tld` will follow its own policy (` p=none` ), not the` sp=reject` from the organizational domain.


### 2. Subdomain follows the organizational domain’s sp tag (less specific policy)


The optional` sp` tag (short for subdomain policy) can be set on the *organizational domain* only. It allows you to *treat subdomains differently* from the organizational domain, without having to publish a DMARC record on each subdomain.


If the subdomain doesn’t publish its own record, it will inherit the` sp` value if it exists.


```text
yourdomain  .  tld    TXT      "v=DMARC1; p=reject; sp=quarantine"
```


Here, the base domain enforces` p=reject` , but all subdomains default to` p=quarantine` since the` sp` tag has higher precedence than the` p` tag for subdomains.


The` sp` tag is useful if your main domain is locked down, but you’re still gradually ramping up enforcement on subdomains.


**Should you use *sp* on a subdomain?**


No, there’s no reason to include` sp=` in a DMARC record on a subdomain. It will always be entirely ignored, as the` sp` tag is only supported on the organizational domain.


```text
mail  .  yourdomain  .  tld    TXT      "v=DMARC1; p=quarantine; sp=reject"
```


In this case,` sp=reject` is meaningless. The` sp` tag doesn’t support “nested inheritance.”


### 3. Subdomain inherits the organizational domain’s policy (default if no other policy is found)


If a subdomain doesn't have its own DMARC record, it will follow the organizational domain’s` p` value.


```text
yourdomain  .  tld    TXT      "v=DMARC1; p=reject"
```


In this case,` mail.yourdomain.tld` and all other subdomains without DMARC records will implicitly enforce` p=reject` from the organizational domain.


## Recommended approach: Start simple, then tighten up


If you're still testing the waters with DMARC or managing multiple systems, your best move is to **start simple** and slowly tighten up.


### 1. Start with a single policy


A safe default that covers your whole domain and subdomains looks like this:


```text
yourdomain  .  tld    TXT      "v=DMARC1; p=none;"
```


### 2. Tighten your organizational domain first


Once you're confident all sending sources on your organizational domain are aligned and authenticated, you can tighten this up:


```text
yourdomain  .  tld    TXT      "v=DMARC1; p=reject; sp=none;"
```


The` sp=none` tag means that subdomains will still use the safer` none` policy, but the organizational domain will enforce` p=reject` .


### 3. Tighten up all subdomains


Once you're confidence all sending sources on your subdomains are aligned and authenticated, you can tighten this up by either changing the` sp` tag to` reject` or by removing the` sp` tag entirely and relying on the` p` tag to cascade:


```text
yourdomain  .  tld    TXT      "v=DMARC1; p=reject; sp=reject;"
```


This configuration ensures that your primary domain and all of its subdomains are protected. If you need to make exceptions, publish explicit policies only on those subdomains.


Unless you’re intentionally managing complex routing or vendor-specific setups, you generally don’t need to use the` sp` tag. Set a strong policy at the organizational level and only override when absolutely necessary.


## Summary


Here's what to remember about DMARC and subdomains:


1. DMARC only checks two places: the domain in the From header and its organizational domain.
2. Subdomains inherit the organizational domain’s` p=` tag unless overridden — or unless` sp=` is specified at the organizational level.
3. ` sp=` only works when defined on the organizational domain.
4. Avoid putting` sp=` on subdomains (it will be ignored), as it can only be applied to the organizational domain.
