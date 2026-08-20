---
schema_version: "1.0.0"
document_id: "8d1c09952da2758202f44090de7ecc6a79abfea3d9c87a2b1f7aa7176b655b19"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46599-changes-to-email-template-customisation-on-free-tier"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:09abd696b35776c45168f42352373d1bdbfa8fecf75386c8add5e52ec4f639e0"
---

# Changes to Email Template Customisation on Free Tier

For the past six months we've been tracking a steady increase in coordinated abuse of Supabase's free-tier email infrastructure. Bad actors were standing up free Supabase projects, rewriting the auth email templates with phishing content, and then triggering signup or password-reset flows against arbitrary email addresses to deliver those phishing emails from our SMTP infrastructure to people who had no relationship with Supabase.


We rolled out increasingly aggressive rate limits on outbound auth emails. We deployed keyword blocklists to catch the most common phishing payloads. We built automated detection that flagged suspicious template content and disabled offending projects. Each one was met with a workaround within days if not hours.


To our knowledge Supabase was the only auth provider that offers both a hosted email service and fully customizable email templates on free tier. Obviously this combination is what made us a uniquely attractive target.


All of this is to say we tried hard to ship changes that wouldn't affect legitimate users. But the abusive accounts — while likely just a handful of individuals — given the scale of their abuse, were responsible for the bulk of spam leaving our infrastructure, and we've reached the point where the volume risks having our email server blacklisted entirely.


### What's changing#


Starting today Wednesday, 3 June 2026, new free-tier projects using Supabase's default email provider will no longer be able to modify their auth email templates. The default templates — confirmation, password reset, magic link, etc. — will be used as-is.


### Who's affected#


Existing free-tier projects keep their current email templates exactly as they are. Nothing changes for projects created before 3 June 2026. Paid plans (Pro and above) are not affected. Template customization continues to work as it does today. Free-tier projects that configure their own SMTP provider can continue to customize templates freely. The restriction only applies when sending through Supabase's default SMTP.


If you're starting a new free-tier project and need branded auth emails, you can configure your own SMTP provider (Resend, Postmark, SendGrid, Amazon SES, etc.) in your project's auth settings — once enabled, you can customize your templates as before.
