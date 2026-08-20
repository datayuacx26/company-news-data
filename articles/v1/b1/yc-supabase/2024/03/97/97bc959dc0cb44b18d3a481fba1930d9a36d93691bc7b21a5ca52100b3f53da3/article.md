---
schema_version: "1.0.0"
document_id: "97bc959dc0cb44b18d3a481fba1930d9a36d93691bc7b21a5ca52100b3f53da3"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/semantic-image-search-amazon-bedrock"
published_at: "2024-03-26T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:581effb20560f32891580c8327ae528b24f65727ebe717823517e77a69da52bd"
---

# Implementing semantic image search with Amazon Titan and Supabase Vector

[Amazon Bedrock](https://aws.amazon.com/bedrock) is a fully managed service that offers a choice of high-performing foundation models (FMs) from leading AI companies like AI21 Labs, Anthropic, Cohere, Meta, Mistral AI, Stability AI, and Amazon. Each model is accessible through a common API which implements a broad set of features to help build generative AI applications with security, privacy, and responsible AI in mind.


[Amazon Titan](https://aws.amazon.com/bedrock/titan/) is a family of foundation models (FMs) for text and image generation, summarization, classification, open-ended Q&A, information extraction, and text or image search.


In this post we'll look at how we can get started with Amazon Bedrock and Supabase Vector in Python using the Amazon Titan multimodal model and the[vecs client](https://supabase.com/docs/guides/ai/vecs-python-client) .


You can find the full application code as a Python Poetry project on[GitHub](https://github.com/supabase/supabase/tree/master/examples/ai/aws_bedrock_image_search) .


## Create a new Python project with Poetry#


[Poetry](https://python-poetry.org/) provides packaging and dependency management for Python. If you haven't already, install poetry via pip:


`
_10


pip install poetry


`


Then initialize a new project:


`
_10


poetry new aws_bedrock_image_search


`


## Spin up a Postgres Database with pgvector#


If you haven't already, head over to[database.new](https://database.new/) and create a new project. Every Supabase project comes with a full Postgres database and the[pgvector extension](https://supabase.com/docs/guides/database/extensions/pgvector) preconfigured.


When creating your project, make sure to note down your database password as you will need it to construct the` DB_URL` in the next step.


You can find the database connection string in your Supabase Dashboard[project connect page](https://supabase.com/dashboard/project/_?showConnect=true) . Select "Use connection pooling" with` Mode: Session` for a direct connection to your Postgres database. It will look something like this:


`
_10


postgresql://postgres.\[PROJECT-REF\]:\[YOUR-PASSWORD\]@aws-0-\[REGION\].pooler.supabase.com:5432/postgres


`


## Install the dependencies#


We will need to add the following dependencies to our project:


- [vecs](https://github.com/supabase/vecs#vecs) : Supabase Vector Python Client.
- [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) : AWS SDK for Python.
- [matplotlib](https://matplotlib.org/) : for displaying our image result.


`
_10


poetry add vecs boto3 matplotlib


`


## Import the necessary dependencies#


At the top of your main python script, import the dependencies and store your` DB URL` from above in a variable:


`
_10


import sys


_10


import boto3


_10


import vecs


_10


import json


_10


import base64


_10


from matplotlib import pyplot as plt


_10


from matplotlib import image as mpimg


_10


from typing import Optional


_10


_10


DB_CONNECTION = "postgresql://postgres.\[PROJECT-REF\]:\[YOUR-PASSWORD\]@aws-0-\[REGION\].pooler.supabase.com:5432/postgres"


`


Next, get the[credentials to your AWS account](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html) and instantiate the` boto3` client:


`
_10


bedrock_client = boto3.client(


_10


'bedrock-runtime',


_10


region_name='us-west-2',


_10


# Credentials from your AWS account


_10


aws_access_key_id='<replace_your_own_credentials>',


_10


aws_secret_access_key='<replace_your_own_credentials>',


_10


aws_session_token='<replace_your_own_credentials>',


_10


)


`


## Create embeddings for your images#


In the root of your project, create a new folder called` images` and add some images. You can use the images from the example project on[GitHub](https://github.com/supabase/supabase/tree/master/examples/ai/aws_bedrock_image_search/images) or you can find license free images on[unsplash](https://unsplash.com/) .


To send images to the Amazon Bedrock API we need to need to encode them as` base64` strings. Create the following helper methods:


`
_44


def readFileAsBase64(file_path):


_44


"""Encode image as base64 string."""


_44


try:


_44


with open(file_path, "rb") as image_file:


_44


input_image = base64.b64encode(image_file.read()).decode("utf8")


_44


return input_image


_44


except:


_44


print("bad file name")


_44


sys.exit(0)


_44


_44


_44


def construct_bedrock_image_body(base64_string):


_44


"""Construct the request body.


_44


_44


https://docs.aws.amazon.com/bedrock/latest/userguide/model-parameters-titan-embed-mm.html


_44


"""


_44


return json.dumps(


_44


{


_44


"inputImage": base64_string,


_44


"embeddingConfig": {"outputEmbeddingLength": 1024},


_44


}


_44


)


_44


_44


_44


def get_embedding_from_titan_multimodal(body):


_44


"""Invoke the Amazon Titan Model via API request."""


_44


response = bedrock_client.invoke_model(


_44


body=body,


_44


modelId="amazon.titan-embed-image-v1",


_44


accept="application/json",


_44


contentType="application/json",


_44


)


_44


_44


response_body = json.loads(response.get("body").read())


_44


print(response_body)


_44


return response_body\["embedding"\]


_44


_44


_44


def encode_image(file_path):


_44


"""Generate embedding for the image at file_path."""


_44


base64_string = readFileAsBase64(file_path)


_44


body = construct_bedrock_image_body(base64_string)


_44


emb = get_embedding_from_titan_multimodal(body)


_44


return emb


`


Next, create a` seed` method, which will create a new Supabase Vector Collection, generate embeddings for your images, and upsert the embeddings into your database:


`
_40


def seed():


_40


# create vector store client


_40


vx = vecs.create_client(DB_CONNECTION)


_40


_40


# get or create a collection of vectors with 1024 dimensions


_40


images = vx.get_or_create_collection(name="image_vectors", dimension=1024)


_40


_40


# Generate image embeddings with Amazon Titan Model


_40


img_emb1 = encode_image('./images/one.jpg')


_40


img_emb2 = encode_image('./images/two.jpg')


_40


img_emb3 = encode_image('./images/three.jpg')


_40


img_emb4 = encode_image('./images/four.jpg')


_40


_40


# add records to the *images* collection


_40


images.upsert(


_40


records=\[


_40


(


_40


"one.jpg", # the vector's identifier


_40


img_emb1, # the vector. list or np.array


_40


{"type": "jpg"} # associated metadata


_40


), (


_40


"two.jpg",


_40


img_emb2,


_40


{"type": "jpg"}


_40


), (


_40


"three.jpg",


_40


img_emb3,


_40


{"type": "jpg"}


_40


), (


_40


"four.jpg",


_40


img_emb4,


_40


{"type": "jpg"}


_40


)


_40


\]


_40


)


_40


print("Inserted images")


_40


_40


# index the collection for fast search performance


_40


images.create_index()


_40


print("Created index")


`


Add this method as a script in your` pyproject.toml` file:


`
_10


\[tool.poetry.scripts\]


_10


seed = "image_search.main:seed"


_10


search = "image_search.main:search"


`


After activating the virtual environtment with` poetry shell` you can now run your seed script via` poetry run seed` . You can inspect the generated embeddings in your Supabase Dashboard by visiting the[Table Editor](https://supabase.com/dashboard/project/_/editor) , selecting the` vecs` schema, and the` image_vectors` table.


## Perform an image search from a text query#


With Supabase Vector we can easily query our embeddings. We can use either an image as the search input or alternatively we can generate an embedding from a string input and use that as the query input:


`
_28


def search(query_term: Optional\[str\] = None):


_28


if query_term is None:


_28


query_term = sys.argv\[1\]


_28


_28


# create vector store client


_28


vx = vecs.create_client(DB_CONNECTION)


_28


images = vx.get_or_create_collection(name="image_vectors", dimension=1024)


_28


_28


# Encode text query


_28


text_emb = get_embedding_from_titan_multimodal(json.dumps(


_28


{


_28


"inputText": query_term,


_28


"embeddingConfig": {"outputEmbeddingLength": 1024},


_28


}


_28


))


_28


_28


# query the collection filtering metadata for "type" = "jpg"


_28


results = images.query(


_28


data=text_emb, # required


_28


limit=1, # number of records to return


_28


filters={"type": {"$eq": "jpg"}}, # metadata filters


_28


)


_28


result = results\[0\]


_28


print(result)


_28


plt.title(result)


_28


image = mpimg.imread('./images/' + result)


_28


plt.imshow(image)


_28


plt.show()


`


By limiting the query to one result, we can show the most relevant image to the user. Finally we use` matplotlib` to show the image result to the user.


That's it, go ahead and test it out by running` poetry run search` and you will be presented with an image of a "bike in front of a red brick wall".


## Conclusion#


With just a couple of lines of Python you are able to implement image search as well as reverse image search using the Amazon Titan multimodal model and Supabase Vector.


## More Supabase#


- [Getting started with Amazon Bedrock and vecs](https://supabase.com/docs/guides/ai/integrations/amazon-bedrock)
- [Matryoshka embeddings: faster OpenAI vector search using Adaptive Retrieval](https://supabase.com/blog/matryoshka-embeddings)
