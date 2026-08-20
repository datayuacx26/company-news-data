---
schema_version: "1.0.0"
document_id: "b288e73335d7d69847ac992ec8082f04d3a6a3b6d970bbf37040dada6960a491"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/04/keeping-server-driven-ui-consistent-across-platforms.html"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:18:26.702617+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:c804e8573ce220fdbfda42780ef69958dcb744ac82d184040cf2463ef1682313"
---

# How Yelp Keeps Server-Driven UI Consistent Across Four Platforms

# How Yelp Keeps Server-Driven UI Consistent Across Four Platforms


- Radu Comaneci, Software Engineer


- Apr 22, 2026


If you’ve read our[earlier post](https://engineeringblog.yelp.com/2024/03/chaos-yelps-unified-framework-for-server-driven-ui.html) , you already know about CHAOS—the server-driven UI (SDUI) framework we built at Yelp that powers our dynamic views. Until now, we’ve explored its architecture, backend implementation, and component model. In this post, we’ll dive into how we integrated CHAOS with Yelp’s cross-platform design system,[Cookbook](https://www.yelp.com/styleguide) , and the auto-generated bridge library, Konbini.


## Introduction to Cookbook


At Yelp, we support two major applications across our Web, iOS, and Android platforms: Yelp and Yelp for Business. This results in six different variations, which makes it challenging to maintain a unified experience.


To address this challenge, we created Cookbook, Yelp’s internal design system. A design system is a collection of reusable components, standards, and guidelines that help teams build consistent, scalable, and cohesive interfaces across different platforms. For example, Cookbook ensures that a UI component like a button looks and behaves consistently on Android, iOS, and the Web. While each platform may have slight variations due to native conventions, the overall appearance and behavior of components remain uniform. Beyond consistency, Cookbook also advocates for implementation discipline. Feature teams are encouraged to use the components as defined by Cookbook and avoid customizing them beyond the provided interfaces.


Cookbook is maintained by a dedicated team of engineers and designers known as Design Systems. This team is responsible for developing and maintaining libraries for all three platforms.


## Why Konbini?


When CHAOS was in the design phase, we quickly realized that Cookbook would need to be extended to support an additional platform: Python/CHAOS.


Before CHAOS, Yelp’s approach to SDUI was fragmented - different teams created their own frameworks, each with their own way of representing UI components on the backend. This led to inconsistent implementations across platforms and no alignment with Cookbook’s standardized components.


Throughout this blog post, we’ll use the example of a simple button to illustrate how the whole ecosystem works.


Here’s what it looked like to create a button before CHAOS:


```text
# Solution A's approach
def    get_components  (  spec  ):
return    [
SDUIButton  (
label  =  "  Click me to go to Yelp  "  ,
button_type  =  "  primary  "  ,
action  =  OpenURLAction  (  url  =  "  https://yelp.com  "  ),
size  =  "  small  "
)
]


```


```text
# Solution B's approach
def    get_components  (  spec  ):
return    [
ServerButton  (
text  =  "  Click me to go to Yelp  "  ,
style  =  "  primary  "  ,
click_handler  =  {  "  type  "  :    "  open_url  "  ,    "  url  "  :    "  https://yelp.com  "  },
display_size  =  "  sm  "
)
]


```


Both solutions were trying to render the same button component, but their backend representations were completely different—different property names, different ways to handle actions, different size values. This created a maintenance burden and made it difficult to ensure a unified user experience. CHAOS unified our SDUI architecture, but we still needed closer integration with Cookbook. Konbini, built by the Design Systems team, bridges this gap by allowing the backend to directly instantiate the same reusable components used on iOS, Android, and Web. This integration makes it far easier to deliver maintainable and scalable UI experiences across all platforms.


## What is Konbini?


Konbini is a collection of automatically generated libraries that expose the Cookbook component interfaces to Python code and provide the serialization/deserialization logic for the clients. The libraries are generated from JSON interface definitions for each component. Coming back to the button example, this is the JSON interface definition:


```text
{
"name"  :     "cookbook.Button"  ,
"version"  :     "0.8"  ,
"description"  :     "A customizable Cookbook button."  ,
"owners"  :     [
"Design Systems <design-systems@yelp.com>"
],
"parameters"  :     {
"text"  :     {
"type"  :     "String"  ,
"description"  :     "The text to show in the button."
},
"style"  :     {
"type"  :     "cookbook.ButtonStyle"  ,
"description"  :     "The style that corresponds to the visual hierarchy and placement of this button."
},
"on_click"  :     {
"type"  :     "Nullable<Action>"  ,
"default"  :     null  ,
"description"  :     "An action to be executed when the button is clicked (web) or tapped (mobile)."
},
"size"  :     {
"type"  :     "cookbook.ButtonSize"  ,
"default"  :     "standard"  ,
"description"  :     "A predetermined value that represents how large the button is."
},
"background_color"  :     {
"type"  :     "Color"  ,
"description"  :     "Background color of the button."
}
}
}


```


Whenever a new commit is pushed to the component_interfaces repository, Jenkins pipelines automatically trigger the code generation process and publish new versions of the libraries.


###


Konbini's automatic library generation pipeline


From this single JSON definition, four different libraries are created:


1. ` componentinterfaces` - the Kotlin package used in the Android clients containing the data classes used for deserializing the JSON response.
2. ` YLInterfaces` - the Swift package used in the iOS clients containing the data classes used for deserializing the JSON response.
3. ` component_interfaces` - the Python package used in the backend services containing the data classes used for serializing the JSON response.
4. ` component-interfaces` - the React package used on the Web containing the Typescript type definitions used for deserializing the JSON response.


Going back to the two examples from the above section, which were instantiating two different buttons, this is how buttons are instantiated in CHAOS using Konbini. This removes the need for different backend representations and reduces the maintenance burden.


```text
def    get_components  (  spec  :    KonbiniSpec  )    ->    List  [  KonbiniComponent  ]:
return    [
CookbookButton  (
text  =  "  Click me to go to Yelp  "  ,
style  =  CookbookButtonStyle  (  "  primary  "  ),
on_click  =  CookbookOpenUrl  (
url  =  "  https://yelp.com  "  ,
),
size  =  CookbookButtonSize  (  "  small  "  ),
background_color  =  ColorToken  .  COM_CAROUSELBUTTON_COLOR_BG_INLINE
]


```


## Serialization and deserialization in Konbini


Continuing with our CookbookButton example, let’s see how Konbini ensures that a button defined in Python ends up rendered identically on iOS, Android, and Web. This is achieved through automatic code generation that creates matching classes on each platform.


###


Serialization/Deserialization process in Konbini


1.


**Backend: Python defines the component**


When a CHAOS view returns a button, Konbini’s generated Python class handles serialization to JSON:


```text
class    CookbookButtonV0  :
parameters  :    CookbookButtonParametersV0


def    __init__  (  self  ,    text  ,    style  ,    on_click  =  None  ,    size  =  CookbookButtonSize  (  "  standard  "  ),    background_color  ):
self  .  parameters    =    CookbookButtonParametersV0  (
text  =  text  ,
style  =  style  ,
on_click  =  on_click  ,
size  =  size  ,
background_color  =  background_color
)
self  .  parameter_types    =    {
"  text  "  :    "  String  "  ,
"  style  "  :    "  cookbook.ButtonStyle  "  ,
"  on_click  "  :    "  Nullable<Action>  "  ,
"  size  "  :    "  cookbook.ButtonSize  "  ,
"  background_color  "  :    "  Color  "  ,
}
self  .  version    =    "  0.8  "


CookbookButton    =    CookbookButtonV0


```


2.


**Over the wire: Consistent JSON**


The serialized JSON looks the same regardless of which client requests it:


```text
{
"name"  :     "cookbook.Button"  ,
"version"  :     "0.8"  ,
"parameters"  :     {
"text"  :     "Click me to go to Yelp"  ,
"style"  :     "primary"  ,
"on_click"  :     {     "type"  :     "open_url"  ,     "url"  :     "https://yelp.com"     },
"size"  :     "small"  ,
"background_color"  :     "COM_CAROUSELBUTTON_COLOR_BG_INLINE"
}
}


```


3.


**Client: Generated classes deserialize and render**


On Android, Konbini generates a matching Kotlin class that deserializes this exact JSON structure:


```text
@JsonClass  (  generateAdapter    =    true  )
data class    CookbookButtonInterface    (
val    parameters  :    CookbookButtonInterfaceParams
)    :    InterfaceModel    {


override    val    name  :    String
get  ()    =    specName


companion    object    {
const    val    specName    :    String    =    "cookbook.Button"
const    val    specVersion    :    String    =    "0.8"
}
}


@JsonClass  (  generateAdapter    =    true  )
data class    CookbookButtonInterfaceParams    (
@Json  (  name    =    "text"  )     val    text  :    String  ,
@Json  (  name    =    "style"  )     val    style  :    CookbookButtonStyle  ,
@Json  (  name    =    "on_click"  )     val    onClick  :    ActionModel  ?    =    null  ,
@Json  (  name    =    "size"  )     val    size  :    CookbookButtonSize    =    CookbookButtonSize  .  standard  ,
@Json  (  name    =    "background_color"  )     val    backgroundColor  :    ColorToken
)    :    ParamsModel    {


companion    object    {
const    val    specName    :    String    =    "cookbook.Button"
const    val    specVersion    :    String    =    "0.8"
}
}


```


Because both the Python and Kotlin code are generated from the same JSON source, the parameter names are guaranteed to stay in sync.


Finally, a render extension method maps these parameters to the actual Cookbook component:


```text
@Composable
fun    CookbookButtonInterface  .  Render  (
renderer  :    KonbiniComposeRenderer  ,
onError  :    (  RenderError  )    ->    Unit  ,
modifier  :    Modifier    =    Modifier
)    {
with  (  parameters  )    {
// Common button parameters
val    buttonParams    =    ButtonParams  (
modifier    =    modifier  ,
text    =    text  ,
style  =  style  ,
color  =  backgroundColor  ,
onClick    =    onClick  ?.  let    {    renderer  .  handler  (  action    =    it  ,    onError    =    onError  )    }    ?:    {},
)


// Render appropriate button size
when    (  size  )    {
small    ->    ButtonSmall  (  buttonParams  )
standard    ->    ButtonStandard  (  buttonParams  )
large    ->    ButtonLarge  (  buttonParams  )
}
}
}


```


## Handling static resources


In addition to the primitive types that Konbini supports (which define customizable variables for each object, like` text` or` size` ), we use Yelp-specific types as well—such as colors, icons, gradients, or shadows—these types are known as tokens. Tokens provide engineers and designers with a set of predefined, use-case-specific values, so they don’t have to decide on these values from scratch for every component.


Each token is serialized as a simple string representing its name, and when exported, includes a` raw_value` field containing its actual value. For example, a color token used in the above example’s button would be serialized like this:


```text
def    serialize_color  (  value  ):
return    {
"  name  "  :    value  ,
"  raw_value  "  :    CookbookResources  .  instance  ().  colors  [  value  ][  "  default  "  ],
}


"  color  "  :    {
"  name  "  :    "  ref-color-black-100  "  ,
"  raw_value  "  :    {
"  a  "  :    1  ,
"  b  "  :    0  ,
"  g  "  :    0  ,
"  r  "  :    0
}
}


```


These tokens are maintained in a separate repository curated by our designers and are provided in JSON format.


## Backward compatibility & versioning


Backward compatibility is always an important consideration when designing mobile APIs. It ensures that any app version will continue to work in the future, even when the definition of the Cookbook components it uses have changed since the release of that app version. So how is this achieved?


Coming back to the button example, let’s assume we want to change the type of the text field from a String to a custom type called FormattedText which supports http links. This is a breaking change because older clients will receive a different type of object for the text field, which they don’t know how to interpret. The change in the JSON definition is straightforward:


```text
{
"  name  "  :    "  cookbook.Button  "  ,
"  version  "  :    "  1.0  "  ,
...
"  parameters  "  :    {
"  text  "  :    {
"  type  "  :    "  FormattedText  "  ,
"  description  "  :    "  A formatted text to show in the button. Can contain http links, too.  "
},
...
}


```


Konbini uses versions to ensure backward compatibility. Each client (Android/iOS/Web) has a spec file that includes a version number and lists all supported components with their versions. It’s represented as a JSON file:


```text
{
"  version  "  :    "  23.0  "  ,
"  interfaces  "  :    {
"  cookbook.Button  "  :    "  1.0  "
},
"  enumerations  "  :    {
"  cookbook.ButtonStyle  "  :    "  0.1  "
}
}


```


The spec file is saved inside the Konbini repository and gets bundled into all of the four generated libraries. Whenever a new component is updated or created, it must be included in the spec so that the classes for it get generated. At the same time, when there’s any change made to a spec, its version has to be bumped.


When a CHAOS view is requested by a client (in our example Android), a Konbini context object that includes the spec name and version is passed, something that looks like` android@23.0` . When the backend receives it, it will look at version 23.0 of the Android spec and see which version of the` CookbookButton` can be sent back to the client without breaking it.


If a component introduces a breaking change (like the one from the example above), then a major version bump should happen, in our case from` 0.8` to` 1.0` . When this occurs, Konbini adds an empty method to the` CookbookButton` class called` migrate` which should be implemented.


```text
def    migrate  (  model  :    CookbookButtonV1  )    ->    CookbookButtonV0  :
"""
The migrate function defined here is meant to backport an instance
of CookbookButtonV1 to an instance of CookbookContainerV0. Its default behavior
is to throw an error signifying that backporting the model is not supported.
However, we highly recommend replacing this behavior with your own custom
logic that uses the data from the provided CookbookContainerV2 to return an object
of type CookbookContainerV1. This will help keep views that use this model
compatible with a wider range of clients.
"""
return    CookbookButtonV0  (
text  =  model  .  text  .  toString  (),
style  =  model  .  style  ,
on_click  =  model  .  on_click  ,
size  =  model  .  size  ,
background_color  =  model  .  background_color
)


```


This method is called whenever the spec version on the client is lower than the one on the backend. In that case, the backend tries to instantiate a` CookbookButtonV1` class but it realizes the client only supports` CookbookButtonV0` .


## Quick recap


This post covered a lot of technical ground. Here are the key takeaways:


- Konbini bridges CHAOS and Cookbook, enabling server-driven UIs to use the same design system as native implementations.
- A single JSON definition generates four platform-specific libraries (Python, Kotlin, Swift, TypeScript) that stay perfectly in sync.
- The migration system and spec versioning ensure that older app versions continue working even as components evolve.


## More CHAOS?


This post provides the first deep dive into the Konbini library and how we keep the design consistent across all our platforms and use cases at Yelp, including CHAOS. Stay tuned to learn more about how we modeled the dynamic UI in CHAOS and implemented the most commonly used components, including stacks.


## Acknowledgements


We’d like to thank the Design Systems team for their partnership in building Konbini, and all the engineers across iOS, Android, Web, and Backend who contributed to making the CHAOS integration with Cookbook through Konbini a reality. Your feedback and collaboration were instrumental in shaping this system.


[Tweet](https://twitter.com/share)


### Become a Software Engineer at Yelp


Want to help us make even better tools for our full stack engineers?


[View Job](https://www.yelp.careers/us/en/c/engineering-jobs?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
