---
schema_version: "1.0.0"
document_id: "6d74c0b1eda539fe99b5163e820d658061e1d31cc3e74c5180b7f2b8be44758d"
company_key: "yc-yassir"
company: "Yassir"
source_id: "yc-yassir-rss-45239b019a57"
canonical_url: "https://medium.com/@Yassirtech/preview-the-compose-way-3123aace6aef"
published_at: "2025-03-03T09:13:04+00:00"
first_seen_at: "2026-07-24T07:42:37.830050+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:66e7b21914ba9e7b61e7c9adb026dae5fe273527162608943ca0712d444856d9"
---

# @Preview the Compose Way

# @Preview the Compose Way


[Welcome to our Yassir Engineering Blog](https://medium.com/@Yassirtech?source=post_page---byline--3123aace6aef---------------------------------------)


4 min read


·


Mar 3, 2025


--


Press enter or click to view image in full size


***@Preview*** in Jetpack Compose is a developer’s secret weapon that is generally available now, but are you truly unlocking its full potential? Most developers barely scratch the surface, using less than 10% of what ***@Preview*** can do. Let’s see how you can use more of it!


**TL;DR:** Compose has advanced Preview features at our fingertips that can help spot UI problems before shipping the app to the world.


For those who are not yet familiar with Jetpack Compose, ***@Preview*** is a DSL command that informs the compiler that the following part is only going to be rendered on the Android Studio design view panel and won’t be shipped as part of the compiled APK or AAB.


Let’s first review the benefits of ***@Preview***


- Preview composables in design view, with live updates as you edit, as mentioned above.
- Specify dimensions for your preview using the widthDp and heightDp attributes.


```text
@Preview(widthDp = 50, heightDp = 50)
```


- Test[dynamic colors](https://m3.material.io/styles/color/dynamic-color/overview) using the **wallpaper** attribute.


Press enter or click to view image in full size


[https://developer.android.com/static/develop/ui/compose/images/preview-devicespec-spec-list.png](https://developer.android.com/static/develop/ui/compose/images/preview-devicespec-spec-list.png)


- Preview famous device models or resolutions using the **device** attribute.


Press enter or click to view image in full size


- Test different locales using the **locale** parameter.


```text
@Preview(locale = “fr-rFR”)
```


- Change background colors using the **backgroundColor** attribute.


```text
@Preview(showBackground = true, backgroundColor = 0xFF00FF00)
```


[https://developer.android.com/static/develop/ui/compose/images/tooling-background-preview.png](https://developer.android.com/static/develop/ui/compose/images/tooling-background-preview.png)


- Display the status and action bars using the **showSystemUi** parameter.


```text
@Preview(showSystemUi = true)
```


[https://developer.android.com/static/develop/ui/compose/images/tooling-decorated-preview.png](https://developer.android.com/static/develop/ui/compose/images/tooling-decorated-preview.png)


- Access code to run only inside ***@Preview*** by adding “ *LocalInspectionMode.current* ” to the Compose function.


```text
@Composable  fun GreetingScreen(name: String) {      if (LocalInspectionMode.current) {          // Show this text in a preview window:          Text("Hello preview user!")      } else {          // Show this text in the app:          Text("Hello $name!")      }  }
```


- Test gestures directly from the design panel.


Press enter or click to view image in full size


[https://developer.android.com/static/develop/ui/compose/images/tooling-interactive-preview-demo.gif](https://developer.android.com/static/develop/ui/compose/images/tooling-interactive-preview-demo.gif)


- Run certain composable previews on a real device or emulator.


Press enter or click to view image in full size


[https://developer.android.com/static/develop/ui/compose/images/tooling-deploy-preview-demo.gif](https://developer.android.com/static/develop/ui/compose/images/tooling-deploy-preview-demo.gif)


- Create an image from a ***@Preview*** by right-clicking on its design panel rendered preview.


[https://developer.android.com/static/develop/ui/compose/images/tooling-copy-render.png](https://developer.android.com/static/develop/ui/compose/images/tooling-copy-render.png)


## Powerful features you probably never heard about


You might know some or most of the previous dozen ***@Preview*** benefits and capabilities, but I’m almost positive the information below is under the radar.


## Multipreview templates


The idea of Kotlin is similar to Compose: to decrease the code needed to implement certain functionalities.


The Android and Jetbrains team in “androidx.compose.ui:ui-tooling-preview 1.6.0-alpha01+” introduced Multipreview API templates ( **@PreviewScreenSizes** , **@PreviewFontScales** , **@PreviewLightDark** , and **@PreviewDynamicColors)** , so that, with one single annotation, you can preview your Compose UI in common scenarios.


Press enter or click to view image in full size


[https://developer.android.com/static/studio/images/design/multipreview-template.png](https://developer.android.com/static/studio/images/design/multipreview-template.png)


While Multipreview templates are a highlight, there’s even more to explore. The most powerful feature is that you can create your own ***Multipreview templates.*** With just a few lines of code, you can eliminate repetitive configurations and apply consistent preview parameters across your entire project


## **Custom Multipreview annotations**


Just imagine that instead of repeating your own ***@Preview*** annotations and selections for localization or Dark/Light modes previews every time, you can just write it once and use it everywhere.


```text
@Preview(      name = "small font",      group = "font scales",      fontScale = 0.5f  )  @Preview(      name = "large font",      group = "font scales",      fontScale = 1.5f  )  annotation class FontScalePreviews
```


```text
@FontScalePreviews  @Composable  fun HelloWorldPreview() {      Text("Hello World")  }
```


Press enter or click to view image in full size


[https://developer.android.com/static/develop/ui/compose/images/tooling/preview-multipreview-1.png](https://developer.android.com/static/develop/ui/compose/images/tooling/preview-multipreview-1.png)


Code Sample: ​​


```text
@Preview(      name = "Spanish",      group = "locale",      locale = "es"  )  @FontScalePreviews  annotation class CombinedPreviews   @CombinedPreviews  @Composable  fun HelloWorldPreview2() {      MaterialTheme { Surface { Text(stringResource(R.string.hello_world)) } }  }
```


Press enter or click to view image in full size


[https://developer.android.com/static/develop/ui/compose/images/tooling/preview-multipreview-2.png](https://developer.android.com/static/develop/ui/compose/images/tooling/preview-multipreview-2.png)


Now, imagine that you need to add a new preview to all your ***@Preview*** in the whole application because you added a new language and want to test or modify an already existing one in the whole application. You won’t need to visit and search-replace all. All you need to do is visit your custom multi-preview annotations class and update it. Voilà, all previews have been updated!


To conclude, ***@Preview*** is more than a convenience — it’s a powerhouse of UI testing and optimization. Explore these features in your next project and share your experience. What’s your favorite @Preview feature? Let us know in the comments!


***Shady Selim***


***Staff Android Engineer***


([Check our open vacancies here](https://jobs.lever.co/Yassir) !)


[Learn more about Yassir](http://yassir.com/)
