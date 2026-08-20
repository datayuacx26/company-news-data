---
schema_version: "1.0.0"
document_id: "852cd5123f2e9dba8c101e074fa7f58b4032235bc27ba4f3135932df5bbc8677"
company_key: "yc-hatchways"
company: "Hatchways"
source_id: "yc-hatchways-news-import-82f6c29a3da8"
canonical_url: "https://www.hatchways.io/blog/an-interviewing-platform-that-supports-any-programming-language-or-framework"
published_at: "2023-09-05T00:00:00+00:00"
first_seen_at: "2026-07-21T22:29:15.346050+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:04beb8d66aa49b43ccf3ff71557336468733529e261dc923ca79d2a4bcf3a85f"
---

# An interviewing platform that supports any programming language or framework

The landscape of technical interviewing platforms is often characterized by limited language and framework support. Consider, for instance, CoderPad which, albeit robust, supports a spectrum of 43 languages and frameworks.


While this might initially seem adequate for interview purposes, it inadvertently leads companies into creating less-than-ideal interview experiences. The constraints mean interviewers can't always replicate real-world scenarios in the interview process. For example, if you are interviewing for an iOS engineering role, instead of asking candidates to develop and enhance an iOS app, you might confine the interview to simpler tasks like using Swift to solve what essentially looks like a LeetCode-style question (which have a[host of problems](https://www.hatchways.io/blog/leetcode-interview-alternatives#what-is-the-problem-with-leetcode-interviews) ).


As the tech world evolves, this problem is only going to get worse. More frameworks and languages (a good example is Flutter) are going to be created and it’ll be harder and harder to test practical skills. It is for this reason, our team sought to create an interviewing platform that is not restricted by any language or framework. We wanted to build a platform that makes interviewing better reflect on-the-job skills.


‍


*Language support for CoderPad*


‍


## GitHub-based Assessments


To take on this challenge of building a platform not limited by certain frameworks and languages, we took a novel approach by focusing on leveraging the concept of a “GitHub-based” assessment to solve this problem. Rather than confining candidates to using an online IDE and completing challenges in a black box (i.e. LeetCode problems), we ask candidates to work on top of a GitHub repository as part of the interview process. This allows us to run any type of assessment. We can even run assessments related to infrastructure as candidates can interact with CI tools like GitHub actions with our tool.


### Automating GitHub-based Assessments


In order to automate GitHub-based assessments, it only felt nature to build our infrastructure on GitHub Actions. This foundation empowers companies to craft any variety of automated tests executable within the GitHub Actions environment. The inherent flexibility here spans not only all languages and frameworks but also permits more sophisticated tests such as running linter tools or end-to-end (E2E) tests on candidate solutions.


A few cool applications of this approach include:


- **Automating mobile assessments:** Traditional interviewing platforms often struggle with comprehensive mobile evaluations, especially for full-scale Android or iOS projects. With GitHub Actions, executing unit and integration tests for these platforms is seamlessly possible.
- **Harnessing Cypress for front-end assessments:** Our platform's capability extends to automating front-end tests using E2E assessments. Additionally, it can capture and archive visual proof via screenshots or video recordings of the assessment in action.


In this article, we delve into the mechanics of leveraging GitHub's robust architecture, allowing us to run and automate technical evaluations across any language or framework.


As a side note, while the infrastructure and methodologies in this post are tailored to GitHub, they're not exclusive to it. We're gearing up to transpose this logic for compatibility with other platforms, such as GitLab, or even self-hosted git solutions.


## A Closer Look at How GitHub-based Assessments Work


Before we go through how our infrastructure works, it’d be helpful to share how assessments generally work on the Hatchways platform. Here are the steps involved to run an assessment:


- Candidates start an assessment by entering their GitHub username and selecting a starter code. The range of starter codes available is flexible, dependent on the assessment's parameters. It can span a broad spectrum, from myriad options to just one blank repository, letting candidates start from a blank slate. ese starter codes align with what we term as a "Source Repo," which is explained further in the subsequent section.
- Following this, our platform creates a dedicated private GitHub repository for the candidate to complete the assessment. The nature of tasks within this repo can vary. Some assessments might require candidates to create a pull request, while others may require candidates to do a code review.
- Lastly, a candidate can submit the assessment on our platform. Once a candidate submits, we run automated tests in a separate hidden repository from the candidate. Depending on the assessment's design, candidates might receive a snapshot of their performance and, if necessary, an opportunity to resubmit.


*The candidate experience on Hatchways*


‍


## How does our Infrastructure work?


Here is a diagram that explains how we built our infrastructure:


*Infrastructure of Hatchways*


Breaking down the individual components:


#### Source Repo


This serves as the backbone repository encompassing essentials required for executing your assessment. Key inclusions are:


- Any starting codebase that you want to provide the candidate to get started.
- Publicly accessible test cases or automated workflows, executed via GitHub Actions. When candidates creates a pull request to submit their solution, they can instantly view results directly on GitHub.
- Any hidden test cases or automated workflows triggered upon submission. To keep these out of the candidate's view, there is a *.hatchways.gitignore* file in this repo. This file outlines which files you don’t want candidates to see—these components are reintroduced when candidate submits the assessment. Dive deeper into its functioning via this[documentation](https://docs.hatchways.io/docs/automating-an-assessment) .


#### Candidate Repo


Every time a candidate starts an assessment, we create for them a dedicated “candidate repo” for them to complete their assessment. This repo is used to contain the candidates solution and to run public tests. This repo will not contain any files specified in the . *hatchways.gitignore* file or any other configuration files in the Source Repo.


#### Marking Repo


Every time a candidate completes the assessment, we create a private fork of the candidate repo. The hidden files from the source repo are merged in this fork. We then leverage GitHub Actions to run the hidden (as well as the public) workflows defined by those hidden files.


#### Hatchways API


In the private GitHub Action workflow, you can use the[Hatchways Github Action](https://github.com/hatchways/hatchways-action) to send test results to Hatchways. This enables our platform to display the results in a formatted way for an interviewer, or to action on the results (i.e. allow candidates to resubmit the assessment or to pass the candidate to the next round of interviews). Here is some[documentation](https://docs.hatchways.io/docs/using-the-hatchways-action-github-action) on this GitHub Action.


‍


## Conclusion


That's the essence of how we execute and automate any language/framework at Hatchways. While it's feasible to craft a DIY variant of this solution (perhaps by scripting something analogous to Hatchways or by incorporating a few manual interventions), you might want to bypass the hassle of maintaining this type of solution internally. If this is interesting to you, don't hesitate to[connect with us](https://calendly.com/hatchways-team/intro?month=2023-09) and we can share more about our platform or any additional tips on how to run this process internally.


‍
