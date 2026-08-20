---
schema_version: "1.0.0"
document_id: "1d13137d54dfcf307fa459c84d78fc2b4795afa7245c8f1db192032b95f2779f"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/nerd-stuff-research-surveys-should"
published_at: "2026-07-31T12:19:37+00:00"
first_seen_at: "2026-07-31T17:27:28.150832+00:00"
fetched_at: "2026-07-31T17:27:29.252328+00:00"
content_hash: "sha256:f88f9a4d0cf50b4644c062566d080e7a1d48c79dd9954fe962aa8dfed4fee6a3"
---

# [Nerd Stuff]: Research Surveys should be Version-controlled files

# \[Nerd Stuff\]: Research Surveys should be Version-controlled files


### And with Expected Parrot, they are!


[John Horton](https://substack.com/@johnjhorton)


Jul 31, 2026


**tl; dr: We, Expected Parrot, decompose research surveys into a number of small files to make a small git repository. This has many benefits: real diffs, history, and clean merges on a survey as it evolves. This might seem complex and have a steep learning curve, but with coding agents that “know” our system, you get these benefits without any effort.**


I realized fairly early in graduate school that: (1) much of empirical social science was a kind of applied software engineering and (2) that software engineering had solved many of the problems that researchers had (but generally didn’t know this). I could write a


**lot** about this topic (


[Gentzkow & Shapiro's](https://web.stanford.edu/~gentzkow/research/CodeAndData.pdf) *[Code and Data for the Social Sciences](https://web.stanford.edu/~gentzkow/research/CodeAndData.pdf)* is ‘old’ but still very good) but let me pick the most important one:


**version-control** .


Thanks for reading! Subscribe for free to receive new posts and support my work.


Distributed version control like git lets people safely collaborate on the same project, without worrying about data loss or (unsolvable) conflicts. Users also get the full history of changes, by whom as well as the ability to create arbitrary paths for a project that can later be used, abandoned, etc.


But to get these benefits, the object being worked on has to have a file format. And that file format should be nicely ‘diffable’ meaning that when you see the changes made over time, you should understand what was done.


However, there are two problems. The first problem: surveys have no canonical, git-friendly file format. The second: even with a good format, version control has historically demanded skills most researchers don't want to acquire. We solve the first with a new format; coding agents solve the second.


## What’s done in practice now?


In research papers, surveys are often low-res screenshots of an interface or a public URL to the survey that will invariably link rot. Or they’re just described. The main reason is that there isn’t a file to be included or referenced. This is bad for several reasons:


-


It’s hard for follow-on researchers to know what, exactly, you did


-


This is particularly true as surveys get more complex with skip-logic, piping, arms etc.


-


It’s hard for follow-on researchers to replicate your work


-


They have to reverse engineer what you did and then reproduce it


-


It causes vendor lock-in that prevents progress


-


How can I take my Google Survey to Qualtrics? Qualtrics to Google?


## File-based workflows and errors


Many surveys end up just having a database representation (Google Forms, SurveyMonkey) or if they do have a file format (Qualtrics QSF), it’s not “meant” to be edited outside their web interface. It’s easier for mistakes to slip in when edits are just being made in a web interface because we don’t have access to all our normal tools for catching errors—nevermind agents.


In contrast, once you commit to a canonical file format that fully captures a survey, you can create linters and validators: a validator catching a skip rule that references a deleted question, or piping that points at a not-yet-asked question.


The problem is even more acute when data from a deployed survey is exported. Because there is no canonical file format, there is no way to provide the full survey context reliably to the analyst (now, often the agent). The core problem is that you need hacks to get non-flat data (as is common in surveys) into a flat tabular format. For example, you often see checkbox question data just exploded into N columns in a CSV with maybe one column for the “Other” free text. If the researcher—or more likely the coding agent—isn’t careful, importing this format as regular tabular data can be badly misleading.


With a canonical representation of the survey afforded by a standard file format, it could reliably decode what the answers mean. And if the research agent can see two sets of responses under different versions of a survey, it can use the git functionality to analyze if changes are seemingly making a difference.


## Working with files is great, but not every researcher wants to be a software engineer!


True! And this was the reason for the proliferation of point-and-click / GUI-based tools for researchers. It’s a real and understandable trade-off: do I spend more time learning about the mathematics of linear regression or learning that regression in R is


` m ← lm(y ~ x, data = df)` which Stata-heads find ridiculous?


HOWEVER, the rise of coding agents radically changes the calculus here and eliminates the trade-off—we can take the points made above and edit them so:


-


It’s hard for follow-on


researchers


coding agents to know what, exactly, you did


Now we can have all the benefits of code-based research work-flows without the “minor in software engineering“ downsides.


## Expected Parrot Solution: Decompose a survey into a collection of smaller files and put git right in the format


Most written programs are text files and line-level “diffs” are fine to illustrate meaning. Data structures like surveys are more complex. You want the “diffs” to be semantic and narrowly scoped is possible. But this is hard if the “Survey” is just some big blob of nested JSON. With this format, it would be harder to understand what, exactly, changed from commit to commit.


Our approach at Expected Parrot / EDSL is to store a survey as a collection of small files. We have one per question in a survey (under


` questions/` ):


```text
example-survey.ep                   one portable ZIP-compatible file
└── unpacked contents               a complete Git working tree
├── .git/                       commits, trees, blobs, refs, and configuration
├── manifest.json               package type, format version, and question order
├── questions/
│   ├── 000001.json             one question per file
│   ├── 000002.json
│   └── 000003.json
└── metadata/
├── memory_plan.json
├── question_groups.json
└── rule_collection.json
```


This way, if a survey has 500 questions and one changes, Git does not need to treat one giant serialized survey document as the unit of review. The diff can focus on the affected question and any metadata that actually changed.


## Why an ‘ep’ file format that’s a zipped directory?


Note also that by putting .git right in the specification, the history of the survey can travel with it. There is a current survey, but it also contains the whole history of that survey.


But note that we zip it—doesn’t that defeat the purpose? No. A survey likely lives inside a larger research project. Nesting git repos is ill-advised, so instead we zip the repo and use the


` .ep` suffix. Yes, the “outer” project repo sees an opaque file, but the survey's own history is complete and portable inside it, and you can unzip to inspect. But more importantly, your agent knows EDSL and thus “knows” how to use it (e.g., when it makes changes to a survey you direct, it creates internal versions), giving you all the benefits of git without having to “shell out” to use git. And to be clear, you can unpack the file if you do want to poke around. You can even push that repo to GitHub / GitLab etc.


## Decomposition makes for efficient, legible changes


Some files under preserve behavior that is not contained in an individual question: The


` manifest.json` identifies the format and reconstructs question order:


```text
{
“edsl_class_name”: “Survey”,
“edsl_version”: “1.0.8.dev1”,
“format”: “edsl.survey.git_package”,
“format_version”: 1,
“n_questions”: 3,
“object_type”: “Survey”,
“question_order”: [
“000001”,
“000002”,
“000003”
]
}
```


What’s good about this is changing, say, the order of questions in a survey, is also not a massive diff that changes every line: it changes one line in


` manifest.json` . This is more efficient (and will matter more when we allow multi-party editing of a survey in real time). Compare this to a monolithic JSON file, where reordering questions rewrites the entire array and the diff is useless.


The repo also has other files in the package that capture other aspects of a survey, such as skip-logic, flows, question groups and so on.


## Showing a diff: A changed question text


Each question is stored separately. For example,


` questions/000001.json` contains:


```text
{
"question_name": "q0",
"question_options": [
"yes",
"no"
],
"question_text": "Do you like school?",
"question_type": "multiple_choice"
}
```


Now suppose we make an edit (in Python, but your coding agent could be doing this work):


```text
survey = Survey.git.load("example-survey.ep")
survey.questions[0].question_text = "Do you enjoy school?"


second_save = survey.git.save(
message="Clarify the first question",
)
second_save
```


If we look at the log for the object, we can see the new commit, just like a regular git repository:


```text
survey.git.log(...)
```


```text
[
{
"commit": "d1f44ff624288e48e8835597d247c039ceffeb48",
"message": "Clarify the first question",
"timestamp": "2026-07-29 07:47:56 -0400",
},
{
"commit": "65801c7e03eb93c0f31c8bee6b2a29cc9152be14",
"message": "Create example survey",
"timestamp": "2026-07-29 07:47:34 -0400",
},
]
```


EDSL exposes the git semantics for looking at a diff:


```text
print(survey.git.diff("HEAD~1", "HEAD"))
```


And we can see the familiar git differences like so:


```text
diff --git a/questions/000001.json b/questions/000001.json
index 353b419..33b47da 100644
--- a/questions/000001.json
+++ b/questions/000001.json
@@
-  "question_text": "Do you like school?",
+  "question_text": "Do you enjoy school?",
```


## Explore alternatives with branches


Branches let collaborators develop alternative instruments without overwriting the main line:


```text
survey.git.branch(”short-form”)
survey.git.switch(”short-form”)


# Make edits to the in-memory Survey here.
survey.git.save(message=”Create short-form variant”)


```


Return to the main version:


```text
survey.git.switch(”main”)


```


` switch()` refreshes the in-memory


` Survey` from the selected branch. EDSL requires a clean package before switching, pulling, or checking out another version, which protects uncommitted changes.


[​](http://localhost:3000/en/latest/survey_ep_format#merge-a-survey-branch)


## Merge a survey branch


Suppose the


` short-form` branch removes the follow-up question


` q1` :


```text
survey.git.switch(”short-form”)
survey.delete_question(”q1”)
survey.git.save(message=”Remove follow-up from short form”)


```


At this point,


` main` still has all three questions and


` short-form` has two. Switch back before merging:


```text
survey.git.switch(”main”)
survey.question_names


```


```text
[”q0”, “q1”, “q2”]


```


Merge the branch through the same Git-backed accessor:


```text
merge_info = survey.git.merge(
“short-form”,
message=”Merge short-form survey”,
no_ff=True,
)
merge_info


```


```text
{
“status”: “ok”,
“path”: “example-survey.ep”,
“commit”: “<merge-commit>”,
“branch”: “main”,
“merged_ref”: “short-form”,
“fast_forward”: False,
“changed”: [
“manifest.json”,
“metadata/memory_plan.json”,
“metadata/rule_collection.json”,
“questions/000002.json”,
],
“conflicts”: [],
“aborted”: False,
“message”: “Merge short-form survey”,
}


```


` no_ff=True` asks Git to create a merge commit even though this simple example could be fast-forwarded. That makes the point where the alternative survey entered


` main` visible in history.


## Ok, how do I use this in my work?


You don’t really have to “do” anything - it’s just what is happening under the hood when you use EDSL and our research agent for your survey work. However, a coding agent working with EDSL understands these capabilities and so you get the benefit of a git-based format without learning a great deal about our plumbing.


Want to try it out?


[Use Expected Parrot](https://www.expectedparrot.com/)


##


Thanks for reading! Subscribe for free to receive new posts and support my work.
