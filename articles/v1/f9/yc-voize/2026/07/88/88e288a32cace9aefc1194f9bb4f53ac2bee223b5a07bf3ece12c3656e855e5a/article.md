---
schema_version: "1.0.0"
document_id: "88e288a32cace9aefc1194f9bb4f53ac2bee223b5a07bf3ece12c3656e855e5a"
company_key: "yc-voize"
company: "voize"
source_id: "yc-voize-news-import-43f8d5e4a29a"
canonical_url: "https://tech.voize.de/posts/pytorch-lite-multiplatform"
published_at: null
first_seen_at: "2026-07-24T06:33:48.232845+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:1a357540f852dfd924d6fdab4dc394088417c4aaa6cfad5c6253b77fd633686c"
---

# PyTorch Lite Multiplatform

[go back](https://tech.voize.de/)


# PyTorch Lite Multiplatform


Write inference code once and run it on both Android and iOS.


### elevator pitch


voize develops speech recognition for healthcare professionals.


Nurses in Germany spent 30% of their time on administrative tasks instead of direct patient care.
With voize, nurses can just freely speak any medical report into our app and we generate the structured medical records. These are directly transferred into their existing healthcare record systems.


This way, voize allows profressionals to save time on administrative tasks and focus on patient wellbeing.


Erik Ziegler


Co-Founder, CTO


10 min read


[voize-gmbh/pytorch-lite-multiplatform](https://github.com/voize-gmbh/pytorch-lite-multiplatform)


## Motivation


At voize, we developed our machine learning models to run on-device to achieve a low latency experience for our users, keep data private and allow users to use voize everywhere in their facility even if they do not have full WiFi coverage.


We use PyTorch to train our models and deploy them for mobile using the[PyTorch Lite interpreter](https://pytorch.org/tutorials/recipes/mobile_interpreter.html) also called *PyTorch Mobile interpreter* . First, a PyTorch module is compiled using the TorchScript compiler producing a representation that can be run with the PyTorch C++ API known as *LibTorch* . Next, the TorchScript module is[optimized for mobile deployment](https://github.com/pytorch/pytorch/blob/f87fbfdb014b464edc573d9a133a07f266b1e097/torch/utils/mobile_optimizer.py#L14) which pre-packs model weights and fuses Linear+ReLU operations.


The final artifact can be consumed and run on-device by the[iOS and Android libraries](https://pytorch.org/mobile/home/) provided by PyTorch which both leverage LibTorch under the hood. If you want to target both platforms you will have to write the inference code two times: once in Swift using the` LibTorch-Lite` pod and once in Kotlin using` org.pytorch:pytorch_android_lite` Maven dependency.


Although writing the inference code on both platforms is relatively straight-forward, maintaining the implementations can be challenging, especially if you regularly update input/output of the model or parts of the post-processing. Even more so if you have multiple models to maintain. You can try to put most of the pre- and post-processing into the TorchScript module, but will still have to write the platform-specific code to handle the input/output of the model.


Because voize runs a whole suite of these types of models on-device we wrote a library that allows us to implement model inferences in Kotlin Multiplatform code once and run it on both Android and iOS: **PyTorch Lite Multiplatform** . We are using this library in production for a long time now, which let us scale to more on-device models and faster iteration speed on model architectures and inference APIs.


Before we dive into the library, let us revisit what Kotlin Multiplatform is.


## Kotlin Multiplatform


[Kotlin Multiplatform](https://www.jetbrains.com/kotlin-multiplatform/) (KMP) allows you to reuse Kotlin code across multiple platforms. This works by compiling Kotlin code to run directly on the JVM, iOS, JavaScript, and native binaries. If you have Kotlin code targeting JVM or Android you can reconfigure your project to compile **the same code** to run on iOS **natively** (not in a JVM). KMP makes it easy to abstract platform-specific implementations into a common Kotlin API that can be shared across platforms.


We use KMP heavily to share business logic between iOS and Android in our products so by creating a library that combines the platform-specific PyTorch Lite inference code into a Kotlin API we would gain a lot of productivity.


## PyTorch Lite Multiplatform


PyTorch Lite Multiplatform is open-source on GitHub and can be used today in your Kotlin Multiplatform project, see the[PyTorch Multiplatform GitHub repository](https://github.com/voize-gmbh/pytorch-lite-multiplatform) . Check out the README for setup instructions. The repository also contains an example project.


### Example


Let's say you want to create a sentiment classifier in PyTorch and run it on-device.


1. **Prepare the model**
We start with our PyTorch model, which could be a simple BERT model with a classification head.


```text
import   torch
import   torch.nn  as   nn
from   transformers  import   BertTokenizer


class    SentimentClassifier  (nn.Module):
def    __init__  (
self,
encoder: nn.Module,  # BERT encoder
hidden_size:  int  ,
tokenizer: BertTokenizer,
):
super  ().__init__()
self.tokenizer = tokenizer
self.encoder = encoder
self.classifier = nn.Linear(hidden_size,  1  )
self.sigmoid = nn.Sigmoid()


def    preprocess  ( self, text:  str   ) -> torch.Tensor:
return   tokenizer.encode(text, return_tensors= "pt"  )


def    forward  ( self, input_ids: torch.Tensor  ) -> torch.Tensor:
_, x = self.encoder(input_ids)  # gets pooled encoder output
x = self.dropout(x)
x = self.classifier(x)
x = self.sigmoid(x)
return   x


```


1. **Export the model**
We export the model for PyTorch Lite inference.


```text
model = SentimentClassifier(...)
model. eval  ()
scripted_module = torch.jit.script(model)
optimized_scripted_module = optimize_for_mobile(scripted_module)
optimized_scripted_module._save_for_lite_interpreter( "model.script.ptl"  )


```


1. **Get the model file on-device**
We distribute the exported model file to the device, e.g. by bundling it into the app.
2. **Implement mobile inference**
Before PyTorch Lite Multiplatform, you would now write an inference implementation for each platform separately. But with our library, we can now directly interact with the model from Kotlin Multiplatform code.


First, we load the PyTorch module into memory:


```text
import   de.voize.pytorch_lite_multiplatform.TorchModule


val   sentimentClassifier = TorchModule(path =  "<path/to/model.ptl>"  )


```


Then, we can prepare our input data by calling the module's` preprocess` method:


```text
val   text =  "this product is great!"
val   input = sentimentClassifier.runMethod( "preprocess"  , IValue.from(text))


```


And finally, we run the model prediction


```text
val   output = sentimentClassifier.forward(input)
val   outputTensor = output.toTensor()


// get the output scalar from the tensor
val   prediction = outputTensor.getDataAsFloatArray().toList().single()


```


And that's it! With just a few lines of code, we get a model inference that runs on iOS and Android.


The library supports all available input and output types with arbitrary nesting like tensors, primitives, lists and dicts.


## How we did it


Writing a multiplatform wrapper library regardless of the framework usually follows the same approach: use the existing native libraries for both platforms if available, come up with a unifying API (hopefully the native libraries already have very similar APIs), implement the unified API in the framework you are targeting and internally call the native libraries.


### Concepts


PyTorch Lite has 3 concepts:


- ` Module` - Represents the exported PyTorch model, it can be loaded from a file, you can call` forward` and other exported methods.
- ` IValue` - A container for any data type, like` int` ,` float` ,` Tensor` ,` List` ,` Dict` , etc.
- ` Tensor` - PyTorch tensor.


The final Kotlin API reflects these concepts.


### Wrapping the native libraries


The Android part is fairly easy because you can directly call the Java/Kotlin API provided by the PyTorch Lite Android library from the Kotlin wrapper code.


iOS is trickier. iOS uses the` LibTorch` C++ library which is distributed for iOS via a CocoaPod. Kotlin Native has excellent[interoperability with C and Objective-C](https://kotlinlang.org/docs/native-c-interop.html) but binding C++ libraries is[not supported and not part of the roadmap](https://discuss.kotlinlang.org/t/kotlin-interop-with-c/7121/4) . Binding` LibTorch` directly would probably be difficult anyway because of its large API surface area.


Instead, we wrap the LibTorch C++ API into an Objective-C API that we call *PLMLibTorchWrapper* which only exposes the required concepts. Since interop with Objective-C is supported in Kotlin Native we can call this API from our Kotlin code. We also need to republish this project as a CocoaPod so apps using PyTorch Lite Multiplatform can include it since the Kotlin Multiplatform library does not embed the Objective-C source.


With both platform implementations ready to be called from Kotlin we can implement the unified API using Kotlin Multiplatform's[expect / actual](https://kotlinlang.org/docs/multiplatform-expect-actual.html#rules-for-expected-and-actual-declarations) concept.


### Tackling Memory Management issues


On iOS, when creating a` Tensor` from data in Kotlin, internally the data has to be copied from Kotlin into a native data structure using for example` allocArray` :


```text
// iosMain/.../Tensor.kt


actual    class    Tensor   {
actual    companion    object   {
actual    fun    fromBlob  (
data  :  IntArray  ,
shape:  LongArray  ,
)   : Tensor = memScoped {
val   nativeTensor = NativeTensor(
intData = allocArray( data  .size) { value =  data  [it] },
shape = allocArray(shape.size) { value = shape[it] },
shapeLength = shape.size.toULong()
)
Tensor(nativeTensor)
}
}
}


```


With this implementation, the allocated native memory is disposed at the end of the` memScoped` block. So when using this API the tensor data might be lost before the tensor is used in the model:


```text
val   tensor = Tensor.fromBlob( data   = intArrayOf( 1  ,  2  ,  3  ), shape = longArrayOf( 3  ))
val   output = model.forward(tensor)  // tensor data might be lost here


```


To mitigate this we provide a` plmScoped` block that lets the user control the lifetime of the native memory:


```text
// iosMain/.../Tensor.kt


actual    class    Tensor   {
actual    companion    object   {
actual    fun    fromBlob  (
data  :  IntArray  ,
shape:  LongArray  ,
scope:  PLMScope  ,
)   : Tensor = with(scope.nativePlacement) {
val   nativeTensor = NativeTensor(
intData = allocArray( data  .size) { value =  data  [it] },
shape = allocArray(shape.size) { value = shape[it] },
shapeLength = shape.size.toULong()
)
Tensor(nativeTensor)
}
}
}


```


```text
val   output = plmScoped {
val   tensor = Tensor.fromBlob(
data   = intArrayOf( 1  ,  2  ,  3  ),
shape = longArrayOf( 3  ),
scope =  this  ,
)
model.forward(tensor)
}  // input tensor data is disposed here


```


On Android,` plmScoped` is a noop because the memory management is handled by the JVM's garbage collector.


## What’s next


As of 2024,[PyTorch Lite is not actively developed by PyTorch anymore](https://github.com/pytorch/pytorch/issues/121597#issuecomment-1988594228) and efforts for inference on mobile devices shifted to[ExecuTorch](https://pytorch.org/executorch-overview) which builds on top of` torch.compile` and` torch.export` .


We are looking forward to do the same thing for ExecuTorch we did for PyTorch Lite, providing a convenient Kotlin Multiplatform wrapper to allow Kotlin developers to easily leverage the latest on-device inference solutions from PyTorch. We will keep you updated!


If you yourself are passionate about PyTorch mobile inference, consider contributing to our effort for supporting **ExecuTorch** in Kotlin Multiplatform! You can apply at voize to come on board and work full time on engineering like this!


# is hiring!


We are hiring Kotlin engineers to join our team in Berlin fulltime and to work one of the most impactful AI products in the German healthcare sector.


[Job Posting Kotlin Engineer](https://join.com/companies/voize/11313345-software-engineer-kotlin-m-w-d-fulltime)
