---
schema_version: "1.0.0"
document_id: "c733eb13f84e45fbaf69a35a8cd72381d70ff0876bf6c485c16814b2413d32b4"
company_key: "yc-axolo"
company: "Axolo"
source_id: "yc-axolo-news-import-c5f1135929e7"
canonical_url: "https://axolo.co/blog/p/part-2-how-to-open-a-pull-request"
published_at: "2021-11-03T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:48.922612+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:18b737fb6a6b7e6969ba5aefc67cbee409ec74b254b787854d100f147d0cb3a8"
---

# How to open a pull request

Pull requests and code reviews are workflows used in high-performing engineering teams. Setting guidelines on how to conduct them will help in rationalizing and being more efficient as a team. In this section, we will underline what we think engineers should focus on to be efficient in handling GitHub pull requests. First thing first, let's understand the lifetime of each pull request.


## Different GitHub pull requests stages and workflows explained


### What are draft GitHub pull requests?


When you're on a GitHub paid plan, engineers can open their pull requests as drafts. A GitHub draft PR helps in indicating other people what you are working on while letting them know it isn't finished yet. If you're wondering how to create a draft PR, it's as simple as selecting the "Create draft pull request" option when opening a new PR. The GitHub draft PR feature is particularly useful for early feedback. It also enables engineers to work on their branch, see the differences with the base branch in real-time and prepare the pull request content for their peers. Usually, they will be able to update the first comment to explain the reasons the pull request was developed, how the developer decided to proceed, and add screenshots if the changes are visible.


From a GitHub draft pull request, you can start the conversation around the code and let your peers know that the code is not yet ready for review. When you're ready to convert draft PR to PR, you can do so with a single click. The process to GitHub change PR from draft to open is straightforward and can be done through the UI or API. You can also convert PR to draft if you need to make additional changes after marking it ready for review. The GitHub draft PR workflow is designed to be flexible and user-friendly.


### My pull request is ready for review


When the code is ready to be reviewed, the developer can convert draft PR to PR, or open the pull request if it was not a draft. This process of converting PR to draft and back is straightforward in GitHub. At that stage, your work is done and you're awaiting a review from one of your peers. Whether you're working with an open PR or need to convert draft PR to PR, the process remains consistent. The GitHub draft pull request workflow is designed to be flexible and user-friendly. You can convert PR to draft at any time if you need to make additional changes.


When the reviewer is ready, he will be able to open the pull request, see from the initial comments the context of the pull request, and from the diff page, the changes made to the code base.


### How to suggest changes on a GitHub pull request?


When reviewers see things that should be changed, they should write a specific comment on the code ('code review comments'), conclude their review with a general comment, and set the review to the state 'Request changes'. This will let the initial engineer know that he has comments to read and identify what reviewers suggest to changes.


### How to handle conflict in pull requests?


If you see the warning *'This branch has conflicts that must be resolved'* , that means that the code you changed has been updated on the base branch and you need to locally update it accordingly before merging your pull request.


To facilitate your work, we recommend automatically checking for potential conflicts whenever it is possible. It is easier to fix small conflicts than conflicts that have accumulated over time when you were working on your pull request. If you are an Axolo user, this feature is natively enabled in pull request channels.


To resolve conflict, there are two possibilities:


#### Resolve pull request conflicts from your IDE


To resolve conflicts from your IDE, open it and change your current branch to your pull request branch with` git checkout BRANCH-NAME` . Then, pull the latest version of the base branch code with` git pull REMOTE-NAME BASE-BRANCH-name` (in our team, this command is` git pull origin main` ).


Your terminal will warn you about conflicts he encountered.


From there, you can open directly these files (ctrl/command + left click). You will see the problematic lines with different highlights. In the following example, in green, you can see that the HEAD (our pull request branch:` branch-conflict` ) and the commit` e2d23ca6f` (commit on the base branch main) has a conflict in line 3.


Your task now is to resolve the conflict by accepting a specific change (from` main` or` branch-conflict` ) or by merging the two changes and committing your work.


If you click on:


- **'Accept Current Change'** , your local changes will replace the updates in the base branch. You should confirm with your team that your changes are validated.
- **'Accept Incoming Change'** , you will erase your local changes and replace them with the base branch,
- **'Accept Both Changes'** , the changes will both stay one after the other and your next commit will resolve the conflict.


In any way, you now need to commit your changes to your local branch to complete the merging process. You should commit with a comment explaining how you resolved the conflict.


#### Resolve pull request conflicts from the GitHub UI


If the conflict is easy to resolve, you can do it directly from the GitHub UI by clicking on the **'Resolve conflicts'** button.


An interface with each conflict opens.


Then, you will have to delete the three lines marked with a red warning, mark each conflict as resolved, and commit your changes with the **'Commit merge'** button.


Your conflict is now resolved!


## How to approve GitHub pull requests?


When reviewers think that the pull request is ready to be merged, they approve the pull request from the GitHub UI. This will let the creator of the pull request know that the base branch can safely receive the changes from this branch. If you're wondering how to approve a pull request in GitHub, it's as simple as clicking the approve button in the review section. The GitHub API pull request endpoints can also be used to automate this process.


Usually, we leave a master comment saying **LGTM** (Looks Good To Me).


### Merging a GitHub pull request


After the first reviews, approval has been agreed upon and all checks have passed successfully, the pull request is now ready to be merged. You can manually click on **'Merge'** to update the base branch with the pull request changes.


There are three possibilities when merging a pull request:


- **create a merge commit** : all commits from the current branch will be added to the base branch via a merge commit,
- **squash and merge** : all commits from the current branch will be combined into one commit in the base branch,
- **rebase and merge** : all commits from the current branch will be rebased and added to the base branch.


### Why do we close GitHub pull requests?


If the changes proposed in a pull request are no longer needed, or if a solution has been found in another branch, you can decide to close pull request to prevent the change from affecting your base branch. When you close pull request, it's important to provide a clear explanation of why the changes are no longer needed. The ability to close pull request is an important part of maintaining a clean repository. You can also use the GitHub API pull request functionality to programmatically close pull requests.


## Stacked pull requests


### Why should you use stacked pull requests?


There are two main reasons to use stacked pull requests:


#### To split larges pull requests into small and coherent units


As we detailed it in our section , *"one pull request should focus only one task"* . Splitting a large pull request into small and coherent units will help both the author of the PR to develop it, and the reviewer to understand what the PR is about. For more information on managing stacked PRs effectively, check out our guide on .


#### To enable two engineers to work on the same feature


Imagine you need to develop a new authentication in your application. Let's say you're working on a marketplace. The foundation of the general authentication is ready but the team is still working on non-core features while you need to implement the buyer and seller auth on top of this branch.


The easiest way to work on it would be to create your branches from the general authentication branch. When your work will be completed, a stacked pull request will enable the rest of the team to validate your change and merge it in the general authentication branch.


Working on stacked branches is easier than working on the same.


#### How to stack pull requests


To stack a pull request from your terminal, jump on the initial branch you want to start from:


```text
$ git checkout initial-branch


```


Then, create a new branch from your initial branch:


```text
$ git checkout -b my-new-branch


```


Now, you can start developing. When your work is completed, open a pull request as usual but instead of asking for a merge on your base branch (` main` or` master` usually), select your` initial-branch` .


#### How to split existing large pull requests into a stack


Let's take the previous example and say you've already developed the buyer authentication on top of the general authentication. Your end goal is to merge your large PR buyer authentication to main, and now, you want to stack the buyer authentication pull request. The process is:


```text
$ git checkout general-authentication
$ git reset --soft main
$ git restore --staged


```


On your branch, you uncommit all the changes that are not present in the` main` branch. This does not remove the changes themselves, but stages them - you can verify this with` git status` . Now, you unstage them with` git restore` .


From there, you can checkout your new branch to reflect the changes between` general-authentication` and your current branch.


### GitHub pull request best practices


You can find here some general examples to follow for handling pull requests. If you wish to learn more about the best way to organize and handle code review, please go to our dedicated section: . For more comprehensive information about code review practices, visit our .


#### Why should I care?


Asking for code reviews with good pull requests is important for several reasons:


- A good pull request will be reviewed quickly,
- It will prevent bugs from being pushed into codebase,
- It does not block your teammates and let them handle the code review faster,
- It speeds up the product development process,
- It facilitates new developers onboarding.


#### Best practices to open a pull request


##### Clearly state the purpose of the pull request


The objective here is to provide an overview of why the work is taking place and share any relevant links to ease the work for the reviewers. For example:


- This simplifies the interface of…
- Fix the error of Linear issue 143 (link)
- Pull request of Jira ticket DEV-298 (link)


##### Use a formal and detached tone


Assume that anyone in the company can review your pull request. Avoid using familiarities as a pull request could be re-opened later on if an issue was found.


##### Be explicit on what you're expecting


If you're looking for feedback on specific parts of your code, be explicit. You can comment on your codes before requesting a review to explain your work or start a conversation.


##### Don't request a review if the pull request is still in Work In Progress.


State clearly WIP in the pull request title or open your pull request as a draft.


##### mention people or team you want to involve


You should @mention individuals or teams that you specifically want to involve in the discussion and explain why. ("@alex is it was the user was expecting?", "@security, any concerns with this logic?")


#### Best practices to ask for changes


##### Understand the context of the pull request


Before looking at the code, you should know why the code needed to change in the first place.


##### Take a moment to think before reacting


If you disagree strongly on a change, write your answer down and wait a few minutes before responding.


##### Ask, don't tell


Try to offer solutions or alternatives with an open question instead of a direct order.


##### Explain with content to state your point


If you ask for changes, explain why with references or company writeups that state your point. Usually, what is not written does not exist.


##### Offer ways to improve


Instead of asking for changes, offer ways to improve or simplify code. Show the creator of the pull request that you're involved.


##### Avoid using derogatory terms and be humble


Try to start a conversation and not give directives.


##### Use code review as a way to improve your team


Keep in mind that code review is one of the best ways to develop professional skills, product quality, group knowledge. Group and team critique are not to judge but to improve.


##### Use positive language


Try to use positive language as opposed to neutral. In online communication, there are a lot of negative biases. Use emoji or gif to clarify tone. At Axolo, we use a[code review emoji guide](https://github.com/axolo-co/code-review-emoji-guide) . It puts more ownership on the reviewer to give the reviewee added context and clarity to follow up on code review.


##### Reward good work!


It's easy to spot errors but take the time to congratulate good logic.


### What are Github pull request status checks?


Pull request checks enable you to know if your commits meet the minimum conditions you need to pass to merge your branch to the base branch. You can add commit to pull request as needed, and the checks will run automatically. Whether you're working with an open PR or need to convert draft PR to PR, these checks ensure code quality. You can also add commit to pull request through the GitHub API pull request endpoints. The GitHub draft PR feature helps teams maintain code quality throughout the development process. Checks are repository-wide and mandatory in today's engineering teams to follow comment CI/CD practices.


Each pull request has a **Checks** tab where you can view details of build outputs from status checks and rerun failed checks.


#### Develop your pull request checks


Using GitHub Action, you can develop your pull request checks. These workflows help you test your code with specific rules or environments and validate your pull request before merging your work. You can use this setup to send a specific notification to your team. At Axolo, we wrote a .


#### Integrate external applications to check your pull request


There are hundreds of external applications on the GitHub marketplace to automatically check your code. These integrations have direct access to your code and can be tailored to your needs to improve your CI/CD workflows.


### Use the GitHub pull request command line


GitHub developed its command line. It brings pull requests and issues to your terminal next to where you're already working. The GitHub pull request commands are a good way to quickly create, checkout, convert a pull request to ready for review, merge, and many other common actions. You can also use the GitHub API pull request endpoints to automate these processes, including how to create a draft PR or convert draft PR to PR programmatically. The GitHub API pull request functionality is particularly useful for teams looking to automate their workflow. For more information on optimizing your GitHub pull request workflow, check out our article on .


GitHub CLI is available for iOS, Windows and Linux users. More information about the installation of the GitHub CLI[can be found in the open-source project.](https://github.com/cli/cli#installation)


### Handle GitHub pull request notifications


There are four ways to stay up to date with GitHub pull requests notifications. Whether you're working with an open PR or a GitHub draft pull request, staying informed about changes is crucial. The GitHub draft PR feature helps teams stay organized and maintain clear communication throughout the development process. The GitHub change PR from draft to open process is designed to be seamless and intuitive.


#### Receive your pull request notifications by emails


By default, GitHub sends you updates concerning your pull requests by email. If someone did review or request a review on a pull request you're assigned to, these updates will be sent directly to your mailbox.


#### Use the default GitHub notifications UI


If you spend your day in GitHub, you can find your recent notification within the GitHub UI. Notifications from all your repositories and pull requests will pile in your inbox. To be efficient, a fair amount of settings will need to be updated to receive only the important notifications.


#### Integrate GitHub pull requests with third-party apps


There are alternative ways to receive notifications from GitHub pull requests in the GitHub marketplace. We wrote a on our blog but you can discover other applications on the marketplace directly. For more information about integrating GitHub with Slack, check out our .


#### Develop your workflows with the GitHub API


If you have resources available and the potential to maintain internal applications, you can quickly access the GitHub API and build your logic for an internal use case.


Continue our pull request study in .
