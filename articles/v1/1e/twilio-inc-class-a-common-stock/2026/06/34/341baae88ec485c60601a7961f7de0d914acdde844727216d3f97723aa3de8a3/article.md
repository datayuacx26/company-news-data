---
schema_version: "1.0.0"
document_id: "341baae88ec485c60601a7961f7de0d914acdde844727216d3f97723aa3de8a3"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/validate-twilio-event-streams-webhooks-rust"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:b8f08d7e092e26a28dd82ed4e4f220f3f6377c5d8992a3d77131e3c2a0c7033b"
---

# How to Validate Twilio Event Streams Webhooks in Rust

Time to read:


-
-
-
-
-


June 24, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


**Reviewed by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


---


## How to Validate Twilio Event Streams Webhooks in Rust


One of Twilio's stand out features is its[Event Streams Webhooks](https://www.twilio.com/docs/events/webhook-quickstart) . These are HTTP requests where the body of each webhook is a JSON array of[CloudEvents](https://github.com/cloudevents/spec/blob/v1.0/spec.md) .


Event Streams Webhooks tell your application about specific events that happen on your Twilio account. For example, they can alert your application when customers receive an SMS message sent from your account, or when your account receives an incoming phone call.


The incoming requests include details of the event, such as the body of an incoming message, the phone or WhatsApp number it was sent from, and the event's name. Here's an example of what a request might include:


Copy code


```text
[
{
"specversion": "1.0",
"type": "com.twilio.eventstreams.test-event",
"source": "Sink",
"id": "AC11111111111111111111111111111111",
"dataschema": "https://events-schemas.twilio.com/EventStreams.TestSink/1.json",
"datacontenttype": "application/json",
"time": "2026-06-10T06:02:54.377Z",
"data": {
"test_id": "cae2f9e2-c277-4612-8ad3-93c1a7a3ef88"
}
}
]
```


After receiving an Event Streams Webhook, your application(s) can respond to the events as and when necessary, making your application extremely helpful to your users. However, like all application input — regardless of its source — **you must validate it!**


The same goes for Event Streams Webhooks.


Gladly, Twilio makes this pretty trivial.[Each webhook request is signed](https://www.twilio.com/docs/usage/webhooks/webhooks-security) ; you can find the signature in the` X-Twilio-Signature` header. The signature, in combination with your account's[Auth Token](https://www.twilio.com/docs/iam/api/authtoken) , the body of the webhook, and the request URL, can be used to verify that the webhook came from Twilio.


The whole process is a little bit involved. However, in this short tutorial, you're going to learn how to do it. Let's begin.


## Architecture


The app has two routes, one which receives webhook POST requests from Twilio, and another that receives GET requests. When requests are received the app validates them, based on whether they use the GET or POST HTTP method. Regardless of which method was used, if the request is valid, " **Request was VALID** " will be logged to the console and an[HTTP 200 OK](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/200) status code is returned. If it's invalid, then " **Request was INVALID** " will be logged and an[HTTP 400 Bad Request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/400) status code is returned.


## Prerequisites and common pitfalls to getting started


You'll need the following to use the application:


- A Twilio account (free or paid).[Create a Twilio account](https://twilio.com/try-twilio) if you don't already have one.
- [Rust](https://rust-lang.org/tools/install/) and[Cargo](https://doc.rust-lang.org/cargo/getting-started/installation)
- [ngrok](https://ngrok.com/) and a free account, or a similar service that can create a secure tunnel between the locally running application and the public internet
- Your preferred code editor or IDE, such as[Neovim](https://neovim.io/) or[Visual Studio Code](https://code.visualstudio.com/)
- Some terminal experience is helpful, though not required


## Build the app


Now, it's time to build the app.


### Step 1: Scaffold a new Rust application


Start off by using Cargo to scaffold a new Rust binary application, named "twilio-event-webhook-validation", then change into the new project directory.


Copy code


```text
cargo new twilio-event-webhook-validation --bin
cd twilio-event-webhook-validation
```


### Step 2: Add the required crates


Now, you need to add a couple of crates, which the application will need to make use of.


To add them to the project, run the command below.


Copy code


```text
cargo add axum axum-macros dotenvy log query-string-builder rustlio \
serde serde_json tokio tracing-subscriber url \
--features derive \
--features macros,rt-multi-threaded \
--features serde
```


If you're using Microsoft Windows, run the command below, instead:


Copy code


```text
cargo add axum axum-macros dotenvy log ordermap query-string-builder rustlio serde serde_json tokio tracing-subscriber url --features derive --features macros,rt-multi-threaded --features serde
```


### Step 3: Register the required environment variables


The application only needs two environment variables:` EXTERNAL_URL` and` TWILIO_AUTH_TOKEN` . These are the public-facing URL (different from its locally-running URL) of the application, and your Twilio Auth Token, respectively.


To set them, first, create a new file named *.env* in the project's top-level directory. Then, paste the configuration below into *.env* .


Copy code


```text
EXTERNAL_URL=
TWILIO_AUTH_TOKEN=
```


Next, in your terminal, have ngrok create a secure tunnel to the application on port 3000.


Copy code


```text
ngrok http 3000
```


You'll see output, similar to the screenshot below, in your terminal.


Copy the **Forwarding** URL and set it as the value of` EXTERNAL_URL` in *.env* .


Now,[log in to the Twilio Console](https://1console.twilio.com/) and open the **Workbench** at the bottom of the page. Then, copy the **AUTH TOKEN** value and paste it into *.env* as the value of` TWILIO_AUTH_TOKEN` .


### Step 4: Create Sink Resources


The next step is to create two[Sink resources](https://www.twilio.com/docs/events/event-streams/sink-resource) , because they're a quick and easy way to simulate sending webhook requests to the application to be validated.


Do this in the[Twilio Console](https://console.twilio.com/) , by navigating through **Explore Products > Developer Tools** **>**[Event Streams](https://console.twilio.com/us1/develop/event-streams/sinks) . Then, click **Create new sink** to start the process.


Set **Sink description** to "Validate Webhook Sink". Set **sink type** to "Webhook". Then, click **Next step** .


After that, set **Destination** to the **Forwarding** URL printed by ngrok to the terminal, with "/webhook" at the end. Leave **Method** set to "POST", and click **Finish** . Then, in the confirmation popup that appears, click " **View Sink Details** ", to view the sink's properties page, confirming that it's configured as required.


With the POST webhook sink created, repeat the process. However, this time, set **Method** to "GET" instead of "POST", and give the webhook sink a name that indicates that it's for GET requests, not POST requests.


### Step 5: Write the Rust code


With the setup out of the way, it's time to write the Rust code. Gladly, there's not that much to write. Replace the contents of *src/main.rs* with the following:


Copy code


```text
use axum::{
Router,
extract::{Query, State},
http::{StatusCode, header::HeaderMap},
response::IntoResponse,
routing::{get, post},
};
use log::info;
use query_string_builder::QueryString;
use rustlio::twilio::security;
use std::collections::{BTreeMap, HashMap};
use url::Url;


#[derive(Clone)]
pub struct AppState {
pub external_url: String,
pub validator: security::WebhookValidator,
}


impl AppState {
fn new() -> Self {
let auth_token = match dotenvy::var("TWILIO_AUTH_TOKEN") {
Ok(value) => value,
Err(error) => {
panic!("Could not retrieve a Twilio Auth Token, because: {error:?}")
}
};


let webhook_url = match dotenvy::var("EXTERNAL_URL") {
Ok(value) => value,
Err(error) => {
panic!("Could not retrieve the application's external URL, because: {error:?}")
}
};


AppState {
external_url: webhook_url,
validator: security::WebhookValidator { auth_token },
}
}


fn build_external_url(&self, path: Option<&str>, params: &HashMap<String, String>) -> Url {
let mut url = Url::parse(&self.external_url).expect("could not parse the provided URL");


if path.is_some() {
let query_path = path.unwrap_or_default();
url.set_path(query_path);
}


if !params.is_empty() {
let sorted_params = BTreeMap::from_iter(params);


let mut qs = QueryString::new();
for (key, value) in sorted_params {
qs.push(key, value);
}


url.set_query(Some(qs.to_string().as_str().trim_start_matches("?")));
}


url
}
}


#[tokio::main]
async fn main() {
tracing_subscriber::fmt::init();


let app = Router::new()
.route("/webhook", post(handle_post_webhook))
.route("/webhook", get(handle_get_webhook))
.method_not_allowed_fallback(handle_405)
.fallback(handle_404)
.with_state(AppState::new());


let listener = tokio::net::TcpListener::bind("0.0.0.0:3000").await.unwrap();
axum::serve(listener, app.into_make_service())
.await
.unwrap();
}


async fn handle_404() -> impl IntoResponse {
(StatusCode::NOT_FOUND, "nothing to see here")
}


async fn handle_405() -> impl IntoResponse {
(
StatusCode::METHOD_NOT_ALLOWED,
"Method not allowed fallback",
)
}


async fn handle_post_webhook(
State(state): State<AppState>,
headers: HeaderMap,
Query(params): Query<HashMap<String, String>>,
body: String,
) -> StatusCode {
let signature_header = match headers.get("X-Twilio-Signature") {
Some(value) => value.to_str().unwrap(),
None => panic!("Could not retrieve the X-Twilio-Signature"),
};


if state.validator.validate_body(
&mut state.build_external_url(Some("webhook"), &params),
signature_header,
body.as_bytes(),
) {
info!("Request was VALID");
StatusCode::OK
} else {
info!("Request was INVALID");
StatusCode::BAD_REQUEST
}
}


async fn handle_get_webhook(
State(state): State<AppState>,
headers: HeaderMap,
Query(params): Query<HashMap<String, String>>,
) -> StatusCode {
let signature_header = match headers.get("X-Twilio-Signature") {
Some(value) => value.to_str().unwrap(),
None => panic!("Could not retrieve the X-Twilio-Signature"),
};


if state.validator.validate(
&mut state.build_external_url(Some("webhook"), &params),
&HashMap::new(),
signature_header,
) {
info!("Request was VALID");
StatusCode::OK
} else {
info!("Request was INVALID");
StatusCode::BAD_REQUEST
}
}
```


The code starts off by defining a struct called` AppState` . The struct has two properties which are used throughout the application:


- **external_url:** the base of the application's public URL
- **validator:** an instance of` WebhookValidator` from the rustlio package. This has the functionality required to validate incoming webhook requests.


Then, two functions are defined on` AppState` :


- **new:** This returns a new AppState


instance with both of the defined properties initialised
- **build_external_url:** This function builds the application's public-facing URL, including any query parameters. This is necessary, as the request's signature is based on the application's external URL which Twilio makes the request to, not the internal version running on localhost.


Next, the application's entry point (` main()` ) is defined. This function defines the two routes in the application's routing table. It also registers fallbacks for requests made to defined routes but with unsupported methods (handled by` handled_405()` ), and for undefined routes (handled by` handled_404()` ). The fallback handler functions are extremely simplistic, returning the relevant status code and a short text message, telling the user what happened.


Following that, the` handle_post_webhook()` function is defined. This function handles webhook requests made with the POST method. The function takes the` AppState` struct and three[extractors](https://docs.rs/axum/latest/axum/extract/index) . Extractors are handy types and traits which simplify extracting data from requests.


Extractors are analogous to[Parameter Binding in Minimal API apps](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/parameter-binding?view=aspnetcore-10.0) , if you've worked with that framework.


Without diving into too much detail, the first,` headers` , simplifies working with a request's headers. The second,` params` , simplifies working with the request's query string. Finally, the third contains the request body.


Within the body of the function, the request's signature is retrieved from the` X-Twilio-Signature` header. This is then passed to the` validate_body()` function, along with the application's external URL and the request's body. If the function returns` true` , " **Request was VALID** " is logged to the console and an[HTTP 200 OK](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/200) status code is returned. Otherwise, " **Request was INVALID** " is logged and an[HTTP 400 Bad Request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/400) status code is returned.


This` validate_body()` function, in essence, does two things:


1. Computes a signature of the request URL. The signature is an[HMAC](https://en.wikipedia.org/wiki/HMAC) using the[SHA1 hashing algorithm](https://www.geeksforgeeks.org/java/sha-1-hash-in-java/) , where the HMAC's key is the provided Twilio Auth Token.
2. Generates[a SHA-256 hash](https://en.wikipedia.org/wiki/SHA-2) of the request's body.


The function returns` true` if:


1. The computed signature matches the signature sent by Twilio in the request
2. The generated SHA-256 matches the hash sent by Twilio in the request's` bodySHA256` query parameter


Finally, comes the` handle_get_webhook()` function. This function is almost identical to` validate_body()` . The key difference is that it calls` WebhookValidator` 's` validate()` function, instead of` validate_body()` . This function computes a signature of the request URL and compares it to the signature sent by Twilio in the request` X-Twilio-Signature` header.


## Test that the code works as expected


With the code written, let's check that it works as expected. Before we can do that, start the application by running the following command:


Copy code


```text
cargo run
```


Then, switch back to the browser tab where you created the webhook sinks in the Twilio Console. There, open the webhook sink for testing POST requests, and scroll to the bottom of the page where you'll see the **Test sink** section. There, click " **Send test event** " to send a test POST webhook event request to the application.


Now, switch to the terminal tab where ngrok is running. There, in the ngrok terminal output, you'll see that the request was received. Then, switch to the terminal tab where the Rust application's running. You should see " **Request was VALID** " written to the terminal output, similar to the example output below.


Copy code


```text
2026-06-15T01:34:28.323840Z  INFO twilio_event_webhook_validation: Request was VALID
```


Now, send a test event from the GET webhook sink and see if " **Request was VALID** " was printed to the terminal's output.


## That's how to validate Twilio Event Webhooks in Rust


Thanks to Event Streams Webhook validation, you can be sure that incoming webhook requests come from Twilio — and weren't tampered with in-transit. What's more, thanks to Rustlio, you only needed to write a minimal amount of code to perform the validation.


I strongly encourage you to check out the documentation link below to find out more about the crate.


- [The Rustlio crate](https://crates.io/crates/rustlio)
- [Webhooks security](https://www.twilio.com/docs/usage/webhooks/webhooks-security#validating-signatures-from-twilio)
- [Debugging Events Webhook](https://www.twilio.com/docs/usage/troubleshooting/debugging-event-webhooks)


You can find[the complete code for this article on GitHub](https://github.com/settermjd/twilio-event-streams-webhook-validation-rust) . I can't wait to see what you build with Twilio and Rust.


*Matthew Setter is a PHP and Go Editor in the Twilio Voices team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him atmsetter@twilio.com . He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*


[Validation icon in the article's main image was created by Freepik](https://www.flaticon.com/free-icons/validation) on Flaticon.
