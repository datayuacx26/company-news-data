---
schema_version: "1.0.0"
document_id: "d153bf76311a1f56c9fe9d6a114c2f9ef5f900ab430399618d8961e33fcdce81"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/the-setup-customizing-android-studio-with-the-intellij-plugin-sdk-part-2-7d1ea0599113"
published_at: "2022-01-26T01:01:45+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:33.441096+00:00"
content_hash: "sha256:f113d4d1b19f892e3d659c31721ee849da45f5f80579825ece6fde745cb4847f"
---

# The Setup — Customizing Android Studio with the IntelliJ Plugin SDK (Part 2)

# The Setup — Customizing Android Studio with the IntelliJ Plugin SDK (Part 2)


[Patches Klinefelter](https://medium.com/@sklinefelter89?source=post_page---byline--7d1ea0599113---------------------------------------)


6 min read


·


Jan 26, 2022


--


Press enter or click to view image in full size


In[part 1](https://medium.com/life360-engineering/customizing-android-studio-with-the-intellij-plugin-sdk-part-1-8827e3ddd217) of this blog series, we started diving into how Android Studio plugins were used to solve real problems faced by our engineering team. Now we can start discussing the prerequisites and setup required as well as how they evolved into something the Android team could feasibly use.


## Creating a New Project


Both Plugins I’ve written so far have taught me valuable lessons about how to work with the IntelliJ Platform and its expectations. I first started by following the[Gradle guide](https://plugins.jetbrains.com/docs/intellij/gradle-prerequisites.html) to create a new project with Kotlin and Groovy:


Press enter or click to view image in full size


New Project Creation


This was before the plugin projects had migrated to Kotlin DSL. You can leverage Java 11 as well, but at the time of writing Java 8 was selected since it was before Android Studio brought in better Java 11 support.


At this point, you will also want to setup your IDE to run the plugin if it’s not already done by default. Under` Run` ->` Edit Configurations` you should see or create a task that looks like this:


Press enter or click to view image in full size


runIde task


## A Plugin’s Structure


A plugin’s project structure is very similar to most pure Java/Kotlin projects where there is a` src/main/kotlin` or` src/main/java` directory where the Kotlin or Java plugin code will be written as well as additional source directories like` test` .


Project Structure


Within the structure, there are two main files that are vital and work together to define a plugin. These are:


1. The` resources/META-INF/plugin.xml` file and
2. The` build.gradle` or` build.gradle.kts`


These files contain crucial information such as what IDEs your plugin supports, the version for the plugin, changelog notes, and what additional SDK features are necessary for the plugin to function. We’ll dive into parts of these one at a time.


## The` build.gradle`


Within the` build.gradle` is where additional plugins are enabled such as the IntelliJ plugin verifier (covered later), KTLint ([https://github.com/pinterest/ktlint](https://github.com/pinterest/ktlint) ), and basic Java/Kotlin support. This is not much different from a typical Gradle plugin setup where ids and versions are defined.


```text
plugins  {        id("java")      // Kotlin support      id("org.jetbrains.kotlin.jvm")  version   "1.4.32"      //  https://github.com/JetBrains/gradle-intellij-plugin       id("org.jetbrains.intellij")  version   "1.1.2"      // Kotlin linter -  https://github.com/JLLeitschuh/ktlint-gradle       id("org.jlleitschuh.gradle.ktlint")  version   "9.4.1"  }
```


Similarly, dependencies, repositories, and publication details are also defined in this file. Below is an example that defines a version and group for publishing, sets up some basic Kotlin stblib imports, and also imports JUnit as well for unit tests.


```text
group   = "com.life360.android"  version   = pluginVersion  repositories   {        mavenCentral()      gradlePluginPortal()  }   dependencies   {        compileOnly  (files("lib/wizard-template.jar"))       implementation  ("org.jetbrains.kotlin:kotlin-stdlib:1.4.32")       implementation  ("org.jetbrains.kotlin:kotlin-reflect:1.4.32")       testImplementation  ("junit:junit:4.13.1")  }
```


The most unique portion of a Gradle file for a plugin project is the` intellij` closure. This is where the version, name, additional Intellij SDK plugin features, and IDEs supported are defined. There’s also a field to set the compile-time version of the IntelliJ Platform SDK similar to how you define the compile SDK version for Android.


```text
// See  https://github.com/JetBrains/gradle-intellij-plugin/   intellij   {        type.set("IC")      version.set(compileIdeVersion)      pluginName.set("Life360 Templating Plugin")      plugins.set( listOf  ("android"))      /**       * Download IntelliJ sources while configuring Gradle project.       */       downloadSources.set(true)      /**       * Patch plugin.xml with since and until build        * values inferred from IDE version.       */       updateSinceUntilBuild.set(false)  }
```


Another key feature is being able to test your plugin in a Sandboxed environment with the option to specify an alternate IDE.` runIde` handles creating a Sandboxed instance already, so there’s no additional work required there. It’s important to note that without defining an alternate IDE, the plugin will default to opening in IntelliJ Community Edition. In the example below, the location provided to the` ideDir` is for Android Studio. This means when` runIde` (sometimes called` runPlugin` ) is executed, the plugin can be utilized and seen in a Sandboxed version of Android Studio for testing.


```text
import org.apache.tools.ant.taskdefs.condition.Os  val androidStudioPath = if (Os.isFamily(Os. FAMILY_WINDOWS  )) {      "C:\\Program Files\\Android\\Android Studio"  } else {      "/Applications/Android Studio.app/Contents"  }  runIde   {        ideDir.set(File(androidStudioPath))  }
```


## Plugin Verifier


Along with manual verification, IntelliJ offers a tool ([https://github.com/JetBrains/intellij-plugin-verifier](https://github.com/JetBrains/intellij-plugin-verifier) ) that will run compatibility checks against a list of specified versions. This is extremely useful so that it is not necessary to track down and manually test each version of the IDE you want to support. By simply running` ./gradlew runPluginVerifier` the tool will run a set of checks against the specified list and output potential issues to look into. It’s also possible to specify the version of plugin verifier to use.


```text
runPluginVerifier   {        verifierVersion.set("1.256")      ideVersions.set( listOf  ("IU-201.8743.12"))  }
```


The` patchPluginXML` closure is another place where information about the plugin is defined. It is important to know this piece of information from the docs ([https://plugins.jetbrains.com/docs/intellij/gradle-guide.html#patching-the-plugin-configuration-file](https://plugins.jetbrains.com/docs/intellij/gradle-guide.html#patching-the-plugin-configuration-file) ):


> If a` patchPluginXml` attribute value is explicitly defined, the attribute value will be substituted in **plugin.xml** .


This means that regardless of values in the` plugin.xml` , the ones from the Gradle file will take priority. The minimum IDE version supported is defined by` sinceBuild` and the maximum is defined by` untilBuild` . You can also see in the example below that change notes can be specified.


```text
patchPluginXml   {        version.set(pluginVersion)      sinceBuild.set(minIdeVersion)      untilBuild.set(maxIdeVersion)      changeNotes.set(          "Removes documentation directories before regenerating docs, updates Kotlin, " +              "stabilizes plugin verifier version used, and updates org.jetbrains.intellij plugin"      )  }
```


The` plugin.xml` feels much like and` AndroidManifest.xml` . The name, id, vendor, and description are defined. It is also possible to specify the minimum and maximum supported IDE versions, but if defined in the Gradle file, they will be override


## The Plugin.xml


It does seem somewhat redundant to define the name and plugins in both the` intellij` closure of the Gradle file and the` plugin.xml` but as far as I can tell, the` intellij` closure does not patch the` plugin.xml` like` patchPluginXml` does.


```text
<idea-plugin>      <id>com.life360.android.templating.plugin</id>      <name>Life360 Templating</name>      <vendor>Life360</vendor>      <description>          A plugin that will add Life360 specific templates to the project creation screen of Android Studio      </description>      <idea-version since-build="201.8743.12" until-build="203.*" />      <!-- Product and plugin compatibility requirements -->      <!--  https://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/plugin_compatibility.html   -->      <depends>org.jetbrains.android</depends>      <extensions defaultExtensionNs="com.android.tools.idea.wizard.template">          <wizardTemplateProvider implementation="com.life360.android.templating.plugin.KitAndEngineTemplateProvider" />      </extensions>  </idea-plugin>
```


With this setup, you should be able to see your plugin live in an IDE and begin to bring additional features to your


Press enter or click to view image in full size


Plugin in the settings list


In the next entry of this series, I will cover some basics about plugins such as extension points, GUI work, and other key classes that can be used to actually start implementing your own neat features.


## Additional Resources


- [https://plugins.jetbrains.com/docs/intellij/welcome.html](https://plugins.jetbrains.com/docs/intellij/welcome.html)
- [https://plugins.jetbrains.com/docs/intellij/gradle-prerequisites.html](https://plugins.jetbrains.com/docs/intellij/gradle-prerequisites.html)
- [https://plugins.jetbrains.com/docs/intellij/dev-alternate-products.html#configuring-pluginxml](https://plugins.jetbrains.com/docs/intellij/dev-alternate-products.html#configuring-pluginxml)
- [https://plugins.jetbrains.com/docs/intellij/build-number-ranges.html](https://plugins.jetbrains.com/docs/intellij/build-number-ranges.html)
- [https://github.com/pinterest/ktlint](https://github.com/pinterest/ktlint)
- [https://github.com/JetBrains/intellij-plugin-verifier](https://github.com/JetBrains/intellij-plugin-verifier)
- [https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl](https://github.com/JetBrains/gradle-intellij-plugin#setup-dsl)


## Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
