---
schema_version: "1.0.0"
document_id: "489e8f3681f2c06ca182b565a8d3370fd78c147f5ac08957fd33f92f1120af30"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/python-workers-rpc/"
published_at: "2026-08-03T13:00:00+00:00"
first_seen_at: "2026-08-03T13:28:47.669131+00:00"
fetched_at: "2026-08-03T14:10:52.670502+00:00"
content_hash: "sha256:06a226f5fc02ce19a478947b048f5adb27170f4789771ae16047e9a002710553"
---

# Workers RPC now works across Python and JavaScript

Two years ago, we introduced[Workers RPC](https://blog.cloudflare.com/javascript-native-rpc/) , built on[Cap’n Proto RPC](https://capnproto.org/) . This made it possible for Workers to call other Workers and Durable Objects’ methods, return live objects and call their methods, return functions, streams and get all the benefits of a Remote Procedure Call (RPC) system, without defining schemas or adding any dependencies. We called it “JavaScript-native RPC” because it made using RPC feel native to the language.


Last year, we made this work between web browsers and servers, and[introduced Cap’n Web](https://blog.cloudflare.com/capnweb-javascript-rpc-library/) .


Now we’re taking it cross-language.


Normally, getting programs written in different languages to talk to each other is complicated: developers usually have to build custom APIs or adopt language-agnostic serialization formats like[protobuf](https://protobuf.dev/) , so the two systems can understand each other. The RPC system built into Workers is able to translate across JavaScript and Python without any additional work.


You can now call methods defined in a Python Worker from a JavaScript Worker and vice versa. You can share objects across Python and JavaScript, and call methods on a Python object from TypeScript. It all just works.


If you define a method` add()` in a Worker written in TypeScript:


```text
import   { WorkerEntrypoint }   from   "cloudflare:workers"  ;


export   class   RpcService   extends   WorkerEntrypoint   {
async   add  (  a  :   number  ,   b  :   number  )  :   Promise  <  number  > {
return   a   +   b;
}
}
```


…you can simply call it from Python:


```text
from   workers   import   Response, WorkerEntrypoint


class   Default  (  WorkerEntrypoint  ):
async   def   fetch  (self, request):
# Get the RPC stub from the TypeScript Worker.
rpc   =   self  .env.  RPC


# Call the TypeScript RPC method.
result   =   await   rpc.add(  42  ,   144  )


return   Response.json({  "result"  : result})
```


There are no dependencies needed. All you need to configure is a[Service binding:](https://developers.cloudflare.com/workers/runtime-apis/bindings/service-bindings/)


```text
"services"  : [
{
"binding"  :   "RPC"  ,
"service"  :   "ts-rpc-server"  ,
"entrypoint"  :   "RpcService"
}
]
```


## So, what can you do with it?


This RPC system allows you to build a complex multi-language system as if you are using a library. Here are some features of cross-language RPC.


- Cross-language RPC calls behave like ordinary function calls that return promises in JavaScript/TypeScript and futures in Python. Exceptions are propagated and are thrown at the call site of the RPC method.
- You can pass any[Structured Cloneable types](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm#supported_types) as the parameters or a return value of an RPC call. These get converted to the appropriate types in Python: for example, a JS Date is converted to a Python datetime
- You can pass JavaScript functions to a Python Worker and return them, and vice versa. When the other side calls the function passed to it, they make a new RPC back for you.
- Typically, RPC to another Worker does not cross a network. The other Worker usually runs in the same thread as the caller. There is near-zero performance overhead compared to running code in the same Worker.
- The implementation is fully open source as part of[workerd](https://github.com/cloudflare/workerd/) and[workers-runtime-sdk](https://github.com/cloudflare/workers-py/blob/4bb01fe72a9af71918746e3851c526335302a3bc/packages/runtime-sdk/src/workers/rpc.py) .


### But wait, how do you convert types across languages?


The main hurdle for making RPC seamless across the JavaScript and Python Workers is bridging their distinct type systems. JavaScript developers expect to work with native JavaScript types, and Python developers expect the same for Python. Bridging two distinct languages with their own type systems required a careful, deliberate type conversion strategy.


Consider how each language handles function arguments. A typical way to define a complex function in JavaScript is passing an` Object` as an argument:


```text
function   myFunction  (  params  :   {   key  :   string  ,   value  :   boolean  ,   optional  ?:   number   }) {   ...   }


// Called like this:
myFunction  ({ key: “myKey”, value:   true  , optional:   1   });
```


In contrast, a Python developer would typically define the same function using keyword arguments:


```text
def   my_complex_function  (key:   str  , value:   bool  , optional:   int   |   None  ):   ...


# Called like this:
my_complex_function(“myKey”,   True  ,   optional  =  1  )
```


Our goal was to make cross-language RPC completely transparent. Developers should feel like they are writing code for a single-language application without needing to worry about the underlying translation layer. We achieved this by combining Pyodide’s Foreign Function Interface (FFI) with a custom type-conversion layer for Python Workers.


### Pyodide FFI already translates between Python and JavaScript types


Pyodide is the CPython interpreter compiled to WebAssembly, and it has powered[Python Workers from the start](https://blog.cloudflare.com/python-workers/#pyodide-and-the-magic-of-foreign-function-interfaces-ffi) . It includes a robust FFI that automatically translates types between JavaScript and Python.


When a Python Worker communicates with a JavaScript Worker via Service bindings, Pyodide’s FFI transparently converts objects during the RPC call. Developers on either side don’t need to know which language the other Worker is written in, and everything is handled under the hood.


Pyodide maps native types between both environments out of the box:


**Python Type**


**JavaScript Equivalent**


int, float


Number


bool


Boolean


dict


Object


list


Array


When direct translation isn’t possible (such as with custom classes or functions), Pyodide creates a` Proxy` object. This proxy forwards attribute accesses and method calls across the boundary, enabling patterns like passing a Python function directly as a callback to JavaScript handlers.


Pyodide FFI also maps Python’s keyword arguments directly to JavaScript’s object-style parameters. For example, imagine a JavaScript Worker with a method that takes an optional options object:


```text
async   get  (key: string, options  ?:   { type: string });
```


When calling this JavaScript Worker from Python, you *could* pass a Python dictionary to represent the JavaScript object:


```text
JSRPC  .get(  "myKey"  , {   "type"  :   "text"   })
```


However, you can also use native Python keyword arguments:


```text
JSRPC  .get(  "myKey"  ,   type  =  "text"  )
```


Pyodide FFI translates both calls into the exact structure the JavaScript Worker expects, giving Python developers a clean, natural API experience.


To explore type translation in more detail, check out the[Pyodide documentation](https://pyodide.org/en/stable/usage/type-conversions.html) .


### Handling Cloudflare Workers objects


While Pyodide FFI seamlessly converts standard built-in types, it doesn’t automatically understand Web API objects such as` Request` ,` Response` ,` Blob` , or` File` . They are commonly used in Cloudflare Workers, but there is no direct built-in equivalent in Python.


As explained in the previous section, Pyodide, by default, treats these non-standard objects as JavaScript Proxies. Rather than converting them into Python objects, it creates a passthrough proxy for attribute lookups and method calls. While functional, this approach leaks underlying JavaScript implementation details into Python. Python developers would have to constantly remember they are interacting with JavaScript proxies, adding unnecessary mental overhead.


To fix this, we introduced the[workers-runtime-sdk Python package](https://github.com/cloudflare/workers-py/blob/4bb01fe72a9af71918746e3851c526335302a3bc/packages/runtime-sdk/src/workers/rpc.py) . This acts as a thin conversion layer built specifically to handle custom Workers types over RPC. When you deploy a Python Worker using[uv run pywrangler deploy](https://developers.cloudflare.com/workers/languages/python/#the-pywrangler-cli-tool) , this package is included by default. In fact, if you import from the` workers` namespace, you’re already using it:


```text
from   workers   import   Response
...
```


Behind the scenes, this SDK wraps the RPC stubs provided by the bindings. It intercepts objects crossing the language boundary and translates them into native forms that both JavaScript and Python Workers can work with naturally.


As a result, Python developers can work with familiar, idiomatic Python objects, making cross-language execution feel completely invisible.


## Use Python packages from your JavaScript Worker


Have you ever wanted to use a great Python package, but your app is written in JavaScript? You can do this with Python Workers. Let’s look at an example.


Pygments is a popular syntax highlighting package, written in Python. To use it from JavaScript, you just need to expose a method from a Python Worker that calls the Pygments package.


We can call this method in our JavaScript by accessing the request’s` env` :


```text
export   default   {
async   fetch  (  request  ,   env  ) {
// Get the RPC stub from the Python Worker.
const   rpc   =   env.  PYTHON_RPC  ;


// Call the Python RPC method.
const   result   =   await   rpc.  highlight_code  (  'print(42)'  ,   'python'  );


return   Response.  json  (result);
}
}
```


Now on the Python side, we define a Python Worker with this method like so:


```text
from   workers   import   WorkerEntrypoint


class   Default  (  WorkerEntrypoint  ):
async   def   highlight_code  (self, code:   str  , language:   str  ) ->   dict  :
# Implementation goes here
```


Now all that’s left is to write the necessary code to do the highlighting in Python. A simplified version of this looks like so:


```text
# ...
from   pygments.formatters   import   HtmlFormatter
from   pygments   import   highlight
from   pygments.lexers   import   get_lexer_by_name


class   Default  (  WorkerEntrypoint  ):
# Retrieve the lexer for the language specified.
lexer   =   get_lexer_by_name(language,   stripall  =  True  )


# Create the formatter and run the highlighter on the specified code.
formatter   =   HtmlFormatter(  linenos  =  True  ,   cssclass  =  "highlight"  ,   style  =  "monokai"  )
highlighted_html   =   highlight(code, lexer, formatter)


# Get the CSS for styling.
css   =   formatter.get_style_defs(  ".highlight"  )


return   {
"html"  : highlighted_html,
"css"  : css
}
```


The JavaScript lives in its own Worker that is separate from the Python Worker. So you also need to define the Service bindings to ensure they can communicate. You can do so by putting this in the JavaScript Worker’s wrangler.jsonc file:


```text
"services"  : [
{
"binding"  :   "PYTHON_RPC"  ,
"service"  :   "py-rpc-server"
}
]
```


The name of the service needs to match the name of your Python Worker here.


To test these, you can run` npx wrangler dev` in the JavaScript Worker’s directory and` uv run pywrangler dev` in the Python Worker’s directory in two separate terminals.


A full example is[available on GitHub](https://github.com/cloudflare/python-workers-examples/tree/main/13-js-api-pygments) . You can run it directly by using the following commands:


```text
git clone git  @  github  .  com  :  cloudflare  /  python  -  workers  -  examples  .  git
cd python  -  workers  -  examples  /  13  -  js  -  api  -  pygments  /


#   Terminal   1
cd ts  /
npx wrangler dev


#   Terminal   2
cd py  /
uv run pywrangler dev
```


## Try it now


In addition to those above, there are far more examples and information about RPC in[our documentation](https://developers.cloudflare.com/workers/runtime-apis/rpc/) .
