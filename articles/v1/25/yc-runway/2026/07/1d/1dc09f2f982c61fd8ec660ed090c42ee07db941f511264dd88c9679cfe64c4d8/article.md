---
schema_version: "1.0.0"
document_id: "1dc09f2f982c61fd8ec660ed090c42ee07db941f511264dd88c9679cfe64c4d8"
company_key: "yc-runway"
company: "Runway"
source_id: "yc-runway-news-import-fe61d44f24da"
canonical_url: "https://www.runway.team/blog/how-to-upload-assets-using-the-app-store-connect-api"
published_at: null
first_seen_at: "2026-07-22T12:25:56.018278+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:c069aaea1e582b8a73eaab44ef093c9cee63c77c7c78b74a79678d5f3aece753"
---

# How to upload assets using the App Store Connect API

When you create a new version of your app in App Store Connect, you might also need to upload new assets like screenshots or previews for the App Store. You can do this manually on the App Store Connect website, but if you have a lot of localizations to support and your assets change often across versions, this can be a tedious, error-prone, and time-consuming process.


Thankfully, you can automate this process using the App Store Connect API, which has a set of endpoints tailored for uploading media such as screenshots and previews. Unfortunately, using these endpoints is not straightforward and they require numerous steps that I will cover in great detail in this article.


While this article uses the App Store Connect API directly, you might want to consider a tool like[Fastlane](https://www.runway.team/tags/fastlane) , specifically the <code>deliver<code>[action](http://(https//docs.fastlane.tools/actions/deliver/) , which abstracts most of the complexity of the API away and provides a more user-friendly interface, or services with built-in App Store Connect API integrations such as[Runway](https://www.runway.team/) (hey, that's us!).


## The process


The App Store Connect API has a[comprehensive guide](https://developer.apple.com/documentation/appstoreconnectapi/uploading_assets_to_app_store_connect) that outlines the process of uploading assets and all the steps that developers need to follow. The process can be summarized in 3 **key** steps:


1. **Making an asset reservation** : Before uploading any data, you must first tell the App Store Connect API what kind of asset you will be uploading and how big it is. If successful, the response to this request will contain the different requests you must make to upload the asset as well as the ID and information about the entity for which you are uploading an asset. **‍**
2. **Uploading the asset** : After a successful reservation, you will have all the requested information needed to upload the asset. As part of the reservation, the App Store Connect API will split the asset into one or more parts and provide you with the URL, headers, offset and size required for each of them. You can at this point split the asset into all its different parts and upload them in parallel. **‍**
3. **Committing the reservation:** If (and only if) all parts of the asset have been successfully uploaded, you can then make a request to the App Store Connect API telling it that the asset is complete. In this step, you also need to send the checksum of the full asset to the API.


As you can see in the steps above, the process is complex and involves file handling, data manipulation, cryptography and concurrent network requests, which can complicate things significantly. Over the next few sections, you will learn how to implement each of these steps in a familiar language for mobile developers: Swift.


## Uploading a screenshot to App Store Connect


For the rest of the article, we will show how the process works by uploading a single screenshot for a specific version's localization using the App Store Connect API and writing a function for each of the steps mentioned previously.


The first thing you need to do to interact with the App Store Connect API is to[create an API key](https://developer.apple.com/documentation/appstoreconnectapi/creating_api_keys_for_app_store_connect_api) and use it to generate a JWT token that you will use to authenticate all your requests. As we mentioned in our[Hitchhiker's guide to the App Store Connect API](https://www.runway.team/blog/a-hitchhikers-guide-to-the-app-store-connect-api#2-do-not-handle-authentication-yourself) , you should leverage the power of existing libraries that handle the authentication process for you and give you type-safe access to the API's endpoints.


For this article, we will use the[appstoreconnect-swift-sdk](https://github.com/AvdLee/appstoreconnect-swift-sdk) , a Swift package that provides a type-safe way to interact with the App Store Connect API. Setting up the SDK is as straightforward as initializing an <code>APIConfiguration<code> object with either a team or individual API key and then using it to create an <code>APIClient<code> object that you can use to make requests to the API:


```text
struct ScreenshotSet {
let id: String
let type: ScreenshotDisplayType
}


func uploadScreenshot(_ file: URL, to set: ScreenshotSet) async throws {
let configuration = try APIConfiguration(
individualPrivateKeyID: "🔐",
individualPrivateKey: "🔐"
)


let provider = APIProvider(configuration: configuration)
}


```


Screenshots need to always be part of a set that describes the device type and provides dimension constraints. For this reason, we will need to pass the <code>ScreenshotSet<code> object that we want to upload the screenshot to, as well as the URL of the file we want to upload.


### Making an asset reservation


The first step in the process is to make a reservation for the asset you want to upload by making a <code>POST<code>[request](https://api.appstoreconnect.apple.com/v1/appScreenshots](https://developer.apple.com/documentation/appstoreconnectapi/create_an_app_screenshot) to the endpoint. The endpoint will then create a screenshot entity and return information about the new entity such as the id as well as all the information needed to upload the asset.


In the body of the <code>POST<code> request, we must send the file name and its size and tell the API which screenshot set we are uploading the asset to:


```text
func createReservation(inSet set: String, fileName: String, imageData: Data, provider: APIProvider) async throws -> (String, [URLRequest]) {
// 1
let screenshotSetData = AppScreenshotCreateRequest.Data.Relationships.AppScreenshotSet.Data(type: .appScreenshotSets, id: set)
let screenshotSet = AppScreenshotCreateRequest.Data.Relationships.AppScreenshotSet(data: screenshotSetData)
let relationships = AppScreenshotCreateRequest.Data.Relationships(appScreenshotSet: screenshotSet)
let attributes = AppScreenshotCreateRequest.Data.Attributes(
fileSize: imageData.count,
fileName: fileName
)
let data = AppScreenshotCreateRequest.Data(
type: .appScreenshots,
attributes: attributes,
relationships: relationships
)
let body = AppScreenshotCreateRequest(data: data)


let reservation = APIEndpoint
.v1
.appScreenshots
.post(body)


// 2
let reservationResponse = try await provider.request(reservation)


// 3
let requests = reservationResponse
.data
.attributes?
.uploadOperations?
.compactMap { uploadOperation -> URLRequest? in
guard let urlString = uploadOperation.url,
let url = URL(string: urlString),
let method = uploadOperation.method,
let offset = uploadOperation.offset,
let length = uploadOperation.length else {
return nil
}


// 4
let chunk = imageData[offset..<(offset + length)]


// 5
var request = URLRequest(url: url)
request.httpMethod = method
for header in (uploadOperation.requestHeaders ?? []) {
if let name = header.name {
request.setValue(header.value, forHTTPHeaderField: name)
}
}
request.httpBody = chunk


return request
} ?? []


// 6
return (reservationResponse.data.id, requests)
}


```


A lot is going on in the function above, so let's break it down:


1. We create a <code>POST<code> request to the <code>appScreenshots<code> endpoint with the file name and size in the body of the request through the body's attributes field, which we pass as parameters to the function. We also specify the screenshot set we want to upload the asset to by using the body's relationships field.
2. We make the request to the API and get the response back.
3. We parse the response and extract the upload operations to map them into an array of <code>URLRequest<code> objects that we will use to upload the asset.
4. We get an image data chunk based on the offset and length provided by the API.
5. We create a <code>URLRequest<code> object for each upload operation and set the method, headers and body of the request based on the information provided by the API.
6. We return the reservation ID and the array of requests that we will use to upload the asset.
