---
schema_version: "1.0.0"
document_id: "aa10b447c72a3353cf3067dd3a243f5d1c6acbe2066db9921838540339d569df"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-04-02-babelplugincloudinary/"
published_at: "2019-04-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:57578eecf0cf36c76194f54551ded321fa2098ed18e3a7cc0275c333bd0f7272"
---

# Presenting babel-plugin-cloudinary

### Compile Cloudinary URLs at build time so that you don't need to ship the cloudinary-sdk in your bundle!


I’m happy to let you know that we are releasing[trivago/babel-plugin-cloudinary](https://github.com/trivago/babel-plugin-cloudinary) to the open source community! Throughout this article I will explain to you the motivation behind this project and how it works in detail.


At trivago, we use images to a large degree to enable our users to get a visual impression of the accommodations that they’re interested in. We all want to see beautiful and good quality pictures so we can have a better feeling about the place where we are going to.


Displaying such a vast amount of images can be costly, especially when measuring the amount of data that mobile devices need to consume when using modern web applications. That’s why we adopted[Cloudinary](https://cloudinary.com/) , a cloud-based image and video management platform, that allows us to optimize our image processing and manipulate them on the fly ─ this maximizes each image’s potential to its specific use case regarding size, effects and quality for the best performance.


## Using Cloudinary


For applications to consume Cloudinary, there is a specific API that allows us to apply all these[neat transformations](https://cloudinary.com/documentation/image_transformation_reference) . At trivago, we started by creating a backend layer that applies the transformations interacting with the[Cloudinary API](https://cloudinary.com/documentation) , this layer is mainly used for Server Side Rendering. There are Cloudinary SDKs available for various programming languages, JavaScript being one of them.


## Moving towards a client-side approach


Soon we realized that we needed to have the ability to apply transformations using this API in the front end as well, for a more dynamic approach.


### Preprocessing Twig templates


We started by building a plugin for[Melody](https://melody.js.org/) , our JS framework. We use the plugin in our[Twig](https://twig.symfony.com/) templates compiling specific function calls into a URL that would be assembled at build time.


#### Original Twig template


```text
<  img   src  =  "{{ imageUrl("mypic.jpg", {
transformation  : [
{  effect  : "cartoonify"},
{  radius  : "max"},
{  effect  : "outline:100",   color  : "lightblue"},
{  background  : "lightblue"},
{  height  :   300  ,   crop  : "scale"}
]
}) }}"  >
```


#### Compiled template


```text
import   { elementVoid }   from   "melody-idom"  ;


export   const   _template  :   {};


const   _statics  :   [
"src"  ,
// in the line below you can see the compiled URL!
"https://imgcy.trivago.com/e_cartoonify/r_max/co_lightblue,e_outline:100/b_lightblue/c_scale,h_300/mypic.jpg"
];


_template.render:   function   (  _context  ) {
elementVoid  (  "img"  ,   "%?aR|+'"  , _statics);
};


if   (process.env.  NODE__ENV   !==   "production"  ) {
_template.displayName:   "Test"  ;
}


export   default   function   Test  (  props  ) {
return   _template.  render  (props);
}
```


Our first approach was functional, but not fully compatible with the Cloudinary API. The base of our solution was an existing simplification of the actual API, a package called[cloudinary-microurl](https://www.npmjs.com/package/cloudinary-microurl) . It offers a minimalist replication of the Cloudinary API in a lighter implementation that still was not the most suitable match for our needs, since not all the Cloudinary features are supported in this package. We realized that this would become something that we would need to update and maintain overtime to keep it partially compliant with the Cloudinary API and its changes over time.


### Thinking about future use cases


Implementing a client-side solution that would integrate with Cloudinary means that you eventually fall into one of the following scenarios:


1. You depend on a solution that is not fully compatible, running risks such as:


- Having some use case that cloudinary-microurl does not support;
- Delegating URL construction to this unofficial 3rd party is undoubtedly more error-prone since the output URL might not be entirely the same as the official Cloudinary SDK would generate. The package has tests, but you’ll never know.


1. You import the Cloudinary SDK to generate URLs at runtime, which could mean an increase of[~20kb (minified+gzipped)](https://bundlephobia.com/result?p=cloudinary-core@2.5.0) in our bundle, but if you look closely there is a total of[~70kb minified](https://bundlephobia.com/result?p=cloudinary-core@2.5.0) JavaScript that still needs to be parsed by the browser ─ I’m betting that you would like to avoid that.


Talking about bundle size: At trivago we use[webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) to measure the impact of our JavaScript modules and dependencies. So more specifically for us, having the cloudinary-core within our bundle would mean an increase of **14.19 kb** in our vendor bundle (around **14%** of the bundle size).


So how could we get our preprocessing solution to work without depending on something like cloudinary-microurl, but at the same time have the flexibility to create dynamic Cloudinary URLs at runtime?


## Designing the babel-plugin-cloudinary


Thinking about the problem a bit more, we realized that there was a third way: a plugin that would use the Cloudinary SDK at build time to generate these URLs. In a few compilation steps we managed to obtain the same result while keeping this completely transparent for our developers.


Drawing this for the first time made it clear to me that the job here would be pretty straightforward! Those three steps in the figure represent the only additional work that the plugin needs to do in order for us to achieve a fully Cloudinary based solution, supporting dynamic expressions. Next, we will look into a real example that will make this diagram more clear.


### A real example


The output of the plugin is a simple plain[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)[template literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) that appends all your URL configurations and the Cloudinary transformation output. To give you a more concrete idea of what this would look like in a real-life scenario, below you can find a sample of the usage of the plugin, first by showing the source, and second by displaying the final result (the compiled code).


#### Original source code


```text
const   useNewVersion  :   false  ;
const   getResourceExtension  :   ()  :  >   'jpeg'  ;
const   dimension  :   180  ;
const   url  :   __buildCloudinaryUrl  (  'el_hotel'  , {
transforms  :   {
transformation  :   'crop'  ,
crop  :   'fill'  ,
width  :   dimension  ,
height  :   dimension
},
prefix  :   'trivago/images'  ,
postfix  :   useNewVersion   ?   '_v8'   :   '_v7'  ,
resourceExtension  :   getResourceExtension  ()
});
```


#### Final result


```text
var   useNewVersion  :   false  ;
var   getResourceExtension  :   function   getResourceExtension  () {
return   'jpeg'  ;
};
var   dimension  :   180  ;


var   url  :   "https://trv.images.com/c_fill,h_${dimension},w_${dimension}/trivago//img/posts/babelplugincloudinary/el_hotel${useNewVersion ? '_v8' : '_v7'}.${getResourceExtension()}"  ;
```


You’re wondering where the` https://trv.images.com` comes from. We made globals configurable via a runtime configuration file (.cloudinaryrc.json), where we define all the Cloudinary parameters such as the cloud name or whether you want safe URLs with scheme https only.


Going back to the previous diagram, we can now link the dots and explain the plugin work step by step. When a call to` __buildCloudinaryUrl` is found in the codebase, the following happens:


##### Step 1


The plugin takes all the dynamic expressions (e.g., the variable dimension or the conditional expression that is used in the postfix) and replaces them with placeholders. The arguments of` __buildCloudinaryUrl` should now look something like this:


```text
{
transforms  : {
transformation  :   'crop'  ,
crop  :   'fill'  ,
width  :   'REPLACE1'  ,
height  :   'REPLACE1'
},
prefix  :   'trivago/images'  ,
postfix  :   'POSTFIX'  ,
resourceExtension  :   'RESOURCE_EXTENSION'
}
```


##### Step 2


We take all the parameters (including the now plain transformations) and call the cloudinary-core to obtain the partially final URL. Cloudinary gives us back the final partial result:


```text
'https://trv.images.com/c_fill,h_REPLACE1,w_REPLACE1/trivago/images/el_hotel'  ;
```


##### Step 3


We replace all the placeholders with their original dynamic expressions (if existent) and we output to the final compiled code the string that is assigned to the variable URL in the above snippet. At the end we obtain the following:


```text
`https://trv.images.com/c_fill,h_${  dimension  },w_${  dimension  }/trivago/images/el_hotel_${
useNewVersion   ?   '_v8'   :   '_v7'
}.${  getResourceExtension  ()  }`  ;
```


### Happy users and happy developers


In the end, **we shift from a solution where we give our clients the tools to build the images’ URLs to a solution where we give them the final images’ URLs** . This benefits our developers and users in the following ways:


- Compiling URLs at build time decreases our JS footprint, consequently shortens the time that our users need to wait for trivago to load on their devices.
- Offers a consistent approach to create URLs on the front-end, compliant with the Cloudinary API. This makes the lives of our developers easier since they don’t need to learn a new tool, or do any additional work to create images’ URLs.


## Let’s open source it


I’m happy to let you know that we are releasing this to the open source community! I hope you found it somehow useful for your project use cases, and of course, we would also love to hear some feedback on the plugin and suggestions to improve it. Take a look at the plugin at[trivago/babel-plugin-cloudinary](https://github.com/trivago/babel-plugin-cloudinary) . If you struggle with some of the issues that we described throughout this article, this might just be the right solution for you.


If you love modern web JavaScript technologies such as[babel](https://babeljs.io/) or[webpack](https://webpack.js.org/) , you will find lots of opportunities to work closely with them and bring your ideas to[life at trivago](https://life.trivago.com/) , so check out our[open positions](https://careers.trivago.com/join/open-positions) . We also advocate open source, so if you’re interested in seeing what we have built for the community, you can check our list of[open source projects](https://tech.trivago.com/opensource/) .
