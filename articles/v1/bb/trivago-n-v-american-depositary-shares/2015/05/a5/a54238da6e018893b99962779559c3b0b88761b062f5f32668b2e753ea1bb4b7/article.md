---
schema_version: "1.0.0"
document_id: "a54238da6e018893b99962779559c3b0b88761b062f5f32668b2e753ea1bb4b7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-05-28-introduce-webpack/"
published_at: "2015-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:44.458928+00:00"
content_hash: "sha256:5e745cfb22d25eae7bd50e440e3515b62d3b31a4961aaa54cd996af95855382b"
---

# Risk-free Migration From Assetic to Webpack

### With an example of using transducers.js


Learn how we introduced webpack to build our JavaScript assets. One of the main challenges was to run both Assetic and webpack in parallel for some time, in order to run tests and to make sure nothing was broken. This was achieved without any code or configuration duplication by developing a custom webpack loader which was tailored to our setup, and which makes use of transducers.


In theory,[webpack](http://webpack.github.io/) calls itself a module bundler. In practice, it is a lot more than that. Among its many features are included:


- Easy-to-use on-demand loading of resources
- The ability to load a combination of JavaScript and CSS in a single asynchronous request
- Support for various module formats like CommonJS, ES6, etc.
- Extensibility through custom loaders


There is already a range of articles which do a great job at covering the fundamentals of webpack (e.g., I can recommend the following two introductions:[“Getting started with webpack”](http://www.uxebu.com/blog/2014/09/getting-started-webpack/) , and[“An introduction to webpack”](http://cuttleblog.tumblr.com/post/63669845272/webpack) ). This post will mainly focus on the *extensibility through custom loaders* feature mentioned above. As a real-world example, we will look at how we managed a transparent, non-disruptive transition towards webpack as our method of generating our JavaScript assets.


## The point of departure


Until recently, our[Symfony 2](http://symfony.com/) Web application used[Assetic](http://symfony.com/doc/current/cookbook/assetic/asset_management.html) to build its JavaScript assets. Using[Twig](http://twig.sensiolabs.org/) as a template engine, we would bundle several JavaScript files together using this syntax:


```text
// File: js.html.twig
{% javascripts
'@CommonBundle/Resources/public/js/compatibility.js'
'@CommonBundle/Resources/public/js/trivagoTranslation.js'


//   ...
//   Imagine a loooooooong list of files here
//   ...


'@PriceSearchBundle/Resources/assets/js/module/Gui/Tooltip.js'
'@PriceSearchBundle/Resources/assets/js/module/Survey/Survey.js'


output=  'js/js-main.js'
filter=  "?yui_js"
combine=true
package=  "javascript"
%}
<  script   type  =  "text/javascript"   src  =  "{{   asset_url   }}"  ></  script  >
{% endjavascripts %}
```


Inside several` {% javascripts ... %}` blocks, we specified a list of .js files which were to be concatenated and minified. The target file was specified using the` output` parameter. For our main JavaScript target file, the source file list was more than 160 entries long, and was frequently being worked on.


We decided to A/B test the Assetic-generated JavaScript next to the webpack-generated one, so that we had a way to make sure webpack did not introduce any cross-browser issues or other problems with our existing code. For this to be a fair baseline comparison (optimizations like[chunking](http://webpack.github.io/docs/code-splitting.html) would come later), we needed webpack to bundle the exact same set of files together. The simplistic way to do this would be to create a new entry file for the webpack build process, and to add a lot of` require` calls to that file:


```text
require  (  'CommonBundle/Resources/public/js/compatibility.js'  );


// ...
// Again, loooooooong list of requires
// ...


require  (  'PriceSearchBundle/Resources/assets/js/module/Gui/Tooltip.js'  );
require  (  'PriceSearchBundle/Resources/assets/js/module/Survey/Survey.js'  );
```


However, do we want to duplicate a list of 160 included files? Surely not, because every change to that list would have to be made twice, and sooner or later somebody would be bound to forget doing that. A better, cleaner solution was needed, and this is where a custom loader came into play.


## Staying free of duplication


The principle how that loader should work and be called is simple:


1. Use the Twig file that holds the JavaScript file list (the one sketched out in the first code block) as an entry point for webpack
2. Have that Twig file loaded through a custom loader…
3. …which should then generate the corresponding` require` calls (as shown in the second code example). Webpack will then take it from there.


The relevant parts of the (slightly simplified) webpack configuration look like this, then:


```text
module  .  exports  : {
// Specify our .twig file as the entry file
entry: __dirname   +   '/src/Trivago/Bundle/MainBundle/Resources/views/js.html.twig'  ,
module: {
loaders: [
{
test:   /  js  \.  html  \.  twig  $  /  ,             // Only match the entry file
loader:   'twig-javascripts-loader'    // Our custom loader
}
]
}
};
```


This snippet shows that we want to pipe only the entry file through our custom loader. So, what is a loader, exactly? A webpack loader is a module that usually exports a single function. This function is passed the input content (whether that be JavaScript source code, or anything else) as a string, and - in the simplest case - returns a transformed version of the input, also as a string. The following loader returns its input as-is:


```text
// Identity loader
module  .  exports  :   function  (  source  ) {
return   source;
};
```


Real-world loaders will perform some kind of transformation on the input, of course. The loader we want to write expects the contents of a .twig file. We’ll start with splitting the input line by line:


```text
module  .  exports  :   function  (  content  ) {
var   lines  :   content  .  split  (/\  n  /);
// ...
};
```


Not very impressive, so far. Now, we could simply iterate over the` lines` array in a` for` loop, and apply various transformation and filtering calls in the loop body. However, since this use case is all about data transformation, it is a wonderful opportunity to use James Long’s[transducers.js library](https://github.com/jlongster/transducers.js) to declaratively define the transformation as a series of individual steps, instead of bothering with the boring details of iterating over data structures. In case you are new to transducers, their most important features are:


1. They separate a transformation from the underlying data structure so that they can be exchanged independently.
2. They avoid the construction of intermediate data structures when it comes to multi-step transformations. Instead, they apply the whole chain of transformations to the first element before moving on to the second element, and so forth. This reduces the need for memory allocation, and can result in[dramatic speedups](http://jlongster.com/Transducers.js-Round-2-with-Benchmarks) for large data sets.


James Long and Tom Ashworth offer excellent explanations on transducers[here](http://jlongster.com/Transducers.js--A-JavaScript-Library-for-Transformation-of-Data) and[here](http://phuu.net/2014/08/31/csp-and-transducers.html) . However, I think you will grasp what’s going on in the following code examples even without reading those posts.


## A transformation pipeline


Let’s take this step by step. We will start out by converting each line into a descriptive object.


```text
var   transducers  :   require  (  'transducers.js'  ),
lineToObject  :   transducers  .  map  (  function  (  line  ) {
var   match  ;
if   (line.match(/\{%\  s  *  javascripts  /)) {
return   {   start  :   true   };           // Mark the start of a new 'javascripts' block
}   else   if   (  match  :   line  .  match  (  /  ^  \s  *  '@  ?  (  .  *  )'  $  /  )) {
return   {   source  :   match  [  1  ] };      // Extract the source files to be loaded
}   else   if   (match: line.  match  (  /  ^  \s  *  output='(  .  *  )'  $  /  )) {
return   {   target  :   match  [  1  ] };      // Extract the target file
}
return   null  ;
});
```


The` lineToObject()` function is our first transformation. It outputs three kinds of objects:


- ` { start: true }` , if a line contains` {% javascripts` . Note that we might face several` {% javascripts` blocks (even if we are, in our case, only interested in one of them), so we need some kind of delimiter between them for subsequent processing. The` start` object is that delimiter.
- ` { source: '...'}` , if a line contains a file path to be included. We do not use wildcards in our file lists, so this approach is valid.
- ` { target: '...'}` , if a line contains` output='...'` and, therefore, a target file name.


All other lines are converted to` null` . The following picture illustrates this step:


Quite nice, but we don’t need the` null` values, so we define a second transducer to filter them out:


```text
var   filterNull  :   transducers  .  filter  (  function  (  line  ) {
return   line   !  ==   null  ;
});
```


This leaves us with a cleaned-up set of description objects, where each object is either of type “start”, “source”, or “target”. It looks something like this:


Next, we turn the file names in the “source” objects into actual` require` calls:


```text
var   generateRequireCode  :   transducers  .  map  (  function  (  line  ) {
if   (line.source) {    // Only act on the "source" type of object
line.source:   "require('"   +   line.source   +   "');"  ;
}
return   line  ;
});
```


This completes our transformation chain, which we stitch together using` compose` :


```text
var   transformLines  :   transducers  .  compose  (
lineToObject  ,
filterNull  ,
generateRequireCode
);
```


Again: Even though I described the transformations step by step, each line passes through all of the stages at once before the next line is even touched.


## From many to few


Our goal now is to reduce these many *line* description objects into few *block* description objects, each one representing a` javascripts` block in the source file. A JSON description of our desired structure looks like this, where three block descriptors have been produced:


```text
[
// *** Array of block objects
{
// *** Block 1
imports: [
"require('CommonBundle/Resources/public/js/compatibility.js');"  ,
"require('CommonBundle/Resources/public/js/trivagoTranslation.js');"  ,


// Long list of others...


"require('PriceSearchBundle/Resources/assets/js/module/Gui/Tooltip.js');"  ,
"require('PriceSearchBundle/Resources/assets/js/module/Survey/Survey.js');"  ,
],
target:   'js/js-main.js'  ,
},
{
// *** Block 2
imports: [
"require('PriceSearchBundle/Resources/assets/js/member/init.js');"  ,
"require('PriceSearchBundle/Resources/assets/js/member/model.js');"  ,
],
target:   'js/someOtherFile.js'  ,
},
{
// *** Block 3
imports: [
"require('CommonBundle/Resources/public/js/someAdditionalFeature.js');"  ,
],
target:   'js/andAThirdFile.js'  ,
},
];
```


The logic to achieve this is relatively simple:


1. Start with an empty list of blocks.
2. If you encounter a “start” object, create a new block object, with an empty list of imports.
3. If you encounter a “source” object, add its file path to the` imports` list of the newest block.
4. If you encounter a “target” object, copy its file name to the` target` property of the newest block.


We place the logic for this transformation in a reducer object, which has an` init` , a` step` , and a` result` function:


```text
var   lineReducer  :   {
// Start out with an empty list of block descriptors
init  :   function  () {
return   [];
},


// "step" is executed for each line object
step  :   function  (  result  ,   val  ) {
if   (val.start) {      // New javascripts block
result.  push  ({     // Add new block object to the results list
imports: []
});
}   else   if   (val.source) {
// Add source file to the newest block descriptor
result[result.  length   -   1  ].imports.  push  (val.source);
}   else   if   (val.target) {
// Set target file for the newest block descriptor
result[result.  length   -   1  ].target: val.target;
}
return   result;
},


// We use "result" to get only the one block we are interested in
result  :   function  (  v  ) {
return   v.  filter  (  function  (  x  ) {
return   x.target:  ==   'js/js-main.js'  ;
})[  0  ];
}
};
```


` init` initializes data structures,` step` processes each intermediate object, and` result` filters out unwanted block descriptor objects. In our case, we are only interested in the block with the target` "js/js-main.js"` .


## Putting it all together


We can now define the whole main function of the webpack loader:


```text
module  .  exports  :   function  (  content  ) {
var   lines  :   content  .  split  (/\  n  /),
blockDescriptor  :   transducers  .  transduce  (
lines  ,            // input data
transformLines  ,   // transformation chain
lineReducer  ,      // reducing step
[]                // initial, empty result data structure
);


return   blockDescriptor.imports.  join  (  '  \n  '  );
};
```


So was it worth bringing in the overhead of` transducers.js` to accomplish this relatively simple task, instead of implementing the whole logic inside a` for` loop? We think yes, because of one of the core selling points of transducers: *composability* . Remember how neatly the library allowed us to specify the transformation pipeline:


```text
var   transformLines  :   transducers  .  compose  (
lineToObject  ,
filterNull  ,
generateRequireCode
);
```


Not only does this code provide a very convenient high-level overview of what’s going on, but it is also super easy to add more filtering or transformation steps. Let’s assume there are certain .js files that we do not want to` require` (e.g.,[almond.js](https://github.com/jrburke/almond) , because it caused trouble with webpack). It is very straightforward to add the filter we need, and keep the code self-explanatory:


```text
var   removedDependencies  :   {
'CommonBundle/Resources/assets/js/almond.js'  :   true
},
filterUnwantedDependencies  :   transducers  .  filter  (  function  (  line  ) {
return   !  removedDependencies  [line.source];
});
```


The call to` compose` would then look like:


```text
var   transformLines  :   transducers  .  compose  (
lineToObject  ,
filterNull  ,
filterUnwantedDependencies  ,    // New filter
generateRequireCode
);
```


## Conclusion


Overall, the custom loader approach has worked very well for us, and it kept the introduction of webpack entirely transparent to most developers, until it was final and proven. Since no code was duplicated, there was no maintenance overhead, and no risk of missing a file in one or the the other deployment configuration.


The use of transducers.js was, at the minimum, a great demonstration of a flexible, multi-stage transformation of text data. Moreover, the transducer solution provides a lot of flexibility to modify the existing transformation pipeline, add stages, or augment existing ones.
