---
schema_version: "1.0.0"
document_id: "35efe75501a5876298cb1194aa3039ad63b49b94aa979460ec45d40565940977"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/05/09/licensing-with-shutterstock-api-subscriptions"
published_at: "2019-05-09T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:0f9df11d19d6bbfbe21aa1ef5ced77f235cba1ee26ed26ad7a69222e7aec00fa"
---

# Licensing Images with Shutterstock API Subscriptions

Now that we offer[API-specific subscriptions](https://www.shutterstock.com/api/pricing) , you can get straightforward access to our media library from almost any programming language or client. These subscriptions include API access, so you can get started using our media in your applications in just a few minutes. There’s even a free option if you want to try things out.


Here’s a walkthrough of how to get an API subscription, use it to search for media, and (if your subscription allows) license and download media, all through the API.


## Setting Up an API Subscription


To get started with the API, go to[https://www.shutterstock.com/api/pricing](https://www.shutterstock.com/api/pricing) and pick a plan, or[contact us](https://developers.shutterstock.com/contact-us) for a customized subscription to fit the needs of your business. You can select the “Free Account” plan to give the API a try for no cost; you’ll be able to search for media but not license or download media. For a summary of what each type of API subscription provides, see[Subscriptions](https://api-reference.shutterstock.com/#subscriptions) in the[API reference](https://api-reference.shutterstock.com/) .


After you pick your plan, you’re sent to the[My Apps page](https://developers.shutterstock.com/user/login) . From this page, you can create an app or select one if you already have one. You’ll need the app’s client ID and client secret, which are like an ID and password for your application.


For a walkthrough of how to use your app information to authenticate to the API, search for images, and license and download images with the API, see[Downloading Shutterstock images with the REST API](https://tech.shutterstock.com/2018/11/27/downloading-shutterstock-images-with-the-rest-api) .


In short, you’ll follow these steps:


1. Get your application’s client ID and client secret.
2. Authenticate to Shutterstock to get a token.
3. Use the token in your requests to search for media, create collections, and get information about media.


Now that you’re set up with an API subscription (or free plan), an application, and a token, you can use the API with any client that can make HTTPS requests, including most programming languages, the cURL program, and Shutterstock’s[JavaScript SDK](https://tech.shutterstock.com/2019/04/09/reduce-integration-time-javascript-sdk-shutterstock-api) .


## Licensing and Downloading Images


If your subscription includes the ability to license and download images, you can do so with a single API command. Before trying to license a piece of media, make sure that you can access it through the API, because media that you see on shutterstock.com may not necessarily be available through your API subscription. Before you license images, you’ll need:


- An API subscription
- An application
- An authentication token with the licenses.create and purchases.view scopes; see[OAuth authentication](https://api-reference.shutterstock.com/#authentication-oauth-authentication)
- The ID of the image that you want to license


When you have this information, you can send a request to the` POST /v2/images/licenses` endpoint to license an image and get a download link. For this request, encode the image ID in the JSON body of the request.


A few things are different about licensing images with an API subscription as opposed to other types of Shutterstock subscription:


- Licensing requests that use an API subscription must include the` price` and` metadata.customer_id` fields. These fields are for resellers to set the ID of the customer that they are reselling the image to and the price that they charged. If you’re not reselling the image, put 0 in the` price` field and any value in the` metadata.customer_id` field.
- Licensing requests that use an API subscription don’t have to include the subscription ID in the request. The API automatically uses the subscription that is tied to the account you used to get the token.


Here’s an example that gets a license for an image. This example assumes that the authentication token is in the` $SHUTTERSTOCK_API_TOKEN` environment variable.


```text
DATA='{
"images": [
{
"price": 12.50,
"image_id": "59656357",
"metadata": {
"customer_id": "12345"
}
}
]
}'


curl -X POST 'https://api.shutterstock.com/v2/images/licenses' \
-H "Authorization: Bearer $SHUTTERSTOCK_API_TOKEN" \
-H 'Content-Type: application/json' \
-d "$DATA"


```


The response includes a download link to the full version of the image:


```text
{
"data": [
{
"image_id": "59656357",
"download": {
"url": "https://download.shutterstock.com/gatekeeper/[random-characters]/shutterstock_59656357.jpg"
},
"allotment_charge": 1
}
]
}


```


For examples in other languages, see[Licensing and downloading](https://api-reference.shutterstock.com/#licensing-and-downloading) .


## What’s Next


That’s all you need to get started. From here, you can use all kinds of programming languages to access Shutterstock through the API. For more information about the API, see[https://developers.shutterstock.com](https://developers.shutterstock.com/) and the endpoint reference and code samples in the[API reference](https://api-reference.shutterstock.com/) .
