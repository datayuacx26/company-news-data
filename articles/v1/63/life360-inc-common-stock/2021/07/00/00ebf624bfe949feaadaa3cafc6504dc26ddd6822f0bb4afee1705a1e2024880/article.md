---
schema_version: "1.0.0"
document_id: "00ebf624bfe949feaadaa3cafc6504dc26ddd6822f0bb4afee1705a1e2024880"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/view-binding-life360-b8af648f3f3a"
published_at: "2021-07-02T01:02:02+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-08-20T00:22:26.884199+00:00"
content_hash: "sha256:79ae09a56b6ad630e320d384e5a74ae0cf84dbbaa824d2cc3264dd583ea41ee2"
---

# View Binding@Life360

> **Section 1: Why view binding and how did we do it?**


### Why did we want to move to view binding?


ButterKnife is deprecated and is replaced by View Binding(VB). View Binding generates a binding class that contains direct references to all the views in the layout. This enables developers to write null safe, type safe and easy to maintain code for referencing UI. View Binding compiles faster because it does not require annotation processing. It also enables developers to implement features with less code.[https://developer.android.com/topic/libraries/view-binding](https://developer.android.com/topic/libraries/view-binding)


### **Benefits of View Binding over ButterKnife, Kotlin Synthetics and findViewById:**


1. **Reference Variables:** View Binding generates a binding class which has reference to all layout components. We would not need variables like TextView, EditText in class files to access the XML components. Thus removing the need to add @BindView or equivalent to each component and reducing significant lines of code from all the View classes.
2. **Null safety:** Since view binding creates direct references to views, there’s no risk of a null pointer exception due to an invalid view ID. Additionally, when a view is only present in some configurations of a layout, the field containing its reference in the binding class is marked with @Nullable.
3. **Type safety:** The fields in each binding class have types matching the views they reference in the XML file. This means that there’s no risk of a class cast exception.


### Phases of Implementation:


The Life360 android codebase contains about ~25 modules, 15 of which have some layout views that needed to be modified. I created a spreadsheet which served as a blueprint of this implementation plan, with modules broken down into sub modules further broken into individual tasks (about 12 files in each task). Breaking the tasks in this pattern made sure that the changes were contained to one app feature at the most which makes testing a lot easier. At Life360 we have component ownership which kinda translates to modules in the code base, so my strategy for task break down also ensured that a PR would not require multiple component owners to sign off, avoiding delays in the process.


In one of our android tech sync’s which is a Triweekly meeting at Life360, I presented the view binding project with task breakdown to kick things off. I had my first PR for VB changes ready before this meeting to give a walk through of code changes and to serve as an example PR when developers picked up these tasks to work on.


I also used the spreadsheet to track progress outside JIRA in one place as all android developers from multiple teams were involved. I would color code every task to keep track of the status, follow up with developers and eventually every task would be green to indicate completion


*Phase 1:* Consolidate to a list of modules that need to be upgraded/modified and create JIRA tickets for reviewable and easier to test PR’s


1. Evaluate and create a list of modules where @BindView is being used
2. Evaluate and create a list of modules where usage of ButterKnife @OnClick needs to be replaced with setOnClickListener()
3. Evaluate and create a list of modules where usage of findViewById needs to be replaced with View Binding
4. Evaluate and create a list of modules where usage of Kotlin Synthetics needs to be replaced with View Binding


*Phase 2:* Introduce View Binding to our code base: Make sure everyone (25 developers) on the android team upgrades to the required Android Studio — 3.6, Android Gradle Plugin Version 3.6.0 and Gradle version 5.6.4.


*Phase 3:* For each module:


Task 1: Module level. Modify build.gradle for every module to enable View Binding in the codebase


Task 2: Depending on the packages


1. Replace usages of ButterKnife @BindView with View Binding
2. Replace usage of ButterKnife @OnClick with setOnClickListener(). @OnClick is the only functionality of ButterKnife that we use in our code which does not have an equivalent alternative in View Binding. We would need to change all the @OnClick implementation to .setOnClickListener() to ensure the click functionality doesn’t break.
3. Replace Kotlin Synthetics with View Binding
4. After all the code changes, run unit tests and perform manual testing on the affected module to ensure all the functionality is intact.


*Phase 4:* Clean Up Tasks:


1. Make sure ButterKnife is removed from the codebase completely.
2. Remove Proguard rules for ButterKnife.
3. Remove ButterKnife code from gradle.properties


### Here’s the code you need for the binding:


View Binding is enabled on a module by module basis. To enable view binding in a module, add the viewBinding element to its build.gradle file


```text
android {      ...      buildFeatures {          viewBinding = true      }  }
```


If view binding is enabled for a module, a binding class is generated for each XML layout file that the module contains. Each binding class contains references to the root view and all views that have an ID. The name of the binding class is generated by converting the name of the XML file to camel case and adding the word “Binding” to the end.


To set up an instance of the binding class for use with an activity, perform the following steps in the activity’s onCreate() method, call the static inflate() method included in the generated binding class. This creates an instance of the binding class for the activity to use. Get a reference to the root view by either calling the getRoot() method or using Kotlin property syntax. Pass the root view to setContentView() to make it the active view on the screen. You can then use the instance of the binding class to reference any of the views.


```text
private lateinit var binding: ResultProfileBinding   override fun onCreate(savedInstanceState: Bundle?) {      super.onCreate(savedInstanceState)      binding = ResultProfileBinding.inflate(layoutInflater)      val view = binding.root      setContentView(view)  }
```


If case of a layout where you do not need to refer its UI components in the class file, you do not need view binding class generated. You can simply cause a layout file to be ignored while generating binding classes by adding the tools:viewBindingIgnore=”true” attribute to the root view of that layout file. This will help reduce the overall APK size.


```text
<LinearLayout          ...          tools:viewBindingIgnore="true" >      ...  </LinearLayout>
```


> Section II: Best Practices


### Inflating Layouts


The inflate method for view binding takes three arguments — Inflater, Parent and AttachToRoot. The inflater is passed using LayoutInflater.from(context) and the attachToRoot needs to be true in most cases. If your layout does not show up on the device check if attachToRoot is set to false.


### Using binding variables without inflating layouts


For view classes where the layout does not need to be inflated but we need the binding variable to reference to UI components use .bind(View) of the binding class.


### Binding variables and referencing UI components


Accessing views should be done through the generated binding object. The binding object should be stored as a class-level variable for the view or passed into the view holder. This will mean that the view types will be automatically updated when the XML is altered and the full benefits of view binding can be taken advantage of.


### Binding for included layouts


<include>ed layouts in XML will generate their own binding class which needs to be referenced through parent XML layout’s binding class. For the included layout binding to be generated correctly, the id in the <include> tag needs to match the container layout’s id of the child layout.


Note: If the id does not match, the child binding variable within the parent layout’s binding is not generated.


subLayout in the third code snippet is the binding variable for the included layout that is auto generated by the SubLayoutBinding.class


**Link to Github Sample project** :


[https://github.com/life360/android-viewbinding-sample](https://github.com/life360/android-viewbinding-sample)


### Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .


---


[View Binding@Life360](https://medium.com/life360-engineering/view-binding-life360-b8af648f3f3a) was originally published in[Life360 Engineering](https://medium.com/life360-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
