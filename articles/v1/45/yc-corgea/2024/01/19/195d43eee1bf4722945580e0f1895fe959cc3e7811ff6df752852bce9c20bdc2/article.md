---
schema_version: "1.0.0"
document_id: "195d43eee1bf4722945580e0f1895fe959cc3e7811ff6df752852bce9c20bdc2"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/introducing-download-fix"
published_at: "2024-01-08T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:8f2f2ec6b08911ebe0015dc0d5756f55b9f703c093273e00bf1427f42d85b3f5"
---

# Introducing "Download Fix"

At Corgea, we’re always looking for ways to enhance the security and efficiency of your software development process. Today, we’re thrilled to announce our latest feature: the “Download Fix” feature. This new addition to[Corgea AI SAST](https://corgea.com/products/ai-sast) is set to revolutionize how developers handle code vulnerabilities detected by Static Application Security Testing (SAST) scanners like Snyk, Semgrep, and others.


## What is “Download Fix”?


“Download Fix” allows users to download Git diffs and files containing fixes for vulnerable source code identified by SAST scanners. This feature addresses a critical need in the software development lifecycle by empowering engineers to take immediate, informed action on security vulnerabilities.


## Why We Built “Download Fix”


Our motivation behind introducing “Download Fix” is simple yet profound: to empower engineers. In the words of Ahmad from Corgea, “we did this to empower engineers to download these fixes locally first, so that they can apply changes or even compound fixes.” This level of autonomy and flexibility ensures that fixes are not just automated but also customizable to each project’s unique needs.


Moreover, “Download Fix” is designed to be inclusive. It’s a solution for teams that may not use GitHub, as Corgea will soon extend support to GitLab and Bitbucket. This broadens our tool’s accessibility, making it a versatile option for various development environments. These in-flow remediation paths are now part of[Corgea developer experience](https://corgea.com/products/developer-experience) .


## How “Download Fix” Works


“Download Fix” integrates seamlessly into your existing workflow. You can see this quick 1m demo on it.


[New Feature: Download Fixes - Watch Video](https://www.loom.com/share/3193cfd1eaf649299568657ddae5cf95)


Here’s a quick rundown of how it works:


1.


**Identify Vulnerabilities:** First, SAST scanners like Snyk or Semgrep to identify vulnerabilities in your source code.


2.


**Generate Fixes:** Corgea’s AI then automatically generates code fixes for these vulnerabilities.


3.


**Download Options:** On Corgea, you can click on “Fix Issue.” This action presents two options:


1.


Git Patch: This option shows a Git diff file that can be applied using the \`git apply\` command.


2.


Fixed File: Alternatively, you can download a full file fix, which can be applied locally in your IDE or file management system.


4.


**Apply Locally:** Once downloaded, you can apply these fixes locally. This step allows for additional customization or compound fixes before finalizing the changes.


5.


**Create Pull-Request:** After applying and verifying the fixes, you can create a pull request for the changes to be merged into your main codebase.


## Looking Ahead


We’re excited about the potential of “Download Fix” to streamline the way developers address code vulnerabilities. As we continue to support more platforms and enhance our features, Corgea remains committed to providing tools that not only secure your code but also respect and enhance your workflow.


Stay tuned for more updates, and thank you for choosing Corgea as your partner in secure and efficient software development.
