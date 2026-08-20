---
schema_version: "1.0.0"
document_id: "1f2e55db0134b8cdbc71aff6f6ab3767fb36e217c7794cab53f3ed91926f9fa3"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/build-passwordless-auth-with-twilio-verify-php"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T16:59:47.521529+00:00"
fetched_at: "2026-08-19T16:59:49.632670+00:00"
content_hash: "sha256:a90c5f8c729378565cae06a19447decc71ca8429dec46370b0dfe8573996049e"
---

# How to Build Passwordless Auth With Twilio Verify in PHP

Time to read:


-
-
-
-
-


August 19, 2026


**Written by**[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


---


## How to Build Passwordless Auth With Twilio Verify in PHP


Typing or generating a unique password for every new account or website is a hassle. Although password managers help, passwordless auth (authentication) offers a more streamlined and secure alternative.


In this tutorial, you will learn about passwordless auth, and build a PHP app that uses[Twilio Verify](https://www.twilio.com/docs/verify) to implement it.


## Prerequisites


Before you begin, ensure you have the following:


- A free Twilio account.[Click here to create a free account](https://www.twilio.com/try-twilio) if you are new to Twilio.
- PHP (ideally, version 8.5)
- A phone that can receive SMS
- Your favourite text editor or IDE (such as neovim or Visual Studio Code)


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


Set up a new project using[the Twilio / Slim Base Project](https://github.com/settermjd/twilio-slim-base-project) , by running the following command, where you store your PHP projects:


Copy code


```text
composer create-project settermjd/twilio-slim-base-project twilio-verify-php
cd twilio-verify-php
```


### Step 2: Install the required dependencies


Next, install the required dependencies, by running the following:


Copy code


```text
composer require bryanjhv/slim-session slim/twig-view twilio/sdk vlucas/phpdotenv
```


In case you're not familiar with them, here's a short description of the dependencies that you just installed:


- [PHP dotenv](https://github.com/vlucas/phpdotenv) : Loads environment variables from *.env* into` getenv()` ,` $_ENV` and` $_SERVER` automagically.
- [Slim Session](https://notes.enovision.net/slim/composer-package-documentation/bryanjhv_slim-session) : Simple middleware for Slim Framework 4, that allows managing PHP
- built-in sessions
- [Slim Framework Twig View](https://github.com/slimphp/Twig-View) : This is a Slim Framework view helper built on top of the Twig templating component.
- [Twilio's PHP Helper Library](https://www.twilio.com/docs/libraries/reference/twilio-php/) : This simplifies integrating with Twilio in PHP


Now, open the project directory in your preferred text editor or IDE.


### Step 3: Set the required environment variables


Dotenv files (commonly named *.env* ) are used to store the configuration information that your app needs during development, separate from the application's code. For this tutorial, it will be your Twilio credentials (i.e., your Twilio Account SID and Auth Token) and a Verify Service[SID](https://www.twilio.com/docs/glossary/what-is-a-sid) .


In your project's top-level directory, create a file named *.env* . Then, in *.env* , add the three variables below.


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


### Step 4: Build the base PHP application


The next thing to do is to load the environment variables. To do that, paste the code, below, into *public/index.php* :


Copy code


```text
<?php


declare(strict_types=1);


use App\Application;
use App\Config\RequiredEnvironmentVariables;
use DI\Container;
use Dotenv\Dotenv;
use Slim\Factory\AppFactory;
use Slim\Views\Twig;
use Slim\Views\TwigMiddleware;
use Twilio\Rest\Client;


require __DIR__ . '/../vendor/autoload.php';


$dotenv = Dotenv::createImmutable(__DIR__ . '/../');
$dotenv->load();
$dotenv->required(
[
'TWILIO_ACCOUNT_SID',
'TWILIO_AUTH_TOKEN',
'TWILIO_VERIFY_SERVICE_SID'
]
)->notEmpty();


$container = new Container();
$container->set(
Client::class,
fn(): Client => new Client(
$_ENV['TWILIO_ACCOUNT_SID'],
$_ENV['TWILIO_AUTH_TOKEN'],
$_ENV['TWILIO_VERIFY_SERVICE_SID'],
),
);
$container->set('session', function () {
return new \SlimSession\Helper();
});


AppFactory::setContainer($container);
$app = AppFactory::createFromContainer($container);
$app->add(
new \Slim\Middleware\Session([
'autorefresh' => true,
'lifetime'    => '1 hour',
'name'        => 'app_session',
]),
);


$twig = Twig::create(__DIR__ . '/../templates', ['cache' => false]);
$app->add(TwigMiddleware::create($app, $twig));


$application = new Application($app);
$application->setupRoutes();
$application->run();
```


The updated code, above, loads environment variables from the variables defined in *.env* , using PHP dotenv, ensuring that` TWILIO_ACCOUNT_SID` ,` TWILIO_AUTH_TOKEN` , and` TWILIO_VERIFY_SERVICE_SID` have been set and are not empty. Then, it adds session and Twig template support to the application, along with a Twilio Rest Client object to the application's DI container; which will be used when sending and verifying OTP codes.


With that done, update *src/Application.php* 's constructor to match the following:


Copy code


```text
public function __construct(private readonly SlimApp $app)
{
$app->add(new ContentLengthMiddleware());
$app->addBodyParsingMiddleware();
$app->addRoutingMiddleware();
$app->addErrorMiddleware(true, true, true);


$this->session          = new SlimSessionHelper();
$this->verifyServiceSid = $_ENV['TWILIO_VERIFY_SERVICE_SID'];
}
```


Then, add the following private class variables to the class:


Copy code


```text
private SlimSessionHelper $session;
private string $verifyServiceSid;
```


And after that, add the following use statement to the top of the file:


Copy code


```text
use SlimSession\Helper as SlimSessionHelper;
```


The revised constructor adds the session support that was initialised in *public/index.php* to the class.


### Step 5: Add the ability to send an OTP


Now, you'll add the first of two features: the ability to send an OTP code when requested by the user. To do that, add the following function to *src/Application.php* .


Copy code


```text
public function showSignInForm(
ServerRequestInterface $request,
ResponseInterface $response,
): ResponseInterface {
$view = Twig::fromRequest($request);
return $view->render($response, 'signin.html.twig', []);
}
```


The` showSignInForm()` function uses Twig to render *templates/sign-in.html.twig* . This function will be called in response to GET requests to the application's default route "/".


Now, in *templates* , create a new file named *sign-in.html.twig* , and in that file, paste the code below:


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Sign In</title>
<link href="https://assets.twilio.com/public_assets/paste-fonts/1.5.2/fonts.css" rel="stylesheet">
<link href="/static/css/styles.css" rel="stylesheet">
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


As you can see from the HTML above, it is a simplistic HTML page with a form, with a single field named "phone". Note that the field's` type` and[inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inputmode) attributes are set to "tel". This hints to browsers on mobile devices to render a virtual keyboard most appropriate for entering telephone data.


Then, back in *src/Application.php* , add the following code after the` showSignInForm()` function.


Copy code


```text
public function sendOtpCode(
ServerRequestInterface $request,
ResponseInterface $response,
): ResponseInterface {
$postData = $request->getParsedBody();
$phone    = $postData['phone'] ?? '';
if ($phone === '') {
$response
->withHeader('Location', '/')
->withStatus(StatusCodeInterface::STATUS_FOUND);
}
$this->session->phone = $postData['phone'];


$twilio = $this->app->getContainer()->get(Client::class);
assert($twilio instanceof Client);
$verification = $twilio->verify->v2
->services($this->verifyServiceSid)
->verifications
->create($postData['phone'], "sms");


$response = $response
->withHeader('Location', '/verify')
->withStatus(StatusCodeInterface::STATUS_FOUND);
return $response;
}
```


The` sendOtpCode()` function processes requests to the sign-in form. It retrieves the phone number parameter from the request and, if it's available, stores it in the current session. If it's not available, it redirects the user back to the sign in form.


Then, it attempts to send an OTP code to the user's phone number via SMS using Twilio Verify. If the code was successfully sent, the user is redirected to the "Verify OTP code" form, which you'll build next.


Now, in *src/Application.php* , update the` setupRoutes()` function with the code below:


Copy code


```text
public function setupRoutes(): void
{
$this->app->get('/', [$this, 'showSignInForm']);
$this->app->post('/', [$this, 'sendOtpCode']);
}
```


The code adds the GET and POST variants of the default route ("/") to the application's routing table; the GET version is handled by` showSignInForm()` , and the POST version by` sendOtpCode()` .


Now, add the following to the use statements at the top of the file:


Copy code


```text
use Slim\Views\Twig;
use Twilio\Rest\Client;
```


### Step 6: Add the ability to verify an OTP


You'll now add the functionality to verify the OTP codes the user receives. Start by adding the code below to the end of *src/Application.php* .


Copy code


```text
public function showVerifyOtpForm(
ServerRequestInterface $request,
ResponseInterface $response,
): ResponseInterface {
$view = Twig::fromRequest($request);
return $view->render($response, 'verify.html.twig', []);
}
public function verifyOtpCode(
ServerRequestInterface $request,
ResponseInterface $response,
): ResponseInterface {
if (! $this->session->exists('phone') || $this->session->get('phone', '') === '') {
$response = $response
->withHeader('Location', '/')
->withStatus(StatusCodeInterface::STATUS_FOUND);
return $response;
}


$postData = $request->getParsedBody();
$code     = $postData['code'] ?? '';
if ($code === '') {
$response = $response
->withHeader('Location', '/verify')
->withStatus(StatusCodeInterface::STATUS_FOUND);
return $response;
}


$twilio = $this->app->getContainer()->get(Client::class);
assert($twilio instanceof Client);
$verificationCheck = $twilio->verify->v2
->services($this->verifyServiceSid)
->verificationChecks
->create([
"code" => $postData['code'],
"to"   => $this->session->phone,
]);


$this->session->delete('phone');


return Twig::fromRequest($request)
->render(
$response,
'verification-status.html.twig',
[
'status'  => $verificationCheck->status === 'approved',
'message' => $verificationCheck->status === 'approved'
? "Verification was successful"
: "Verification failed",
],
);
}
```


The` showVerifyOtpForm()` method renders *templates/verify.html.twig* , providing the form for the user to enter to validate their OTP code.


The` verifyOtpCode()` function retrieves the phone number from the session. If it's not present or empty, the user is redirected to the sign in form. Otherwise, it attempts to parse the request data and retrieve the "code" parameter. If the "code" parameter wasn't present or was empty, the user is redirected to the verify OTP code form.


If the parameter was present, it uses the Twilio Rest Client to validate that the code supplied was the same as the one that was sent to the user's phone number. If the code was valid:


- The user's phone number is deleted from the current session
- *templates/verification-status.html.twig* is rendered as the function's response, along with two template variables:` status` set to` true` and` message` set to "Verification was successful".


If the code was not valid, *templates/verification-status.html.twig* will be rendered with` status` set to` false` and` message` set to "Verification failed".


With that done, in the *templates* directory create a new file named *verify.html.twig* and paste the code below into the file.


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Verify OTP</title>
<link href="/static/css/styles.css" rel="stylesheet">
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


Now, create another file in *templates* named *verification-status.html.twig* and paste the code below into the file.


Copy code


```text
<!DOCTYPE html>
<html>
<head>
<title>Verification Status</title>
<link href="/static/css/styles.css" rel="stylesheet">
</head>
<body class="container">
<main>
<h1>Verification Status</h1>
<p class="alert {{if .Success}}alert-success{{else}}alert-error{{end}}">{{ .Message }}</p>
</main>
</body>
</html>
```


The HTML renders the message in the` Message`[template variable](https://pkg.go.dev/text/template) , and styles it as either a success or error, based on the value of the` Success` template variable.


With the templates created, the final thing to do is to add the final two route definitions to the application's routing table. Do that by updating` setupRoutes()` in *src/Application.php* to the following:


Copy code


```text
public function setupRoutes(): void
{
$this->app->get('/', [$this, 'showSignInForm']);
$this->app->post('/', [$this, 'sendOtpCode']);
$this->app->get('/verify', [$this, 'showVerifyOtpForm']);
$this->app->post('/verify', [$this, 'verifyOtpCode']);
}
```


These add the GET and POST forms of the "/verify" route, handled by the` showVerifyOtpForm()` and` verifyOtpCode()` functions, respectively.


### Step 7: Download the application's CSS file


The last step in the process is to download the application's CSS file from the project's GitHub repository, to the project's *assets/css* directory, naming it *styles.css* .


## Test that the application works


Finally, it's time to test that the code works as expected. Start the application by running the following command.


Copy code


```text
composer serve
```


Your server will start on port 8080, as shown by terminal output similar to the following.


Copy code


```text
2026/07/31 13:29:11 Server starting on :8080
```


You can now navigate to http://localhost:8080 to test your passwordless auth flow.


Enter your phone number and click **Send OTP** .


You will be redirected to the Verify OTP Code form. After you receive the code via SMS enter it into the form and submit the form. Whether the code was or not, you'll see a confirmation printed in the browser.


## That's the essentials of implementing passwordless authentication in PHP using Twilio Verify


Passwordless authentication using PHP and Twilio Verify presents a compelling alternative to traditional password-based systems. It offers a blend of enhanced security *and* user convenience, making it an attractive option for modern applications. While passwordless auth introduces some extra complexity to your application, the benefits — especially regarding security and user experience — are worth the investment.


*Matthew Setter is a PHP, Go, and Rust Editor in the Twilio Dev Content team. He’s also the author of[Mezzio Essentials](https://mezzioessentials.com/) and[Deploy with Docker Compose](https://deploywithdockercompose.com/) . You can find him atmsetter@twilio.com . He's also on[LinkedIn](https://www.linkedin.com/in/matthewsetter/) and[GitHub](https://github.com/settermjd) .*
