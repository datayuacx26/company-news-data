---
schema_version: "1.0.0"
document_id: "dd6f8845c4d20ff1e3c5f17d59e6c974a9905bdea371d628ec7b73ea915901fd"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/build-passwordless-auth-with-twilio-verify-rust"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T16:59:47.521529+00:00"
fetched_at: "2026-08-19T16:59:49.632670+00:00"
content_hash: "sha256:f7db7477bf1f65fed85d9f3aefd3a1fd59c27b970714509c91fd07c6e2acd58e"
---

# How to Build Passwordless Auth With Twilio Verify in Rust

Time to read:


-
-
-
-
-


August 19, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


---


## How to Build Passwordless Auth With Twilio Verify in Rust


Typing or generating a unique password for every new account or website is a hassle. Although password managers help, passwordless authentication (auth) offers a more streamlined and secure alternative.


In this tutorial, you will learn about passwordless auth, and build a Rust app that uses[Twilio Verify](https://www.twilio.com/docs/verify) to implement it.


## Prerequisites


Before you begin, ensure you have the following:


- A free Twilio account.[Click here to create a free account](https://www.twilio.com/try-twilio) if you are new to Twilio.
- A phone that can receive SMS
- [Rust and Cargo](https://doc.rust-lang.org/cargo/getting-started/installation)
- Your favourite text editor or IDE (such as neovim or Visual Studio Code)
- Your favourite web browser


## Architecture


This application will be a small API with four routes:


- **A GET and a POST route with the path "/".** These display the sign-in form where users can enter their mobile phone number to receive the OTP code, and validate submissions of that form. If the form successfully validates, then an OTP code will be sent to the phone number provided.
- **A GET and a POST route with the path "/verify".** These display a form to validate the OTP code that the user received, and validate submissions of that form. After submission of the form, the user will see if the code was valid or not.


## What is passwordless auth?


Passwordless auth is an authentication method where the user does not need a password in order to log into an app or system. Rather, the user's mobile device receives a one-time code. In this authentication method, users are authenticated using other unique and more secure alternatives like one-time passwords (OTP), SMS,[Passkeys](https://fidoalliance.org/passkeys/) ,[Silent Network Authentication](https://www.twilio.com/en-us/blog/silent-network-authentication-sna-overview) (SNA),[Voice](https://www.twilio.com/en-us/blog/voice-biometric-authentication-with-twilio-html) , or[email notification](https://www.twilio.com/en-us/blog/email-verification-twilio-verify-and-twilio-sendgrid-nodejs) .


Here's a breakdown of how it works, using SMS:


1. **User initiation:** When a user attempts to log in, instead of entering a password, they provide their phone number as their unique identifier and are redirected to a form where they can validate the OTP code that they will receive.
2. **OTP generation and delivery:** The server initiates an OTP generation request with Twilio Verify. Twilio Verify then dispatches a unique, temporary code via SMS to the user.
3. **User verification:** The user receives the OTP on their mobile device and input this code into the validation form.
4. **Code validation:** Using Twilio Verify, the server verifies the entered OTP against the one that was sent, earlier. If the codes match, the user is successfully authenticated and granted access.


This approach significantly enhances security by leveraging the inherent security features of the user's mobile device as a second factor in the authentication process. It also eliminates the need for them to memorize passwords, thereby simplifying the authentication process and enhancing user convenience.


Passwordless authentication has additional advantages, including the following:


- **Enhanced security:** Reduces the risk of password-related breaches.
- **Convenience:** Eliminates the need for users to remember and manage multiple passwords.
- **Reduced friction:** Streamlines the login process, improving the user experience.
- **Scalability:** Easily scalable with Twilio's infrastructure.


## Build the app


### Step 1: Set up the project


Set up a new Rust project using by running the following commands, where you store your Rust projects:


Copy code


```text
cargo new twilio-verify-rust --bin
cd twilio-verify-rust
mkdir -p {assets/css,templates/forms}
```


If you're using Microsoft Windows, replace the third line in the command above with the following:


Copy code


```text
mkdir assets/css templates/forms
```


### Step 2: Install the required dependencies


Next, install the required dependencies, by running the following:


Copy code


```text
cargo add axum axum-macros axum-template axum_session dotenvy handlebars rustlio serde stringcase tokio tower tower-http --features axum/form --features axum/macros --features axum/tokio --features axum/tower-log --features axum-template/handlebars --features serde/derive --features tokio/macros --features tokio/rt-multi-thread --features tower-http/fs --features tower-http/trace
```


In case you're not familiar with the crates, here's a short description of the dependencies that you just installed:


Now, open the project directory in your preferred text editor or IDE.


### Step 3: Set the required environment variables


Dotenv files (commonly named *.env* ) are used to store the configuration information that your app needs during development, separate from the application's code. For this tutorial, it will be your Twilio credentials (i.e., your Twilio Account SID and Auth Token) and a Verify Service[SID](https://www.twilio.com/docs/glossary/what-is-a-sid) .


In your project's top-level directory, create a file named *.env* . Then, in *.env* add the three variables below.


Copy code


```text
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
TWILIO_VERIFY_SERVICE_SID=
```


With placeholders for the environment variables set in *.env* , you next need to retrieve the credentials to set as their values. To do that, sign into[the Twilio Console](https://console.twilio.com/) . There, click the black and white up arrow at the bottom of the page, and you should see your **Account SID** and **Auth Token** in the **Workbench** ; as shown in the image below.


Copy these values and paste them into *.env* as the values for` TWILIO_ACCOUNT_SID` and` TWILIO_AUTH_TOKEN` .


Next, navigate to **Products and Services > Verify > Services** . On this page, click **Create new** . Then, fill out the initial form with the configuration values shown in the screenshot below, and click **Continue** .


In the next step, leave **Enable Fraud Guard** set to "Yes" and click **Continue** to finish creating the service.


After creating the service, copy the **Service SID** and paste it into *.env* as the value of` TWILIO_VERIFY_SERVICE_SID` .


### Step 4: Build the base Rust application


The next thing to do is start fleshing out the base application. To do that, replace the existing code in *src/main.rs* with the code below:


Copy code


```text
use axum::{
Form, Router, debug_handler,
extract::State,
response::{IntoResponse, Redirect, Response},
routing::{get, post},
};
use axum_macros::FromRef;
use axum_session::{Session, SessionConfig, SessionLayer, SessionNullPool, SessionStore};
use axum_template::{RenderHtml, engine::Engine};
use handlebars::Handlebars;
use rustlio::TwilioRestClient;
use rustlio::verify::{VerificationCheckRequestParams, Verify};
use serde::{Deserialize, Serialize};
use std::{collections::HashMap, env};
use tower::ServiceBuilder;
use tower_http::services::{ServeDir, ServeFile};


type AppEngine = Engine<Handlebars<'static>>;


#[derive(Clone, FromRef)]
struct AppState {
config: HashMap<String, String>,
engine: AppEngine,
}


#[tokio::main]
async fn main() {
dotenvy::dotenv().ok();


let app_config = HashMap::from([
(
String::from("TWILIO_ACCOUNT_SID"),
env::var("TWILIO_ACCOUNT_SID").unwrap_or("Twilio Account SID was not set".to_string()),
),
(
String::from("TWILIO_AUTH_TOKEN"),
env::var("TWILIO_AUTH_TOKEN").unwrap_or("Twilio Auth Token was not set".to_string()),
),
(
String::from("TWILIO_VERIFY_SERVICE_SID"),
env::var("TWILIO_VERIFY_SERVICE_SID")
.unwrap_or("Twilio Verify Service SID was not set".to_string()),
),
]);


let config = SessionConfig::default();
let session_store = SessionStore::<SessionNullPool>::new(None, config)
.await
.unwrap();
let mut hbs = Handlebars::new();
let app = Router::new()
.route("/", get(show_signin_form))
.with_state(AppState {
config: app_config,
engine: Engine::from(hbs),
})
.layer(SessionLayer::new(session_store));


println!("Server starting on :8080");
let listener = tokio::net::TcpListener::bind("0.0.0.0:8080").await.unwrap();
axum::serve(listener, app.into_make_service())
.await
.unwrap();
}
```


The updated code, refactors the` main()` function to load environment variables from the variables defined in *.env* using dotenvy, and loads three variables in the application's configuration (` app_config` ): TWILIO_ACCOUNT_SID


, TWILIO_AUTH_TOKEN


, and TWILIO_VERIFY_SERVICE_SID


. These are your Twilio Account SID and Auth Token, and a Verify Service SID. You'll retrieve these shortly.


Then, it adds session and template support, backed by[the Handlebars templating language](https://handlebarsjs.com/guide/) to the application, sets up the application's routing table with a single route, adds session support, then starts the application listening on port 8080.


### Step 5: Add the ability to send an OTP


Now, you'll add the first of two features: the ability to send an OTP code when requested by the user. To do that, add the following to *src/main.rs* .


Copy code


```text
async fn show_signin_form(engine: AppEngine) -> impl IntoResponse {
RenderHtml("sign-in", engine, NoData {})
}


#[derive(Serialize)]
struct NoData {}
```


Then, after the initialisation of` hbs` in the` main()` function, add the following:


Copy code


```text
hbs.register_template_file("sign-in", "templates/forms/signin.hbs")
.unwrap();
```


The` show_signin_form()` function will be called in response to GET requests to the application's default route "/", rendering *templates/forms/signin.hbs* with no template data (` NoData` ).


Now, in the *templates/forms* directory, create a new file named *signin.hbs* , and in that file paste the code below:


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Sign In</title>
<link href="https://assets.twilio.com/public_assets/paste-fonts/1.5.2/fonts.css" rel="stylesheet">
<link href="/assets/css/styles.css" rel="stylesheet">
</head>
<body class="container">
<main>
<h1>Sign In</h1>
<form action="/" method="post" class="mt-60">
<div class="mb-60">
<label for="phone">Enter your phone number:</label>
<input
id="phone"
inputmode="tel"
name="phone"
required
type="tel"
>
</div>
<button type="submit" class="primary">Send OTP</button>
</form>
</main>
</body>
</html>
```


As you can see from the HTML above, it is a simplistic HTML page with a form with a single field named "phone". Note that the field's` type` and[inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inputmode) attributes are set to "tel". This hints to browsers on mobile devices to render a virtual keyboard most appropriate for entering telephone data.


Then, back in *src/main.rs* , add the following code at the end of the file.


Copy code


```text
#[derive(Deserialize, Serialize)]
struct SignIn {
phone: String,
}


#[debug_handler]
async fn send_otp_code(
session: Session<SessionNullPool>,
State(state): State<AppState>,
Form(sign_in): Form<SignIn>,
) -> Redirect {
if sign_in.phone.is_empty() {
return Redirect::to("/");
}


session.set("phone", &sign_in.phone);


let Some(account_sid) = state.config.get("TWILIO_ACCOUNT_SID") else {
return Redirect::to("/");
};


let Some(auth_token) = state.config.get("TWILIO_AUTH_TOKEN") else {
return Redirect::to("/");
};


let verify = Verify {
client: &TwilioRestClient {
account_sid,
auth_token,
},
..Default::default()
};


let Some(verify_sid) = state.config.get("TWILIO_VERIFY_SERVICE_SID") else {
return Redirect::to("/");
};


let response = verify
.send_verification_token(verify_sid, sign_in.phone.as_str(), "sms")
.await;
match response {
Ok(token_response) => println!(
"Successfully sent the token (status: {})",
token_response.status.unwrap_or("unknown".to_string())
),
Err(err) => println!("Unable to send token because: {}", err),
}


Redirect::to("/verify")
}
```


The` send_otp_code()` function processes requests to the sign-in form. It retrieves the phone number parameter from the request and, if it's available, stores it in the current session. If it's not available, it redirects the user back to the sign in form.


Then, it attempts to send an OTP code to the user's phone number via SMS using Twilio Verify. If the code was successfully sent, the user is redirected to the "/verify" route, which you'll build next.


Now, in the` main()` function in *src/main.rs* , update the initialisation of` app` to match the following:


Copy code


```text
let app = Router::new()
.route("/", get(show_signin_form))
.route("/", post(send_otp_code))
.with_state(AppState {
config: app_config,
engine: Engine::from(hbs),
})
.layer(SessionLayer::new(session_store));
```


The code adds the POST variant of the default route ("/") to the application's routing table, having requests to it handled by` send_otp_code()` .


### Step 6: Add the ability to verify an OTP


You'll now add the functionality to verify the OTP codes the user receives. Start by adding the code below to the end of *src/main.rs* .


Copy code


```text
#[derive(Deserialize, Serialize)]
struct VerificationStatus<'a> {
status: bool,
message: &'a str,
}


#[derive(Deserialize, Serialize)]
struct VerifyOtp {
code: String,
}


async fn show_verify_otp_form(engine: AppEngine) -> impl IntoResponse {
RenderHtml("verify", engine, NoData {})
}


async fn verify_otp_code(
engine: AppEngine,
session: Session<SessionNullPool>,
State(state): State<AppState>,
Form(verify_otp): Form<VerifyOtp>,
) -> Response {
let phone: std::option::Option<String> = session.get("phone");


let Some(phone_number) = phone else {
return Redirect::to("/").into_response();
};


if verify_otp.code.is_empty() {
return Redirect::to("/verify").into_response();
}


let Some(account_sid) = state.config.get("TWILIO_ACCOUNT_SID") else {
return Redirect::to("/verify").into_response();
};


let Some(auth_token) = state.config.get("TWILIO_AUTH_TOKEN") else {
return Redirect::to("/verify").into_response();
};


let verify = Verify {
client: &TwilioRestClient {
account_sid,
auth_token,
},
..Default::default()
};


let check_params = VerificationCheckRequestParams {
code: verify_otp.code,
to: phone_number,
verification_sid: "".to_string(),
amount: "".to_string(),
payee: "".to_string(),
sna_client_token: "".to_string(),
};


let Some(verify_sid) = state.config.get("TWILIO_VERIFY_SERVICE_SID") else {
return Redirect::to("/verify").into_response();
};


let response = verify
.check_verification_token(verify_sid, check_params)
.await;


let verification_check_response = match response {
Ok(response) => {
println!("Successfully validated the token");
println!("Here is the response: {:?}", response);
response
}
Err(error) => {
println!("Here is the error: {:?}", error);
return Redirect::to("/verify").into_response();
}
};


let check_status = verification_check_response
.status
.unwrap_or(String::from("unknown"));
let message = if check_status == "approved" {
session.remove("phone");
"Verification was successful"
} else {
"Verification failed"
};


let status = check_status == "approved";
let data = VerificationStatus { status, message };


RenderHtml("verification-status", engine, data).into_response()
}
```


Then, update the initialisation of` hbs` in` main()` to match the following:


Copy code


```text
let mut hbs = Handlebars::new();
hbs.register_template_file("sign-in", "templates/forms/signin.hbs")
.unwrap();
hbs.register_template_file("verify", "templates/forms/verify.hbs")
.unwrap();
hbs.register_template_file(
"verification-status",
"templates/forms/verification-status.hbs",
)
.unwrap();
```


The` show_verify_otp_form()` method renders *templates/forms/verify.hbs* , providing the form for the user to enter to validate their OTP code.


The` verify_otp_code()` function retrieves the phone number from the session. If it's not present or empty, the user is redirected to the sign in form. Otherwise, it attempts to parse the request data and retrieve the "code" parameter from the POST request.


If the code


parameter wasn't present or was empty, the user is redirected to the verify OTP code form. If the parameter was present, it uses the Twilio Rest Client to validate that the code supplied was the same as the one that was sent to the user's phone number.


If the code was valid:


- The user's phone number is deleted from the current session
- *templates/forms/verification-status.hbs* is rendered as the function's response, along with two template variables:` status` set to` true` and` message` set to "Verification was successful".


If the code was invalid, *templates/forms/verification-status.hbs* will be rendered with` status` set to` false` and` message` set to "Verification failed".


With that done, in the *templates/forms* directory create a new file named *verify.hbs* and paste the code below into the file.


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Verify OTP</title>
<link href="/assets/css/styles.css" rel="stylesheet">
</head>
<body class="container">
<main>
<h1>Verify OTP Code</h1>
<form action="/verify" method="post" class="mt-60">
<div class="mb-60">
<label for="code">Enter the OTP (6 digits):</label>
<input
autocomplete="one-time-code"
id="code"
inputmode="numeric"
maxlength="6"
name="code"
pattern="\d{6}"
required
type="text"
>
</div>
<button type="submit" class="primary">Verify</button>
</form>
</main>
</body>
</html>
```


The HTML renders another, simplistic, form with a single field for the received OTP code. The field uses the[pattern](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/pattern) and[maxlength](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/maxlength) field attributes to ensure that the only valid input is a 6-digit code. It also sets the[inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inputmode) attribute to "numeric" to have the browser display a keyboard appropriate for entering digits, making it easier for the user to only enter numeric input.


Now, create another file in the *templates/forms* directory named *verification-status.hbs* and paste the code below into the file.


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Verification Status</title>
<link href="/assets/css/styles.css" rel="stylesheet">
</head>
<body class="container">
<main>
<h1>Verification Status</h1>
<p class="alert {{# if status }}alert-success{{ else }}alert-error{{/if}}">
{{ message }}
</p>
</main>
</body>
</html>
```


The HTML renders the message in the` Message`[template variable](https://pkg.go.dev/text/template) , and styles it as either a success or error, based on the value of the` Success` template variable.


With the templates created, the final thing to do is to finalise the definitions in the application's routing table. Do that by updating the initialisation of` app` in *src/main.rs* to the following:


Copy code


```text
let app = Router::new()
.route("/", get(show_signin_form))
.route("/", post(send_otp_code))
.route("/verify", get(show_verify_otp_form))
.route("/verify", post(verify_otp_code))
.with_state(AppState {
config: app_config,
engine: Engine::from(hbs),
})
.nest_service(
"/assets",
ServiceBuilder::new().service(ServeDir::new("assets")),
)
.nest_service(
"/favicon.ico",
ServiceBuilder::new().service(ServeFile::new("assets/favicon.ico")),
)
.layer(SessionLayer::new(session_store));
```


The revised initialisation adds the GET and POST forms of the "/verify" route, handled by the` show_verify_otp_form()` and` verify_otp_code()` functions, respectively. It also adds two static route definitions:


- "/assets", which serves files from the */assets* directory
- "/favicon.ico" to serve the application's favicon. This isn't *strictly* necessary, but numerous services and clients still request it and[it helps improve brand recognition and trust](https://ecompapi.com.au/blogs/what-is-a-favicon-guide-2026) , so it's handy to have.


### Step 7: Download the application's CSS file


The last step in the process is to[download the application's CSS file](https://raw.githubusercontent.com/twilio-samples/twilio-verify-passwordless-authentication-rust/refs/heads/main/assets/css/styles.css) from the project's GitHub repository, to the project's *assets/css* directory, naming it *styles.css* . Additionally,[download the application's favicon.ico file](https://raw.githubusercontent.com/twilio-samples/twilio-verify-passwordless-authentication-rust/refs/heads/main/assets/favicon.ico) to the *assets* directory.


## Test that the application works


Finally, it's time to test that the code works as expected. Start the application by running the following command.


Copy code


```text
cargo run
```


Your server will start on port 8080, as shown by terminal output similar to the following.


Copy code


```text
2026/07/31 13:29:11 Server starting on :8080
```


You can now navigate to http://localhost:8080 to test your passwordless auth flow.


Enter your phone number and click **Send OTP** .


You will be redirected to the Verify OTP Code form. After you receive the code via SMS enter it into the form and submit the form. Whether the code was or not, you'll see a confirmation printed in the browser.


## That's the essentials of implementing passwordless authentication in Rust using Twilio Verify


Passwordless authentication using Rust and Twilio Verify presents a compelling alternative to traditional password-based systems. It offers a blend of enhanced security **and** user convenience, making it an attractive option for modern applications. While passwordless auth introduces some extra complexity to your application, the benefits — especially regarding security and user experience — are worth the investment.


*Matthew Setter is a PHP, Go, and Rust Editor in the Twilio Voices team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him atmsetter@twilio.com . He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*
