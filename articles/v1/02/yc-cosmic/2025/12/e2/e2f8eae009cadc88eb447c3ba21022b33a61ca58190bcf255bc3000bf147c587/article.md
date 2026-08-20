---
schema_version: "1.0.0"
document_id: "e2f8eae009cadc88eb447c3ba21022b33a61ca58190bcf255bc3000bf147c587"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/security-update-nextjs-1607-and-react-1901-now-deployed-across-all-new-cosmic-apps"
published_at: "2025-12-04T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:2dd8e5887cf55f8b9d3c6922a5ee614020e32c38fe2d5ce59b2adc77db336e07"
---

# Security Update: Next.js 16.0.7 and React 19.0.1 Now Deployed Across All New Cosmic Apps

# Security Update: Next.js 16.0.7 and React 19.0.1 Now Deployed Across All New Cosmic Apps


We're committed to keeping your applications secure. Today, we're announcing that all new Next.js applications deployed through the Cosmic AI Platform will automatically use the latest patched versions of Next.js and React, addressing a critical security vulnerability.


## What Happened?


On December 3, 2025, a critical-severity vulnerability in React Server Components ([CVE-2025-55182](https://vercel.com/changelog/cve-2025-55182) ) was disclosed. This vulnerability affects React 19 and frameworks that use it, including Next.js (CVE-2025-66478). Under certain conditions, specially crafted requests could lead to unintended remote code execution.


The vulnerability was present in:


- React versions 19.0, 19.1.0, 19.1.1, and 19.2.0
- Next.js versions ≥14.3.0-canary.77, ≥15, and ≥16
- Other frameworks embedding React Server Components


## Our Response: Immediate Action


As soon as the patched versions were released, we took swift action to protect all applications built on the Cosmic platform:


### ✅ What We've Done


1. **Updated All New Deployments** - Every new Next.js application deployed through Cosmic now uses:


- **Next.js 16.0.7** (patched version)
- **React 19.0.1** (patched version)


2. **Proactive Protection** - All future projects using Next.js 16 on the Cosmic AI Platform are now protected
3. **Comprehensive Coverage** - The latest secure versions are deployed by default for all new applications
4. **Continuous Monitoring** - We're continuously monitoring for security updates and will apply critical patches promptly


### 📋 Action Required for Existing Applications


If your application was deployed **before this security update** , you'll need to take action to ensure you're protected:


1. **Redeploy Your Application** - Trigger a new deployment to receive the patched versions
2. **Update Local Dependencies** - If developing locally, update your` package.json` to use the patched versions
3. **Verify Versions** - Check that you're using Next.js 16.0.7 and React 19.0.1 or later


### 🛡️ Additional Protection Layers


While new Cosmic applications automatically use the patched versions, it's worth noting that Vercel (where many Cosmic apps deploy) also implemented:


- Custom WAF rules deployed globally to protect all projects
- Automatic mitigation at the infrastructure level
- Collaboration with major CDN and WAF providers


These multiple layers of protection provide additional security for your applications.


## What This Means for Your Applications


### If You're Using Next.js on Cosmic:


✅ **New Deployments Are Protected** - All new applications automatically use the patched versions


⚠️ **Existing Apps Need Updates** - If your application was deployed before this update, redeploy to receive the security patches


✅ **No Configuration Required** - New deployments automatically include the secure versions


✅ **Continued Security** - We'll continue monitoring for future security updates


### Technical Details


The patched versions include hardened handling of user inputs to prevent unintended behavior in React Server Components. The vulnerability specifically affected how certain untrusted input was processed, potentially allowing attackers to execute remote code.


**Fixed Versions:**


- React: 19.0.1, 19.1.2, 19.2.1
- Next.js: 15.0.5, 15.1.9, 15.2.6, 15.3.6, 15.4.8, 15.5.7, **16.0.7**


## Our Commitment to Security


At Cosmic, security isn't an afterthought - it's built into everything we do:


1. **Rapid Response** - We monitor security advisories and apply critical patches immediately
2. **Secure by Default** - All new deployments use the latest patched versions automatically
3. **Multiple Protection Layers** - Infrastructure-level security complements application-level fixes
4. **Transparency** - We communicate security updates clearly and promptly
5. **Best Practices** - The Cosmic AI Platform generates code following security best practices


## Building Secure Applications with Cosmic


The Cosmic AI Platform makes it easy to build and deploy secure applications:


### 🚀 Modern Framework Support


- Always use the latest stable, patched versions of frameworks
- Automatic security updates for new deployments
- Support for Next.js, Astro, and other modern frameworks


### 🔒 Security by Default


- Code generation follows security best practices
- Secure API communication with Cosmic CMS
- Environment variable management for sensitive data
- HTTPS everywhere for data in transit


### 📊 Enterprise-Grade Infrastructure


- Deployed on Vercel's global edge network
- DDoS protection and WAF capabilities
- Automatic SSL certificate management
- Built-in security monitoring


## What You Should Do


### For Existing Applications (Deployed Before This Update)


2. **Verify Versions** - Ensure Next.js 16.0.7 and React 19.0.1 are in use
3. **Update Local Environment** - Match your development environment to production versions


### For New Applications


✅ Your applications automatically use the patched versions - no action required


### General Best Practices


1. **Review Your Dependencies** - If you have custom code or local development environments, ensure they're using the patched versions
2. **Monitor Security Advisories** - Stay informed about security updates affecting your stack
3. **Keep Applications Current** - Periodically redeploy to ensure you're using the latest secure versions
4. **Report Concerns** - If you notice anything unusual, contact our support team immediately


## Credit and References


We're grateful to the security researchers and teams who identified and addressed this vulnerability:


- **Lachlan Davidson** - For identifying and responsibly reporting the vulnerability
- **Meta Security and React Team** - For their partnership in developing and releasing the fix
- **Vercel** - For rapid deployment of WAF rules and coordination with the ecosystem


For complete technical details about the vulnerability and fix, see:


- [Vercel Security Advisory](https://vercel.com/changelog/cve-2025-55182)
- [Next.js GHSA](https://github.com/vercel/next.js/security/advisories)
- [React GHSA](https://github.com/facebook/react/security/advisories)


## Questions?


If you have questions about this security update or security practices on the Cosmic platform:


- **Check Your Deployments** - Visit your project's deployment logs to confirm the updated versions
- **Review Documentation** - Our[security documentation](https://www.cosmicjs.com/docs) covers best practices
- **Contact Support** - Reach out to our support team with any concerns
- **Community** - Join our[Discord community](https://discord.gg/MSCwQ7D6Mg) for discussions


## Building with Confidence


Security vulnerabilities are an inevitable part of the software ecosystem, but how we respond to them matters. At Cosmic, we're committed to:


- **Swift action** when security issues are discovered
- **Automatic protection** for all new deployments on our platform
- **Transparent communication** about security matters
- **Continuous improvement** of our security practices


Your applications built with the Cosmic AI Platform benefit from:


- Automatic security updates for new deployments
- Best-practice code generation
- Enterprise-grade infrastructure
- Proactive security monitoring


## Continue Building Securely


Ready to build secure, modern applications with confidence?


- **Start Building** - Create a[free Cosmic account](https://app.cosmicjs.com/signup) and experience secure-by-default development
- **Explore the Platform** - Learn about the[Cosmic AI Platform](https://www.cosmicjs.com/blog/introducing-the-cosmic-ai-platform)
- **See Examples** - Browse[Community Projects](https://www.cosmicjs.com/community/projects) built with latest secure versions
- **Read Documentation** - Explore our[comprehensive docs](https://www.cosmicjs.com/docs) for security best practices


The Cosmic AI Platform handles the complexity of keeping your applications secure and up-to-date, so you can focus on building great products for your users.


---


*This security update was applied to all new Next.js deployments on the Cosmic platform starting December 3, 2025. Existing applications require redeployment to receive updates. For questions or concerns, contact our support team.*
