---
schema_version: "1.0.0"
document_id: "fb0ea28aa5ac5f8aa053a8e8be8b16f6bac0d86eedb654a135bde1ac1ea5a6dd"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/how-we-automated-our-changelog-thanks-to-chatgpt"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:1a9617254340fed53cdff6ef96794e8bed010630b90b5f09326b2afe5de7c0aa"
---

# How we automated our changelog thanks to ChatGPT

At Photoroom, we're no strangers to rapid iterations. With our web application being updated daily – usually more than once – it had become crucial to communicate all the changes made to our codebase internally.


One approach is to create a changelog, which is essentially a record of all changes made to a project.


However, updating a changelog can be tedious since the frequency of updates increases as the team grows.


Could the creation and maintenance of that changelog be a task for AI? As[we use AI to craft perfect product images](https://www.photoroom.com/tools/instant-backgrounds) , could we harness its ability to write our changelog entries automatically?


## OpenAI entered the chat


At Photoroom, we believe in the power of AI to enhance the creative process and make it more efficient; this is why we don't shy away from using ChatGPT in our day-to-day work.


My goal was for us to receive a crisp and well-structured weekly update on our Slack channel with the latest changes, a link to the associated GitHub Pull Request for each entry, and a link to its associated Linear Issue, when applicable.


And here's the result – note that I used a[public repository](https://github.com/facebook/react) here for testing.


Now, let's see how I got there and how you can easily replicate this for your team!


## Setting the foundations: Conventional Commits


Before embarking on our journey towards automation, we identified the need for a solid foundation:[Conventional Commits](https://www.conventionalcommits.org/) . This standardized approach to commit messages ensures that every change, be it a feature, a bug fix, or a minor tweak, is clearly labeled and categorized.


Switching to conventional commits is not a prerequisite in and of itself, but it helps organize the different changes in sections in our changelog.


I also wanted to link our Linear Issues so readers could access more context. Fortunately, our branch names are also normalized thanks to the[Linear GitHub integration,](https://linear.app/docs/github#basics) which automates our Pull Requests workflow.


## Putting the pieces together


Now that we were all set, we just had to put together our script, and for that, I asked ChatGPT to write a script that would:


1.


Retrieve all the Pull Requests merged during the last seven days


2.


Process the data to forge links to Linear Issues


3.


Write the changelog


4.


Post the result on Slack


5.


Automate the process using GitHub Actions


I had to iterate on the script as I started asking it to fetch commits, but I was missing the Issue numbers, so I used the merged Pull Requests instead – given that our main branch is protected and can only be updated via Pull Requests.


I also had to iterate on the prompt, asking ChatGPT to help me refine the prompt that I would feed to its API.


Finally, to make the process even more efficient, we automated it using GitHub Actions. The script was also written by ChatGPT, which made the process even smoother.


You can[find the final code in this GitHub Gist](https://gist.github.com/jeremybenaim/7ee9f1835e5973773bdefccb51d0fae9) .


And there you have it, an automated changelog our team looks forward to every Friday. It's not just about the updates anymore; it's about seeing AI, the core of our product, in action in yet another facet of our operations!


## Conclusion


I hope this post will inspire you to introduce the same to your team and, overall, to embrace AI as a productivity tool.


At Photoroom, our AI models are designed to streamline the editing process, allowing users to edit their photos quickly and easily without sacrificing quality. Just like ChatGPT has automated our changelog process, we strive to automate and simplify our users' image editing experience.


If this resonates with you, I invite you to[explore our job openings](https://jobs.lever.co/photoroom) . We look forward to speaking with you soon!
