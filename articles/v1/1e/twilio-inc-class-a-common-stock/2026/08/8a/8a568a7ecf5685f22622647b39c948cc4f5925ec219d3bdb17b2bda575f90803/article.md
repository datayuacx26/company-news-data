---
schema_version: "1.0.0"
document_id: "8a568a7ecf5685f22622647b39c948cc4f5925ec219d3bdb17b2bda575f90803"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/integrations/pingone-custom-server-mfa-verify"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T14:12:18.695062+00:00"
fetched_at: "2026-08-12T14:12:20.064132+00:00"
content_hash: "sha256:3204f53f97505bd6be0e6684df31000d2c95efd2f8c05cbb107ba0fd96dbc98f"
---

# Deliver Secure and Reliable OTPs with Twilio Verify and PingOne

## Deliver Secure and Reliable OTPs with Twilio Verify and PingOne


PingOne now supports[direct integration](https://docs.pingidentity.com/pingone/settings/p1_using_twilio_verify_for_notifications) with[Twilio Verify](https://www.twilio.com/en-us/user-authentication-identity/verify) through its built-in custom server notification provider. This enables customers to improve one-time passcode (OTP) delivery success and reduce fraud such as[SMS pumping](https://www.twilio.com/docs/verify/preventing-toll-fraud) and fake account creation. By plugging Twilio Verify into PingOne’s MFA notification system, customers gain the benefits of[global sender management](https://www.twilio.com/en-us/blog/9-reasons-to-use-the-verify-api) ,[intelligent fallback](https://www.twilio.com/docs/verify/fallback-scenarios) , and[built-in fraud protection](https://www.twilio.com/docs/verify/preventing-toll-fraud/sms-fraud-guard) .


This guide walks through how to configure the integration using PingOne settings, with no additional orchestration tools required.


## Why use Twilio Verify?


Twilio empowers businesses to stop fraud before it starts, ensuring that only legitimate users gain access while maximizing conversion rates.[Twilio Verify](https://www.twilio.com/docs/verify) is a fully managed OTP delivery platform that helps secure and streamline user authentication across multiple[channels](https://www.twilio.com/docs/verify/authentication-channels) . Twilio Verify[manages global sender pools](https://www.twilio.com/en-us/blog/9-reasons-to-use-the-verify-api) for you so you can scale worldwide without managing phone number registrations.


Unlike basic messaging APIs, Twilio Verify adds layers of[automation](https://www.twilio.com/docs/verify/api/verification) and[routing intelligence](https://www.twilio.com/docs/verify/fallback-scenarios#optimal-channel-selection-features) that increase OTP delivery, and[fraud protection](https://www.twilio.com/docs/verify/preventing-toll-fraud/sms-fraud-guard) against SMS pumping attacks. Features like[Geo Permissions](https://www.twilio.com/docs/verify/preventing-toll-fraud/verify-geo-permissions) allow you to block OTP delivery to unwanted or high-risk countries based on the country code, helping protect your application from suspicious traffic. Twilio Verify also includes[intelligent channel orchestration](https://www.twilio.com/docs/verify/fallback-scenarios) , built-in[message localization](https://www.twilio.com/docs/verify/supported-languages) , pre-send carrier checks using[Twilio Lookup](https://www.twilio.com/docs/lookup) , and configurable[rate limiting](https://www.twilio.com/docs/verify/api/programmable-rate-limits) to help ensure OTPs reach real users quickly while minimizing delivery failures and reducing operational complexity.


With seamless integration and reduced overhead, businesses can accelerate time to market, scale authentication with confidence, and focus on growth without compromising security or user experience.


## How PingOne's Custom Server setup works with Twilio Verify


The sequence of events for SMS/Voice one-time passcode (OTP) authentication includes:


- The user enters their email and phone number on the login page of the application.
- (Optional) The application sends the user’s phone number to the Twilio Lookup API to access features like number formatting and line type detection. This step applies only if the[Twilio Lookup packages](https://www.twilio.com/docs/lookup/v2-api#data-packages) have been purchased.
- The Twilio Lookup API returns the correctly[formatted “To Number](https://www.twilio.com/docs/glossary/what-e164) ,” and confirms the number is a valid mobile number. The user is redirected to PingOne for authentication.
- PingOne checks if MFA is already configured for the user.
- If MFA is already configured, PingOne calls your custom server, which in turn uses the Twilio Verify API to initiate the OTP request.
- If MFA is not configured, PingOne prompts the user to enroll in MFA (e.g., SMS or Voice).
- Twilio Verify delivers the OTP to the user via the selected channel (SMS, Voice).
- Once the OTP is delivered, the user enters it on the PingOne authentication screen. PingOne verifies the code, completes authentication, and confirms the user’s identity and MFA status.
- The user can select a fallback method if they cannot receive the OTP
- After successful authentication, the user is granted access to the protected application. For this blog, a success screen is shown displaying the user’s name, email, PingOne ID, and token details.


When using the custom server integration, PingOne automatically sends feedback to Twilio Verify about the OTP verification outcome. This enables real-time monitoring and analytics within Twilio.


## Prerequisites for Sending OTPs with PingOne and Twilio Verify


Before you begin, make sure the following are ready:


1. [Node.js v24+](https://nodejs.org/) installed on your system
2. A[PingOne account](https://www.pingidentity.com/en/try-ping)
3. A Twilio account ([Sign up or log in](https://www.twilio.com/try-twilio) )
4. A Twilio Verify Service created from the[Twilio Console](https://www.twilio.com/console/verify/services)
5. Twilio Account SID (starts with` AC...` )
6. Twilio Auth Token
7. Verify Service SID (starts with` VA...` )
8. Custom Code enabled on your Verify service.
9. Navigate to your Verify service → Code Configuration → Enable Custom Verification Code


Once you have those in place, we can get started.


## Step 1: Create or edit the MFA Policy


For this demo, you’ll need a[PingOne MFA policy.](https://docs.pingidentity.com/pingone/strong_authentication_mfa/p1_creating_an_mfa_policy_for_strong_auth) You can either edit the default policy or create a new one. In this guide, we edited the default policy to enable SMS and Voice as MFA options. To do this:


1. Go to **Authentication** > **MFA**
2. Select **Default MFA Policy**
3. Click on the **pencil** icon and check the *SMS* and *Voice* options.
4. Click **Save**


## Step 2: Create an Authentication Policy in PingOne


For this demo, you’ll need a[PingOne authentication policy](https://docs.pingidentity.com/pingone/authentication/p1_add_an_auth_policy) that requires MFA. If one doesn’t already exist in your environment, follow the steps below to create a new policy:


1. In the PingOne admin console, go to **Authentication** > **Authentication** .


2. Click + Add Policy.


3. Enter a Policy Name (e.g.,` Multi_Factor` ).


4.[For Step Type, select Login](https://docs.pingidentity.com/pingone/authentication/p1_add_login_auth_step) .


5. Under Login settings:


- Enable **Account Recovery**
- Enable **Registration**
- Set **Registration Method** to **PingOne**
- Set **Population** to **Default**


6. Click[+ Add Step and select Multi-factor Authentication](https://docs.pingidentity.com/pingone/authentication/p1_add_mfa_step) from the dropdown.


7. Under MFA Settings:


- Choose the default MFA policy
- Set Bypass option to: *None or Incompatible Methods*
- Under **Required When** , select: *Last sign-on older than 1 minute* (You can adjust this time setting based on your security requirements.)


8. Click **Save** to create the policy.


## Step 3: Create a PingOne Application


Below are the steps to add a new Single Page Application in PingOne, which will be used to log in and test Twilio Lookup and Twilio Verify. You can find more details in the PingOne documentation linked[here](https://docs.pingidentity.com/pingone/applications/p1_applications_add_applications) .


To create a PingOne Application:


1. Log in to your PingOne admin console.


2. Navigate to **Applications** > **My Applications** and click **+ Add Application** .


3. Select[Single-Page Application](https://docs.pingidentity.com/pingone/applications/p1_edit_application_singlepage) as the type.


4. Enter your Application Name and Description, then click **Save** .


5. Open your newly created application and go to the **Configuration** tab.


6. Set the Redirect URI to:[http://localhost:3000/auth/callback](http://localhost:3000/auth/callback)


7. Switch to the Resources tab and assign the following scopes:


1. ` email`
2. ` openid`
3. ` phone`
4. ` Profile`


8. Navigate to the **Policies** tab and select an **Authentication Policy** ( *Multi_factor* ) with MFA enabled.


9. Click Save to apply the changes.


10. Finally, make note of the following configuration values for later use:


1. Environment ID
2. Client ID
3. Client Secret
4. Token Endpoint
5. User Info Endpoint


## Step 4: Configure Twilio Verify in the PingOne Custom Server option


In this step, you’ll configure Twilio Verify as a custom notification provider in PingOne using the Custom Server option. This setup allows PingOne to call your backend, which then triggers the Verify API to send OTPs. To do this:


1. Log in to your PingOne admin console.
2. Navigate to **Settings** > **Sender** and click the pencil icon.
3. Select *Custom Server*
4. In the **Sms/Voice Sender** list, select **Twilio Verify** .
5. Enter the Account Sid for your Twilio account.
6. Enter the Auth Token for your Twilio account.
7. Enter the Twilio Verify Service SID.
8. Click **Save** .


## Step 5: Set Notification Policies and Templates


There are two ways to manage notification templates for MFA in PingOne:


### Option 1: Use Twilio Verify Templates


With Twilio Verify, you can use the default template provided or request a custom template through Twilio Support.


To configure templates:


- In the[Twilio Console](https://console.twilio.com/) , go to **Verify** > **Services**
- Select your Verify Service
- Under the **General** tab, scroll down to the **Message Template Configuration** section
- From the dropdown, choose the[default SMS template](https://www.twilio.com/docs/verify/verification-templates#verify-default-template) , pick a[pre approved template](https://www.twilio.com/docs/verify/verification-templates#pre-approved-templates) or a[custom template](https://www.twilio.com/docs/verify/verification-templates#custom-templates) *Note:*[Read this support article](https://help.twilio.com/hc/en-us/articles/9960174409627-Is-it-Possible-to-Customize-the-Verification-Message-for-Verify-?_gl=1*1cg6fev*_gcl_aw*R0NMLjE3NTAwOTM2ODguQ2p3S0NBandnYl9DQmhCTUVpd0EwcDNvT1BUMzl2cE1UOEQzMzhKYXZqM2ZiXzJzOWYwU3JoMEQ0M0hRZm81MUZ1V2diVmJXSDZYbkFob0NxdHNRQXZEX0J3RQ..*_gcl_au*MTEyNTM1MjY2OC4xNzUwMDkzNjg4LjQ0MTAxOTQwOS4xNzUxNDA2MTgyLjE3NTE0MDYxODI.*_ga*NTk0NTcwNTkuMTcxMjAwMjM4OA..*_ga_RRP8K4M4F3*czE3NTI2NDI3MzMkbzk4JGcxJHQxNzUyNjQyNzMzJGo2MCRsMCRoMA..) for more information on how to request a custom template if our other templates do not suit your business needs.
- Save the service


### Option 2: Use PingOne Templates


Alternatively, you can use PingOne’s built-in templates.


The first step is to set the Notification Policies for SMS/Voice. To do this:


1. Log in to the PingOne admin console
2. Navigate to **User Experience** > **Notification Policies**
3. Click the policy you want to edit and then click the **pencil** icon, or click the **plus icon (+)** to create a new notification policy
4. Under **Provider Locations** , configure the providers for SMS and Voice:
5. Make sure the **First Provider** is set to *Twilio Verify*
6. Optionally, set the Second and Third Providers using the dropdown if available
7. Check the boxes for **SMS** and **Voice**
8. Click **Save**


For MFA Templates:


- Use the Strong Authentication Notification Template for OTP delivery


- Use the Device Pairing Template when new users are adding an MFA method


You can customize Twilio Verify templates and set locale and language preferences. If you select a Twilio custom template and leave the PingOne configuration unchanged, the message sent to users will follow the Twilio template. If you select a template from the PingOne template list, that template will override the Twilio configuration and the message will follow the PingOne version.


If you need to use your own custom templates, you[must request approval](https://help.twilio.com/articles/9960174409627-Is-it-Possible-to-Customize-the-Verification-Message-for-Verify-) by the Twilio Verify support team. After approval, they can be used in PingOne with the appropriate locale settings.


## Step 6: Set up the demo application to test the integration


### Clone the demo app


Clone the Node.js app from[this repo](https://github.com/twilio-samples/ping-verify-integration) :


Copy code


```text
git clone git@github.com:twilio-samples/ping-verify-integration.git
```


### Set up environment variables


Create a .env file by copying the example:


Copy code


```text
cp .env.example .env
```


Update the` .env` file with the following values:


- ` TWILIO_ACCOUNT_SID` =Twilio Account SID
- ` TWILIO_AUTH_TOKEN` =Twilio Auth Token
- ` PINGONE_ENV_ID` =PingOne environment ID
- ` PINGONE_APP_ID` =PingOne App ID from step 3
- ` PINGONE_REDIRECT_URI` =http://localhost:3000/auth/callback


### Install dependencies


Make sure you're using Node.js v18+ (use` nvm` if needed):


Copy code


```text
npm install
```


### Start the application


Run the application


Copy code


```text
node app.js
```


### Test your application


Follow the steps below to test the end-to-end login flow using PingOne and Twilio:


#### 1. Launch the app


1. Visit:[http://localhost:3000](http://localhost:3000/)
2. Enter your email address and phone number
3. Click **Login**


#### 2. Twilio Lookup check


1. Your terminal window will show the application's activity and debug output.


2. If the phone number is invalid, the app will stop and prompt you to enter a valid number.


3. This validation is powered by the Twilio Lookup API, which ensures only valid mobile numbers proceed to authentication.


#### 3. Redirect to PingOne Login


1. If the number is valid, you will be redirected to the PingOne login screen.
2. Make sure the user you're logging in with is already created in PingOne and has MFA enabled.
3. You can[customize the branding and themes](https://docs.pingidentity.com/pingone/user_experience/p1_branding_themes) for this login page in the PingOne admin console.


#### 4. First-time login experience


If this is the user's first login:


1. PingOne will prompt the user to reset their password.


2. After that, it will prompt the user to enroll in an MFA method (SMS or Voice).


3. Click **Enroll in MFA** .


4. A new window will open to add an authentication method.


#### 5. Enroll in MFA


1. Click **Add Method**


2. Choose your preferred channel Text Message (SMS) or Voice


3. For this demo, we selected **Text Message**


4. You should receive an SMS based on your configured **PingOne notification template**


5. Plug in the OTP code in the MFA setup screen


6. MFA will now be set up for this user


#### 6. Log in again with MFA


1. Go back to[http://localhost:3000](http://localhost:3000/)


2. Log in again with the same email and phone number


3. Now you’ll see the MFA prompt to request an OTP from your profile


4. Click on **Text Message** and you will receive a new OTP based on the selected PingOne template.


5. Once verified, you will be successfully logged in to the application using Twilio and PingOne!


#### 7. View OTP Delivery Status in your Twilio Console


Once the OTP is sent, you can confirm that the request was successful in the Twilio Console.


1. Log in to your Twilio[Console](https://console.twilio.com/?frameUrl=/console)
2. Navigate to **Monitor** > **Logs** > **Verify**
3. Click on a verification attempt
4. The status should show as approved once the user enters the correct OTP


## Delivering OTPs with Twilio Verify in PingOne


Your users can now log in securely using multi-factor authentication, with one-time passwords sent via Twilio Verify. Want to explore more about what Twilio Verify can do? Check out[Twilio Verify API documentation](https://www.twilio.com/docs/verify) . We can't wait to see what you build and secure!
