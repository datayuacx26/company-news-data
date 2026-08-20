---
schema_version: "1.0.0"
document_id: "b0e27741150aa34a1c031eb631a9e789a36aeb4fe34dcfb48f4ba0ad82704151"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/android-ci-build-performance-faster-i-o-faster-builds-0cf2e4718e6a"
published_at: "2026-02-09T22:43:44+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:21:12.159869+00:00"
content_hash: "sha256:0c897e333b0974f45a50d7e8b85252a89bbc12bf5f4001c2c84d0e9b02499922"
---

# Android CI Build Performance — Faster I/O, Faster Builds

Android App Development


Android Build System


Ci Cd Pipeline


Android Gradle


# Android CI Build Performance — Faster I/O, Faster Builds


[Pablo Baxter](https://medium.com/@pablobaxter?source=post_page---byline--0cf2e4718e6a---------------------------------------)


5 min read


·


Feb 9, 2026


--


Press enter or click to view image in full size


Photo by[benjamin lehman](https://unsplash.com/@abject?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


It’s not always easy maintaining and improving Android builds, especially as projects continue to grow over time. At Life360, we were looking at our continuous integration (CI) run times, which were averaging ~24 minutes on every pull request. There was an effort to try and reduce this by optimizing JVM flags, optimizing dependency downloads, re-writing the Gradle build-logic, and bringing in Develocity to identify other areas of improvement. Eventually we found that one of the biggest bottlenecks in our Android build was I/O.


## Identifying the Bottleneck


To better explain why I/O is such a bottleneck, let’s dive a little bit into how Android builds APKs and AARs. When an Android binary is built using Gradle, plugins can use process annotations, generate code, bundle assets and resources, transform libraries, and compile Kotlin/Java code. Many of these tasks are cacheable in Gradle, however the Android Gradle Plugin disables caching for tasks that perform I/O only work. Caching I/O only work can have worse performance impact than just re-running the tasks.


Press enter or click to view image in full size


Before optimizing CI for I/O


One thing we noticed was that these tasks took a significant amount of time to run, mainly because there were so many. The image above shows an example of two tasks that correlate to the`[BundleLibraryClassesDir](https://cs.android.com/android-studio/platform/tools/base/+/mirror-goog-studio-main:build-system/gradle-core/src/main/java/com/android/build/gradle/internal/tasks/BundleLibraryClasses.kt?q=BundleLibraryClassesDir)` task and the`[DataBindingMergeDependencyArtifact](https://cs.android.com/android-studio/platform/tools/base/+/mirror-goog-studio-main:build-system/gradle-core/src/main/java/com/android/build/gradle/internal/tasks/databinding/DataBindingMergeDependencyArtifactsTask.kt?q=DataBindingMergeDependencyArtifactsTask)` task, both of which only perform I/O with hardly any computational work. Yet they each take a non-significant amount of time to run. These tasks are also the longest of these non-cacheable I/O only tasks, with the rest of the 600+ tasks taking 3+ seconds each.


## Testing a Hypothesis


**If I/O was faster, our builds should be faster.** Obviously increasing memory or processing power won’t improve I/O performance, so we looked at using a different storage medium. Currently, we use AWS instances for our CI pipeline, which we are able to swap for other instances with relative ease. We identified two instances we wanted to test against, one being memory optimized and the other being storage optimized. Due to Android builds being memory hungry, we wanted to prioritize memory over processing power in our benchmarks. We also didn’t want to incur a greater AWS cost, so we chose instances that were about the same price as those we currently used.


As for our setup, we used a simple[Gradle Profiler](https://github.com/gradle/gradle-profiler) scenario that only ran the debug APK assemble task for our Android project. Each run was done in a pipeline we created that allowed us to easily swap out the AWS instance types. We ran five iterations for each instance, with only one warm up run.


We first established the baseline of the current instance that ran our build task, which was the m8i.4xlarge instance type. This instance type doesn’t have an SSD, and was using an EBS volume.


### [m8i.4xlarge](https://instances.vantage.sh/aws/ec2/m8i.4xlarge?currency=USD) Results


Average time: 514,685.43 ms (~8.6 minutes)


This result aligned with what we had seen in our current CI pipelines. Next, we tested the r6idn.2xlarge instance, which is a memory optimized instance that contains a SSD and provided the same amount of memory as the m8i, but had half the number of vCPUs. Although we didn’t want to lose processing power, the cost of the r6idn.4xlarge was almost double of the m8i.4xlarge, so we opted for the reduced number of vCPUs.


### [r6idn.2xlarge](https://instances.vantage.sh/aws/ec2/r6idn.2xlarge?currency=USD) Results


Average time: 393,328.13 ms (~6.6 minutes)


Even with half the number of vCPUs, we saw a 24% improvement over the m8i instance! These I/O only tasks seemed to cause a significant negative impact on our build times. The next instance we tested was the i7i.2xlarge instance, which is a storage optimized instance that had the same amount of vCPUs and memory as the r6idn and was about the same price as well.


### [i7i.2xlarge](https://instances.vantage.sh/aws/ec2/i7i.2xlarge?currency=USD) Results


Average time: 289,587.15 ms (~4.8 minutes)


The result of the first benchmark was fantastic, however this one was phenomenal! This had a 26% improvement in build performance over the last benchmark, and a >40% improvement over our baseline. When the results were reconfirmed in another round of tests, it was clear the i7i instances were going to run our Android CI pipelines.


Press enter or click to view image in full size


Gradle Profiler benchmark results


## Measuring the Actual Improvements


Press enter or click to view image in full size


Before and after swapping to i7i


The graphs on the left represent the state of our CI times before deploying the i7i instances. The numbers are the average time for each phase of our CI. They range about 6 months before we swapped to the i7i instances, and were actually trending towards a positive improvement at the time due to some of the work mentioned earlier (JVM flag fine-tuning and Gradle build-logic re-write).


The right side of the graphs capture of CI run times from just before the i7i deployment, up until the end of January. Not only were the improvements clearly visible in these charts, the Android team at Life360 noticed them in their day to day almost immediately as well.


In summary, just as the[Block](https://youtu.be/Er_goz33q7c?si=TicThO8z5Y_PXBVv&t=1257) and[Peloton](https://youtu.be/o5ERrt53RIs?si=hvzZtCZ1okxAsXC4&t=666) teams have also found, it is clear that I/O is a significant bottleneck with Android builds on CI. Whether the AWS i7i instances are the best choice for Android builds isn’t as clear though. Additional investigations, analysis, and benchmarking would be necessary to answer that.


## Come join us


Life360 is creating the largest membership service for families by developing technology that helps managing family life easier and safer. There is so much more to do as we get there and we’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
