---
schema_version: "1.0.0"
document_id: "ca4027ac8ed852f5a82eb800da8bf9378a24acfa335893cd7082788f37529381"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2020/03/23/computer-vision"
published_at: "2020-03-23T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:48.304506+00:00"
content_hash: "sha256:6f3ebdf529f03266d03122429fc8ed5d22b7a361c91256dcfec7ffa5b1fb8999"
---

# Computer Vision API now Available to All Accounts

We use some powerful search technology to get customers to the media that they want, and a lot of that runs under the covers. However, we’re now making some of our computer vision-based search tools available to all users of our API so you can try them out with no commitment and make use of them in your applications.


That means that all Shutterstock API accounts, even free accounts, can use search features that are based on a neural net that we trained on our image and video library:


- Reverse image search: The API provides images and videos that are visually similar to a reference image that you supply or to an image in our library.
- Keyword suggestion: The API returns a list of keywords based on things and concepts that it “sees” in the image.


Competitors at[ConUHack](https://conuhacks2020.devpost.com/) , Concordia University’s official annual hackathon, came up with some interesting applications of our API, and the winner was[LingoExplore](https://devpost.com/software/lingoexplore) , a nifty language-learning app that uses the keyword suggestion feature to guide a user’s scavenger hunt in a new language. See[Shutterstock ConUHack Winner: Interactive Language Learning with Computer Vision](https://www.shutterstock.com/blog/conuhacks-shutterstock-api)


There’s no end to the things that you can do with computer vision, so we’re looking forward to hearing about what people do with it!


Here’s how to give computer vision search a try for free, with no commitment:


## Signing up for a free account and application


To get started with the API, go to[https://www.shutterstock.com/api/pricing](https://www.shutterstock.com/api/pricing) and pick a plan, or[contact us](https://developers.shutterstock.com/contact-us) for a customized subscription to fit the needs of your business. You can select the “Free Account” plan to give the API a try for no cost; you’ll be able to search for media and use the computer vision endpoints but not license or download media. For a summary of what each type of API subscription provides, see[Subscriptions](https://api-reference.shutterstock.com/#subscriptions) in the[API reference](https://api-reference.shutterstock.com/) .


After you pick your plan, you’re sent to the[My Apps page](https://www.shutterstock.com/account/developers/apps) . From this page, you can create an app or select one if you already have one. You’ll need the app’s client ID and client secret, which are like an ID and password for your application.


For a walkthrough of how to use your app information to authenticate to the API, search for images, and license and download images with the API, see[Downloading Shutterstock images with the REST API](https://tech.shutterstock.com/2018/11/27/downloading-shutterstock-images-with-the-rest-api) .


Free accounts can make a limited number of requests per minute. If the API returns an error response with the status code 429, your application has exceeded its limit.


## Uploading images


After you have an account and an application, the next step in using the computer vision endpoints is to upload a reference image. You can’t upload videos to the computer vision endpoints, so if you want to use a video, take a representative frame from that video and use it as a reference image.


The images that you upload to the computer vision endpoints must be:


- In JPG or PNG format
- No larger than 25MB
- No larger than 10,000 pixels in width or height
- Base 64 encoded


There’s no official minimum size, but the image should have enough detail that the figure or object that you’re referencing is clearly visible.


Regardless of whether you want to get similar images or videos or get keywords from an image, the process of uploading the image is the same. Using any programming language, encode the image in base 64 and send it as a POST request to` https://api.shutterstock.com/v2/cv/images` .


For example, I wondered what the computer vision endpoints would see in this photo of Hilde, my plush hedgehog:


This Linux command-line example uses the` base64` command to encode Hilde’s picture and the` curl` command to to send it. It assumes that your app’s client ID and client secret are` 123abc456def` and` 1a2b3c4d` . Note that these two fields are separated by a colon and used as the value of the` --user` parameter:


```text
curl  -X   POST  'https://api.shutterstock.com/v2/cv/images'    \
--user   123abc456def:1a2b3c4d  \
-H    'Content-Type: application/json'    \
-d    "{  \"  base64_image  \"  :  \"  `  base64   hedgehog.jpg `  \"  }"


```


The response includes the upload ID for the image:


```text
{
"upload_id"  :     "Uc92bf0b79797b6bc404b597cc6ddda68"
}


```


If you’re using JavaScript code, you can use the[Shutterstock JavaScript SDK](https://github.com/shutterstock/public-api-javascript-sdk) , which includes commands for the computer vision endpoints. This example uses the` computerVisionApi.uploadImage()` method to upload the reference image:


```text
const    sstk    =    require  (  "  shutterstock-api  "  );


const    applicationClientId    =    "  123abc456def  "  ;
const    applicationClientSecret    =    "  1a2b3c4d  "  ;
sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  "  ./hedgehog.jpg  "  );
const    base64File    =    Buffer  .  from  (  imageFile  ).  toString  (  "  base64  "  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  data  .  upload_id  );
});


```


For more information about this endpoint, see[POST /v2/cv/images](https://api-reference.shutterstock.com/?shell#computer_vision-upload-images) in the Shutterstock API reference.


## Using reverse image search


First, let’s see what visually similar images the Shutterstock API can find for this portrait of Hilde. I’ve uploaded her picture to the` POST /v2/cv/images` endpoint and gotten the upload ID, so now I can send that ID to the` GET /v2/cv/similar/images` endpoint, as in this example:


```text
curl  -X   GET https://api.shutterstock.com/v2/cv/similar/images  \
-H    "Accept: application/json"    \
--user   123abc456def:1a2b3c4d  \
-G    \
--data-urlencode    "asset_id=Uc92bf0b79797b6bc404b597cc6ddda68"


```


To get videos that are similar to a reference image, send the upload ID to the` GET /v2/cv/similar/videos` endpoint, as in this example:


```text
curl  -X   GET https://api.shutterstock.com/v2/cv/similar/videos  \
-H    "Accept: application/json"    \
--user   123abc456def:1a2b3c4d  \
-G    \
--data-urlencode    "asset_id=Uc92bf0b79797b6bc404b597cc6ddda68"


```


To use one of our images as a reference image, use that image’s ID number as the value of the` asset_id` in the request.


The response includes a list of images or videos that are visually similar:


```text
{
"data"  :     [
{
"contributor"  :     {
"id"  :     "166260988"
},
"id"  :     "622747925"  ,
"media_type"  :     "image"  ,
"aspect"  :     0.6667  ,
"assets"  :     {
"large_thumb"  :     {
"height"  :     150  ,
"url"  :     "https://thumb9.shutterstock.com/thumb_large/166260988/622747925/stock-photo-hand-sewn-doll-622747925.jpg"  ,
"width"  :     100
},
"preview"  :     {
"height"  :     450  ,
"url"  :     "https://image.shutterstock.com/display_pic_with_logo/166260988/622747925/stock-photo-hand-sewn-doll-622747925.jpg"  ,
"width"  :     300
}
},
"description"  :     "Hand sewn doll"  ,
"has_model_release"  :     false  ,
"image_type"  :     "illustration"
},
{
"id"  :     "1309903591"  ,
"aspect"  :     0.75  ,
"assets"  :     {
"preview"  :     {
"url"  :     "https://image.shutterstock.com/display_pic_with_logo/208284063/1309903591/stock-photo-japanese-lucky-cat-1309903591.jpg"  ,
"width"  :     337  ,
"height"  :     450
},
"small_thumb"  :     {
"url"  :     "https://thumb7.shutterstock.com/thumb_small/208284063/1309903591/stock-photo-japanese-lucky-cat-1309903591.jpg"  ,
"width"  :     75  ,
"height"  :     100
},
"large_thumb"  :     {
"url"  :     "https://thumb7.shutterstock.com/thumb_large/208284063/1309903591/stock-photo-japanese-lucky-cat-1309903591.jpg"  ,
"width"  :     113  ,
"height"  :     150
}
},
"contributor"  :     {
"id"  :     "208284063"
},
"description"  :     "japanese lucky cat."  ,
"image_type"  :     "illustration"  ,
"has_model_release"  :     false  ,
"media_type"  :     "image"
},
{
"contributor"  :     {
"id"  :     "168430588"
},
"id"  :     "1242489100"  ,
"media_type"  :     "image"  ,
"aspect"  :     1.489  ,
"assets"  :     {
"large_thumb"  :     {
"height"  :     101  ,
"url"  :     "https://thumb1.shutterstock.com/thumb_large/168430588/1242489100/stock-photo-different-textile-handmade-felt-toys-hares-rabbits-harts-birds-on-christmas-tree-in-paper-box-1242489100.jpg"  ,
"width"  :     150
},
"preview"  :     {
"height"  :     302  ,
"url"  :     "https://image.shutterstock.com/display_pic_with_logo/168430588/1242489100/stock-photo-different-textile-handmade-felt-toys-hares-rabbits-harts-birds-on-christmas-tree-in-paper-box-1242489100.jpg"  ,
"width"  :     450
}
},
"description"  :     "Different textile handmade felt toys - hares, rabbits, harts, birds on christmas tree in paper box. Gift for christmas holidays."  ,
"has_model_release"  :     false  ,
"image_type"  :     "illustration"
}
],
"search_id"  :     ""  ,
"total_count"  :     1000  ,
"page"  :     1  ,
"per_page"  :     20
}


```


In this case, the examples include photos of dolls that look like Hilde. Here’s a photo of a doll that shows the wood background and light red color of the table runner in my picture, as well as Hilde’s dark prickles in the doll’s dark hair:


It also returned this image of a *manekineko* , or a Japanese welcoming cat, that has a similar color scheme and pose to my picture:


Here’s a photo that includes the colors in my picture in a different way, with brown boxes, red bows, and white plush rabbits:


You can also use the Shutterstock JavaScript SDK, as in this example, which uploads the image and gets similar images with the` computerVisionApi.getSimilarImages()` method:


```text
const    sstk    =    require  (  "  shutterstock-api  "  );


const    applicationClientId    =    "  123abc456def  "  ;
const    applicationClientSecret    =    "  1a2b3c4d  "  ;
sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  "  ./hedgehog.jpg  "  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  data  .  upload_id  );
return    computerVisionApi  .  getSimilarImages  (  data  .  upload_id  );
})
.  then  ((  data  )    =>    {
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


For more information about these endpoints, see[GET /v2/cv/similar/images](https://api-reference.shutterstock.com/?shell#computer_vision-list-similar-images) and[GET /v2/cv/similar/videos](https://api-reference.shutterstock.com/?shell#computer_vision-list-similar-videos) in the[API reference](https://api-reference.shutterstock.com/) .


## Getting keywords for an image


Another computer vision feature that we’ve recently made public is the ability to get keywords that are based on a reference image.


The applications for keyword suggestion are endless because you can use it to do much more than get visually similar images and videos; it can tell you what is in an image and what people might think of when they look at that image. The keyword suggestion is a lot smarter than just identifying things that are in an image — it can return moods, themes, and concepts that are related to an image.


That makes the applications for keyword suggestion endless. You can use it to:


- Help sort photos by the things in them and to get related images not just from our catalog but from your own collection.
- Check images for inappropriate content.
- Search your library for new uses for old media.
- Train your own AI models to recognize images.


To get keywords for an image, get the upload ID for the reference image in the same way and then send the upload ID to the[GET /v2/cv/keywords](https://api-reference.shutterstock.com/?shell#computer_vision-list-suggested-keywords) endpoint. This Linux command-line example uploads the reference image, uses the` jq` program to get the upload ID from the response, and sends the upload ID to the` GET /v2/cv/keywords` endpoint:


```text
RESPONSE  =  $(  curl  -X   POST  'https://api.shutterstock.com/v2/cv/images'    \
--user   123abc456def:1a2b3c4d  \
-H    'Content-Type: application/json'    \
-d    "{  \"  base64_image  \"  :  \"  `  base64   hedgehog.jpg `  \"  }"  )


echo    "The next step requires the jq program."


UPLOAD_ID  =  $(  jq  -r   .upload_id  <<<    $RESPONSE  )


curl  -X   GET https://api.shutterstock.com/v2/cv/keywords  \
-H    "Accept: application/json"    \
--user   123abc456def:1a2b3c4d  \
-G    \
--data-urlencode    "asset_id=  $UPLOAD_ID  "


```


The response includes a list of keywords that are related to that image. Here’s what came back for the picture of Hilde:


```text
{
"data"  :     [
"cute"  ,
"toy"  ,
"decoration"  ,
"background"  ,
"christmas"  ,
"red"  ,
"white"  ,
"holiday"  ,
"fun"  ,
"celebration"  ,
"beautiful"  ,
"funny"  ,
"gift"  ,
"art"  ,
"design"  ,
"winter"  ,
"isolated"  ,
"doll"  ,
"happy"  ,
"season"  ,
"animal"  ,
"color"  ,
"handmade"  ,
"object"  ,
"button"  ,
"girl"  ,
"festive"  ,
"homemade"  ,
"hobby"  ,
"pink"
]
}


```


Aside from identifying colors and items in the picture, the API can tell that Hilde is cute, a toy, and an animal. It also got Christmas, festive, and happy vibes from the picture.


Here’s an example that uses the JavaScript SDK and the` computerVisionApi.getKeyword()` method:


```text
const    sstk    =    require  (  "  shutterstock-api  "  );
const    fs    =    require  (  "  fs  "  );


const    applicationClientId    =    "  123abc456def  "  ;
const    applicationClientSecret    =    "  1a2b3c4d  "  ;
sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  "  ./hedgehog.jpg  "  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  data  .  upload_id  );
return    computerVisionApi  .  getKeywords  (  data  .  upload_id  );
})
.  then  ((  data  )    =>    {
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


That’s all you have to do to use our computer vision API endpoints and incorporate them into your application. We hope you’ll give these features a try and let us know how they’re useful to you!
