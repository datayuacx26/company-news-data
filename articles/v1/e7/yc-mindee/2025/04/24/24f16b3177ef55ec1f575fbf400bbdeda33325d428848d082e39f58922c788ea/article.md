---
schema_version: "1.0.0"
document_id: "24f16b3177ef55ec1f575fbf400bbdeda33325d428848d082e39f58922c788ea"
company_key: "yc-mindee"
company: "Mindee"
source_id: "yc-mindee-news-import-e1ee36c4fe8b"
canonical_url: "https://www.mindee.com/blog/how-to-extract-data-from-invoices-or-receipts-using-python"
published_at: "2025-04-11T03:42:20+00:00"
first_seen_at: "2026-07-22T04:25:17.970569+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:1b395bd7a5556dce75d6080352a425b427818fbd2dbad6b742f1d15d6c0660ab"
---

# How to Extract Data from Invoices or Receipts using Python

*How can I automate data extraction from my receipts and invoices?* Perhaps you’ve wondered about this. Well, you’ve come to the right spot, as we’ll be discussing how to quickly and easily extract data from your documents using just a few lines of code. However, before we get into that, let’s take a quick look at what data extraction is and why it’s so important.


## **What is Data Extraction?**


Extracting information from data sources for further processing, storage, or analysis is referred to as data extraction. It is possible to extract data from a wide range of sources, including emails, web pages, images, accounting records, PDF files, etc.


You have the option of doing this manually or through an automated process. In manual data extraction, the information is gathered by hand, whereas, in automated data extraction, the information is extracted using tools or software.


Businesses utilize the information they get through data extraction for different purposes such as advertising, analysis, strategic planning, etc.


*Here is a list of some great[data extraction tools](https://theecmconsultant.com/top-data-extraction-tools/) .


## **Significance of Data Extraction**


When it comes to automated data extraction, it offers quite a few advantages, some of which are:


1. **It can help reduce long-term and short-term costs** : Short-term and long-term cost savings may be achieved when tedious and time-consuming processes are automated. You can focus on operating and growing your company without worrying about hiring a huge data crew to support it.
2. **It’s a time-saver for companies** : Timing is essential because time equals money. When implemented properly, automated data extraction helps save time and resources, allowing employees to focus on higher-priority work.
3. **It makes work more precise and less prone to mistakes** : When humans are responsible for inputting massive volumes of data on a regular basis, mistakes and inaccuracies are inevitable. These inaccuracies can be avoided, and better data may be obtained by automating the data extraction process– leaving companies to make better and more informed decisions, which is beneficial for everyone.
4. **It has the potential to increase employees’ output** : When your employees aren’t bogged down with tedious data entry work, they will have more time to devote to other important tasks.


## **Using OCR API for Data Extraction**


The Optical Character Recognition (OCR) API can scan, recognize, and extract text from image files and documents – it will then transcribe the text into a format that can be understood by your computer, and you will be able to receive the data that has been extracted in file formats such as JSON, CSV, etc. OCR APIs are developed using OCR technology. What sets them apart is that they are trained to extract data from particular documents, and as a result, they have a higher degree of accuracy.


This is clearly a beneficial technology for developers who work with a wide variety of applications, including those dealing with accounting, e-commerce, healthcare, finance, logistics, etc.


The[Mindee OCR API](https://mindee.com/) is perfect for fast detection and extraction of key information from common documents such as invoices, receipts, passports, etc. You may, however, build your own API to extract data from any type of document not listed above using the Mindee API builder. The Mindee API is quick, accessible around-the-clock, and outputs JSON by default.


It is also capable of extracting data from documents saved in various file formats, which include jpg, png, pdf, tiff, heic, and webp. To fully understand how to use the Mindee API, let’s look at the following section.


## **How to Extract Receipt or Invoice Data using Python**


Using the Mindee Python client library, you can quickly and accurately extract data from your invoice or receipt. A few lines of code is all that’s needed. To demonstrate how to do this, we will use the sample receipt document below.


To get started:


- First, you will need to install[Mindee’s Python Client Library](https://pypi.org/project/mindee/) in your working environment


```text
pip install mindee
```


- Once installed, sign up for a[Mindee account](https://platform.mindee.com/) to get your[API key](https://developers.mindee.com/docs/create-api-key) .
- With your API key, you can run your Mindee client:


```text
from mindee import Client


mindee_client = Client().config_receipt("api-key")
receipt_data = mindee_client.doc_from_path("/path/to/file").parse("receipt")
print(receipt_data.http_response)
```


Below is the output of the extracted field.


```text
{
“api_request”: {
“error”: {},
“resources”: [
“document”
],
“status”: “success”,
“status_code”: 201,
“url”: “http://api.mindee.net/v1/products/mindee/expense_receipts/v3/predict”
},
“document”: {
“annotations”: {
“labels”: {}
},
“id”: “dd457c26-5baa-4612-827b-e10c3d1b7b3d”,
“inference”: {
“extras”: {},
“finished_at”: “2022-07-07T13:12:47+00:00”,
“pages”: [
{
“extras”: {},
“id”: 0,
“orientation”: {“value”: 0},
“prediction”: {
“category”: {
“confidence”: 0.99,
“value”: “food”
},
“date”: {
“confidence”: 0.99,
“polygon”: [
[0.479, 0.173],
[0.613, 0.173],
[0.613, 0.197],
[0.479, 0.197]
],
“raw”: “26-Feb-2016”,
“value”: “2016-02-26”
},
“locale”: {
“confidence”: 0.82,
“country”: “GB”,
“currency”: “GBP”,
“language”: “en”,
“value”: “en-GB”
},
“orientation”: {
“confidence”: 0.99,
“degrees”: 0
},
“supplier”: {
“confidence”: 0.71,
“polygon”: [
[0.394, 0.068],
[0.477, 0.068],
[0.477, 0.087],
[0.394, 0.087]
],
“value”: “CLACHAN”
},
“taxes”: [
{
“code”: null,
“confidence”: 0.98,
“polygon”: [
[0.19, 0.403],
[0.698, 0.403],
[0.698, 0.432],
[0.19, 0.432]
],
“rate”: 20,
“value”: 1.7
}
],
“time”: {
“confidence”: 0.99,
“polygon”: [
[0.62, 0.173],
[0.681, 0.173],
[0.681, 0.191],
[0.62, 0.191]
],
“raw”: “15:20”,
“value”: “15:20”
},
“total_incl”: {
“confidence”: 0.99,
“polygon”: [
[0.549, 0.619],
[0.715, 0.619],
[0.715, 0.64],
[0.549, 0.64]
],
“value”: 10.2
}
}
}
],
“prediction”: {
“category”: {
“confidence”: 0.99,
“value”: “food”
},
“date”: {
“confidence”: 0.99,
“page_id”: 0,
“polygon”: [
[0.479, 0.173],
[0.613, 0.173],
[0.613, 0.197],
[0.479, 0.197]
],
“raw”: “26-Feb-2016”,
“value”: “2016-02-26”
},
“locale”: {
“confidence”: 0.82,
“country”: “GB”,
“currency”: “GBP”,
“language”: “en”,
“value”: “en-GB”
},
“supplier”: {
“confidence”: 0.71,
“page_id”: 0,
“polygon”: [
[0.394, 0.068],
[0.477, 0.068],
[0.477, 0.087],
[0.394, 0.087]
],
“value”: “CLACHAN”
},
“taxes”: [
{
“code”: null,
“confidence”: 0.98,
“page_id”: 0,
“polygon”: [
[0.19, 0.403],
[0.698, 0.403],
[0.698, 0.432],
[0.19, 0.432]
],
“rate”: 20,
“value”: 1.7
}
],
“time”: {
“confidence”: 0.99,
“page_id”: 0,
“polygon”: [
[0.62, 0.173],
[0.681, 0.173],
[0.681, 0.191],
[0.62, 0.191]
],
“raw”: “15:20”,
“value”: “15:20”
},
“total_incl”: {
“confidence”: 0.99,
“page_id”: 0,
“polygon”: [
[0.549, 0.619],
[0.715, 0.619],
[0.715, 0.64],
[0.549, 0.64]
],
“value”: 10.2
}
},
“processing_time”: 0.748,
“product”: {
“features”: [
“locale”,
“category”,
“date”,
“time”,
“total_incl”,
“taxes”,
“supplier”,
“orientation”
],
“name”: “mindee/expense_receipts”,
“type”: “standard”,
“version”: “3.1”
},
“started_at”: “2022-07-07T13:12:46+00:00”
}
```


**Note** : You can use this same method to extract data from your invoice document. Simply change in the code receipt to invoice and link the path to your invoice document.


## **Extract Key Fields from your Invoices or Receipts.**


You may want to extract key or additional fields from your document. For instance, you may want to extract the following information from your receipt document: *total amounts, expenditure categories, date, supplier information, location, and time, among others* . This can also be applied to your invoice document, you may want to extract the following information: *invoice number, invoice date, customer name, payment details, etc* . To do this, you must define in your code the fields you want to extract.


Using the same[receipt document](https://lh4.googleusercontent.com/HqOlSaxbDpH_uy4YLKhztKCsqtRZvU-u9M4KQKS6DZM0bxhkxNExgcoiVG0d2rY9F1Xw8sc5yUThfCJrYFp5v-TPPItqn1lH8JcTRGCZk8kL-JyrsUE7Ft7oikXjkuE5arZjQz_jLQqUfDHsqo0) , we will extract the following key fields listed below from our receipts.


- Total amount
- Supplier information
- Date
- Category


**Note:** While this article will only cover a small subset of the information that may be extracted from an[invoice](https://developers.mindee.com/docs/python-invoice-ocr) or[receipt](https://developers.mindee.com/docs/python-receipt-ocr) , the[Mindee documentatio](https://developers.mindee.com/docs) n has many more examples.


- **Total Amounts:** To get the total amount, including taxes


```text
To get the total amount including taxes value


total_incl = receipt_data.receipt.total_incl.value
print("total with tax", total_incl)
```


Output


```text
total with tax 7.27
```


- **Supplier Information:** To get the supplier’s name as written on the receipt.


```text
# To get the supplier name


supplier_name = receipt_data.receipt.merchant_name.value
print("Supplier Name: ", supplier_name)
```


Output


```text
Supplier Name: MINDEE TAKE OUT
```


- **Date:** To get the payment date as seen on the receipt.


```text
# To get the receipt date of issuance


date = receipt_data.receipt.date.value
print("Date on receipt: ", receipt_date)
```


Output


```text
Date on receipt:  2022-04-03
```


- Category: To get the receipt category as seen on the receipt.


```text
# To get the category


category = receipt_data.receipt.category.value
print("purchase category: ", category)
```


Output


```text
purchase category: food
```


**Note** : You can use this same method to extract data from your invoice document. But there are a few tweaks as you need to appropriately specify the invoice fields in your code. For more guidance, check out the[Mindee documentation](https://developers.mindee.com/docs/python-invoice-ocr#additional-attributes) .


## Conclusion


As you can see, the processes are straightforward and easy to implement. If your application is developed in a language other than Python, you don’t have to worry as Mindee provides code samples in different languages, including Node.js, PHP, Ruby, Curl, etc.
