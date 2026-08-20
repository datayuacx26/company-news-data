---
schema_version: "1.0.0"
document_id: "f1fba464dc4539ab2835fc4828492f2ee6e8b7af401823fb9897d74db9fedc56"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude"
published_at: "2025-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:c0d64297d9f56ffd80bcb7db600b1661d1a13b47513d66503699a86e4445a178"
---

# How we automated GitHub Actions Runner updates with Claude

We recently launched[Claude Code Sessions in Depot](https://depot.dev/blog/now-available-claude-code-sessions-in-depot) , a feature that allows you to share Claude code sessions with both developers and your CI workflows. In our previous blog post, we noted that "we've been using Claude Code at Depot since pretty much the moment it dropped," but we didn't elaborate on *how* . This article will demonstrate one of our most valuable CI uses: consistently keeping our forks updated.


## Background


### Problem: We waste hours updating the GHA upstream image


All of our GHA runners run on their own isolated EC2 instances. As such, we need to build an image that these runners can load, called an AMI. This sounds like it'd be pretty easy! Github keeps the definitions for their runner images[open source](https://github.com/actions/runner-images) , so it'd just be a matter of modifying their source to work with our runners, and then building the AMI. Unfortunately, it's not quite that simple. We make a large number of modifications to the image to help improve performance, and alongside that, Github's runners run on Azure and ours run on AWS. This means that, instead of just using their source as is, we need to modify it to work with our AMI build software. The way we go about this is by keeping upstream in a git submodule, and then running many of their scripts with a patch file that modifies them to work with our runners. This is a lot of work, and it takes a lot of time to keep up with their changes.


Keeping our fork in sync with upstream requires a pretty significant amount of developer effort that compounds over time. Each pull from upstream involves pulling in dozens of commits, reviewing every change for compatibility issues, and ensuring that everything introduced by upstream will be compatible with our existing runner software.


It isn't just the raw time investment that's a problem either, it's the cognitive load of context switching from other deep and intensive work to this task. Kyle, our co-founder and CEO here at Depot, talks about it frequently, but context switching is one of the biggest hidden killers of developer productivity. It shatters whatever flow you had going, and quite frankly, it just doesn't feel great.


For many months now, we've been wanting to automate this process. Generative AI is the hot new thing, and consuming large amounts of data to output smaller amounts is one area where LLMs can really shine.


## What we needed Claude to do


There's two main issues we want to tackle here:


- Keeping a patch file for arm64 up to date
- Knowing what changed between each update


The first is keeping a file called` arm64.patch` up to date. Essentially, Github's runner images are built for x86_64, and we need to modify them to work on arm64. This patch file contains all the modifications we need to make to the upstream source to get it building for our arm machine images. We also need to generally know 'what actually changed since our last update?'. While there are many commits made to the upstream repository, not all of them are relevant to us. We need to be able to quickly identify which changes are important or potentially breaking, and which ones we can ignore. Using` depot claude` , we can make this process pretty smooth, by having a CI workflow that consistently keeps us up to date!


**Solution: Using Depot Claude to summarize and analyze changes**


## Generating the arm64 patch


Let's get the more tedious work out of the way first: regenerating our arm64 patches. Thinking about what we need Claude to do, we need it to:


1. Attempt to apply the existing arm64 patch file
2. If it fails, modify the patch file to apply cleanly, keeping in mind the goal of ensuring this new machine image will build and run correctly on arm64
3. Repeat the first two tasks until the patch applies cleanly


Here's the actual prompt we're using as of writing, with a little bash to set up the context for Claude:


```text
cat   <<  'EOF'
# ARM64 Patch Conflict Resolution


## Context
We maintain ARM64 patches that modify the upstream GitHub Actions runner image to work on ARM64 architecture. These patches are ESSENTIAL and must be preserved. The patches failed to apply cleanly after an upstream update, and I need you to fix the conflicts while keeping all the ARM64 modifications.


## CRITICAL: YOU MUST MODIFY THE PATCH TO WORK FOR ARM64
The ARM64 patches are necessary for the runner to work on ARM64. You must:
1. Keep ALL existing ARM64 modifications
2. Only adjust line numbers or context to match the new upstream code
3. NEVER suggest removing a patch file or patch content
4. ALL files that are patched MUST remain patched
5. **CRITICAL**: The new patch MUST make it so that, post-patch, things will run for ARM64


## Your Task
You need to modify the arm64/arm64.patch file to resolve conflicts AND ensure ARM64 functionality. The patch application is failing, which means the line numbers or context in the patch no longer match the upstream files.


**IMPORTANT**: You are NOT just copying the patch file. You must MODIFY it to ensure ARM64 compatibility.


To fix this:
1. Look at the current arm64/arm64.patch file
2. Check the upstream files that are being patched to understand how they've changed
3. Update the patch file with the correct line numbers and context
4. Ensure all ARM64-specific changes are preserved and functional:
- Change amd64 → arm64, x86_64 → aarch64
- Update download URLs to ARM64 versions
- Modify tool configurations for ARM64 (e.g., JAVA_HOME paths)
- Remove/disable features that don't work on ARM64
- Ensure binary installations use ARM64 binaries
5. **VERIFY**: The final patch must result in a working ARM64 runner image


## Failed Files
The following issues were detected:
EOF
echo   "  $failed_files  "


cat   <<  'EOF'


Please update the arm64/arm64.patch file to fix these conflicts. Remember:
- Keep ALL ARM64 modifications
- Only update line numbers and context
- Test that the patch applies cleanly
- DO NOT remove any patches or suggest workarounds that would lose ARM64 functionality
- **CRITICAL**: The new patch MUST make it so that, post-patch, things will run for ARM64
- You are MODIFYING the patch to ensure ARM64 compatibility, not just copying it
- You may not consider the task complete until `make` runs without issue
EOF
```


We have a shell script around running` depot claude` that essentially sets up the context needed, checks if the patch file needs to be modified at all, and then runs the prompt. Once Claude modifies the patch file, we can run it through our build process to verify that it applies correctly:


```text
resolve_conflicts  () {
print_message   "  $YELLOW  "   "Attempting automatic conflict resolution with depot claude..."
# grabs the error output from attempting to apply patches
make_output  =  $(  make   2>&1   ||   true  )


failed_files  =  "${  make_output  }"


prompt  =  $(  create_prompt   "  $failed_files  "  )


print_message   "  $YELLOW  "   "Running depot claude to resolve conflicts..."


session_id  =  "arm64-patch-$(  date   +%Y%m%d-%H%M%S)"


if   !   depot   claude   --model   opus   --org   <  org-i  d  >   --session-id   "  $session_id  "   --allowedTools   "Read Edit Write Grep Glob Bash"   -p   "  $prompt  "  ;   then
print_message   "  $RED  "   "depot claude failed to run"
return   1
fi


print_message   "  $YELLOW  "   "Testing if conflicts were resolved..."
if   apply_arm64_patches  ;   then
print_message   "  $GREEN  "   "Conflicts resolved successfully!"
if   generate_patch_file  ;   then
print_message   "  $GREEN  "   "Patch file updated successfully!"
return   0
else
print_message   "  $RED  "   "Failed to generate patch file after conflict resolution"
return   1
fi
else
print_message   "  $RED  "   "Conflicts still exist after automatic resolution attempt."
return   1
fi
}
```


One of the really nice things about using` depot claude` for this kind of task is that, after a workflow run, we can take a look at *why* it made certain decisions. That lets us tweak the prompt as needed to make further improvements.


```text
⏺   Now   let   me   check   the   upstream   install-kubernetes-tools.sh   file   to   see   what   has   changed:


⏺   Read  (  upstream/images/ubuntu/scripts/build/install-kubernetes-tools.sh  )
⎿    Read   50   lines   (ctrl+r   to   expand  )


⏺   Update   Todos
⎿    ☒   Examine   the   current   arm64/arm64.patch   file
☒   Check   the   failing   file   install-kubernetes-tools.sh   in   upstream
☐   Analyze   the   reject   file   to   understand   the   conflict
☐   Update   the   arm64.patch   file   with   correct   context   and   line   numbers
☐   Test   that   the   patch   applies   cleanly
☐   Run   make   to   verify   everything   works


⏺   Let   me   check   the   reject   file   to   understand   exactly   what   failed:


⏺   Read  (  arm64/generated/scripts/build/install-kubernetes-tools.sh.rej  )
⎿    Read   20   lines   (ctrl+r   to   expand  )


⏺   I   can   see   the   issue.   The   patch   is   trying   to   apply   changes   for   minikube   v1.34.0   with   specific
pinned   version,   but   the   upstream   file   now   uses   "latest"   version   with   a   different   download   pattern.
Let   me   update   the   patch   file   to   match   the   new   upstream   structure:


⏺   Update   Todos
⎿    ☒   Examine   the   current   arm64/arm64.patch   file
☒   Check   the   failing   file   install-kubernetes-tools.sh   in   upstream
☒   Analyze   the   reject   file   to   understand   the   conflict
☐   Test   that   the   patch   applies   cleanly
☐   Run   make   to   verify   everything   works


⏺   Now   I'll update the arm64.patch file to fix the kubernetes-tools section:
```


Finally, we run this bash script in a larger GHA workflow. It's set to run daily to ensure that we always have the latest patch file ready to go:


```text
# this is a simplified version of the actual workflow file
name  :   Update Upstream


on  :
schedule  :
-   cron  :   '0 17 * * *'


jobs  :
update-upstream  :
runs-on  :   depot-ubuntu-latest-small
permissions  :
contents  :   write
pull-requests  :   write


steps  :
# ... a few setup steps
-   name  :   Update upstream submodule
run  :   |
cd "$WORKTREE_DIR"
git submodule update --init --force


CURRENT_COMMIT=$(git submodule status upstream | awk '{print $1}' | sed 's/^[+-]//')
echo "Current submodule commit: $CURRENT_COMMIT"


cd upstream
git fetch origin main
LATEST_COMMIT=$(git rev-parse origin/main)
echo "Latest upstream commit: $LATEST_COMMIT"


if [ "$CURRENT_COMMIT" != "$LATEST_COMMIT" ]; then
echo "Update needed: $CURRENT_COMMIT -> $LATEST_COMMIT"
git checkout main
git pull origin main
cd ..
git add upstream
echo "CHANGES_EXIST=true" >> $GITHUB_ENV
else
echo "Submodule already up to date"
cd ..
echo "CHANGES_EXIST=false" >> $GITHUB_ENV
fi


-   name  :   Generate ARM64 patches
env  :
GH_TOKEN  :   ${{ secrets.GITHUB_TOKEN }}
DEPOT_TOKEN  :   ${{ secrets.DEPOT_TOKEN }}
ANTHROPIC_API_KEY  :   ${{ secrets.ANTHROPIC_API_KEY }}
run  :   |
cd "$WORKTREE_DIR"
bash .github/scripts/generate-arm64-patches.sh


if [ "$(git status --porcelain arm64/ x86/ | wc -l)" -gt "0" ]; then
echo "ARM64_PATCHES_UPDATED=true" >> $GITHUB_ENV
else
echo "ARM64_PATCHES_UPDATED=false" >> $GITHUB_ENV
fi


-   name  :   Commit ARM64 patches
if  :   env.ARM64_PATCHES_UPDATED == 'true'
run  :   |
cd "$WORKTREE_DIR"
git add arm64/ x86/


COMMIT_MSG="Update ARM64 patches for upstream changes"$'\n\n'"- Applied patches using make"$'\n'"- Regenerated patches using make generate-patch"$'\n'"- Resolved any patch conflicts automatically"


git commit -m "$COMMIT_MSG"


-   name  :   Push changes
if  :   env.CHANGES_EXIST == 'true'
run  :   |
cd "$WORKTREE_DIR"
git push -u origin $BRANCH_NAME
```


This is already a **major** improvement to the productivity of Depot developers. Testing, iterating on, and applying the patch file was not only time consuming, but also required a lot of context switching. Now, we can just run this script and let Depot Claude handle the heavy lifting, with human developers just verifying Claude's output and making any necessary adjustments.


## Analyzing breaking changes


Now let's go even further. The real heavy work with updating` upstream` involves reviewing all the changes made. Most of these are just version upgrades, or other minor details. The more serious changes can include:


1. Upgrades to the Linux kernel or other system libraries
2. BuildKit or Buildx upgrades
3. Major version upgrades of commonly used packages


Given this context, being able to ask Claude to summarize the changes and analyze them for potential breaking changes could be a huge time saver. This would still, of course, require a human touch to verify that the changes are acceptable, but it would allow us to focus on the most important changes rather than having to sift through every single line of the diff.


One interesting problem is that there's been dozens of changes in the past that have caused issues. Having to update the prompt in git to constantly account for these changes would be a pain. Instead, we can create an original session file that all other breaking changes analyses work off of.


We already have a nice list of every issue that's been introduced in our Git history, so let's go ahead and feed that to a new` depot claude` session:


```text
depot   claude   --session-id=update-upstream


╭───────────────────────────────────────────────────╮
│   ✻   Welcome   to   Claude   Code!                           │
│                                                     │
│     /help   for   help,   /status   for   your   current   setup    │
│                                                     │
│     cwd:   /Users/billy/Work/github-actions-image       │
╰───────────────────────────────────────────────────╯


※   Tip:   Run   claude   --continue   or   claude   --resume   to   resume   a   conversation


>   Here is a list of all previous upstream changes that have caused issues:
...
**  Upstream Change  **  : Updated Docker Compose from 2.35.1 to 2.36.0/2.36.2
**  Issue  **  : Version mismatch between different architectures and platforms
**  Fixes  **  :
-   `  a341ec95  `   -   fix:   update   ubuntu   22.04   docker   compose   to   2.36.0
-   `  711e1cf5  `   -   fix:   upgrade   x86   docker   compose   to   2.36.0
-   `  9a0ad0d7  `   -   fix:   apply   docker   compose   2.36.0   to   x86
-   `  4317f9c9  `   -   fix:   update   arm64   and   x86   patches   (updated   to   2.36.2  )


**  Issue  **  : PowerShell execution permissions broken since 7.4.2
**  Fix  **  :   `  1c87a95f  `   -   fix:   pwsh   issue   since   7.4.2   by   changing   execution   permissions
...
Do   you   understand   so   far?
```


Next, we'll ask Claude to summarize the changes in the upstream repository, and analyze them for potential breaking changes based on previous issues. We'll also ask it to provide its response in JSON, so that we can easily parse it later:


```text
cat   >   "  $PROMPT_FILE  "   <<   'EOF'
Analyze the upstream changes and identify any breaking changes that might affect users of this GitHub Actions runner image.


Please respond with ONLY a JSON object (no markdown, no explanations before or after) in this format:
{
"breaking_changes": [
{
"file": "filename",
"description": "Brief description of the breaking change",
"impact": "How this affects users",
"mitigation": "How users can adapt"
}
],
"summary": "One-line summary of the analysis"
}


Changed files:
EOF
echo   "  $CHANGED_FILES  "   >>   "  $PROMPT_FILE  "


echo   -e   "\n\nCommit messages:"   >>   "  $PROMPT_FILE  "
echo   "  $COMMIT_MESSAGES  "   >>   "  $PROMPT_FILE  "


echo   -e   "\n\nDiff content:"   >>   "  $PROMPT_FILE  "
echo   "  $DIFF_CONTENT  "   >>   "  $PROMPT_FILE  "
EOF
```


We want` depot claude` to start with the previous session we created, but save its output (and thus its analysis we can review later) to a new session. We can do this by taking advantage of the` --session-id` and` --resume` flags:


```text
depot   claude   --resume=update-upstream   --session-id="breaking-changes-$(  date   +%Y%m%d)"   --model   opus   -p   "$(  cat   $PROMPT_FILE  )"
```


Notice, we're resuming from the` update-upstream` session we made earlier, but saving to a new` breaking-changes` session.


Next, we'll want to take that JSON output, and turn it into a nice to read text summary:


```text
JSON_CONTENT  =  $(  cat   breaking-changes-analysis.json  )
JQ_OUTPUT  =  $(  mktemp  )
JQ_ERROR  =  $(  mktemp  )


if   echo   "  $JSON_CONTENT  "   |   jq   -r   '
if .breaking_changes | length > 0 then
"## Breaking Changes Detected\n" +
(.breaking_changes[] |
"\n### " + .title +
"\n**Risk Level:** " + .risk_level +
"\n\n" + .description +
"\n\n**Mitigation:** " + .mitigation + "\n"
) +
"\n---\n**Summary:** " + .summary
else
.summary
end
'   >   "  $JQ_OUTPUT  "   2>   "  $JQ_ERROR  "  ;   then
cat   "  $JQ_OUTPUT  "   >   breaking-changes-summary.txt
echo   "Breaking changes summary written to breaking-changes-summary.txt"
cat   breaking-changes-summary.txt
else
echo   "jq processing failed:"
cat   "  $JQ_ERROR  "
echo   "Failed to process JSON with jq"   >   breaking-changes-summary.txt
fi
```


With this new` breaking-changes-summary.txt` , we can go ahead and commit those changes to a branch.


```text
-   name  :   Check for breaking changes
id  :   breaking-changes
env  :
GH_TOKEN  :   ${{ secrets.GITHUB_TOKEN }}
DEPOT_TOKEN  :   ${{ secrets.DEPOT_TOKEN }}
ANTHROPIC_API_KEY  :   ${{ secrets.ANTHROPIC_API_KEY }}
run  :   |
cd "$WORKTREE_DIR"
bash .github/scripts/detect-breaking-changes.sh


echo "BREAKING_CHANGES<<EOF" >> $GITHUB_OUTPUT
cat breaking-changes-summary.txt >> $GITHUB_OUTPUT
echo "EOF" >> $GITHUB_OUTPUT


echo "BREAKING_CHANGES_SUMMARY<<EOF" >> $GITHUB_ENV
cat breaking-changes-summary.txt >> $GITHUB_ENV
echo "EOF" >> $GITHUB_ENV


-   name  :   Commit upstream changes
run  :   |
cd "$WORKTREE_DIR"
git add upstream


CURRENT_SHA=$(git submodule status upstream | awk '{print $1}' | sed 's/^[+-]//')
NEW_SHA=$(cd upstream && git rev-parse HEAD)


COMMIT_MSG="Update upstream repository"$'\n\n'"Updates upstream from $CURRENT_SHA to $NEW_SHA"$'\n\n'"Commits:"


# List all commits between old and new
cd upstream
COMMITS=$(git log --oneline --no-decorate $CURRENT_SHA..$NEW_SHA)
cd ..


COMMIT_MSG="${COMMIT_MSG}"$'\n'"${COMMITS}"


# Add information about breaking changes if any were found
if [ -n "$BREAKING_CHANGES_SUMMARY" ] && [ "$BREAKING_CHANGES_SUMMARY" != "No significant breaking changes detected." ]; then
COMMIT_MSG="${COMMIT_MSG}"$'\n\n'"Breaking changes detected - see PR description for details"
fi


COMMIT_MSG="${COMMIT_MSG}"


git commit -m "$COMMIT_MSG"
```


We have some code to either create a new PR, or to update an existing one, depending on whether it already exists or not. The PR is kept in a draft state until a human engineer has time to review and merge the changes.


## Fin


What used to consume hours of developer time every week now runs automatically in the background. Our arm64 patches stay fresh, breaking changes get flagged before they break anything, and we can focus on building features instead of maintaining forks.


We're already looking at expanding this approach to other forks we're maintaining, and all of us at Depot are pretty excited for future improvements we can make. If you have a similar need to maintain forks, run and monitor many agents, or just to analyze your codebase, get started with` depot claude` today!


## FAQ


How does Claude Code handle patch conflicts automatically?


We feed Claude the error output from failed patch applications along with a detailed prompt that instructs it to preserve all ARM64 modifications while updating line numbers and context to match the new upstream code. Claude reads the patch file, examines the upstream changes, and modifies the patch to apply cleanly. The script then tests the updated patch to verify it works.


Can I run depot claude in my GitHub Actions workflows?


Yes. Set your` DEPOT_TOKEN` and` ANTHROPIC_API_KEY` as GitHub secrets, then call` depot claude` from your workflow steps. You can use the` --session-id` flag to save sessions for later review, and` --allowedTools` to restrict which tools Claude can use. We run it daily via a cron schedule to keep our patches updated automatically.


Do I need Depot to use Claude Code in CI, or can I use the regular Claude Code CLI?


You can use the regular Claude Code CLI in any CI environment by setting the` ANTHROPIC_API_KEY` environment variable. Depot's` depot claude` command adds the ability to save and share sessions with your team, which is why we use it for reviewing Claude's decisions later and resuming from base sessions.


What happens if Claude Code fails to automatically resolve patch conflicts?


Our automated workflow creates draft pull requests that engineers review before merging. We can view the full Claude Code session to understand exactly what decisions it made and why it chose a particular approach. If Claude's automated fix doesn't work correctly, the PR stays in draft state and we handle the conflicts manually. This safety net means we've reduced manual intervention from weekly to occasional while maintaining quality control over critical infrastructure changes.


## Related posts


- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Claude Code sessions in Depot](https://depot.dev/blog/now-available-claude-code-sessions-in-depot)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Billy Batista


Software Engineer at Depot
