---
schema_version: "1.0.0"
document_id: "96f1eb068a6f5b81b4e670dc27ea1cd63bf30e932cad9d5a4b871017a5cb29df"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2020/07/13/computer-vision-tutorial"
published_at: "2020-07-13T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:46.047008+00:00"
content_hash: "sha256:d53635ea88e924f5d309cb57fd1cc54940744176e65c8ab31ef58c34ca9b5eb1"
---

# Tutorial: Using the computer vision features of the Shutterstock API

Here’s a tutorial that walks you through a simple scenario of setting up a free Shutterstock account and writing a Node.JS application to use the computer vision (CV) features of the Shutterstock API. You’ll use the API to search for images and then upload pictures of your own to use with reverse image search. Finally, you’ll use CV keyword suggestion to identify objects in photos.


At the end of this tutorial, you’ll have a starter application that you can use to work with the API and try out its features. You’ll be able to see how applications search the API and use reverse image search to get information about any image.


## Before you begin


To run the examples in this tutorial, you need Node.JS. You can download and install it at[http://nodejs.org](http://nodejs.org/) .


In the tutorial you’ll work with JavaScript code. All the code is provided for you, so you don’t need experience with JavaScript or Node.JS. You just need to be able to copy and paste example code into a text editor.


The tutorial also requires a few command-line commands, and these commands are provided, so you don’t need much experience working with the command line. You need to be able to open the command-line window, change folders, and paste and run commands. For information about using the command line on your system, see the documentation for your operating system.


## Limitations of free accounts


Here are a few things to note about using the API with free accounts:


- The API has a rate limit for general requests and a separate rate limit for computer vision requests. In particular, free accounts can make 5 computer vision requests per minute. If you exceed this rate limit, the API returns a 429 error code, so if you see this error, wait a minute and try again.
- Free accounts do not have access to our full media library, so if you search for an image on shutterstock.com, you may not be able to find it through the API. Paid accounts get access to larger collections of media.


For more information about what free and paid accounts can do, see[Pricing](https://www.shutterstock.com/api/pricing) .


## Steps


This tutorial follows these general steps:


- **Step 1: Set up a free account and API application** : In this step you sign up for a Shutterstock account if you don’t have one already and register an application for the API, which authorizes you to use the API.
- **Step 2: Set up a project** : In this step you create a simple Node.JS application to hold the code that accesses the API and the images that you upload.
- **Step 3: Search for images** : In this step you start with a simple use case for the API: searching for images.
- **Step 4: Get images that are similar to a reference image** : In this step you upload an image and get reverse image search results.
- **Step 5: Get keywords for an image** : In this step you use the keyword suggestion feature of the API to identify objects in pictures.


## Step 1: Set up a free account and API application


To use the API, at minimum, you need a free account and an API application. The API application represents the computer code or program that will access the API. With the free account and app, you can use some features of the API like image and video search, but not others, like licensing and downloading images.


1. In a web browser, go to[https://shutterstock.com](https://shutterstock.com/) , click **Sign up** , and follow the steps to create an account. If you’ve already got a Shutterstock customer account, whether you have an active subscription or not, you can use that account.
2. Go to[https://www.shutterstock.com/account/developers/apps](https://www.shutterstock.com/account/developers/apps) .
3. Click **Create new app** and give the app a name, such as “My tutorial app.”
4. In the **Callback URL** field, enter` localhost` . This field lists the URLs that the application is running on; in this case, use` localhost` for local testing on your computer.
5. Leave the **Referrer** field blank.
6. Under “Included products,” make sure that Computer Vision and Self Serve are selected. These products represent areas of functionality that your application can use.
7. Enter a company name, web site, intended use, and description for your application. For the purposes of the tutorial, you can use “Tutorial application” for the company name, “http://test.com” for the web site, and “Personal, academic, or hackathon project” for the intended use.
8. Read and agree to the terms of service.
9. Click **Save** to create the application.
10. In the new application, note the consumer key and consumer secret.


The Create new app page looks like this:


The consumer key and secret work like a user name and password for your application, so don’t share them with anyone.


For local work like this tutorial, you can authenticate to the API with this key and secret. In a production application, you use the key and secret to get a token to use with the API so you don’t have to use your key and secret everywhere. Most API commands, including the commands that are used in this tutorial, accept key and secret authentication (also called “HTTP basic authentication”), but some require Oauth token authentication instead. For more information, see[Authentication](https://api-reference.shutterstock.com/?javascript--nodejs#authentication) in the Shutterstock API reference.


## Step 2: Set up a project


Most programming languages include commands to access APIs, so you can use almost any programming language that you are familiar with to access the API.


For a straightforward way to use the API, follow these steps to set up a Node.JS application:


1. Open a command-line window on your computer:


- For Windows computers, press the Windows key, type` cmd` , and press Enter.
- For Apple computers, search for the Terminal application.


2. Verify that Node.JS is installed by typing the command` node -v` and pressing Enter. If Node is installed correctly, the command line prints the version of Node that is installed. If you see an error, see the documentation for Node.JS on your platform to troubleshoot.
3. Verify that the npm program is installed by running` npm -v` . This program is part of the Node.JS framework and is usually installed along with Node itself.
4. Create a folder and turn it into a Node project by running these commands one at a time:


- ` mkdir api-tutorial`
- ` cd api-tutorial`
- ` npm init --yes`


The` npm init --yes` command creates a file named` package.json` and prints its contents. This file contains information about your project. You don’t need to work with this file directly in this tutorial, but if you decide to continue with the tutorial, you can put information about your project in the file. Larger projects store information about project dependencies and commands in this file.


5. Install the Shutterstock API JavaScript SDK in your project with this command:


- ` npm install --save shutterstock-api` .


The JavaScript SDK is a library of commands that you can use in JavaScript files, including the files in this Node.JS project. These commands make it more convenient to use the Shutterstock API because you don’t have to manage individual requests to the API. If you prefer, you can use almost any programming language that can handle HTTP requests to send requests to the API. See the[API reference](https://api-reference.shutterstock.com/) for more information and for examples in JavaScript, PHP, and cURL.


Now you can send requests to the Shutterstock API from this project.


## Step 3: Search for images


To test that your setup works correctly, try using the API’s image search with the` ImagesApi.searchImages()` command. This command accepts an object with the search parameters, including the` query` parameter that represents the search text. The SDK’s search command uses the same parameters as the API, so for information about the valid parameters and their types, you can refer to either the API reference or the SDK reference. Here’s where to look for the parameters for the image search command:


- SDK:[https://github.com/shutterstock/public-api-javascript-sdk/blob/master/docs/ImagesApi.md#searchImages](https://github.com/shutterstock/public-api-javascript-sdk/blob/master/docs/ImagesApi.md#searchImages)
- API:[https://api-reference.shutterstock.com/?javascript--nodejs#images-search-for-images](https://api-reference.shutterstock.com/?javascript--nodejs#images-search-for-images)


Here’s how you can search for images and verify that you’re set up correctly:


1. Open any text editor, paste this example code, and save it in your project’s folder with a name like` search.js` :


```text
const    sstk    =    require  (  "  shutterstock-api  "  );


const    applicationClientId    =    "  12345-abcde-67890-fghij-09876-edcba  "  ;
const    applicationClientSecret    =    "  edcba-09876-fghij-67890-abcde-12345  "  ;
sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    imagesApi    =    new    sstk  .  ImagesApi  ();


const    queryParams    =    {
"  query  "  :    "  hot air balloon  "  ,
"  sort  "  :    "  popular  "  ,
"  orientation  "  :    "  horizontal  "
};


imagesApi  .  searchImages  (  queryParams  )
.  then  (({    data    })    =>    {
data  .  forEach  ((  result  )    =>    {
console  .  log  (  `  ${  result  .  description  }  :\n  ${  result  .  assets  .  large_thumb  .  url  }  \n`  );
});
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


2. Change the value of the` applicationClientId` variable from “12345-abcde-67890-fghij-09876-edcba” to your app’s consumer key, in quotes.
3. Change the value of the` applicationClientSecret` variable from “edcba-09876-fghij-67890-abcde-12345” to your app’s consumer secret, in quotes.
4. Save the file.
5.


To run the script, run this command from the project folder in your command-line window:


```text
node search.js


```


This search returns popular landscape-oriented images of hot air balloons and prints out the title of each image and a link to a thumbnail:


```text
Retro hot air balloon sky background old paper texture. Vintage texture grouped and easy removable:
https://thumb9.shutterstock.com/thumb_large/221395/87797209/stock-vector-retro-hot-air-balloon-sky-background-old-paper-texture-vintage-texture-grouped-and-easy-removable-87797209.jpg


Hot Air Balloons with Sunrise applying Retro Instagram Style Filter:
https://thumb9.shutterstock.com/thumb_large/442570/258790112/stock-photo-hot-air-balloons-with-sunrise-applying-retro-instagram-style-filter-258790112.jpg


hot air balloon is flying at sunrise:
https://thumb7.shutterstock.com/thumb_large/2507905/365963861/stock-photo-hot-air-balloon-is-flying-at-sunrise-365963861.jpg


```


You can adapt this code to search for anything you want in the Shutterstock API.


For example, to search for videos, you can replace the images search command with the` VideosApi.searchVideos()` command:


```text
const    sstk    =    require  (  "  shutterstock-api  "  );


sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    videosApi    =    new    sstk  .  VideosApi  ();


const    queryParams    =    {
"  query  "  :    "  hot air balloon  "  ,
"  duration_from  "  :    30  ,
"  sort  "  :    "  popular  "
};


videosApi  .  searchVideos  (  queryParams  )
.  then  (({  data  })    =>    {
data  .  forEach  ((  result  )    =>    {
console  .  log  (  `  ${  result  .  description  }  :\n  ${  result  .  assets  .  thumb_mp4  .  url  }  \n`  );
});
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


Each of these commands uses different parameters, so check the API or SDK reference. For more information about the video search command, see[Search for videos](https://api-reference.shutterstock.com/?javascript--nodejs#videos-search-for-videos) .


Now you can search for whatever you want in our media library.


## Step 4: Get images that are similar to a reference image


Now that you’ve got the JavaScript SDK set up, you can use some of the powerful features of the API in your programs. We’ve recently allowed free accounts to use our[computer vision (CV) search](https://tech.shutterstock.com/2020/03/23/computer-vision) , so you can try it out in your code:


1. Find a reference image and put it in your project folder. You can use a picture of a person, a place, an activity, an object, or just about anything, even abstract drawings and patterns. The image must be:


- In JPG or PNG format
- No larger than 25MB
- No larger than 10,000 pixels in width or height


You must also encode the image in Base 64 format, which the following code will handle for you.


I’m going to use this picture of[Indian roller birds](https://www.shutterstock.com/image-photo/action-flight-scene-two-birds-rollers-559744408) :


2. In a text editor, open a new file named` similar.js` to use the CV API.
3. In the file, put the reference image file name in a variable. This path should be relative to where you run your command-line commands from, usually the root folder of your project.


```text
const    fileName    =    '  ./shutterstock_559744408.jpg  '  ;


```


4. Load the image and convert it to base 64:


```text
const    fs    =    require  (  '  fs  '  );


const    imageFile    =    fs  .  readFileSync  (  fileName  );
const    base64File    =    Buffer  .  from  (  imageFile  ).  toString  (  "  base64  "  );


```


5. Create an object to represent the CV endpoints of the Shutterstock API:


```text
const    sstk    =    require  (  "  shutterstock-api  "  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


```


6. Authenticate to the API as usual:


```text
sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


```


7. Upload the image with the` uploadImage()` command and get a temporary upload ID:


```text
computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  data  .  upload_id  );
})


```


8. Use the upload ID to get images that are similar to your image:


```text
computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  data  .  upload_id  );
return    computerVisionApi  .  getSimilarImages  (  data  .  upload_id  );
})
.  then  (({    data    })    =>    {
data  .  forEach  ((  result  )    =>    {
});
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


9. In the command-line window, run the script:


```text
node similar.js


```


Like it did with the image search, the script prints similar image descriptions and thumbnail URLs. In the case of the photo of the Indian rollers, I got images of birds with similar poses and colors, including a hoopoe, a northern carmine bee-eater, and a giant kingfisher:


Here’s the completed code. You’ll need to replace the file name with the file that you want to upload and put in your client key and secret:


```text
const    fs    =    require  (  '  fs  '  );
const    sstk    =    require  (  "  shutterstock-api  "  );


const    fileName    =    '  ./myImage.jpg  '  ;


sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  fileName  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  `Upload ID:   ${  data  .  upload_id  }  `  );
return    computerVisionApi  .  getSimilarImages  (  data  .  upload_id  );
})
.  then  (({  data  })    =>    {
data  .  forEach  ((  result  )    =>    {
});
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


Now you can use reverse image search to look for images that are similar to any image you want. We use this feature on Shutterstock.com to help people find just the right image for what they need, and you can use it for all kinds of things like organizing and identifying images.


Free accounts are limited to 5 CV requests per minute, so if you see an error code 429 in a response, wait a minute and try again. The API stores images only temporarily, so if you want to use your image again later, you must re-upload it.


## Step 5: Get keywords for an image


One of the best features of our computer vision API is the ability to provide keywords based on what the API “sees” in image. These keywords can help programs identify what objects are in an image and also what moods or themes the image might evoke. For example, if you upload a picture of a car, the API returns not just concrete words like “car,” “tires,” and “engine,” but also abstract concepts like “speed,” “power,” and “transportation.”


You can use these keywords in many different ways. Here are just a few:


- Getting ideas for keywords for images that you are posting to social media or contributing to Shutterstock
- Categorizing images by similarity
- Identifying inappropriate or off-topic images
- Providing augmented reality for cell phones and other devices to identify what’s in front of the camera


To test the keyword suggestion, I took photos of fruits and vegetables in my kitchen and looked at the keywords that the API returned:


Food Photo Selected keywords


Avocado natural, delicious, chocolate, avocado, vegetable, milk, cookie


Lemon yellow, fruit, ripe, sweet, sour, citrus, lemon, juicy


Blackberries black, sweet, rice


Red bell pepper red, pepper, tomato, bell, paprika


Onion red, vegetable, onion, purple, root


I’ve found that the API can identify the type of food in most cases, although there are red herrings in the results. For example, I got “milk” in some results, probably because of the white background. The API also misidentified my bowl of blackberries as rice and picked up on the chocolate-brown color of the avocado skin. Using this technology in applications still requires some thought about how to interpret and use the results.


Here’s how you can try it out for yourself and see what foods the API can identify from your kitchen:


1. Take some photos of food items that you have available and put them in your project folder in JPG or PNG format. For best results, isolate the food in the picture or crop the photo to show only the food item itself.
2. In a new file named` keywords.js` , set up the image upload in the same way that you did for the similar image search:


```text
const    fs    =    require  (  '  fs  '  );
const    sstk    =    require  (  "  shutterstock-api  "  );


const    fileName    =    '  ./myFoodPicture.jpg  '  ;


sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  fileName  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  `Upload ID:   ${  data  .  upload_id  }  `  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


3. Use the` getKeywords()` command to get the keywords that the API “sees” in the image, replacing the code in the` keywords.js` file with the following code. Like the` getSimilarImages()` command, this command accepts the upload ID that the` uploadImage()` command returns. It returns an array of keywords:


```text
const    fs    =    require  (  '  fs  '  );
const    sstk    =    require  (  "  shutterstock-api  "  );


const    fileName    =    '  ./myFoodPicture.jpg  '  ;


sstk  .  setBasicAuth  (  applicationClientId  ,    applicationClientSecret  );


const    computerVisionApi    =    new    sstk  .  ComputerVisionApi  ();


const    imageFile    =    fs  .  readFileSync  (  fileName  );


const    body    =    new    sstk  .  ImageCreateRequest  (  base64File  );


computerVisionApi  .  uploadImage  (  body  )
.  then  ((  data  )    =>    {
console  .  log  (  `Upload ID:   ${  data  .  upload_id  }  `  );
return    computerVisionApi  .  getKeywords  (  data  .  upload_id  );
})
.  then  (({    data    })    =>    {
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


4. Change the name of the file in the code to the file name of the picture that you took.
5. Run the script as usual:


```text
node    keywords  .  js


```


The script prints the list of keywords from the API.


Here are the keywords I got from my picture of a lemon:


```text
[ 'fresh', 'yellow', 'food', 'background', 'organic', 'healthy', 'fruit',
'ripe', 'sweet', 'white', 'natural', 'vegetarian', 'isolated', 'lemon',
'juicy', 'diet', 'citrus', 'closeup', 'nature', 'vitamin', 'tasty',
'ingredient', 'color', 'freshness', 'health', 'delicious', 'raw', 'sour',
'juice', 'summer' ]


```


A lot of those keywords are accurate, but “sweet” isn’t.


Now you can get keyword suggestions based on any image that you upload. The uses for keyword suggestion are endless, because it allows computers to “see” what’s in an image, both the physical objects and the more abstract concepts that are represented in an image.


## Next steps


That’s all you need to do to use Shutterstock’s API and its computer vision features. Now that you’ve got it working, you can incorporate CV into your applications. For a real-life example, check out this language learning program that lets users learn French words by taking pictures of matching objects:[Shutterstock ConUHack Winner: Interactive Language Learning with Computer Vision](https://www.shutterstock.com/blog/conuhacks-shutterstock-api) .


To use the CV endpoints in production applications, you’ll need to set up token authentication so you don’t have to include your application’s key and secret in the program. See[Authentication](https://api-reference.shutterstock.com/?javascript--nodejs#authentication) in the Shutterstock API reference.


You’ll also need a higher rate limit if you use your code in a real application, so[contact our partner team](https://www.shutterstock.com/developers/contact-us) and discuss your application’s needs.
