---
schema_version: "1.0.0"
document_id: "73fcbb69ec2a3e24a1fd69d610801047fbf10c49efcac307e735ab98ce8f7953"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/finding-joy-in-git-conflict-resolution"
published_at: "2023-01-31T16:50:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:5c419e68e0c7253271ef67d7933cfb2502f93940452ff787436e22ee6ee290d8"
---

# Finding Joy in Git Conflict Resolution

Your big feature is tested and ready to go! Time to merge, and then…


Nooooo! 😭 Memories arise of hours of uncertainty spent trying to resolve past conflicts. Sure, there are lots of UIs that make picking one side or the other easier than using the command line, but is picking sides really the right answer?


I’ll share a hidden gem that, for me, has turned conflict resolution from frustration into something of a joy. Git has a built-in feature that you can enable called the` diff3` conflict resolution strategy. Turning this setting on enables a predictable approach to understanding and resolving the conflicts you encounter.


I’ll note that git conflicts can be minimized through practices such as Trunk Based Development with Feature Flags, breaking features into smaller chunks that you can deliver incrementally, and refactoring hotspots with high churn. Fewer conflicts is always a win. Even still, we’ll need to deal with conflicts occasionally whenever there are parallel development branches.


Before going further, let’s take a closer look at what’s missing from the default conflict markers that Git has always provided (for backward compatibility with other tools).


## Example


When Git is told to merge two branches of work that modify the same lines, Git won’t try to determine how to apply both changes to the same line. Instead, it inserts a set of conflict markers and relies on your beautiful human brain to work it out. These conflict markers delineate the end result of the lines with conflicting changes in both the currently checked-out branch (` HEAD` ) and the branch you’ve attempted to merge. (During a rebase, it checks out the commit you’re rebasing onto, which gets labeled` HEAD` in the first section, and the commits you’re rebasing show up in the second section as the change being “merged”.)


Take a look at this conflict. How would you resolve it?


```text
<<<<<<< HEAD
GreenMessage.send(include_signature: true)
=======
BlueMessage.send(include_signature: false)
>>>>>>> merged-branch


```


Spoiler: Looking at the conflict alone, it’s impossible to tell what each branch changed or what its intent was. We only have the end results. **We need more context to determine the intent behind each of the changes so that we can merge the intent of each.** This missing information is the biggest thing that makes conflict resolutions frustrating and incorrect.


You might find the author of the other branch and talk through it. You might use` git blame` to dig up the commit(s) or pull request(s) that introduced conflicting changes. Maybe you just think through it and put it together in a way that makes sense to you. Hopefully, you don’t just take a blind guess in the dark. 😬 Either way, it can be a lot of work to figure out and merge the larger intent of both branches, especially when it’s a large, complex change that’s difficult or impossible to hold entirely in your head.


diff3 to the rescue!


## let’s try this!


Running this command will add a configuration line to your` ~/.gitconfig` file, which you can remove later if desired:


```text
git config --global merge.conflictstyle diff3


```


After using this to turn on diff3, each new conflict will have a third section in the middle, the “merged common ancestor”.


If you’re already in the middle of a conflict and have just enabled the configuration, then you can have Git insert the new conflict markers using` git checkout --merge <files>` . The markers will be labeled *ours* , *base* , and *theirs* , but the concept is the same as the labels used below. (This command also helps if git rerere has already applied a previous resolution.)


## How to read a diff3 conflict


```text
<<<<<<< HEAD
GreenMessage.send(include_signature: true)
||||||| merged common ancestor
BlueMessage.send(include_signature: true)
=======
BlueMessage.send(include_signature: false)
>>>>>>> merged-branch


```


Same as before, the code in the first section shows what the code looks like at HEAD, the current commit (or when rebasing, the rebase target).


```text
<<<<<<< HEAD
GreenMessage.send(include_signature: true)
||||||| merged common ancestor


```


The **merged common ancestor** section shows what the conflicting line(s) were *before* —what the starting point was.


```text
||||||| merged common ancestor
BlueMessage.send(include_signature: true)
=======


```


The code in the last section shows what the code looks like at` merged-branch` —the branch or commit being merged.


```text
=======
BlueMessage.send(include_signature: false)
>>>>>>> merged-branch


```


Note that any of the section markers may have 0 lines between them, indicating an empty section. I’ll cover this in an example below.


## **The conflict resolution pattern**


With diff3 or without, the pattern of merging changes remains the same:


1. Compare the merged common ancestor with each of the conflicting changes. In other words, what did each branch do?
2. Choose one side as your starting point (usually the more complex change), leaving the other side (the simpler one) to apply.
3. Merge! Apply the simpler change to the other side.


Let’s try this with our example.


First, compare each change (HEAD, and then merged-branch) with the merged common ancestor, trying to determine each change’s intent. HEAD changes` BlueMessage` to` GreenMessage` . Its intent is to change the class used to GreenMessage, passing the same parameters.` merged-branch` changes` include_signature` from` true` to` false` , intending to stop including a signature in messages.


In this case, both changes are simple, so I’ll arbitrarily use the HEAD section as my starting point, and apply the other change:` include_signature` changes from` true` to` false` . Starting from the other side would produce the same result.


After removing the other sections and their conflict markers, we have this:


```text
GreenMessage.send(include_signature: false)


```


Repeat for each conflict, and we’re done! Add the resulting merged files, and continue with the merge/rebase/cherry-pick.


## More examples, with explanations


### Beyond a single line


When multiple lines are involved, the concept remains the same. we’ll follow the same resolution pattern in this example:


```text
<<<<<<< HEAD
create_table "comments" do |t|
||||||| merged common ancestor
create_table "comments", id: :integer do |t|
=======
create_table "comment_user_mentions" do |t|
t.integer "user_id", null: false
t.integer "comment_id", null: false
t.datetime "created_at", precision: 6, null: false
t.datetime "updated_at", precision: 6, null: false
t.index ["comment_id"]
t.index ["user_id"]
end


create_table "comments", id: :integer do |t|
>>>>>>> origin/other-branch


```


1. Examine what each side changed, compared to the common ancestor section.


- HEAD: a fairly small change, removing` , id: :integer` from the` create_table "comments"` line
- origin/other-branch: Add a new` create_table` block for` comment_user_mentions` before creating the comments table.


2. Choose the simplest change to apply. they’re both straightforward, but removing` , id: :integer` seems easiest to me.
3. I’ll use the bottom section as my starting point and remove` , id: :integer` . Having applied the top change to the bottom, I’ll keep the bottom and get rid of everything else, leaving this resolution:


```text
create_table "comment_user_mentions" do |t|
t.integer "user_id", null: false
t.integer "comment_id", null: false
t.datetime "created_at", precision: 6, null: false
t.datetime "updated_at", precision: 6, null: false
t.index ["comment_id"]
t.index ["user_id"]
end


create_table "comments" do |t|


```


If you were to choose the opposite section as a starting point, then you’d add the create_table block (including the new blank line that’s not present in the middle section) to the top section. The end result is the same!


Pro Tip: Select full lines with a triple-click drag to avoid having to clean up stray whitespace when copying or deleting multiple lines.


### Lines added to the same location; order ambiguous


```text
@import 'alpha_centauri';
<<<<<<< HEAD
@import 'some_file';
||||||| merged common ancestor
=======
@import 'other_file';
>>>>>>> merged-branch
@import 'the_ultimate_library';


```


When the merged common ancestor is a blank section, it indicates each branch added lines at the same position. In other words, both started with nothing between the non-conflicting lines at that location. In these situations, unless both variations have the same intent, we want to keep both added lines, ordered however makes the most sense.


An example resolution, maintaining alphabetical order:


```text
@import 'alpha_centauri';
@import 'other_file';
@import 'some_file';
@import 'the_ultimate_library';


```


### Changes with the same intent


```text
<<<<<<< HEAD
# Broken; see issue #135
||||||| merged common ancestor
=======
# this doesn't work
>>>>>>> merged-branch
function calculateTotal() {


```


Here again, each branch added lines at the same position. However, in this case, both changes share a common intent: document breakage in the following code. This type of conflict can also happen when multiple people fix the same bug in different ways. With the same intent, keeping both additions would be redundant here and harmful in other scenarios. Instead, we’ll pick the implementation that best accomplishes the intent or combine the best of both.


Example resolution:


```text
# Broken; see issue #135


```


### Moved or deleted lines


```text
<<<<<<< HEAD
||||||| merged common ancestor
BlueMessage.send(include_signature: true)
=======
BlueMessage.send(include_signature: false)
>>>>>>> merged-branch


```


When one of the branch’s sections is empty, this indicates that the lines were deleted or moved in that branch. In this case, either a message is no longer being sent, or the call to` BlueMessage.send` has moved elsewhere. You must determine which. If the call was deleted, then` merged-branch` ’s change may have become irrelevant. If the change in HEAD moved the call elsewhere or changed how it was done, then we must find it and apply the` include_signature` change if it’s still relevant.


The resolution here is likely to delete this conflict section and search for where we may need to apply the` include_signature` change elsewhere.


## Conflicts requiring context beyond the diff3 ancestor


Every example so far has provided a clean answer right there within the conflict markers. This is great and happens a lot, but sometimes you need more. Git on the command line makes it easy to view the changes on either side of a conflict.


let’s take the common scenario where you’re pulling or merging updates from` origin/main` into your branch, and there’s a conflict. Using git diff with the` ...` operator lets you view the changes introduced on each side. The magic with` ...` vs the` ..` (or no) operator is that` ...` uses the common ancestor as the starting point for comparison.


```text
git diff origin/main...HEAD # show changes that the HEAD side made
git diff HEAD...origin/main # show changes origin/main made


```


It can be useful sometimes to include a filename or glob to limit the scope of the diff to what’s relevant.


```text
git diff HEAD...origin/main my-app/src/App.js


```


### Working with moved and deleted files


When one branch removes a file that the other branch changes, git status will report “deleted by us” or “deleted by them”. *Us* and *them* refer not to authorship but rather to which merge parent deleted the file, where *us* refers to HEAD—whatever is checked out when applying the merge/cherry-pick/rebased commit.


Like with removed lines of code, a removed file may actually have moved to a new location. Git detects and handles renames except when the file changed substantially in addition to being renamed. In these situations, you need to determine what the intent was. Use` git log -1 -- <removed filename>` to find the commit that removed it. (Note: The` --` before the filename enables git to work with files that don’t exist due to being deleted in HEAD. Without it, git can’t know whether you mean a file path or a branch name.) You may find clues in the commit message or may want to` git show <commit sha>` to examine the rest of the commit diff for clues to discover the intent behind the removal. If the file was just deleted, the change to that file might have become irrelevant. If moved, you’ll want to look to apply the change in the new location.


Once you’ve applied any relevant changes from the deleted file to the correct location, mark that conflict as resolved using` git rm <file>` .


## Dependency lock files


A special case that may not require manual resolution is a conflict in a dependency lockfile such as Ruby’s` Gemfile.lock` or NodeJS’s` yarn.lock` . Conflicts here can be resolved by hand as demonstrated above, but when a conflict is large and tedious, regenerating can be the fastest option. Lockfiles get updated for 2 main reasons:


1. A plain old install, e.g.` bundle install` /` yarn install` . The lockfile will change in response to any dependency changes (e.g. in` Gemfile` /` package.json` ) or updates to the package manager itself.
2. An explicit dependency update or with special options, e.g.` bundle update <gem>` /` yarn upgrade <package> --latest` .


When regenerating, it’s important to know what triggered the lockfile updates in at least one side of the merge and regenerate it in the same way. As an author, you might already be familiar with the reason for the update, and if not, commit messages can be helpful. You can list commits from either side of the merge using` git log` ’s` ..` operator:


```text
git log origin/main..HEAD # list commits unique to the HEAD side
git log HEAD..origin/main # list commits unique to the merged side


```


First, be sure to resolve any conflicts in the dependency specification file (e.g.` Gemfile` /` package.json` ), because recreating the lockfile depends on this being valid.


Let’s imagine that your checked-out feature branch made updates in the` Gemfile` , and you simply ran` bundle install` to have those changes applied to` Gemfile.lock` . These changes were committed to your branch, and now you’re merging` origin/main` . The main branch you’re merging might contain lockfile changes from multiple other branches, so without even looking, we can assume that your branch has the simplest change. From here, we start with the other side of the merge and re-apply our action.


```text
git checkout --theirs Gemfile.lock
bundle install


```


The` --theirs` option resets the file to how it is in the merged branch. (Again, in a rebase, “theirs” represents the commit in the branch you’re rebasing onto the new base.) To do the opposite, to start with your side and apply actions from the merged branch, use` --ours` .


After regenerating the lockfile using the same action(s),` git add` the updated file, and continue as usual.


## Semantic Conflicts


A semantic conflict is one that Git cannot detect, where the intent of one branch is not fully applied to new lines introduced in the other branch. For example, your branch renames a class that can be referenced throughout the codebase. Meanwhile, another branch adds a new reference to the old name and merges to main. When you merge main, that new reference won’t conflict (unless by coincidence), and you’ll need to apply the rename to the new reference. This fix should ideally be amended into the merge commit, but this isn’t strictly necessary—the merge commit will just always remain broken.


When performing merges, especially ones with conflicts, it’s good to be aware of any broad-sweeping changes made by either side and ensure they’re applied during merges.


## Joy in Conflict Resolution


Nothing builds a skill and makes it stick quite like practice! I’ve curated a set of real-life conflicts from various open-source projects at[https://github.com/nilbus/conflicts](https://github.com/nilbus/conflicts) . You can reproduce the conflict, resolve it, and then compare your solution to the author’s original solution. This practice will also prepare you to review your teammates’ or contributors’ conflict resolutions as a part of code review. If you’re ready to take your conflict resolution skill to the next level, try these out!


I hope that using this technique will help provide a more predictable pattern for you to follow and a more joyful experience when you encounter git conflicts, as it has for me.


---


*Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you’d like to be notified about new blog posts.*
