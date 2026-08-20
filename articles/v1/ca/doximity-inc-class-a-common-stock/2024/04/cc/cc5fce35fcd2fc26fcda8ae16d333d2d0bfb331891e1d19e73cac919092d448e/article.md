---
schema_version: "1.0.0"
document_id: "cc5fce35fcd2fc26fcda8ae16d333d2d0bfb331891e1d19e73cac919092d448e"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/scaling-with-deeplinks-on-android"
published_at: "2024-04-08T11:34:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:967fd9b1012b19983fbee4ca8064f2c61fbca2084f175ae6a0538744611bd882"
---

# Scaling with Deeplinks on Android

Deeplinking support in Android is relatively straightforward. You declare your deeplinks in the app's manifest, receive the incoming` Intent` passed in to your` Activity` and then navigate the user to the associated destination. This pipeline works just fine when you only need to support a handful of deeplinks. What happens though as the number of deeplinks grow? How do you deal with several dozen, or possibly even hundreds, of deeplinks?


## First Stab at Deeplinks


When we first implemented deeplinking in our app, we only had to support 12 different deeplinks. We introduced a` DeeplinkRouter` class that would take the incoming` Intent` and determine where to navigate the user:


```text
interface    Deeplink    {
fun    route  (  uri  :    Uri  ):    Boolean
}


class    NotificationsDeeplink  (  val    navigator  :    Navigator  )    :    Deeplink    {
override    fun    route  (  uri  :    Uri  ):    Boolean    {
return    if    (  uri  .  path    ==    "/notifications"  )    {
navigator  .  goToNotifications  ()
true
}    else    {
false
}
}


class    ProfileDeeplink  (
val    navigator  :    Navigator  ,
val    userRepository  :    UserRepository
)    :    Deeplink    {
override    fun    route  (  uri  :    Uri  ):    Boolean    {
val    profileId    =    uri  .  getQueryParameter  (  "id"  )
val    isCurrentUser    =    userRepository  .  getCurrentUser  ().  id    ==    profileId


return    if    (  uri  .  path    ==    "/profile"    &&    profileId    !=    null  )    {
navigator  .  goToProfile  (  profileId  ,    showColleagues    =    isCurrentUser  )
true
}    else    {
false
}
}
}


// ... more deeplink definitions ...


class    DeeplinkRouter  (
notificationsDeeplink  :    NotificationsDeeplink  ,
profileDeeplink  :    ProfileDeeplink  ,
// ...
)    {


private    val    deeplinks    =    listOf  (
notificationsDeeplink  ,
profileDeeplink  ,
// ...
)


fun    route  (  intent  :    Intent  )    {
intent  .  data  ?.  let    {    uri    ->
deeplinks  .  find    {    it  .  route  (  uri  )    }
}
}
}


```


## Problems at Scale


As the number of deeplinks and the complexity of their definitions grew, two big issues started to emerge. The first was managing the sheer number of deeplinks - we had nearly 50 at one point! Each of these had to be injected into` DeeplinkRouter` and then passed to its internal` deeplinks` list. We decided to resolve this by introducing an annotation that would allow us to eliminate this boilerplate:


```text
@RegisterDeeplink
class    NotificationsDeeplink  (  val    navigator  )    :    Deeplink    {
override    fun    route  (  uri  :    Uri  ):    Boolean    {
// ...
}
}


class    DeeplinkRouter  (  val    register  :    DeeplinkRegister  )    {
fun    route  (  intent  :    Intent  )    {
intent  .  data  ?.  let    {    uri    ->
register  .  deeplinks  .  find    {    it  .  route  (  uri  )    }
}
}
}


```


The` DeeplinkRegister` class is generated at build time (more on this later) and it's responsible for maintaining the list of` deeplinks` .


The second issue comes from the fact that certain deeplink definitions are order-sensitive. For example, take the following two deeplinks:


```text
class    ArticleDeeplink  (  val    navigator  :    Navigator  )    :    Deeplink    {
override    fun    route  (  uri  :    Uri  ):    Boolean    {
return    if    (  uri  .  path  ?.  startsWith  (  "/article"  )    ==    true    &&    uri  .  pathSegments  .  size    ==    2  )    {
navigator  .  goToArticle  (  id    =    uri  .  pathSegments  [  1  ])
true
}    else    {
false
}
}
}


class    CommentsDeeplink  (  val    navigator  :    Navigator  )    :    Deeplink    {
override    fun    route  (  uri  :    Uri  ):    Boolean    {
return    if    (  uri  .  path  ?.  startsWith  (  "/article"  )    ==    true
&&    uri  .  path  ?.  endsWith  (  "/comments"  )    ==    true
&&    uri  .  pathSegments  .  size    ==    3
)    {
navigator  .  goToComments  (  id    =    uri  .  pathSegments  [  1  ])
true
}    else    {
false
}
}
}


```


If the user opens[www.doximity.com/article/{id}/comments](http://www.doximity.com/article/%7Bid%7D/comments) and` ArticleDeeplink` is called first,` CommentsDeeplink` will never have a chance to route the deeplink. We have to ensure that` CommentsDeeplink` is placed before` ArticleDeeplink` in the list. This problem is subtle and becomes increasingly difficult to manage at scale.


Instead, it would be better to merge the two deeplinks and enforce the rule that only one deeplink definition can map to a specific top-level path (i.e.` /article` ):


```text
interface    Deeplink    {
fun    route  (  uri  :    Uri  )    // no longer returns Boolean
}


@RegisterDeeplink  (  "/article"  )
override    fun    route  (  uri  :    Uri  )    {
// could break this down into helper functions/classes
when    {
uri  .  pathSegments  .  size    ==    2    ->    {
navigator  .  goToArticle  (  id    =    uri  .  pathSegments  [  1  ])
}
uri  .  pathSegments  .  size    ==    3    &&    uri  .  path  ?.  endsWith  (  "/comments"  )    {
navigator  .  goToComments  (  id    =    uri  .  pathSegments  [  1  ])
}
}
}
}


class    DeeplinkRouter  (  val    register  :    DeeplinkRegister  )    {
fun    route  (  intent  :    Intent  )    {
intent  .  data  ?.  let    {    uri    ->
val    topLevelPath    =    uri  .  pathSegments  .  firstOrNull  ()
register  .  findDeeplink  (  "/$topLevelPath"  )  ?.  let    {    it  .  route  (  uri  )    }
}
}
}


```


` DeeplinkRegister` will maintain a mapping of top-level paths to their corresponding deeplink definition. If a match is found, the deeplink is returned to the` DeeplinkRouter` and given the incoming` Uri` . This greatly reduces the complexity around managing deeplinks.


## Adding Annotation Support


Let's take a look at building the annotation processor for our deeplinks. First, we'll want to create a new Kotlin library module called` :deeplinks-annotation,` which will house our` @RegisterDeeplink` annotation:


```text
@Target  (  AnnotationTarget  .  CLASS  )
annotation    class    RegisterDeeplink  (  val    topLevelPath  :    String  )


```


Next, we'll create a second module called` :deeplinks-processor` , which will use the following dependencies:


```text
dependencies    {
implementation    project  (  ':deeplinks-annotation'  )    // access our annotation
implementation    "com.google.devtools.ksp:symbol-processing-api:{version}"    // ksp annotation processor
implementation    "com.squareup:kotlinpoet-ksp:{version}"    // writing kotlin code
}


```


Before we dive into the implementation of our annotation processor, let's take a look at the code we're trying to generate:


```text
// DeeplinkRegister.kt is generated at build time by our annotation processor


class    DeeplinkRegister  (
notificationsDeeplink  :    NotificationsDeeplink  ,
profileDeeplink  :    ProfileDeeplink  ,
articleDeeplink  :    ArticleDeeplink  ,
// the rest of our deeplinks
)    {


private    val    deeplinks  :    Map  <  String  ,    Deeplink  >    =    mapOf  (
"/notifications"    to    notificationsDeeplink  ,
"/profile"    to    profileDeeplink  ,
"/article"    to    articleDeeplink  ,
// the rest of our mappings
)


fun    findDeeplink  (  topLevelPath  :    String  ):    Deeplink  ?    =    deeplinks  [  topLevelPath  ]
}


```


With this in mind, let's start writing the actual annotation processor. The first step is to acquire all of the classes that are annotated with` @RegisterDeeplink` :


```text
DeeplinkRegisterProcessor  (
val    codeGenerator  :    CodeGenerator  ,    // we'll write our generated file to this
val    logger  :    KSPLogger    // for logging error messages to the user
)    :    SymbolProcessor    {
override    fun    process  (  resolver  :    Resolver  )
val    symbols    =    resolver
.  getSymbolsWithAnnotation  (  RegisterDeeplink  ::  class  .  qualifiedName  .  orEmpty  ())
.  filterIsInstance  <  KSClassDeclaration  >()


// ...
}
}


```


We'll maintain a map of deeplink names to paths as we visit each annotated class. We'll also use[KotlinPoet](https://square.github.io/kotlinpoet/) to assist us in writing the code for the` DeeplinkRegister` constructor:


```text
override    fun    process  (  resolver  :    Resolver  )
// ...


val    deeplinkMap    =    mutableMapOf  <  String  ,    String  >()    // deeplink names to paths
val    registerConstructor    =    FunSpec  .  constructorBuilder  ()
symbols  .  forEach    {    it  .  accept  (  Visitor  (  deeplinkMap  ,    registerConstructor  ),    Unit  )    }


// ...
}


inner    class    Visitor  (
val    deeplinkMap  :    Map  <  String  ,    String  >,
val    registerConstructor  :    FunSpec  .  Builder
)    :    KSVisitorVoid  ()    {
override    fun    visitClassDeclaration  (  classDeclaration  :    KSClassDeclaration  ,    data  :    Unit  )    {
super  .  visitClassDeclaration  (  classDeclaration  ,    data  )


// ...
}
}


```


When visiting each class, we'll first obtain the` topLevelPath` argument from the annotation. Then we'll verify it hasn't already been associated with another deeplink. Finally, we'll create the constructor parameter for the deeplink and update the map:


```text
super  .  visitClassDeclaration  (  classDeclaration  ,    data  )


val    topLevelPath    =    classDeclaration  .  getAnnotationsByType  (  RegisterDeeplink  ::  class  )
.  first  ()
.  topLevelPath


if    (  deeplinksMap  .  values  .  contains  (  topLevelPath  ))    {
logger  .  error  (  "Multiple deeplinks with the same path were found: $topLevelPath"  )
}


val    typeName    =    classDeclaration  .  toClassName  ()
val    variableName    =    typeName  .  simpleName  .  replaceFirstChar    {    it  .  lowercase  ()    }
val    parameter    =    ParameterSpec  .  builder  (  variableName  ,    typeName  ).  build  ()


deeplinkMap  [  variableName  ]    =    topLevelPath
registerConstructor  .  addParameter  (  parameter  )
}


```


In addition to checking for duplicate paths, it would also be a good idea to check that the path is formatted correctly (i.e. starts with a` '/'` , no subdirectories or query parameters).


After visiting each class declaration, we'll build the internal` deeplinks` property with the path to deeplink mappings:


```text
override    fun    process  (  resolver  :    Resolver  )    {
// ...


val    deeplinkPackage    =    "..."    // the full package name where the Deeplink interface is located in your project
val    deeplinkType    =    ClassName  (  deeplinkPackage  ,    "Deeplink"  )


val    deeplinksMapType    =    MAP  .  parameterizedBy  (  STRING  ,    deeplinkType  )
val    deeplinksProperty    =    PropertySpec  .  builder  (  "deeplinks"  ,    deeplinksMapType  ,    KModifier  .  PRIVATE  )
.  initializer  (
CodeBlock  .  builder  ()
.  add  (  "mapOf("  )
.  withIndent    {
registerConstructor  .  parameters  .  forEach    {    parameter    ->
add  (  CodeBlock  .  of  (  "\n\"%L\" to %L,"  ,    deeplinkMap  [  parameter  .  name  ],    parameter  .  name  ))
}
}
.  add  (  "\n)"  )
.  build  ()
)
.  build  ()


// ...
}


```


Then we'll create the` findDeeplink` function:


```text
override    fun    process  (  resolver  :    Resolver  )    {
// ...


val    findDeeplinkFunction    =    FunSpec  .  builder  (  "findDeeplink"  )
.  addParameter  (  "topLevelPath"  ,    STRING  )
.  returns  (  deeplinkType  .  copy  (  nullable    =    true  ))
.  addCode  (  "return deeplinks[topLevelPath]"  )
.  build  ()


// ...
}


```


The last thing to do here is create the` DeeplinkRegister` class, adding each of the pieces we previously built, and writing it to a` DeeplinkRegister.kt` file:


```text
override    fun    process  (  resolver  :    Resolver  )    {
// ...


val    deeplinkRegister    =    TypeSpec  .  classBuilder  (  "DeeplinkRegister"  )
.  primaryConstructor  (  registerConstructor  .  build  ())
.  addProperty  (  deeplinksProperty  )
.  addFunction  (  findDeeplinkFunction  )
.  build  ()


try    {
FileSpec  .  get  (  deeplinkPackage  ,    deeplinkRegister  )
.  writeTo  (  codeGenerator  ,    dependencies  )
}    catch    (  e  :    FileAlreadyExistsException  )    {
// fixes issue with multiple rounds of processing
}


return    symbols  .  filterNot    {    it  .  validate  ()    }.  toList  ()    // unprocessed symbols
}


```


Before we can use the annotation processor in our project, we need to register it. We can do this by declaring a provider for it:


```text
class    DeeplinkRegisterProcessorProvider    :    SymbolProcessorProvider    {
override    fun    create  (  environment  :    SymbolProcessorEnvironment  ):    SymbolProcessor    {
return    DeeplinkRegisterProcessor  (  environment  .  codeGenerator  ,    environment  .  logger  )
}
}


```


Then we add a new file named` com.google.devtools.ksp.processing.SymbolProcessorProvider` to the` /resources/META-INF/services` directory, which will contain the fully qualified name of the provider we just created:


```text
com.package.name.DeeplinkRegisterProcessorProvider


```


Finally, we'll add the two modules to our app's` build.gradle` file:


```text
apply    plugin:    'com.google.devtools.ksp'


dependencies    {
api    project  (  ':deeplinks-annotation'  )
ksp    project  (  ':deeplinks-processor'  )
}


```


## Other Considerations


There's more improvements we can make to our solution. For instance, not all deeplinks can be determined from its path alone. Some may have different domains that require different routing. Others might not even be HTTP (i.e.` tel:` and` mailto:` links). One option is to handle these deeplinks separately inside of` DeeplinkRouter` . Another is to update` @RegisterDeeplink` to include optional fields for the scheme and domain parts of the URL, having these be part of the underlying deeplink mapping in` DeeplinkRegister` .


Sometimes different paths require the same routing, for example` /profile` and` /profiles` . You can delegate any secondary deeplinks to the primary one, preventing the need to rewrite the same definition twice:


```text
@RegisterDeeplink  (  "/profile"  )
class    ProfileDeeplink  (  val    navigator  :    Navigator  )    :    Deeplink    {
override    fun    route  (  uri  :    Uri  )    {
// ... deeplink definition ...
}
}


@RegisterDeeplink  (  "/profiles"  )
class    ProfilesDeeplink  (  profileDeeplink  :    ProfileDeeplink  )    :    Deeplink    by    profileDeeplink


```


Another consideration is testing. We pass in` android.net.Uri` to all of our deeplinks. Since classes from the Android framework are stubbed out in JUnit tests, we have to do a lot of mocking! Consider swapping this class for` java.net.URI` or create a wrapper type that can be faked during tests.


Overall though, the solution provided here gives us a good framework for dealing with deeplinks at scale!


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
