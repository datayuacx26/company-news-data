---
schema_version: "1.0.0"
document_id: "9bf5ced3c4c983ac3790b4e1ab7df5a655a219248441fd94c26f1e8fee6728ed"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/11/27/downloading-shutterstock-images-with-the-rest-api"
published_at: "2018-11-27T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:fb39eaf29b00ec101efd08d379be24a1359cbf98817e1b28ecbb9b08ee10e6e8"
---

# Downloading Shutterstock images with the REST API

Lots of people are familiar with downloading Shutterstock images directly from shutterstock.com to use in marketing, advertising, and news content. What most of our small and medium-sized customers don’t know is that when they search for images or download images from the website, they’re using the same computer interface that our enterprise customers use to connect their huge systems and applications to our media catalog. The only difference is that enterprise customers access Shutterstock directly through a computer interface called a REST API, cutting out the need to visit shutterstock.com.


If you’re running a lot of searches or accessing a lot of media, using this REST API directly is much faster than the website. Computers can access it directly, which allows our partners to automate and scale their use of our media. For more information on how the API allows rapid scaling and growth, see[The 5 Ways Creative Platforms Use the Shutterstock API to Drive Growth](https://www.shutterstock.com/blog/5-ways-shutterstock-api-drive-growth) .


The API is better suited for use by computer programs because it delivers images, search results, and other information in formats that programs understand. For example, we use our own API to connect with plugins for Adobe Photoshop® and Microsoft PowerPoint® so you can use our media directly in your work. For some examples of how these API integrations help us build partnerships with our customers, see[How Shutterstock Creates API Partnerships that Last](https://www.shutterstock.com/blog/shutterstock-api-partnerships-last) .


If you’ve got a website, program, mobile app, or other application that needs Shutterstock media, or if you just want to try it out, you can use the API to access that media directly. Here’s how:


There are three main steps to getting images from the API:


- Authenticating to Shutterstock
- Searching for images
- Licensing and downloading images


Each step uses *REST resources* , which represent things like images and licenses, and *REST endpoints* , which are URLs that can accept requests like “search for images” and “request a license for an image.” Most programming languages have tools that can access REST resources and endpoints, which makes it easy to access APIs like the Shutterstock API from your applications. There’s lots of info online about accessing REST APIs if you need guidelines for your platform, and reference information about the Shutterstock REST API is available on the[Developer portal](https://developers.shutterstock.com/documentation) .


This diagram shows the main endpoints that you use to authenticate to the server, search, and download images. These endpoints (and many more) are covered in the[REST API reference](https://api-reference.shutterstock.com/) , and we’ll give examples of how to use each of these endpoints as we walk through the process.


## Step 1: Authenticating to Shutterstock


Instead of using your shutterstock.com username and password, the REST API uses an access token that is tied to your account and to an application. This application represents the application or program that you’re using to access the API. That application can be a big, complicated program, or some simple commands like the examples here. Here’s how to get a token:


1.


Create an account at[https://www.shutterstock.com](https://www.shutterstock.com/) if you don’t already have one.


2.


Create an application at[https://developers.shutterstock.com/applications](https://developers.shutterstock.com/applications) and note the Client ID and Client Secret, which are like an ID and password for your application.


3.


Give the application a callback host name. If you’ve got an application running on a server, use the host name of the server. Otherwise, leave the default host name` localhost` for testing purposes. (You can get an access token on this page, but that’s not how programs and apps do it.)


4.


Pass your application information to the` /v2/oauth/authorize` endpoint to get a temporary authentication code. Here’s an example using the Linux and Mac` curl` command:


```text
curl "https://api.shutterstock.com/v2/oauth/authorize" \
-X GET \
-G \
--data-urlencode "scope=licenses.create licenses.view purchases.view" \
--data-urlencode "state=demo_`date +%s`" \
--data-urlencode "response_type=code" \
--data-urlencode "redirect_uri=http://localhost:3000/callback" \
--data-urlencode "client_id=860bde70bb335163e2e4"
```


This command includes your Client ID, a callback URI, and some other information. This callback URI must use a host name that you set up in your application. (Again, for testing purposes, you can use “localhost,” as in` http://localhost:3000/callback` .) The` scope` field is a list of permissions for the access token. (For more information on scope, see[OAuth scopes](https://developers.shutterstock.com/documentation/authentication#oauth-scopes) .) The` state` parameter is a field that the endpoint passes back to your application so you can include other information or verify that the request worked.


Again, most programming languages have libraries that allow you to make HTTP and HTTPS requests to REST APIs. The documentation for your platform will be helpful here.


The endpoint returns a 301 return code and a URL. Here’s an example:


```text
Moved Temporarily. Redirecting to https://accounts.shutterstock.com/login?next=%2Foauth%2Fauthorize%3Fscope%3Dlicenses.create%20licenses.view%20purchases.view%26state%3Ddemo_1498496256%26response_type%3Dcode%26redirect_uri%3Dhttp%3A%2F%2Flocalhost%3A3000%2Fcallback%26client_id%3D860bde70bb335163e2e4%26realm%3Dcustomer
```


If you don’t want to use the command line, try a REST API client like[Postman](https://www.getpostman.com/) .


5.


Open the URL in a web browser, log in, and allow your Shutterstock account to access the callback host name. The Permission Request window lets you make sure that you want to let programs with the token access your Shutterstock account:


When you click Allow, the Shutterstock API redirects your web browser to the callback URL with information in the parameters. If you don’t have a full application set up yet, the browser gives an error because the web page isn’t available, but that’s OK because for now, all you need is the URL. Here’s an example:


```text
http://localhost:3000/callback?code=VaRLQ3rICmWjGr4ciI-GwR&state=demo_1498496256
```


For testing the API, copy the authentication code from the URL (in the previous example, it’s` VaRLQ3rICmWjGr4ciI-GwR` ) and use it in the next step. When you’re ready to code your own application, you can set it up to embed or refer to the login web page, accept the request from this URL, and store the information from the URL parameters. This code can be used only once, and it expires after 5 minutes.


6.


Finally, authenticate to the API and get an access token. Use the` /v2/oauth/access_token` endpoint, as in this example:


```text
curl "https://api.shutterstock.com/v2/oauth/access_token" \
-X POST \
--data-urlencode "client_id=860bde70bb335163e2e4" \
--data-urlencode "client_secret=225d245d28e5b1a37db7fd4ceb8cdf360a3ae5a7" \
--data-urlencode "grant_type=authorization_code" \
--data-urlencode "code=VaRLQ3rICmWjGr4ciI-GwR"
```


The parameters for this endpoint include the application’s Client ID, Client Secret, and the code you got from the URL in the previous step.


This endpoint returns the access token in a JSON response:


```text
{"access_token":"v2/ODYwYmRlNzBiYjMzNTE2M2UyZTQvMTc4NzI2OTM4L2N1c3RvbWVyLzIvWEtXR01HQ1FaVHRLOG85a","token_type":"Bearer"}
```


Now you can use the token to access the API by passing it as an authorization header, as you’ll see when you start searching for images. You’ll want to store the token text (the part that starts with` v2/` , not the whole JSON response) in a variable to use it easily. Keep this token private, though, because other people could use it to access your subscriptions and media.


## Step 2: Searching for images


If you’re familiar with searching for images on shutterstock.com, using the REST API is similar. The` /v2/images/search` endpoint is the main way to find images based on keywords, categories, and sizes.


1.


Search for one or more images with the` /v2/images/search` endpoint. Here’s an example that searches for photos of kites:


```text
curl "https://api.shutterstock.com/v2/images/search" \
-X GET \
-G \
--data-urlencode "query=kites" \
--data-urlencode "image_type=photo" \
--data-urlencode "page=1" \
--data-urlencode "per_page=5" \
--data-urlencode "sort=popular" \
--data-urlencode "view=minimal" \
--header "Authorization: Bearer $SHUTTERSTOCK_API_TOKEN"
```


In this example, the access token is stored in the variable` $SHUTTERSTOCK_API_TOKEN` . We’re using the search keyword “kites” and selecting the first 5 results, sorted by popularity. The JSON results include info about the search results, including links to thumbnail images and the dimensions of the full image. Most importantly, the results include the IDs of the images, which you’ll need to download full images.


Here’s an abbreviated example of what the search results look like:


```text
{
"page": 1,
"per_page": 5,
"total_count": 31595,
"search_id": "iBh7nPL1MQj3qlqZtciKlA",
"data": [
{
"id": "59656357",
"aspect": 1.5,
"assets": {
"preview": {
"height": 300,
"url": "https://image.shutterstock.com/display_pic_with_logo/582721/582721,1282308803,4/stock-photo-family-having-fun-with-kite-in-sand-dunes-59656357.jpg",
"width": 450
},
"small_thumb": {
"height": 67,
"url": "https://thumb1.shutterstock.com/thumb_small/582721/582721,1282308803,4/stock-photo-family-having-fun-with-kite-in-sand-dunes-59656357.jpg",
"width": 100
}
},
"contributor": {
"id": "582721"
},
"description": "Family Having Fun With Kite In Sand Dunes",
"image_type": "photo",
"media_type": "image"
},
{"id": "1212121212"},
{"id": "3434343434"},
{"id": "5656565656"},
{"id": "6767676767"}
]
}
```


Most programming languages have libraries for working with JSON code like this and putting it into usable objects. From there, you can grab the URLs and use the preview images or thumbnails. Here’s the watermarked[preview image](https://image.shutterstock.com/display_pic_with_logo/582721/582721,1282308803,4/stock-photo-family-having-fun-with-kite-in-sand-dunes-59656357.jpg) :


For examples of running this command in other programming languages, see the[API reference](https://api-reference.shutterstock.com/) .


2.


(Optional) Get further details about one image with the` /v2/images/{id}` endpoint. For example, we picked out an image with the ID 59656357 and got full details about it with this command:


```text
curl "https://api.shutterstock.com/v2/images/59656357" \
-X GET \
--data-urlencode "view=full" \
--header "Authorization: Bearer $SHUTTERSTOCK_API_TOKEN"
```


That’s all there is to searching for images with the API. The reference for the` /v2/images/search` endpoint has many more fields to search on, and other endpoints like` /v2/images/{id}/similar` and` /v2/images/recommendations` get images in different ways. For full details, see the[images resource](https://api-reference.shutterstock.com/#images) .


## Step 3: Licensing and downloading images


To request a license and download images from Shutterstock, you need:


- A[subscription](https://www.shutterstock.com/subscribe/) .
- An account with full API access. As of February 2019, by default, applications (“free API accounts”) have limited access to the API. Free API accounts get access to a limited media library, not the complete Shutterstock library. They also can’t license media, even if they are connected to an account with a paid subscription. For more information, or to get full API access, see[Accounts and limitations](https://developers.shutterstock.com/documentation/authentication#accounts-and-limitations) .


1.


Look up your subscription ID from your account, or get it from the` /v2/user/subscriptions` endpoint like this:


```text
curl "https://api.shutterstock.com/v2/user/subscriptions" \
-X GET \
--header "Authorization: Bearer $SHUTTERSTOCK_API_TOKEN"
```


2.


Pass your subscription ID and the image ID to the` /v2/images/licenses` endpoint to request a license and download link.


For this POST request, encode the image ID or IDs in JSON, but put the subscription ID into the URL. Here’s an example for one image. In this case, we’ve put our subscription ID in a variable and added it to the URL. We’ve also embedded the JSON string in the` curl` command; your programming language probably has a way of reading that JSON string from a file or assembling it in your code and including it in the request.


```text
curl "https://api.shutterstock.com/v2/images/licenses?subscription_id=$subscription_id" \
-X POST \
--header "Authorization: Bearer $SHUTTERSTOCK_API_TOKEN" \
--header "Content-Type: application/json" \
--data '{ \
"images": [ \
{ "image_id": "59656357" } \
] \
}'
```


The response includes the download link for the full image, as in this example:


```text
{
"data": [
{
"image_id": "59656357",
"download": {
"url": "https://download.shutterstock.com/gatekeeper/1234abcd/shutterstock_59656357.jpg"
},
"allotment_charge": 1
}
]
}
```


Open the URL, and congratulations, here’s the full image!


Now the image is available for your use (according to the terms of the license, of course). Some subscription types allow you to download the image again later by using the` /v2/images/licenses/{id}/downloads` endpoint.


## What’s next


That’s all you need to get started. From here, you can use all kinds of programming languages to access Shutterstock through the API. For more information about the API, see[https://developers.shutterstock.com](https://developers.shutterstock.com/) , and the endpoint reference and code samples in the[API reference](https://api-reference.shutterstock.com/) .


Lots of example code is available on GitHub at[https://github.com/shutterstock](https://github.com/shutterstock) , such as an example of how to authenticate with the API in JavaScript:[shutterstock-oath-example](https://github.com/shutterstock/shutterstock-oauth-sample) .
