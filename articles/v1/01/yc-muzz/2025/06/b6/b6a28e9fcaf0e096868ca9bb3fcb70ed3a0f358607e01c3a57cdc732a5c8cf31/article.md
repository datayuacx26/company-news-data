---
schema_version: "1.0.0"
document_id: "b6a28e9fcaf0e096868ca9bb3fcb70ed3a0f358607e01c3a57cdc732a5c8cf31"
company_key: "yc-muzz"
company: "Muzz"
source_id: "yc-muzz-news-import-4a23516b0f87"
canonical_url: "https://engineering.muzz.com/screenshot-testing-at-muzz"
published_at: "2025-06-20T15:07:06+00:00"
first_seen_at: "2026-07-25T16:24:04.083021+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:2294e7f34376b7d80d000002231be25cc472988515e1e96bf58476ff9fef722e"
---

# Screenshot Testing at Muzz

## Background


The **Muzz Android** app powers the marriage journey of millions of users worldwide. Due to the number of people we serve – and the potentially life-changing nature of our app – we have a responsibility to ensure consistently high quality. One way we do this is through screenshot testing, which helps us catch unintended UI changes before they reach production.


Screenshot testing is a process that involves capturing an image of the app’s UI and comparing it to a reference image. If the images differ, the tests fail and provide developers with important feedback of changes to the app’s UI.


## UiState + ViewModel


The Muzz Android app uses ViewModels that expose a flow of UiState objects to a Compose based view. The UiState is a single class that holds all of the data needed to render the view.


A typical UiState might look like this:


```text
data class UiState(
val title: String,
val subtitle: String,
val imageUrl: String,
val buttonText: String,
val isButtonEnabled: Boolean,
)
```


You can then generate previews based on your UiState:


```text
@Composable
@Preview
internal fun MyScreenPreview() {
MyScreen(
uiState = UiState(
title = "Alice, Welcome to Muzz",
subtitle = "Find your perfect match",
imageUrl = "https://example.com/image.png",
buttonText = "Get Started",
isButtonEnabled = true
)
)
}
```


With some setup around running the screenshot tests (we use[Paparazzi](https://github.com/cashapp/paparazzi?ref=engineering.muzz.com) ), this
will generate a screenshot test.


## Flaws in the Approach


There are a couple of things that could be better with our screenshot tests:


- Any logic that generates the UiState is not included. Future changes to the logic may not be reflected in the tests, and the previews may not accurately reflect the current state of the app.
- If you have multiple previews, each preview needs to create the correct UiState. Any refactoring of the UiState could give compiler errors that need to be fixed in each preview.


## UiMapper


To address these issues, we introduced a new component called a` UiMapper` . The UiMapper is responsible for taking in domain-level data, and returning a UiState. The ViewModel collects the domain level data, passes it to the UiMapper, and then puts the UiState into a flow that the view can observe.


We then have a small reflection-based utility that can create a UiMapper for use in previews:


```text
@Composable
private fun MyScreenBasePreview(
profile: MarriageProfile = MarriageProfile.default()
) {
val uiMapper = rememberPreviewMapper<MyScreenUiMapper>()
val uiState = uiMapper.map(profile = profile)
MyScreen(uiState)
}


@Composable
@Preview
internal fun MyScreenPreview() {
MyScreenBasePreview()
}
```


The` .default()` function is a test-only utility that returns a domain object with relevant default values, so when new fields are added to the domain object, the default function is updated with a sensible default value and test classes still compile.


Because of the reflection-based utility, UiMappers are constrained in what dependencies it can have, generally only allowing` Context` and` Resources` references, and other classes with those dependencies.


An example UiMapper would look like:


```text
class MyScreenUiMapper @Inject constructor(
private val resources: Resources,
) {
fun map(profile: MarriageProfile, buttonEnabled: Boolean): UiModel {
val title = resources.getString(R.string.welcome_title, profile.name)
return UiModel(
title = title,
subtitle = resources.getString(R.string.welcome_subtitle),
imageUrl = profile.profileImageUrl,
buttonText = resources.getString(R.string.welcome_cta),
isButtonEnabled = buttonEnabled,
)
}
}
```


With this setup:


1. The generated preview now matches the current state of the app much more closely. If we update the subtitle to include the age of the profile, the screenshots will update automatically without having to alter any of the previews.
2. We can write unit tests for functionality inside the UiMapper. This is especially helpful when the mapper contains more complicated logic which isn’t suitable for preview testing (e.g. you change a string based on how many days in the past something happened). We can have comprehensive quick unit testing of some behaviour, and screenshot testing of other behaviour.
3. We maintain a single default function for each domain model, meaning new previews can be created quickly without adding lots of maintenance for future changes to the UiState (most changes to the UiState require 0 changes to preview code to get the new screenshot). In the example above, updating the subtitle to include the age of the profile requires 0 changes to any of the previews and accurately reflects the new logic. If you add another domain model to the mapper, then previews can be updated to add the default domain object.


## Summary


Screenshot testing is a powerful tool for catching unintended UI changes before they reach production. By introducing a new component into our architecture, we supercharged our screenshot tests to make them faster to create, easier to maintain, and more relevant to the screen being shown to our users day to day.


### Footnote –` rememberPreviewMapper`


While not important for explaining the total concept, some of the implementation of this reflection-based utility might be of interest. Full implementation below, but it uses reflection to lookup the constructors of the target class, and then each parameter of the constructor it will check against types it knows about (\`Context\`,
\`Resources\`). If it matches a known type then that known type is returned, otherwise it calls itself recursively to inject the unknown type, hoping that type will have a constructor with only known types.


If it finds a constructor it can call then it calls it with the required arguments, otherwise it throws an exception with some (hopefully) helpful information.


```text
@Composable
@VisibleForTesting
inline fun <reified T : Any> rememberPreviewMapper(): T {
val context = LocalContext.current
return remember(context, T::class) {
injectPreviewMapper(T::class, context) as T
}
}


@VisibleForTesting
fun injectPreviewMapper(
mapperClass: KClass<out Any>,
context: Context,
): Any {
val injectables: List<Pair<(KParameter) -> Boolean, (KParameter) -> Any>> = listOf(
{ it: KParameter -> it.type.classifier == Context::class } to { context },
{ it: KParameter -> it.type.classifier == Resources::class } to { context.resources },
{ it: KParameter ->
val classifier = it.type.classifier
classifier is KClass<*> && classifier.constructors.any { it.hasAnnotation<Inject>() }
} to {
try {
injectPreviewMapper(
mapperClass = it.type.classifier as KClass<*>,
context = context,
)
} catch (e: IllegalArgumentException) {
throw IllegalArgumentException(
"Cannot inject ${mapperClass.simpleName} with ${it.type.classifier}",
e,
)
}
},
)


val constructor = mapperClass.constructors.firstOrNull { constructor ->
constructor.hasAnnotation<Inject>()
} ?: throw IllegalArgumentException(
"Cannot inject class ${mapperClass.simpleName} as no @Inject constructors",
)
val args = mutableMapOf<KParameter, Any?>()
constructor.parameters.forEach { param ->
if (!param.isOptional) {
val injectable = injectables.firstOrNull { it.first(param) }
?: throw IllegalArgumentException(
"${mapperClass.simpleName}: Cannot inject dependency ${param.name} of unknown type ${param.type}",
)
args[param] = injectable.second(param)
}
}


return constructor.callBy(args)
}
```


##
