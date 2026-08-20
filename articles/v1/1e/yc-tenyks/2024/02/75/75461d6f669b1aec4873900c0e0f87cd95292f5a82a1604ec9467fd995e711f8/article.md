---
schema_version: "1.0.0"
document_id: "75461d6f669b1aec4873900c0e0f87cd95292f5a82a1604ec9467fd995e711f8"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance---part-2-of-3"
published_at: "2024-02-22T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:2c87193e8c01d40abc7b19658c18d705e46b67b65c694c432a3ecd375cfce139"
---

# NVIDIA TAO Toolkit: How to Build a Data-Centric Pipeline to Improve Model Performance - Part 2 of 3

During this series, we will use Tenyks to build a Data-Centric pipeline to debug and fix a model trained with the NVIDIA TAO Toolkit.


‍


[Part 1](https://medium.com/@tenyks_blogger/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance-part-1-of-3-de8b26f55ec7) . We demystify the NVIDIA ecosystem and define a Data-Centric pipeline based on a model trained with the NVIDIA TAO framework.


‍


Part 2 (current). Using the Tenyks API, we show you how to upload a dataset and a model to the Tenyks platform.


‍


Part 3. We identify failures in our model due to data issues, fix these failures and improve our model’s performance with the fixed dataset.


‍


## **Table of Contents**


1. Recap from Part 1
2. Getting an access token
3. Uploading our dataset
4. Uploading our model
5. Manually uploading a dataset/model
6. What’s next


‍


## 1. Recap from Part 1


In the[first post](https://medium.com/@tenyks_blogger/nvidia-tao-toolkit-how-to-build-a-data-centric-pipeline-to-improve-model-performance-part-1-of-3-de8b26f55ec7) of this series we focused on three main things:


1. We introduced a blueprint for building a Data-Centric pipeline.
2. We broke down the NVIDIA TAO Toolkit and several of the moving parts of the NVIDIA ecosystem.
3. We briefly introduced the[Tenyks API](https://docs.tenyks.ai/reference/introduction) .


‍


Before continuing, make sure that you have the following pre-requisites:


- A trained model on the[dataset](https://drive.google.com/drive/folders/1BbAgoLwLtWT_A7U8fwb-ep7XmZQKBHzF?usp=sharing) we introduced in Part 1. You can either adventure yourself with the NVIDIA TAO Toolkit using this[Colab notebook](https://colab.research.google.com/github/NVIDIA-AI-IOT/nvidia-tao/blob/main/tensorflow/yolo_v4/yolo_v4.ipynb) or download a trained model[here](https://drive.google.com/drive/folders/1wKXRL8hB1zvA9mEYN6c5QUOy2y5jIe3m?usp=sharing) .
- Both the annotations of your dataset and the predictions of your model are expected to be in[COCO format](https://docs.tenyks.ai/docs/coco-format) — If you opt for[downloading a trained model](https://drive.google.com/drive/folders/1wKXRL8hB1zvA9mEYN6c5QUOy2y5jIe3m?usp=sharing) (already in COCO format) instead of training one by yourself, you can disregard this step.
- One provider to store your data with the appropriate configuration (click on the links to configure your data storage provider):[AWS](https://docs.tenyks.ai/docs/aws-s3) ,[Google Cloud Storage](https://docs.tenyks.ai/docs/google-cloud-storage) or[Azure](https://docs.tenyks.ai/docs/azure-folder-structure) .
- A Tenyks account.


‍


⚠️ Tenyks’ advanced features, including the API, are available for premium users only. However, to make the most out of this series, Section 5 describes how you can upload a dataset/model on the freemium version of Tenyks. Please, sign up for a sandbox account[here](http://sandbox.tenyks.ai/) .


‍


## 2. Getting an access token


To obtain an access token, first generate your API keys (see Figure 1).


‍


Figure 1. Find and save your Tenyks API keys


‍


The API keys will serve you to retrieve an[access token](https://docs.tenyks.ai/reference/access-token) , as shown on the code below.


‍


💡 Hint:[Tenyks’ documentation](https://docs.tenyks.ai/reference/introduction) contains examples in other programming languages different from Python (e.g., Node, Go, PHP, Ruby, etc).


‍


```text


import requests


url = "https://dashboard.tenyks.ai/api/auth/apikey"


payload = {
"api_key": "my_api_key",
"api_secret": "my_api_secret"
}
headers = {
"accept": "application/json",
"content-type": "application/json"
}


response = requests.post(url, json=payload, headers=headers)


```


‍


A successful response is shown below:


‍


```text


{
"access_token": "eyJraWQ...QH_HAbA",
"id_token": "eyJraWQiOiJ...daHusA",
"expires_in": 3600,
"refresh_token": "eyJjd....gvqtm5",
"token_type": "Bearer"
}


```


‍


Save the value of` access_token` , we’ll use it during the rest of the article.


‍


⚠️ Be aware that the access token has an **expiration time** of 3,600 seconds or 60 minutes — follow the same procedure to obtain a new one, if required.


‍


💡Hint: Our following code examples might appear verbose to the trained eye, and in fact they are: for this series, we aim to be as explicit as possible!


‍


## 3. Uploading our dataset to Tenyks


⚠️️ For the code examples in Section 3 and 4:


- We use[Azure](https://azure.microsoft.com/en-ca/free/search/?ef_id=_k_Cj0KCQiAoKeuBhCoARIsAB4WxtcvXKyN2VOYUWJdEm3a-HYN6y-1zR60yA9T9jE-Dm5JcRzrT1P8MxEaAmQkEALw_wcB_k_&OCID=AIDcmmqz3gd78m_SEM__k_Cj0KCQiAoKeuBhCoARIsAB4WxtcvXKyN2VOYUWJdEm3a-HYN6y-1zR60yA9T9jE-Dm5JcRzrT1P8MxEaAmQkEALw_wcB_k_&gad_source=1&gclid=Cj0KCQiAoKeuBhCoARIsAB4WxtcvXKyN2VOYUWJdEm3a-HYN6y-1zR60yA9T9jE-Dm5JcRzrT1P8MxEaAmQkEALw_wcB) as data storage provider during this post. For more information, follow this[Azure walk-through](https://docs.tenyks.ai/reference/azure-workflow) .
- Replace` credentials.value` and` azure_uri` with your own values.
- Use your` access_token` in the headers.


‍


### 3.1 Images & Annotations Ingestion


We assume you have configured your data storage provider as mentioned on Section 1. If you haven’t, detailed instructions of how to do the setup Azure, can be found[here](https://docs.tenyks.ai/docs/azure-folder-structure) .


‍


Let’s push our dataset to Tenyks! 🚀


- Uploading images:


```text


# Instructions
# - In "payload", for {value} and {azure_uri} use your own values.
# - Use your {access_token} in the "authorization" key of the headers.
import requests


url = "https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets"


payload = {
"task_type": "object_detection",
"images_location": {
"type": "azure",
"credentials": {
"type": "connection_string",
"value": "DefaultEndpointsProtocol=https;AccountName=tenyksapi;AccountKey=q1UXCLR5JKyG//hcZSztJu+l/cQhpIS2+ASt5FbjSw==;EndpointSuffix=core.windows.net"
},
"azure_uri": "https://azure_storage_account.blob.core.windows.net/tenyks-datasets/kitti/images/"
},
"key": "tshirts_nvidia_tao", # replace with your own value
"display_name": "tshirts_nvidia_tao", # replace with your own value
}
headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiO...JeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.post(url, json=payload, headers=headers)


```


‍


- Uploading annotations:


```text


# Instructions
# - In "payload", for {value} and {azure_uri} use your own values.
# - Use your {access_token} in the "authorization" key of the headers.
import requests
my_dataset_key = "road_traffic_nvidia"


url = f"https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets/{my_dataset_key}/images/annotations"


payload = {
"type": "azure",
"credentials": {
"type": "connection_string",
},
"azure_uri": "https://azure_storage_account.blob.core.windows.net/tenyks-datasets/kitti/annotations.json"
}
headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiO...JeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.put(url, json=payload, headers=headers)


```


‍


- Ingesting the images and the annotations into Tenyks:


```text


# Instructions
# - Use your {access_token} in the "authorization" key of the headers.
import requests


url = "https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets/tshirts_nvidia_tao/ingest"


headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiO...JeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.put(url, headers=headers)


```


‍


Voila! You will now see your data on the platform! (see Figure 2).


Figure 2. Images ingested on the Tenyks platform


‍


## 4. Uploading our model to Tenyks


### 4.1 Model Predictions Ingestion


Next, let’s upload our model to Tenyks.


- Creating a model:


```text


# Instructions
# - Use your {access_token} in the "authorization" key of the headers.
import requests
my_dataset_key = "road_traffic_nvidia"


url = f"https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets/{my_dataset_key}/model_inferences"


payload = {
"iou_threshold": "0.5", # you can change this value
"confidence_threshold": "0.5", # you can change this value
"display_name": "yolo_v8", # replace with your own value
"key": "yolo_v8" # replace with your own value
}
headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiO...JeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.post(url, json=payload, headers=headers)


```


‍


- Uploading model predictions:


```text


# Instructions
# - In "payload", for {value} and {azure_uri} use your own values.
# - Use your {access_token} in the "authorization" key of the headers.
import requests
my_dataset_key = "road_traffic_nvidia"
my_model_key = "yolo_v8"


url = f"https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets/{my_dataset_key}/model_inferences/{my_model_key}/predictions"


payload = {
"type": "azure",
"credentials": {
"type": "connection_string",
},
"azure_uri": "https://azure_storage_account.blob.core.windows.net/tenyks-datasets/kitti/predictions.json"
}
headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiO...JeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.put(url, json=payload, headers=headers)


```


‍


- Ingesting model predictions:


```text


# Instructions
# - Use your {access_token} in the "authorization" key of the headers.
import requests
my_dataset_key = "road_traffic_nvidia"
my_model_key = "yolo_v8"


url = f"https://dashboard.tenyks.ai/api/workspaces/tenyks/datasets/{my_dataset_key}/model_inferences/{my_model_key}/ingest"


headers = {
"accept": "application/json",
"content-type": "application/json",
"authorization": "Bearer eyJraWQiOiJrcFdXU0hjc1I2M1o1NVJBa3E4WWRCVVpGQ2Z3a2pUVUp1Wkd6dVlUNHNRPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI3ODlmN2U0Yy0yYTg5LTQ1ZWYtODZmMi1hYmY3NjMzMzMyYjEiLCJkZXZpY2Vfa2V5IjoiZXUtY2VudHJhbC0xX2Q1MGViYmRkLWUyZTEtNDhjZC1hNjQ5LWE1Mzk5ZWZjNzAzNCIsImNvZ25pdG86Z3JvdXBzIjpbImFwcHJvdmVkX3VzZXJzIiwidHJpYWxfdXNlcnMiXSwiaXNzIjoiaHR0cHM6XC9cL2NvZ25pdG8taWRwLmV1LWNlbnRyYWwtMS5hbWF6b25hd3MuY29tXC9ldS1jZW50cmFsLTFfUWdlN2NsODY4IiwiY2xpZW50X2lkIjoiNjNlajJhbWRibWwxZ3BudGo0a280cjVkbGoiLCJvcmlnaW5fanRpIjoiNmIzYTAxZDctOTIyZi00ZGY4LTg5YWUtOWY3ZjVmZDE4MGVhIiwiZXZlbnRfaWQiOiI1MDMyNjFhMC1jZWZlLTQzNjYtYjIwMS1iMGYyZTk3MDcwZmEiLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6ImF3cy5jb2duaXRvLnNpZ25pbi51c2VyLmFkbWluIiwiYXV0aF90aW1lIjoxNzA3NzczNzgxLCJleHAiOjE3MDc3NzczODEsImlhdCI6MTcwNzc3Mzc4MSwianRpIjoiZmE4OWEzM2MtZWI3YS00NDE3LWFkNzYtYTU0OWMyZjdmOTkyIiwidXNlcm5hbWUiOiI3ODlmN2U0Yy0yYTg5LTQ1ZWYtODZmMi1hYmY3NjMzMzMyYjEifQ.bW_rKvM8kNQ9PK21vhavVTCyFPDqzMS-Sf2EsbJ-CjhHkWcSyKUj9CzGg3d3NNe-3UIK5aaLK3zqXe46BuRnw2EMpxyJuYiNy4yX-1G2JRNOwRipds0CIU0LzhcW8gp7YI8cM0be_t2roZsgK0041v5KpllDNVN-Ryd_bE8Ih52ExpRMfdHmUql4V33_YBk4g7jnBMh3GsN-m6uJUsZizkyLBMzLbK05ITKyNF0C22DvwrgRVeCT9sPvdzzK1okudHCIpOacPq9Nl2XhVziy2H7J4xIkBPacNJSSQu8Ul5NqUg3QlESDsXXbJeTh-RN8WPOULqr7NFP8_ETrJ4ECpg"
}


response = requests.put(url, headers=headers)


```


‍


After completing this procedure, this time you will also see a model on your Tenyks dashboard (see Figure 3).


Figure 3. Predictions ingested on the Tenyks platform


You are all set! ⛳️


‍


## 5. Manually uploading a dataset/model


In case you don’t have access to the Tenyks API, you can quickly upload your dataset/model on your sandbox account.


‍


Before we start:


- If you haven’t signed up for a free sandbox account, please[create an account](https://sandbox.tenyks.ai/) .
- The trained NVIDIA TAO model, that you can download[here](https://drive.google.com/drive/folders/1wKXRL8hB1zvA9mEYN6c5QUOy2y5jIe3m?usp=drive_link) , contains all what you need in the required format.


‍


This step by step walk-through show you can you[upload your first dataset](https://docs.tenyks.ai/docs/dataset-upload) . This other guide show you how to[upload your model](https://docs.tenyks.ai/docs/model-upload) .


‍


## 6. What’s next


We have defined a Data-Centric pipeline to detect data failures and improve performance on a trained NVIDIA TAO model (Part 1).


‍


In Part 2, we have set up a data storage provider (Azure for this article) to interact with the Tenyks platform. In addition, we have learned how to actually use the Tenyks API to ingest a dataset into our Tenyks account.


‍


In Part 3, we will delve into finding model failures as well as fixing these errors following a Data-Centric pipeline.


‍


Stay tuned for Part 3! 💙


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


‍


*If you would like to know more about* ***Tenyks*** *, sign up for a*[sandbox account](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*
