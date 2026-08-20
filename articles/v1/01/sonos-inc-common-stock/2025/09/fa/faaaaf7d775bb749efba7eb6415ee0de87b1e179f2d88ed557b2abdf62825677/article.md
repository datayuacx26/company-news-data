---
schema_version: "1.0.0"
document_id: "faaaaf7d775bb749efba7eb6415ee0de87b1e179f2d88ed557b2abdf62825677"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/torch-2-nnef-open-sourcing/"
published_at: "2025-09-25T14:46:05+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:c77d344f8d35807f4db1603870e01875c38aff92a537c1fde4f3249781e2b121"
---

# Shipping neural networks with Torch to NNEF

We’re excited to announce the open-sourcing of[torch_to_nnef](https://sonos.github.io/torch-to-nnef/latest/) , a tightly integrated toolchain that enables seamless export of PyTorch models to the **NNEF** format for use with[tract](https://github.com/sonos/tract) , our Rust-based neural inference engine. This post explores why model exchange formats matter, what makes NNEF unique, and why we built torch_to_nnef to power on-device machine learning at Sonos.


## Why convert a neural network's format ?


Developing a machine learning–powered product generally involves two distinct stages:


Figure 1. illustration of the 2 stages to build a neural network and their antagonistic goals


### Training and Evaluation


Performed in a research environment, this stage emphasizes experimentation and iteration to best solve a user task within computational and data constraints. The training is most commonly supervised, that is based on a dataset of input and output pairs. It is composed of the following main steps: 1. The neural network processes the inputs and provides the predicted outputs in a step known as the forward pass. 2. A loss score is computed based on the distance between expected outputs and the predictions. 3. A correction is applied to the network backwards (from outputs to inputs) based on the error value; this step is known as backpropagation and is guided by an optimizer. The process is repeated many times during training. Evaluation happens during and after training to evaluate models with the ‘fixed’ parameters just learned.


### Production


Once a model meets quality criteria it is frozen and shipped to production, where it is optimized for efficient, reliable inference. This ‘frozen’ state is obtained by keeping only the forward pass of the neural network. Also this transformation replaces the set of operators applied to the tensors, so that they are hardware agnostic and decomposed into a primitive set (often called an operator set), more easily reinterpretable by other engines. The ‘inference engine’ running this frozen model often has far less dependencies, is more stable, and puts more emphasis on computational-efficiency for the targeted hardware.


These two distinct environments have inherently different goals (see Figure 1), which naturally creates the need for a **bridge** between them. That bridge is typically an **intermediate representation (IR)** , or an **exchange format.** Popular examples include[ONNX](https://onnx.ai/) and[PyTorch IR](https://docs.pytorch.org/docs/stable/export.ir_spec.html) .


### What composes a neural network model asset ?


A **neural network asset** is the packaged output of training that is designed for efficient inference. It typically contains two essential components:


-


A list of **tensors** with names and values (the parameters learned during the training process).


-


**A computation graph** stitching the tensors together: inputs, outputs, data types, tensor shapes, and the sequence of operators.


## What is NNEF ?


Many machine learning practitioners will be familiar with ONNX as a neural network model exchange format. NNEF is also an exchange format, but while ONNX focuses on trainable networks, NNEF is designed to address the needs of neural networks at a different stage of their lifetime, inference instead of training.


The Neural Network Exchange Format (NNEF), developed by the Khronos Group, is an open standard designed for interoperability across frameworks, tools, and hardware platforms. It provides a framework-agnostic way to represent trained neural networks, enabling models to be:


-


**Shared** across environments.


-


**Optimized** for performance.


-


**Deployed** across diverse hardware without loss of fidelity.


Interested readers can dive into the specifications of this format[here](https://registry.khronos.org/NNEF/specs/1.0/nnef-1.0.5.html) .


## Why Torch to NNEF ?


At Sonos we build machine learning solutions from training to inference, serving millions of customers. We have been pushing hard to allow neural network computation to happen on devices. As part of this journey we develop an open-source neural network inference engine written in Rust:[tract](https://github.com/sonos/tract) .


[NNEF](https://registry.khronos.org/NNEF/specs/1.0/nnef-1.0.5.html) is the preferred format to store neural networks on disk in tract. This allows short model load time, good human readability, while being easily extensible and debuggable.


Our neural modeling teams investigate the use of compression techniques and in particular quantization. This ability to export quantized models is critical because of limited on-device resources and the advent of multi-billion parameter models.


To complement tract[ONNX](https://onnxruntime.ai/docs/performance/model-optimizations/quantization.html) support, we wanted something more tailored to our needs when shipping neural model assets to tract. Especially:


-


To specify quantized networks with 4 bits or less in their tensor data type


-


To unlock the ability to export advanced quantization/dequantization functions


-


To get a deeper integration when needed for specific models like ‘Large Language Models’.


Protocol Buffer, is used as a meta binary format by ONNX and for model storage by TensorFlow. Both use it to bundle the model graph and the tensor values together. While there is merit in this approach, that bundling makes it hard to easily extend or build upon.


In that regard NNEF sounds more attractive to us. Keeping the graph description in plain text allows for a human readable format accessible without intermediate tools. Modifying a text specification is easier and can even be done in an editor during prototyping or debug sessions. The NNEF graph can be seen as a Domain Specific Language (DSL) with control-flow limited to compilation time. The specification enables the definition of ‘fragments’ that can be seen as pure functions. Neural networks being mostly defined by repeating blocks of transformations, it makes a lot of sense to avoid repeating the same sequence of operations through composition. Sometimes it’s convenient to share tensors between multiple graphs; in NNEF, distinct graph files can be defined and share stored tensors references (tract propose such mechanism). Each tensor in the graph is stored in a distinct binary file, making it easy to manipulate and reference (opening possibilities for[PEFT](https://huggingface.co/docs/peft/en/index) export for example). The tensor format structure proposed shines in its flexibility to add new data types depending on the need.


In 2022, we started the development of "torch_to_NNEF" to support the use of these desired features. We are excited to announce that we are open-sourcing this Python library, enabling anyone to directly export neural networks from PyTorch to the NNEF format compatible with tract. This new capability made possible the productization of the neural assets inside Sonos Voice Control as well as the recent Speech Enhancement feature described in a previous blog post ([link](https://tech-blog.sonos.com/posts/arc-ultra-speech-enhancement-announcing-a-step-change-in-speech-enhancement-using-ai/) )


## Live demos


To showcase the practicality of[torch_to_nnef](http://github.com/sonos/torch-to-nnef) and[tract](http://github.com/sonos/tract) , we’ve built interactive demos running entirely in **WebAssembly (WASM)** . These demonstrate PyTorch-to-NNEF conversion and tract efficient inference on real workloads:


-


[Image Classifier](https://sonos.github.io/torch-to-nnef/latest/html/demo_image_classifier.html)


-


[Human pose estimation](https://sonos.github.io/torch-to-nnef/latest/html/demo_pose_estimation.html)


-


[Voice Activity Detection (VAD)](https://sonos.github.io/torch-to-nnef/latest/html/demo_vad.html)


-


[Poem Generator](https://sonos.github.io/torch-to-nnef/latest/html/demo_poem_generator.html)


## Conclusion


By open-sourcing[torch_to_nnef](https://sonos.github.io/torch-to-nnef/latest/) , we aim to make it easier for practitioners to bring PyTorch models into production environments that require efficient inference on constrained devices. Whether for audio, speech, or other on-device ML workloads, we hope this contribution will enable broader adoption of NNEF and tract within the ML community.


## Acknowledgements


This project would not have been possible without the contributions and support of many colleagues across Sonos:


****


**Sonos Voice Control team**


*Raffaele Tavarone* – trusted the first prototype in 2022.


*Mathieu Poumeyrol* – lead developer of tract, with whom we co-designed many features.


*Emrick Sinitambirivoutin* – my manager, contributor and supportive of open-sourcing.


*Hubert De La Jonquiere* – gave insightful ideas on LLM integration.


*Joseph Dureau* – saw the potential early and encouraged its adoption.


And the full team, for their feedback and patience as the tool matured.


**Sonos Audio Team**


*Matt Benatan* and his team, who shipped the first neural Speech Enhancement feature in production with the tool.


**Sonos Tech blog committee**


That helped to make this article clear and more legible.


**Sonos Legal team**


That helped to clarify the open-source licensing and made it in such a short time period


Finally, a special thanks to **Francesco Caltagirone** and **Nick Millington** for championing the open sourcing of this technology.


Share
