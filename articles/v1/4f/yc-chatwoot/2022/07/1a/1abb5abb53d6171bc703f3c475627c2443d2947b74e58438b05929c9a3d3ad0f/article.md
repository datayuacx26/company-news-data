---
schema_version: "1.0.0"
document_id: "1abb5abb53d6171bc703f3c475627c2443d2947b74e58438b05929c9a3d3ad0f"
company_key: "yc-chatwoot"
company: "Chatwoot"
source_id: "yc-chatwoot-news-import-2810aa3d6c74"
canonical_url: "https://www.chatwoot.com/blog/improving-chatwoot-file-upload"
published_at: "2022-07-28T00:00:00+00:00"
first_seen_at: "2026-07-21T13:15:56.789660+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:7ca459f5263c95af50ab15163426f3ae51b7b7508bb2ace3a55177f6881d5fd0"
---

# How we improved Chatwoot File Upload

Don't you hate waiting? When you know your internet connection is great, but the site takes a long time to load, or the uploaded files appear broken, or UI rendering is weird – it is frustrating.


We ended up facing one of the above issues. Actually, not us, but a few of our customers. So yeah, us. :D


## Here's what happened...


One of our customers was facing issues with uploading files. So, we dug in.


One of the major problems in Chatwoot File upload was that the uploaded image appeared broken even if the upload was completed from the front end.


This happened because uploading the attachment from our application servers to the cloud took time.


## So, we talked about possible solutions


We considered various methods to improve file upload in Chatwoot:


**Uploading the attachment on the front end and saving it to the cloud from the sidekick**


But then delaying the upload would also create a similar issue – we won't be able to fetch/update or delete the message with an attachment unless the upload is completed. So this would bring us back to square one.


**Using *` aws-sdk-s3`* direct upload, where we send a request to the Amazon S3 cloud with our credentials.**


The S3 provides the pre-signed URL to upload the file, the solution which would work because they came with the pre-signed URL just for this problem.


In one of our previous projects, we had seen a performance improvement with the pre-signed URL issue. Still, we discarded this idea because it is limited to S3 cloud storage, and we can't expect our self-hosted customers to stick to one cloud storage.


**Rails active storage direct upload**


We decided to go with this, but wait.


Rails let you implement this in just three steps, and it works like a charm for all the cloud storage providers supported by Chatwoot.


1. Send the request to the` direct_upload` endpoint provided by rails which is a` direct_upload_controller` .
2. Rails will return the authorized signed key to your cloud storage by reading your *` storage.yml` .*
3. Upload the file directly to the storage with the signed key.


There is no intermediate application server to slow down the upload process.


Great! This should solve our issue for sure, right?


But as we progressed with the implementations, we encountered quite a few hurdles.


- How are we authenticating the user?
- How are we ensuring that the request comes from an authorized user in Chatwoot?
- What if the signed key gets compromised?
- The cloud storage can get piled up, especially if someone bulk uploaded to the storage with many files in the given expiry time.


## We broke the issue down


1. We couldn't use` rails/active_storage/direct_uploads` endpoint as it doesn't have any authentication check and is extended from the action controller base.


Proposed Solution: We need a separate endpoint for direct upload with the *` rails/direct_upload`* functionalities.


2. We should be able to send the auth token with the direct upload signed key request. But it wasn't available with the current DirectUpload JS library and the documentation.


Proposed Solution: We can be more ambitious and build out the extension to the existing library.


### Let's solve it one by one


**1. ****Add a custom endpoint to get the direct upload signed key******


The first issue and solution were easy to build and could be handled by the current` direct_upload.js` library.


We wrote a new controller where we extended *` ActiveStorage::DirectUploadsController`* and added the logic to check if the user is a valid user asking for the signed key.


```text
class Api::V1::Widget::DirectUploadsController < ActiveStorage::DirectUploadsController
include WebsiteTokenHelper
before_action :set_web_widget
before_action :set_contact


def create
return if @contact.nil? || @current_account.nil?


super
end
end
```


Reference:[direct_uploads_controller.rb](https://github.com/chatwoot/chatwoot/blob/62d60f000bda12fb17876fcea39e8527c52feec9/app/controllers/api/v1/widget/direct_uploads_controller.rb?ref=www-internal-blog.chatwoot.com)


```text
include WebsiteTokenHelper
before_action :set_web_widget
before_action :set_contact


def create
return if @contact.nil? || @current_account.nil?


super
end
end
```


We updated the endpoint with the new controller in the new DirectUpload call.


```text
const upload = new DirectUpload(file, url)
```


So the first problem was solved.


We had a new endpoint for direct upload. Hitting the new endpoint from the front end, the controller had a check for the website token and authentication token.


**2. ****Authenticating the direct upload signed key request.******


Now let's move on to the second issue, how could we send the website token and authentication token from the front end to the new endpoint in the new DirectUpload call?


We checked the[documentation](https://edgeguides.rubyonrails.org/active_storage_overview.html?ref=www-internal-blog.chatwoot.com#direct-uploads) . Was there a way to send extra data with the request? We couldn't find any way.


We checked if there was an event on which we could bind the token, but none was mentioned. There were just file and URL parameters that you could send to DirectUpload class.


```text
const url = input.dataset.directUploadUrl
const upload = new DirectUpload(file, url)


upload.create((error, blob) => {
if (error) {
// Handle the error
} else {
// Add an appropriately-named hidden input to the form with a
//  value of blob.signed_id so that the blob ids will be
//  transmitted in the normal upload flow
const hiddenField = document.createElement('input')
hiddenField.setAttribute('type', 'hidden')
hiddenField.setAttribute('value', blob.signed_id)
hiddenField.name = input.name
document.querySelector('form').appendChild(hiddenField)
}
})
```


At one point, we thought about extending the *` DirectUpload.js`* and making our **Chatwoot** version. But, then we thought: let's open the library first, and hey! So there we had it; there was an event bound to creating a blob process when we sent the request to get the signed key. The event was ***` DirectUploadWillCreateBlobWithXHR`*** .


You can call this event while sending the parameters to DirectUpload and add the new tokens in XHR headers.


Here is how we do it:


1) We are sending the website token in the URL


2) Setting the auth token in the XHR header, and


3) Sending it to the new URL endpoint.


```text
const { websiteToken } = window.chatwootWebChannel;
const upload = new DirectUpload(
file.file,
`/api/v1/widget/direct_uploads?website_token=${websiteToken}`,
{
directUploadWillCreateBlobWithXHR: xhr => {
xhr.setRequestHeader('X-Auth-Token', window.authToken);
},
}
);


upload.create((error, blob) => {
if (error) {
window.bus.$emit(BUS_EVENTS.SHOW_ALERT, {
message: error,
});
} else {
this.onAttach({
file: blob.signed_id,
...this.getLocalFileAttributes(file),
});
}
});
```


I think we met the criteria to resolve the second issue.


## To conclude


With both the issues resolved, we were good to go with this feature, which is already a bug fix for one of our customers. :D


Long story short, rails documentation is not up to the mark, but the feature is, and there is a way to authenticate your direct upload request with this active storage's new feature.


And you can always refer to our blog if you are stuck at direct upload.


Here are the links to related GitHub issues:


https://github.com/chatwoot/chatwoot/issues/3567 https://github.com/chatwoot/chatwoot/issues/4147


Thanks for reading. ✌️
