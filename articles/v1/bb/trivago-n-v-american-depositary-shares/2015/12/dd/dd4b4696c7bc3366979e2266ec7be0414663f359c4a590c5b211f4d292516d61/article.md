---
schema_version: "1.0.0"
document_id: "dd4b4696c7bc3366979e2266ec7be0414663f359c4a590c5b211f4d292516d61"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-12-17-export-multiple-javascript-module-formats/"
published_at: "2015-12-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.834035+00:00"
content_hash: "sha256:08ef19dcfa80d57aabe5d92f49f448830f45a28458351cc936a977d5518f228f"
---

# How to export a JavaScript module to multiple formats

When publishing a JavaScript library, we usually want to make it available to as many people as possible to maximize the library’s usefulness and adoption. In that respect, it can be helpful to users to have the library available in their preferred module format - CommonJS, AMD, ES6, etc. This article shows how to use webpack to automatically export multiple formats without having to maintain them separately.


Maybe you know the situation: You would like to pull a helpful JavaScript library into your client-side project, but it consists entirely of CommonJS modules, and you cannot or do not want to add an additional translation step into your build process to make it browser-compatible. Depending on the project, it would be more convenient to access the library through a single global variable. Or maybe this project is using RequireJS, so we would like to pull in an AMD module.


If we switch the perspective now, and think as library *authors* , how can we provide several ways of using our library, without causing additional maintenance overhead? At trivago, we use[webpack](http://webpack.github.io/) to help us with that, because it has this functionality basically built in - you just have to configure it accordingly.


## webpack configuration


The key to exporting our library in multiple formats is the *libraryTarget* option in the *output* configuration section. The following code example configures webpack to output a “global variable” version of our library:


```text
var   config  :   {
entry  :   './index.js'  ,
output  :   {
path  :   './dist/'  ,
filename  :   'ourLibrary.var.js'  ,
library  :   'OurLibrary'
libraryTarget  :   'var'
},
// ...
```


The *path* and *filename* options determine where to generate the file. We use the *library* option to specify a variable name for the library in the export. In the example, the name is set to “OurLibrary”, which will result in output code like:


```text
var   OurLibrary  :   ...
```


Finally, the *libraryTarget* option specifies the type of export. Possible options are:


- ` var` : The module is exported by setting a variable, as just mentioned. This is the default setting.
- ` this` : The module is exported by setting` this\["OurLibrary"\]: ...` .
- ` commonjs` : Export by setting` exports\["OurLibrary"\]: ...`
- ` commonjs2` : Export by setting` module.exports: ...`
- ` amd` : Export as an[AMD](http://requirejs.org/docs/whyamd.html) module.
- ` umd` : Export as a[UMD](https://github.com/umdjs/umd) module.


See the[webpack documentation](https://webpack.github.io/docs/configuration.html#output-librarytarget) for more information.


## Varying the build configuration


So let us say we want to export our library as a global variable (` var` ), as a CommonJS module (` commonjs2` ), and as AMD and UMD modules (` amd` and` umd` , respectively). We prefer` commonjs2` to` commonjs` since its output is more practical to include. Since we have multiple build targets, we will need multiple webpack compiler runs ([example](https://github.com/webpack/webpack/tree/master/examples/multi-compiler) from the webpack documentation). So, instead of exporting a single webpack configuration, we export an array of configurations:


```text
module   exports  : [
createConfig  (  'var'  ),
createConfig  (  'commonjs2'  ),
createConfig  (  'amd'  ),
createConfig  (  'umd'  )
];
```


The` createConfig` function might look something like the following:


```text
function   createConfig  (  target  ) {
return   {
entry:   './index.js'  ,
output: {
path:   './dist/'  ,
filename:   'ourLibrary.'   +   target   +   '.js'  ,
library:   'OurLibrary'  ,
libraryTarget: target,
},
// ...
};
}
```


The only difference to the first code snippet in the article (where we hard-coded` var` as the` libraryTarget` ) is that we now pass a variable to webpack’s` libraryTarget` option, and also use this variable to generate the target filename. We will get 4 output files from this configuration:


And that’s basically it. A developer using our library can now choose from multiple formats. If she wants our library as a global variable, she can pull the` var` file in:


```text
<!-- <script src="assets/js/ourLibrary.var.js"></script> -->
```


Somebody using CommonJS can access our code using` require` , provided that the environment is set up correctly:


```text
var   ourLib  :   require  (  'ourLibrary'  );
```


## Build time considerations


Now, some of you might say: “Ok, so I’m running webpack 4 times instead of just once, so I end up with a quadrupled build time, more or less. That’s not good.” And you’re right, the build time will increase. But: We can work around that by using[parallel-webpack](https://www.npmjs.com/package/parallel-webpack) , which can distribute the compilation work to multiple processor cores.


First, we install parallel-webpack. On the command line, go to your project directory, and type:


```text
npm   install   parallel-webpack   --save-dev
```


Instead of creating the array to be exported manually, we will use the` createVariants` function of parallel-webpack:


```text
var   createVariants  :   require  (  'parallel-webpack'  ).  createVariants  ;


// ...


// At the end of the file:
module  .  exports  :   createVariants  ({
target: [  'var'  ,   'commonjs2'  ,   'umd'  ,   'amd'  ]
}, createConfig);
```


The` createVariants` function receives an object containing the options that vary from build to build, along with all the possible values. In our simple case, we have only one option,` target` , and its values are the four export formats.


The second parameter to` createVariants` is a callback function (` createConfig` ) which will be called once for every generated parameter combination. At every invocation, the parameter combination is passed to` createConfig` as an` options` object. As our existing` createConfig` function is expecting the target format as a single string parameter, we have to adapt it slightly:


```text
function   createConfig  (  options  ) {
return   {
entry:   './index.js'  ,
output: {
path:   './dist/'  ,
filename:   'ourLibrary.'   +   options.target   +   '.js'  ,
library:   'OurLibrary'  ,
libraryTarget: options.target,
},
// ...
};
}
```


That is already all. If we now run` webpack` to build our library, the individual builds will be distributed to separate CPU cores (if available) to make use of parallelism. For more information on parallel-webpack usage, see also[out related tech blog article](https://tech.trivago.com/2015/12/15/parallel-webpack/) .


## Conclusion


The number of relevant JavaScript module formats might decrease in future, with more projects ultimately switching to ES6 modules. However, for the time being, we can still increase the number of users of our JavaScript library by providing not one, but several final formats in which it is offered for consumption. webpack has pre-defined functionality that lets us do exactly that very easily, and that shows[once more](https://tech.trivago.com/2015/05/28/introduce-webpack/) its flexibility and usefulness.
