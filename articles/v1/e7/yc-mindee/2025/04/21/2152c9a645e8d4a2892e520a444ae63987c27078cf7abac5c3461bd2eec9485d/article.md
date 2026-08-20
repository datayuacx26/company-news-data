---
schema_version: "1.0.0"
document_id: "2152c9a645e8d4a2892e520a444ae63987c27078cf7abac5c3461bd2eec9485d"
company_key: "yc-mindee"
company: "Mindee"
source_id: "yc-mindee-news-import-e1ee36c4fe8b"
canonical_url: "https://www.mindee.com/blog/ocr-explained"
published_at: "2025-04-11T03:44:13+00:00"
first_seen_at: "2026-07-22T04:25:17.970569+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:b747aa9285793d2a3f467066052f42344b87bc11efe1e8e72501cac02632b564"
---

# What is OCR: Optical Character Recognition Explained - Mindee

## **What does OCR stand for?**


OCR stands for **Optical Character Recognition** . The technical definition refers to software technologies capable of capturing text elements from images or documents and converting them into machine-readable text format.


OCR reconstructing a fully digital document


Bare OCR technologies have a limited usage scope. Without any extra processing layer, it’s usually purposed to retrieve machine-encoded text from images or scanned documents and store them in document management software.


For other use cases, such as key information extraction from documents, it’s required to build another layer of intelligence on top of the OCR output, based on semantic information or other visual or natural language features. Regardless of the context, OCR is the initial step when the entry point of your workflow gets structured text input from scanned documents or photos.


Therefore, the acronym OCR is commonly used to refer to any OCR-based application such as a consumer mobile application turning a picture into an editable text document. For example, software capable of extracting key information from invoices is often called an *“Invoice OCR”.*


‍


## OCR for documents and photos


In elementary schools, you have learned how to perform the image-to-text conversion using your eyes as visual inputs, and your knowledge about characters’ shapes as a model. Admittedly, at that age, you were able to read properly written/typed words on a sheet of paper, but you might have struggled reading handwritten graffiti.


> Before diving into OCR technology, it’s important to understand the main use cases and how it is used within software. Structuring text from images can help solve multiple real-world use cases.


In this manner, we shall divide the use cases into two distinct visual domains:


- OCR in the wild: a love message written on the sand of the beach
- OCR on documents: your train ticket


These two domains display significant differences in terms of element density (much higher in documents) and spatial/structural arrangements. However, we will focus on the most specific domain: OCR on documents.


‍


## **What is OCR used for?**


### **Convert image to text using OCR**


Image-to-text conversion is the most common OCR use case. It refers to any web or mobile application transforming a picture containing text into plain machine-readable text. The main goal of those tools is to help the user transcribe all the text captured in a photo in a few seconds, instead of manually typing the whole text. The resulting text can be then copied into the user’s clipboard and used on his device.


‍


### **Digitize documents with OCR**


Also known as dematerialization, digitizing a document consists of[creating a machine-readable text copy](https://www.mindee.com/blog/create-ocrized-pdfs-in-2-steps) of a document to store it in document management software. It can either be a plain text format or an editable copy with the same layout. In the second scenario, an OCR capable of detecting the position of each word along with their text content is required to detect the layout of the documents.


Most digitization companies are providing their clients with the appropriate hardware (scanners) to handle the conversion from paper documents to digital data.


Document digitization process using OCR, paper, scan, OCR, and document management software


### **Make PDFs searchable and indexable with OCR**


Any archive of unstructured documents can be transformed into machine-readable text in order to make each document searchable using a natural language query. Using only an OCR, you can simulate a CTRL/CMD+F search within a scanned document on the text it contains. For more advanced OCR use cases, it’s likely that you need to build a search engine to look up different semantic information written in your document. Adding key information extraction features on top of your OCR might be required before indexing the extracted data in a search engine.


{{cta-awareness-1="/in-progress/global-blog-elements"}}


‍


### **Optical character recognition for document recognition**


Also known as document classification, this task is about automatically classifying a new document by assigning it a type among a predefined set of document classes. The role of the OCR, in this case, is to extract all the words and characters from a document and use them as features for a classifier down the road. This classifier can be based on simple keyword detection rules (i.e. taxes for invoices or receipts,[passport numbers for passport documents](https://www.mindee.com/blog/mrz-passport-protect-your-identity-online) …) as well as machine learning (ML) algorithms for more complex classification. Using an ML model is a real advantage for this task, as it doesn’t require an extremely robust OCR to get very high performances.


Document recognition or classification


1. **In procurement workflows** : Given a new invoice-like document, the actions taken in a procurement workflow are different depending on the document’s type. An order form needs to be approved, an invoice needs to be paid, and finally, a receipt needs to be uploaded to the accounting system.
2. **In expense management** :[Expense management software use the receipts data](https://mindee.com/product/receipt-ocr-api) to apply the right approbation process, or to detect potential fraud. For instance, some companies only partially reimburse their employees’ expenses for gas or restaurants but fully reimburse parking or hotel receipts. Some of them also don’t reimburse their employee’s restaurant expenses if the receipt time is not in a specific time window.
3. **In loan application** : For some loan application processes, the applicant sends a unique PDF including a set of different documents (IDs, tax return, payslips, certificate of incorporation for businesses, etc..). That unique PDF must be split into the proper types of documents to send each of them into different workflows.


‍


### E **xtract key information on documents using OCR**


This use case is probably the most complicated and crucial in the workflow automation space. It consists of extracting specific information from documents and outputting them in a machine-readable format that can be used in other software.


Compared to the document classification problem, the performance of the OCR used is extremely important for this use case as it doesn’t rely on an extrapolation of the text content, but on some specifically chained characters.


ALT: Document Key Information Extraction


1. **Customer onboarding workflows:** This involves getting structured data from legacy documents in order to populate the customers’ account information into the required software. In contracts management software, for example, users need to get their past and ongoing contracts stored with their key data to benefit from the product features. These key data mainly include the parties, contract type, start and end dates, and the renewal policy. Automating the[key information extraction](https://www.mindee.com/blog/invoice-ocr-line-items-extraction) (KIE) from contracts is important to ease the onboarding process.
2. **In procurement workflows** : Accounts payable (AP) automation requires[extracting the key data written in invoices](https://mindee.com/product/invoice-ocr-api) to automate or ease the payment process of incoming invoices. The use of the term *“invoice OCR”* for this use case is widespread but it doesn’t correspond to the technical definition of an OCR. The set of data that needs to be extracted slightly depends on the workflow and the software specifications, but it generally includes:


- Vendor name (sometimes along with a company identifier such as TIN (Tax identification number), SIRET, VAT Number…)
- Invoicing date
- Payment due date
- Invoice total amounts: one including taxes and one excluding them
- Invoice total taxes (sometimes with the tax line details)
- Vendor bank wire details


Adding extra key data on top of this list can help cover more procurement use cases:


- Three-way matching consists of associating an order form, an invoice, and a payment receipt using the purchase order number (PO) that needs to be extracted from the three documents;
- Accounts receivable (AR) and factoring automation requiring the automatic extraction of the client name and address;
- Procurement approval requires extracting the line items written in the invoices. Line item extraction is a task that doesn’t exactly fit the key information extraction problem as it relies on table detection and structuration.


{{cta-consideration-1="/in-progress/global-blog-elements"}}


‍


## OCR Technology


How do you distinguish plain word sequences from a document? How do you perceive the difference between “writing 1 or 2 paragraphs on a topic” and “creating a document on a topic”?


Admittedly, consecutive paragraphs are not enough to make a document. Documents encode information by combining semantic/writing conventions (what a given sequence of characters refers to) and visual/structural arrangements(e.g. form-like documents). Documents can easily integrate non-textual information (e.g. a picture) which makes it a richer information medium than words as sequences only.


OCR generic pipeline


As a first step, let’s consider how a human interprets a document:


- Partial localization of information
- Focus on words sequentially to recognize textual information
- Use semantic knowledge to make sense of this text.


We can also argue that a human uses its semantic knowledge as a feedback loop for text recognition: if you have visual doubts about a character in the middle of a word, your language knowledge will help you clear that doubt.


OCR technologies rely on the same steps for detecting and recognizing text inside images. The first step is about localizing text elements in the image, and the second one is to recognize the characters at those locations in order to convert them into a machine-readable sequence of characters.


‍


## OCR **Text detection**


In general, an object detection problem in computer vision refers to the task of detecting object positions in images. The output of such algorithms is a list of bounding boxes corresponding to the positions of each object detected in the image.


OCR Text Detection


The text detection algorithm in an OCR pipeline is responsible for detecting every chain of characters in the image that doesn’t contain a blank space. The high-level definition of the text detection algorithm is:


- Input: image with multiple word sequences
- Output: localization of each word / uninterrupted character sequence.


As a convention, we’ll consider that the localization is performed using bounding boxes (the smallest enclosing rectangular box covering the word). But in general, the output can be any polygon enclosing the desired object.


‍


### A first approach to text detection: **Plain object detection**


Popular computer vision models architectures: Faster R-CNN, YOLO


Object detection for text detection


#### Image feature extraction


Most deep learning models process visual information using several layers of convolutions. Convolutions are a type of mathematical function used in a neural network typically used to recognize patterns present in images. Each layer extracts information from its previous neighbor by identifying local spatial patterns. Such layers in stacks thus increasingly extract wider and more complex spatial patterns.


This process is generally called (spatial) feature extraction and yields a set of complex spatial patterns identified by your model. For a detailed introduction to convolutional neural networks, check this excellent article with[in-browser interactive animation](https://poloclub.github.io/cnn-explainer) .


‍


#### **Feature pooling**


Each object is represented by a set of coordinates which is the bounding box that includes the object we are looking for (words in that case). For each object’s candidate, let’s imagine that the extracted features are a set of (N, N) matrices, (N being generally much smaller than the input image’s size), where the top left corner of those corresponds to the spatial patterns that were extracted from the top left of your input image. We need to focus now on a wide range of object sizes in the image. To do this, we will set some priors:


- a set of expected aspect ratio
- a set of expected scales
- a set of expected object centers


More specifically, at a given location/object center, we will only consider objects that are close to any combination of our predefined aspect ratios and scales.


Now that we have our extracted features and the approximate estimated localization (bounding box) of an object, we will extract the features from each location and resize them using an operation called region pooling (RoI pooling in Faster-RCNN, RoI Align in Mask-RCNN ) illustrated below.


Feature pooling computer vision


Using this, if we had M expected object centers, 3 possible aspect ratios, and 3 possible scales, we would get a set of 9 * M pooled features (each corresponding to one combination of center, aspect ratio, and scale). Please note that this is true irrespective of the size of our extracted features.


‍


#### **Box classification and regression**


For each of our object candidates, we can now perform different tasks. In object detection, we will refine the bounding box (starting from our corresponding prior of center + scale + aspect ratio), and classify the object (e.g. is it a dog or a cat?). In our context, the classification will simply be binary (is this a word or not?).


‍


### A second approach to text detection: **image segmentation**


Popular computer vision models architectures: U-Net, DeepLab


Image segmentation for text detection


Using the same extracted features as in object detection, the set of (N, N) spatial features, we now need to upscale those back to the image dimensions (H, W). Remember that N is smaller than H or W.


**Feature upscaling**


Especially if our (N, N) matrices are much smaller than the image, basic upsampling would bring no value. Rather than just interpolating our matrices to (H, W), architectures have different tricks to learn those upscaling operations such as transposed convolutions. We then obtain fine-grained features for each pixel of the image.


‍


**Binary classification**


Using the many features of these pixels, a few operations are performed to determine their category. In our case, we will only determine whether the pixel belongs to a word, which produces a result similar to the “segmentation” illustration from the previous figure.


‍


**Bounding box conversion**


Finally, the binary segmentation map needs to be converted into a set of bounding boxes. It sounds like a much easier task than producing the segmentation map, but two words close to each other might look like they are the same according to your segmentation map. Some post-processing is required to produce relevant object localization.


‍


## OCR Text recognition


OCR Text Recognition


The text recognition block in an OCR pipeline is responsible for transcribing the character sequence of an image:


- Input: image with a single character sequence
- Output: the value of the word


The assumption that there is a single character sequence allows us to consider two approaches.


‍


### A first approach to text recognition: rolling character classification


Rolling Character Text Recognition


**Split words into character crops**


You can find numerous computer vision techniques to horizontally localize each character, such as horizontal color histograms. Generally, this operation does not require extensive computation power thanks to our assumption of a single-character sequence.


‍


**Character classification**


Now instead of having a character sequence to identify, this becomes a simple image classification problem. Instead of predicting whether this is a dog or a cat (is it a word or not), we will have to identify directly the character in our image.


Fortunately for us, this does not require large input images or very deep architectures as Yann Lecun proved with his LeNet5 model trained to classify handwritten digits back in 1998. Concisely, the model will extract spatial features, aggregate them, and will classify the whole set of features to predict the result. Then we only need to assemble our character predictions into word predictions.


‍


### **A second approach for text recognition: sequence modeling**


OCR Sequence modeling character recognition


**Image feature extraction**


Our word image crops can now be fed to a model to extract spatial features similar to what we have done in object detection (at a much much lower resolution this time). For simplification purposes, let’s imagine this yields a set of (1, N spatial features.


‍


**Aligning and classifying**


Now it is very important to note that the extracted features will always have the same output size irrespective of the length of the word. Meaning that in our (1, N spatial features, there are rarely N characters. You might have understood it: we have an alignment problem.


This alignment problem is common to all sequence modeling problems such as transcribing audio into text, image into a character sequence, etc.


One of the modern approaches to this involves using the connectionist Temporal Classification (brilliantly explained in this article ) which proceeds in two steps:


- alignment: going from our N-6 features (x1, …, x6) to N aligned characters (“c”, “c”, “a”, “a”, “a”, “t”)
- collapsing: merging the N-aligned characters into a clean output (“c”, “a”, “t”) OCR obstacles real-world obstacles


‍


## **OCR resource** s


Depending on your constraints and internal resources, you can leverage OCR in multiple ways: from implementing your own OCR and handling its production-ready deployment to delegating the whole process to a third-party commercial service.


‍


### **Open-source OCR libraries**


While open-source resources usually don’t come without any direct financial expenses, you need to have internal resources to orchestrate your service’s deployment. However, this usually makes room to customize your prediction block (re-training, optimization, interfacing) since you have access to the entire codebase.


Mindee provides developers and data scientists with an open-source OCR in python[docTR](https://github.com/mindee/doctr) , you’ll find a demo below.


‍


### **Mindee OCR API**


The positive aspect of commercial services is that you will only need to interface the API with your information system. With this option, you delegate to third parties the cloud orchestration and model development responsibilities in exchange for money for your paid bill. Mindee is a trusted OCR API that is used to parse documents for developers. With our free API key, you can quickly and easily begin using our Optical Character Recognition (OCR) APIs to extract information from your documents.


{{cta-conversion-1="/in-progress/global-blog-elements"}}
