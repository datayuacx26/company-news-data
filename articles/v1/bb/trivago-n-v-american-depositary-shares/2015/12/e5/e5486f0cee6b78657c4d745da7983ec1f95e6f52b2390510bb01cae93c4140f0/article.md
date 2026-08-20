---
schema_version: "1.0.0"
document_id: "e5486f0cee6b78657c4d745da7983ec1f95e6f52b2390510bb01cae93c4140f0"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-12-15-parallel-webpack/"
published_at: "2015-12-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.834035+00:00"
content_hash: "sha256:dc421b8bb197443261964b693b257efa494de1c3a52e73dc13c988394a6d1035"
---

# Speeding up webpack performance with parallel builds

### How we reduced our build time from 16 to 2 minutes


When using webpack to build your assets, it’s only a matter of time until you wish for targeted builds. Whether it’s the output of the library you’re working on (CJS, UMD, AMD, Var, etc.), or the specific feature set (IE8 support, no IE8 support).` parallel-webpack` can run those builds in parallel, thus making full use of the multi-core processing capabilities of modern devices.


Ever since we introduced webpack in December of last year, we have been using it to create targeted builds for our applications and libraries. More specifically, we generate separate versions of our assets for users of IE8 and modern browsers as well as variants for right-to-left and left-to-right languages amongst other criteria.


We’ve also been using targeted builds for our internal JavaScript framework so that we can deliver it in different module formats, such as a global variable, CommonJS,[UMD](https://github.com/umdjs/umd) or[AMD](http://requirejs.org/docs/whyamd.html) packages. This way, the framework can readily be integrated with almost any existing application, regardless of the module structure it employs.


There was one problem though: Creating 32 variants of a build took about 16 minutes - way too long for us.


We started searching for a solution, but didn’t find anything that worked for us. Thus, we created our own solution:[parallel-webpack](http://github.com/trivago/parallel-webpack) .


By using[parallel-webpack](http://github.com/trivago/parallel-webpack) we were able to reduce our build performance down to **2 minutes** - a massive improvement that has made our lives a lot easier. Naturally, the improved speed comes at the price of a higher CPU usage. By default,` parallel-webpack` will use all available CPU cores to build your assets as fast as possible.


## Usage


The Installation of` parallel-webpack` is as simple as installing any other npm module:


```text
npm   install   parallel-webpack   --save-dev
```


` parallel-webpack` works with any standard` webpack.config.js` and will automatically run all provided configurations in parallel.


```text
module  .  exports  : [{
entry:   'pageA.js'  ,
output: {
path:   './dist'  ,
filename:   'pageA.bundle.js'
}
}, {
entry:   'pageB.js'  ,
output: {
path:   './dist'  ,
filename:   'pageB.bundle.js'
}
}];
```


Once you run` parallel-webpack` for this, you’ll notice that it builds` pageA.bundle.js` at the same time as` pageB.bundle.js` .


Often, you’ll just want to build variations of a standard configuration, and` parallel-webpack` can support you with that as well. All you have to do is call the` createVariants` function.


```text
// Import the createVariants helper.
var   createVariants  :   require  (  'parallel-webpack'  ).  createVariants  ;


// Those options will be the same for every variant.
var   baseOptions  :   {
devtool  :   'eval'
};


// This object defines the potential option variants
// the key of the object represents the option name, its value must be an array
// which contains all potential values of your build.
var   variants  :   {
minified  :   [  true  ,   false  ],
debug  :   [  true  ,   false  ],
target  :   [  'commonjs2'  ,   'var'  ,   'umd'  ,   'amd'  ]
};


// This function transforms the given options variant into the actual webpack configuration.
function   createConfig  (  options  ) {
var   plugins  :   [
new   webpack  .  optimize  .  DedupePlugin  (),
new   webpack  .  optimize  .  OccurenceOrderPlugin  (),
new   webpack  .  DefinePlugin  ({
DEBUG  :   JSON  .  stringify  (  JSON  .  parse  (  options  .  debug  ))
})
];
if  (options.minified) {
plugins.  push  (  new   webpack.optimize.  UglifyJsPlugin  ({
sourceMap:   false  ,
compress: {
warnings:   false
}
}));
}
return   {
entry:   './index.js'  ,
output: {
path:   './dist/'  ,
filename:   'MyLib.'   +
options.target   +
(options.minified   ?   '.min'   :   ''  )   +
(options.debug   ?   '.debug'   :   ''  )
+   '.js'  ,
libraryTarget: options.target
},
plugins: plugins
};
}


// Export the final build variants.
module  .  exports  :   createVariants  (baseOptions, variants, createConfig);
```


The above configuration will create 16 variants of your build configuration and` parallel-webpack` will build them in parallel, producing an output like this for you:


```text
[WEBPACK] Building 16 targets in parallel
[WEBPACK] Started building MyLib.umd.js
[WEBPACK] Started building MyLib.umd.min.js
[WEBPACK] Started building MyLib.umd.debug.js
[WEBPACK] Started building MyLib.umd.min.debug.js


[WEBPACK] Started building MyLib.amd.js
[WEBPACK] Started building MyLib.amd.min.js
[WEBPACK] Started building MyLib.amd.debug.js
[WEBPACK] Started building MyLib.amd.min.debug.js


[WEBPACK] Started building MyLib.commonjs2.js
[WEBPACK] Started building MyLib.commonjs2.min.js
[WEBPACK] Started building MyLib.commonjs2.debug.js
[WEBPACK] Started building MyLib.commonjs2.min.debug.js


[WEBPACK] Started building MyLib.var.js
[WEBPACK] Started building MyLib.var.min.js
[WEBPACK] Started building MyLib.var.debug.js
[WEBPACK] Started building MyLib.var.min.debug.js
```


` parallel-webpack` also supports watching and allows you to configure how often it should retry a build that failed before giving up. You can find the source code at[GitHub](http://github.com/trivago/parallel-webpack) and, as with any of our open source projects, we’d be more than happy to read up on your feedback.
