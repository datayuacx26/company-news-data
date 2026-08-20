---
schema_version: "1.0.0"
document_id: "fa1d593a5b174d16a0d7bb7f8e975720b2b0554bfdb8ad31a2bd39b9c629958b"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/validate-phone-numbers-rust"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b70553408ef4bb303653b2a714ab0e31a8c42bd354cc9395bb98ac5e11076760"
---

# Validate Phone Numbers in Rust

Time to read:


-
-
-
-
-


July 09, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


**Reviewed by**[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## Validate Phone Numbers in Rust


When building apps that make use of phone numbers, it's helpful to know if they're valid before attempting to message or call them. In many scenarios, it doesn't matter if customer phone numbers were input incorrectly or aren't valid phone numbers, you'll still be charged when you attempt to message your customers.


Gladly, Twilio provides an efficient and powerful way of validating phone numbers using[the Lookup v2 API](https://www.twilio.com/docs/lookup/v2-api) .


In this tutorial, you'll learn how to use it to validate phone numbers. That way, you can avoid being charged unnecessarily.


## Prerequisites


You'll need the following to use the application:


- A Twilio account (free or paid).


- [Create an account](https://twilio.com/try-twilio) if you don't already have one.


- Rust (1.85 or above) and Cargo (1.94 or above).[Follow the installation docs](https://doc.rust-lang.org/cargo/getting-started/installation) if you haven't installed them yet.
- One or more phone numbers that you'd like to validate (ideally your own or those of people that you know)
- [Git](https://doc.rust-lang.org/cargo/getting-started/installation)
- Your preferred code editor or IDE, such as neovim or[Visual Studio Code](https://code.visualstudio.com/)
- Some terminal experience is helpful, though not required


## What is Twilio's Lookup v2 API?


If this is your first time hearing about the Lookup v2 API, it provides real-time phone number intelligence to help you verify users and prevent fraud. It ensures accurate information about phone number type, carrier, country, ownership, consent, if the phone number is active or reachable, SIM attribution details, and more without disrupting the user experience.


The basic service — available as part of all Twilio accounts — performs basic phone number validation. However, if you're on a paid plan, there is a range of[add-on data packages](https://www.twilio.com/docs/lookup/v2-api#data-packages) that provide even more in-depth carrier and caller information.


For example, you can retrieve the phone number's line type, such as whether it's a mobile, landline, fixed VoIP, non-fixed VoIP, etc, the number's call forwarding status, if a phone number's been reassigned since a given date, ownership of the number, and when a SIM was last changed for the number.


## Step 1: Create a base Rust project


To get started, run the following command to bootstrap a small Rust binary application using Cargo, wherever you keep your Rust applications.


Copy code


```text
cargo new twilio-lookup-api-with-rust
cd twilio-lookup-api-with-rust
```


## Step 2: Set the required environment variables


Next, you will set the environment variables` TWILIO_ACCOUNT_SID` and` TWILIO_AUTH_TOKEN` . These store your Twilio credentials, required to make authenticated requests to[Twilio's Lookup v2 API](https://www.twilio.com/docs/lookup/v2-api) .


To do that, first, create a new file in the project's top-level directory named *.env* . Then, in that file, paste the configuration below.


Copy code


```text
PHONE_NUMBER=
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
```


Now, retrieve the values for the two` TWILIO_*` environment variables, by[opening the Twilio Console](https://console.twilio.com/) in your browser of choice, and copying the **Account SID** and **Auth Token** values.After that, in *.env* , set the copied values as the values of` TWILIO_ACCOUNT_SID` and` TWILIO_AUTH_TOKEN` , respectively.


If you need help finding those credentials, you can[find details here](https://www.twilio.com/docs/iam/api/authtoken) .


With that done, in *.env* , set` PHONE_NUMBER` to any[E.164-formatted phone number](https://www.twilio.com/docs/glossary/what-e164) that you want to validate.


## Step 3: Add the required crates


The next thing to do is to add the required crates, to simplify development of the code. The application only requires a few:


To install them, run the following command in a new terminal tab or session:


Copy code


```text
cargo add dotenvy reqwest rustlio serde serde_json tokio \
--features reqwest/json \
--features serde/derive
```


If you're using Microsoft Windows, in the command above, replace the backslashes with carets.


## Step 4: Write the application's code


With the dependencies installed, it's time to write the core code of the application. To do that, update *main.rs* in the *src* directory to match the code below.


Copy code


```text
use rustlio::twilio::lookup;
use std::collections::HashMap;
use std::env;


#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
dotenvy::dotenv()?;


let phone_number = env::var("PHONE_NUMBER").expect("Recipient phone number is not available");
let account_sid = env::var("TWILIO_ACCOUNT_SID").expect("Twilio Account SID is not available");
let auth_token = env::var("TWILIO_AUTH_TOKEN").expect("Twilio Auth Token is not available");
let data = lookup::lookup_phone_data(
&phone_number,
&HashMap::new(),
&reqwest::Client::new(),
&account_sid,
&auth_token,
)
.await
.unwrap_or_else(|_| panic!("Unable to retrieve data for {phone_number}"));


if data.is_valid() {
println!("{phone_number} is valid")
} else {
println!("{phone_number} is NOT valid")
}


Ok(())


}
```


Thanks to[the Rustlio crate](https://crates.io/crates/rustlio) , the code is quite short. It starts off using dotenvy to load the variables in *.env* as environment variables. It then initialises three local variables (` user` ,` pass` , and` phone_number` ) from those three environment variables (` TWILIO_ACCOUNT_SID` and` TWILIO_AUTH_TOKEN` , and` PHONE_NUMBER` ), respectively.


After that, it uses the` lookup_phone_data()` function in Rustlio's` lookup` module, to retrieve details of the phone number in` pass` and[awaits](https://rust-lang.github.io/async-book/) a response. If the request was successful, a (partially) populated[PhoneNumber](https://docs.rs/rustlio/0.2.2/rustlio/twilio/lookup/struct.PhoneNumber) object will be returned. This is a simple struct with public properties for a number of the properties that Twilio sends back in response to requests to the Lookup v2 API.


If the phone number[is valid](https://docs.rs/rustlio/0.2.2/rustlio/twilio/lookup/struct.PhoneNumber#method.is_valid) , "<PHONE NUMBER> is valid" will be printed to the terminal, where` <PHONE NUMBER>` is the phone number that you set in *.env* . Otherwise, "<PHONE NUMBER> is NOT valid" will be printed instead *.*


## Test that the application works as expected


To validate a phone number, run the command below.


Copy code


```text
cargo run
```


If the phone number is valid, you should see output similar to the following printed to the terminal.


Copy code


```text
+61123456789 is valid
```


## Bonus: Retrieve extra phone number information using Data Packages


The base functionality, which you've just learned, only tells Twilio to retrieve a minimum of information about a phone number. However, by using[Data Packages](https://www.twilio.com/docs/lookup/v2-api#data-packages) , you can retrieve so much more. These are optional when calling the` lookup_phone_data()` function, as they're a paid feature, and three of them are in private beta.


In this final example you're going to use[the Line Type Intelligence Data Package](https://www.twilio.com/docs/lookup/v2-api/line-type-intelligence) to retrieve the phone number's line type, i.e., whether the phone number is a mobile, landline, fixed VoIP, non-fixed VoIP, toll-free, the number's carrier, and mobile country code.


To do that, update the call to` lookup::lookup_phone_data()` in *src/main.rs* and the if/else block after it as follows.


Copy code


```text
let data = lookup::lookup_phone_data(
&phone_number,
&HashMap::from([(lookup::DataPackage::LineTypeIntelligence, true)]),
&reqwest::Client::new(),
&account_sid,
&auth_token,
)
.await
.unwrap_or_else(|_| panic!("Unable to retrieve data for {phone_number}"));


if data.is_valid() {
println!("{phone_number} is valid");
println!("{:?}", data.get_line_type_intelligence());
} else {
println!("{phone_number} is NOT valid")
}
```


Now, if you run the code again, with` cargo run` , you should see it print out Line Type Intelligence data, similar to the example below.


Copy code


```text
LineTypeIntelligence { line_type: Some("mobile"), mobile_country_code: Some("505"), mobile_network_code: Some("01"), carrier_name: Some("Telstra Corporation(01)"), error_code: None }
```


## That's how to validate a phone number in Rust using Twilio's Lookup v2 API


Thanks to the power of the Lookup v2 API and the Rustlio crate, only a minimum of Rust code was required to validate a phone number.


If you'd like to extend the code, check out the documentation for[Twilio's Lookup v2 API](https://www.twilio.com/docs/lookup/v2-api) and[Rustlio's lookup module's documentation](https://docs.rs/rustlio/0.2.2/rustlio/twilio/lookup/index) .


Otherwise, I can't wait to see what you build!


*Matthew Setter is (primarily) the PHP, Go, and Rust editor on the Twilio Voices team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him at msetter\[at\]twilio.com. He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*


[Validation](https://www.flaticon.com/free-icons/validation) and[phone](https://www.flaticon.com/free-icons/phone) icons created by Freepik on Flaticon, and the[Rust logo was retrieved from PNGWING](https://www.pngwing.com/en/free-png-zslte) .
