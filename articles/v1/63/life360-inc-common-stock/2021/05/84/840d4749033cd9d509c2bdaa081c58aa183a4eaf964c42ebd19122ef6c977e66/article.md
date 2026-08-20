---
schema_version: "1.0.0"
document_id: "840d4749033cd9d509c2bdaa081c58aa183a4eaf964c42ebd19122ef6c977e66"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/lint-and-why-we-remove-it-600a521f1ecd"
published_at: "2021-05-28T22:18:38+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:36.656438+00:00"
content_hash: "sha256:953d1d08e5bac0035e7f0067a40b274b1de092019cd0bed431e04101ad90f8c3"
---

# Lint and why We Remove It

Linter


Android


Warning


# Lint and why We Remove It


[Meghneel Gore](https://medium.com/@meghneelgore?source=post_page---byline--600a521f1ecd---------------------------------------)


5 min read


·


May 28, 2021


--


Press enter or click to view image in full size


Lint Warnings: No one likes them, no one likes to see them, no one wants to clean them up. They don’t cause your release to stop. They’re like Error-lite!


L


int warnings have a tendency to accumulate. Sometimes, their sheer number makes one balk at fixing them. Most warnings, however, are important flags that signal the health of your codebase.


January 2020, unbeknownst to us, we had about 5,000 warnings in the Android codebase. Fixing these warnings would give us great returns. It is an established fact that fewer warnings means fewer potential future or current bugs and a cleaner codebase. That’s why we came up with Project Zero. We did it in 3 phases.


## Tabulation, Categorization, and Baselining Phase


In the beginning, we had no idea about the number of warnings we had in the codebase. Phase 1 consisted of **tabulation, categorization , and baselining** of warnings.


### Tabulation & Categorization


Since 5,000 warnings is a lot to try to resolve in one go, we started with tabulating and categorizing them. Below, you can see an excerpt of this tabulation. While tabulating, we were also able to categorize the warnings. This proved quite useful in the subsequent phases because we were easily able to find warnings that were Engineering-bound* — like` UnusedResources` versus ones that were Product-bound* — like` ContentDescription.`


Press enter or click to view image in full size


Top 10 warning types in our codebase


### Baselining


Lint allows us to stop the warnings list from growing even before reducing the list of warnings by setting the baseline warnings state. Baselining proved to be a very big stepping stone towards getting rid of warnings. It provided a way to disregard existing warnings. Combined with some other lint flags, we were able to make sure that we could keep paring down the issues while not allowing new issues to be merged into the codebase.


The way to do lint baselining is to add


```text
android {    ...    lintOptions {      baseline file("lint-baseline.xml")    }  }
```


In your` build.gradle` file’s` lintOptions`


Now, the next time lint is run, a` lint-baseline.xml` file is created with all the current lint issues listed out. These issues do *not* get flagged as warnings.


Once the baseline file is created, the following two lines can also be added to` lintOptions`


```text
android {    ...    lintOptions {      baseline file("lint-baseline.xml")       warningsAsErrors true      abortOnError true         }  }
```


These make sure that any *new* warnings that come up in a PR will be flagged as errors and if the build process is properly configured, the code change can’t be merged unless the warnings are fixed.


As and when old warnings are fixed, we delete the` lint-baseline.xml` file and re-run lint. This creates a new` lint-baseline.xml` file with only the remainder of the warnings.


One thing to remember here is that baselining is very literal with line numbers in the code. If a previously baselined warning gets offset due to code changes, it will start showing up as a warning (or error, depending on your config) again.


Since all newly introduced warnings were build errors, we sat back assured that no *new* warnings would be inadvertently added to the codebase. Now we started to pare down existing warnings!


## Organization & Execution Phase


This phase consisted of swarming on the warnings that didn’t require coordination between organizations to resolve. As mentioned above, we were able to determine that some of the warnings — like` UnusedResources` can be easily fixed whereas others — like` ExtraTranslation` would have a less obvious of a fix. To mitigate this dichotomy and to make sure that we could distribute the task of fixing all warnings to the entire Android team, we created “Example PRs” which fixed one instance of a warning.


### Organization & Execution


Next, we ticketed the different warnings and engineers volunteered to fix them. We took every opportunity to pare down warnings. We had a “Quality Week” in which we got rid of many warnings. We have an Android Slack channel in which certain amounts of prodding and *cough-cough* pestering were done. We have a regular Android tech sync meeting during which, multiple “opportunities” were taken to remind everyone to fix the warnings. We even employed it as a ramp-up tool. Two of our engineers from Ukraine got a taste of the codebase by actually fixing some of the warnings!


Over the course of 5 months, while continuing to work on feature projects and bugs, our Android team closed off ~4,500 warnings.


As mentioned above, we found out that some warnings would need Product or Design involvement. Moreover, there were some that just didn’t need fixing. We decided to ignore such warnings en-masse by adding the` disable` option in` lintOptions` like so:


```text
android {    ...    lintOptions {      baseline file("lint-baseline.xml")      disable 'NewerVersionAvailable', 'TypographyFractions',    'TypographyQuotes', 'TypographyDashes'              warningsAsErrors true      abortOnError true      }  }
```


We created separate tickets for these warnings and these little gems we we saved for the next phase!


## End Boss Phase


This phase is currently an ongoing effort consisting of multiple departments working together.


### Baselining Compilation Warnings


Java and Kotlin compilation warnings are also being targeted in this phase. Unfortunately, there’s no easy way to baseline compilation warnings like lint warnings. However, there *is* a hard way.


In order for gradle to spit out warning numbers, we added


```text
gradle.projectsEvaluated  {        tasks.withType(JavaCompile)  {            options.compilerArgs << "-Xlint:all"       }  }
```


In the` allProjects` section of the app-level` build.gradle`


This makes the compiler spit out warnings with a “w:” prefix for Kotlin like so


```text
w: Type mismatch: inferred type is String? but String was expected
```


and a “warning:” prefix for Java warnings like so


```text
warning: [deprecation] getDefaultDisplay() in WindowManager has been deprecated
```


Using bash programming, we added a function in our CI that counts the current number of` warning:` and` w:` instances and compares the number to a constant. If the number of current warnings is lower than the constant, the build passes, if not, it fails. In this way we have a pseudo-baseline to get a quick heuristic for new warnings.


```text
numWarns=$(./gradlew assemble --no-daemon 2>&1 | grep -c 'w: /\|warning: ')  echo "Current number of warnings: $numWarns"  maxCappedWarnings=<some constant>  if (($numWarns > $maxCappedWarnings)); then    echo "You went over the magic number. Please fix a few warnings!"    exit 1  else    echo "You may have reduced the number of warnings! If so, thank you! Please consider opening a separate PR to reduce maxCappedWarnings"  fi
```


### Conclusion


We’re continuing to reduce the number of warnings. We’re committed to Project Zero and to have zero warnings in our codebase soon!
