---
schema_version: "1.0.0"
document_id: "55612f7928d42d3d3ca049347fc6b175f689e8287c1535a0e1beb49f37b94ddd"
company_key: "wealthfront-corporation-common-stock"
company: "Wealthfront Corporation"
source_id: "wealthfront-corporation-common-stock-rss-9fbe6ef385e3"
canonical_url: "https://eng.wealthfront.com/2025/09/09/going-edge-to-edge-how-android-sdk-35-modernizes-the-wealthfront-app/"
published_at: "2025-09-09T17:12:54+00:00"
first_seen_at: "2026-07-20T23:18:22.615485+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:cb69c334633d340917ebfded65f6f0fccd17bfe14e37e3d68c2c242ef65209c7"
---

# Going Edge-to-Edge: How Android SDK 35 Modernizes the Wealthfront App

One of our core engineering principles at Wealthfront is maintaining a high standard of quality and platform infrastructure, and that includes keeping our platforms up to date on the latest releases. On Android, this means keeping our targetSdk up to date, which provides Wealthfront’s Android users with the latest security and platform stability features, as well as some quality of life upgrades. The new edge-to-edge feature in the Wealthfront app comes along with SDK 35, and provides a more immersive and modern-looking experience for our Android users.


## The Goal: Upgrade targetSDK to 35


In October 2024, Google released Android 15 (SDK level 35), which introduced a new concept to the Android experience – Edge-to-Edge UI. In short, an app targeting SDK 35 and running on Android 15 or higher will now be rendered edge-to-edge. This means that the app’s content will be drawn behind the status and navigation bars, which are now partially or entirely transparent, depending on whether or not you use gesture navigation.


Before edge-to-edge


After edge-to-edge


As you can see, the edge-to-edge rendering is a much cleaner look, and a welcome improvement to app immersion.


## The Implementation


For apps targeting SDK 35, edge-to-edge will be enabled by default on devices running Android 15+. This means that if we want to target SDK 35, we need to support edge-to-edge. We wanted to keep the experience consistent across Android versions, so we decided to manually enable this for Android versions 11+, because edge-to-edge isn’t well supported on Android 10. We do this by calling enableEdgeToEdge() at app startup, which allows us to support edge-to-edge on devices running Android 11+, rather than only Android 15+.


Once we do this, window insets need to be handled manually, otherwise some content in the app may get cut off. On the left is a device with edge-to-edge enabled, but not handled by the app. You can see that the toolbar is cut off (because we don’t use the Material 3 Toolbar component, which handles insets automatically), and the bottom button is too close to the system navigation bar. On the right is what the same screen looks like with the window insets applied as padding to the components that touch the edges of the screen.


Before edge-to-edge


After edge-to-edge


So how do we support this? In a Compose-only app using Material 3 components, this would be fairly simple. Many Material 3 components, including Toolbar, BottomNavigationBar, and others support edge-to-edge out of the box with no additional changes needed. You can also use Scaffold to receive app insets and apply them to your view, which might look something like this:


```text
setContent {
EdgeToEdgeTheme {
Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
AppContent(
modifier = Modifier.padding(innerPadding)
)
}
}
}
Code language:    Kotlin    (  kotlin  )
```


In this case, innerPadding will include the status bar padding, system navigation padding, keyboard padding, etc., and apply that to the content inside the Scaffold.


This approach works well enough, but presents two problems:


Scrolling content still gets cut off by the system bars, so the app doesn’t feel truly edge-to-edge
Many apps (including Wealthfront) use a mix of XML and Compose, so this approach won’t work for non-Compose screens


To solve these problems, we need to handle insets on a per-screen basis. This way, each screen can decide where the insets need to be based on its content.


On Compose screens, this is a piece of cake – all you need to do is add Modifier.windowInsetsPadding(LocalWindowInsets.current) to each screen’s base view. This is a Compose-provided method that provides the current window’s insets and applies it as padding to the view the modifier is attached to.


For XML screens, we chose to do this with a custom InsetsHandler interface that we inject into each screen.


```text
interface    InsetsHandler     {
fun    handleInsets  (
view:  View  ?,
insetDirection:  InsetDirection   = InsetDirection.EXCLUDE_TOP,
consumeInsets:  Boolean   =  true  ,
useMargin:  Boolean   =  false  ,
)


fun    handleInsetsWithPinnedButton  (
mainContent:  View  ?,
bottomPinnedContent:  View  ?,
screenHasToolbar:  Boolean   =  true  ,
)


fun    handleInsets  (
view:  View  ?,
consumeInsets:  Boolean   =  true  ,
handleInsets: ( View  ,  Insets  ) ->  Unit  ,
)
}
Code language:    Kotlin    (  kotlin  )
```


This interface contains three different ways to add padding to a view. The first adds inset padding in one or more directions – the default is EXCLUDE_TOP, which adds padding to the left, right, and bottom of the view, and in the Wealthfront app is the most common. This represents a screen that has a toolbar, where the toolbar handles its own insets, and the screen’s content needs to handle the other three directions.


```text
insetsHandler.handleInsets(binding.screenContent)
Code language:    Kotlin    (  kotlin  )
```


The second function handles our second most common use case, which is a screen with a button pinned to the bottom. In this case, the toolbar handles its own padding, just like the first case, but we need to apply padding to the scrolling screen content and bottom button separately so that no content is cut off. The scrolling content gets horizontal padding, and the bottom button gets left, right, and bottom padding with EXCLUDE_TOP (since we’re adding padding to all sides except the top edge).


```text
insetsHandler.handleInsetsWithPinnedButton(
mainContent = view.scrollingContent,
bottomPinnedContent = view.confirmButton,
)
Code language:    Kotlin    (  kotlin  )
```


The final function is a bit of a catch-all. It takes in a callback function, to which it provides the calling view and the insets. That view can then do whatever it wants with those provided insets at the call site. This is useful in cases where there’s conditional logic to show and hide views, or in cases with unique UI that may have specific padding needs.


```text
insetsHandler.handleInsets(view) { v, insets ->
v.updatePadding(left = insets.left, top = insets.top, right = insets.right)
}
Code language:    Kotlin    (  kotlin  )
```


##
Future-proofing


This implementation works for our existing screens, but what about future ones? New engineers may not know to use these insets functions, and it’s easy to forget even for seasoned engineers. At Wealthfront, we have many custom Lint rules to handle this kind of situation – our builds enforce these rules to make sure that our code quality stays high and that all engineers are following the same standards.


To do this for our edge-to-edge implementation, we created a new Lint rule to enforce that every one of our Magellan Steps calls one of our handleInsets functions. The Lint rule applies to any class that extends one of Magellan’s Steps and throws an error if it doesn’t call one of our insets handling functions in its onShow() method.


```text
internal    val   ENFORCE_EDGE_TO_EDGE_SUPPORT = Issue.create(
id = LegacyStepEdgeToEdgeDetector:: class  . simpleName  !!,
briefDescription = "Error description"  ,
explanation = "Error explanation"  ,
category = CORRECTNESS  ,
priority = 7  ,
severity = ERROR  ,
implementation = Implementation   (LegacyStepEdgeToEdgeDetector:: class  . java  ,  EnumSet.of   (JAVA_FILE, TEST_SOURCES)),
)


internal    class    LegacyStepEdgeToEdgeDetector   :  Detector   (), SourceCodeScanner {
private    val   onShowSuperClasses = listOf( "com.wf.Screen"  )


override    fun    applicableSuperClasses  ()   : List<String> = onShowSuperClasses


override    fun    visitClass  (context:  JavaContext  , declaration:  UClass  )    {
val   className = declaration.name ?:  ""
val   psiClass = declaration.javaPsi
val   directSuper = psiClass.superClass
val   onShowMethod = declaration.methods.find { it.name ==  "onShow"   }


val   onShowErrorMessage =  "Some error message"


if   (onShowMethod ==  null  ) {
context.report(
ENFORCE_EDGE_TO_EDGE_SUPPORT,
declaration.nameIdentifier ?: declaration,
context.getNameLocation(declaration),
onShowErrorMessage,
)
}  else    if   (!onShowMethod.callsEdgeToEdgeInsets()) {
context.report(
ENFORCE_EDGE_TO_EDGE_SUPPORT,
declaration.nameIdentifier ?: declaration,
context.getNameLocation(declaration),
onShowErrorMessage,
)
}
}


private    fun   UMethod. callsEdgeToEdgeInsets  ()   :  Boolean   {
var   found =  false


this  .uastBody?.accept( object   : AbstractUastVisitor() {
override    fun    visitCallExpression  (node:  UCallExpression  )   :  Boolean   {
if   (node.methodIdentifier?.name  in   setOf( "handleInsets"  ,  "handleInsetsWithPinnedButton"  )) {
found =  true
}
return    super  .visitCallExpression(node)
}
})


return   found
}
}
Code language:    Kotlin    (  kotlin  )
```


Now, all new classes will have to handle their insets before being allowed to merge to master!


## Final Thoughts


The new edge-to-edge feature in the Wealthfront app provides a more immersive and modern-looking experience for our Android users, and allows us to support a new SDK with the latest security and privacy features. We also were able to future-proof this implementation by using our build pipelines and custom Lint checks to make sure this experience remains consistent going forward.


Everybody loves a fresh UI upgrade every once in a while, and getting the best of the latest Android SDK features gives us the best of both worlds.


---


## Disclosures


The information contained in this communication is provided for general informational purposes only, and should not be construed as investment or tax advice. Nothing in this communication should be construed as a solicitation or offer, or recommendation, to buy or sell any security. Any links provided to other server sites are offered as a matter of convenience and are not intended to imply that Wealthfront Corporation (“Wealthfront”) or its affiliates endorses, sponsors, promotes and/or is affiliated with the owners of or participants in those sites, or endorses any information contained on those sites, unless expressly stated otherwise.


Investment advisory services are provided by Wealthfront Advisers LLC, an SEC-registered investment adviser. Brokerage services are provided by Wealthfront Brokerage LLC, Member FINRA/SIPC. Financial planning tools are provided by Wealthfront Software LLC.


All investing involves risk, including the possible loss of money you invest, and past performance does not guarantee success. Please see our Full Disclosure for important details.


Wealthfront Advisers LLC, Wealthfront Brokerage LLC, and Wealthfront Software LLC are wholly owned subsidiaries of Wealthfront Corporation.


© 2025 Wealthfront Corporation. All rights reserved.
