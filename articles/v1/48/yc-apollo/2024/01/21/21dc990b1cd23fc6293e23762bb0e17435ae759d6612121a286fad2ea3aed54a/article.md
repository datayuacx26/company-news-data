---
schema_version: "1.0.0"
document_id: "21dc990b1cd23fc6293e23762bb0e17435ae759d6612121a286fad2ea3aed54a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-apollo-manages-swift-packages-in-a-monorepo-with-git-subtrees"
published_at: "2024-01-30T07:45:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:802770788b7209813b75affdcc8125800a097434887122cc7dc505eeb6a53903"
---

# How Apollo Manages Swift Packages in a Monorepo with Git Subtrees

The monorepo is a common structure for many software projects, and Apollo iOS is no different. Our project was structured in a single repo containing many different Swift Package Manager (SPM) library targets, along with test code and code only used as part of development. This presents a few problems however, one being that when a user adds the SPM package as a dependency, it will pull down all of the code and files in the repo, even though most of them aren’t needed. Another is that all of the individual libraries within the SPM package share the same version number.


With this in mind, we set out to look for a way to restructure our libraries in such a way that we could provide users with smaller, more concise SPM packages, as well as give ourselves flexibility in providing separate features that can be versioned independently of the main Apollo iOS and Code Generation libraries.


In thinking about what we wanted to achieve, the high-level goal became clear: separate our libraries into separate smaller repos which can be provided as their own SPM packages. However, separating the libraries out into different repositories could make the development and testing of the libraries much more difficult due to how they work together.


So the question became how could we reach our end goal of providing separate library packages, while still maintaining an efficient development workflow?


## **Git Submodules**


The first thing we looked into was creating a development repo and using git submodules to include all of the SPM package repos so they could be developed in the same workspace together. The submodule functionality would pull down each SPM package repo into a sub-directory of the development repo, and keep a reference to the commit hash being referenced for each repo. This would solve one of our problems, which was to be able to develop and test all of our libraries together, while distributing them separately.


The downside to this is that we would have to manage changes in each submodule individually as well as always ensure the latest commit hash is checked out in the development repo when working on the libraries. This introduces a lot of overhead to the development process and opportunity for error.


The other issue with this approach is that it meant changes for each library would need to be pushed in their own pull requests (PR) in their respective repo. Which means even though you may be working on a feature that contains changes across multiple SPM packages, you would not be able to review the PRs in a context which allows you to easily see and review all of the changes related to your work.


## **Enter Git Subtree**


In doing further research we came across the git subtree functionality. While this functionality is similar to git submodules, the way in which it functions is different. Instead of including separate repositories as sub-directories and references to commit hashes, the subtree functionality pulls all of the code of a repo into your repo from a specific reference, which can then be worked on independently as part of your repo. Although it can be worked on independently you can still push/pull changes to/from the remote repository.


This seemed like a promising way to create our new repo structure, so we began working on a proof of concept to see if we could get the subtree functionality to meet our needs. Through some testing we found that we could make changes to any combination of code in the development repo and subtrees, and then use the subtree split and push commands to separate out individual subtree changes, and push them to their remote repo.


The subtree` split` command, along with the` rejoin` option, will find the last split that was done for the given subtree and then search all commits since then for changes to the subtree code only, and then pull those commits out into their own separate commit(s) which can then be pushed to the subtree remote repo. Using the` squash` option will take all of the commits found during the split search and combine them into a single commit vs keeping all of the commits separate. The rejoin option is what essentially creates the “checkpoints” in the repo so the split command can avoid searching the entire git history every time it is run.


With this type of workflow we would be able to develop all of our libraries together, including creating single PRs in our development repo for review, and then pushing changes out to the subtree remote repositories as they merged in.


## **Automating the Workflow**


With the workflow figured out, the only thing left to do was to automate as much of it as possible to create a seamless development workflow. This led us to build[a GitHub Actions workflow](https://github.com/apollographql/apollo-ios-dev/blob/main/.github/workflows/pr-subtree-push.yml) that watches for PRs to be merged into the main branch of our development repo. When a PR merge is detected we then run the appropriate git subtree commands to check each subtree for changes, and if changes are found then split them out and push them to their remote repository.


This automation allowed us to fully reach our goal to provide an easy and seamless development workflow for all of our libraries, while still maintaining them as separate SPM packages.


## **Wrapping Up**


Throughout this process, and some trial and error testing out git submodules and subtrees, we were able to meet the goals we set out to achieve for our project and workflow:


- The ability to provide more concise SPM packages for each of our features.
- The ability to version our SPM feature packages independently if necessary.
- Maintain an easy development workflow that allows us to develop and review everything together, but distribute separately.


Here is a look structure of our Git repos before implementing subtrees, and after they were implemented in the 1.6.0 release:


This specific subtree use case meets our needs perfectly as an open-source project with multiple feature libraries, and is great for our uses because of the benefits we have described in this article. Hopefully this article can provide insight into how other projects may benefit from a subtree setup. Whether this is your first time hearing about subtrees, or you just didn’t believe they could benefit your project, there is a lot of power in the commands available through git subtrees that could be combined to achieve many different outcomes for different use cases.


We always welcome your feedback, so please don’t hesitate to reach out on[GitHub](https://github.com/apollographql/apollo-ios) , the[Apollo Community Forums](https://community.apollographql.com/tag/mobile) , or the[Apollo Discord](https://discord.gg/graphos) server.


Written by


Zach FettersMoore


[Read more by Zach FettersMoore](https://www.apollographql.com/blog/author/zach)
