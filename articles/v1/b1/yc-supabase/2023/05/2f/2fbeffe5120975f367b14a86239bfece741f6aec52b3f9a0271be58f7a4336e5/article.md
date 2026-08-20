---
schema_version: "1.0.0"
document_id: "2fbeffe5120975f367b14a86239bfece741f6aec52b3f9a0271be58f7a4336e5"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/flutter-multi-factor-authentication"
published_at: "2023-05-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:3c8641f05df608845e6c209b380bed2af9c55d3e99faf809023839a3fe962676"
---

# Securing your Flutter apps with Multi-Factor Authentication

Multi-factor authentication or MFA is an essential part of security for any kind of app.


We will take a look at an example app where a user has to sign in via MFA in order to view the contents of the app to demonstrate how easy it is to get started with MFA on Flutter.


## What is Multi-Factor Authentication?#


Multi-factor authentication (MFA), sometimes called two-factor authentication (2FA), is an additional security layer on top of traditional login methods such as email and password login.


There are several forms of MFA, such as with an SMS or through using an authenticator app such as Google Authenticator. It is considered a best practice to use MFA whenever possible because it protects users against weak passwords or compromised social accounts.


## Why Multi-Factor Authentication matters for Flutter apps#


In the context of Flutter apps, MFA is important because it helps protect sensitive user data and prevent unauthorized access to user accounts. By requiring users to provide an additional factor, MFA adds an extra layer of security that makes it harder for attackers to gain access to user accounts.


Given how Flutter is widely used MFA might be a requirement rather than a nice-to-have. Implementing MFA in a Flutter app can improve overall security and give users peace of mind knowing that their data is better protected.


## Building the App#


We are building a simple app where users register with an email and password. After completing the registration process, the users will be asked to set up MFA using an authenticator app. Once verifying the identity via the authenticator app, the user can go to the home page where they can view the main content.


Login works similarly, where after an email and password login, they are asked to enter the verification code to complete the login process.


The app will have the following directory structure, where` auth` contains any basic auth-related pages,` mfa` contains enrolling and verifying the MFA, and we have some additional pages for us to see that MFA is working correctly.


You can find the complete code created in this article[here](https://github.com/supabase/supabase/tree/master/examples/auth/flutter-mfa) .


### Step 1: Setup the scenes#


Let’s start with the` flutter create` command.


`
_10


flutter create mfa_app


`


Also, if you do not have a Supabase project yet, create one by heading to[database.new](https://dstabase.new/) . Within a few minutes, you will have a new Supabase project.


### Step 2: Add the dependencies#


Install the[supabase_flutter](https://pub.dev/packages/supabase_flutter) package by running the following command in your terminal.


`
_10


dart pub add supabase_flutter


`


Then update your` lib/main.dart` file to initialize Supabase in the main function. You should be able to find your Supabase URL and AnonKey from the` settings -> api` section of your dashboard. We will also extract the` SupabaseClient` for easy access to our Supabase instance.


`
_13


import 'package:flutter/material.dart';


_13


import 'package:supabase_flutter/supabase_flutter.dart';


_13


_13


void main() async {


_13


await Supabase.initialize(


_13


url: 'SUPABASE_URL',


_13


anonKey: 'SUPABASE_ANONKEY',


_13


);


_13


runApp(const MyApp());


_13


}


_13


_13


/// Extract SupabaseClient instance in a handy variable


_13


final supabase = Supabase.instance.client;


`


Also, add[go_router](https://pub.dev/packages/go_router) to handle our routing and redirects.


`
_10


dart pub add go_router


`


We will set up the routes towards the end when we have created all the pages we need. With this, we are ready to jump into creating the app.


Also, if we want to support iOS and Android, we need to set up deep links so that a session can be obtained upon clicking on the confirmation link sent to the user’s email address.


We will configure it so that we can open the app by redirecting to` mfa-app://callback` .


For iOS, open` ios/Runner/info.plist` file and add the following deep link configuration.


`
_24


<!-- ... other tags -->


_24


<plist>


_24


<dict>


_24


<!-- ... other tags -->


_24


_24


<!-- Deep Links -->


_24


<key>FlutterDeepLinkingEnabled</key>


_24


<true/>


_24


<key>CFBundleURLTypes</key>


_24


<array>


_24


<dict>


_24


<key>CFBundleTypeRole</key>


_24


<string>Editor</string>


_24


<key>CFBundleURLSchemes</key>


_24


<array>


_24


<string>mfa-app</string>


_24


</array>


_24


</dict>


_24


</array>


_24


<!-- Deep Links -->


_24


_24


<!-- ... other tags -->


_24


</dict>


_24


</plist>


`


For Android, open` android/app/src/main/AndroidManifest.xml` file and add the following deep link configuration.


`
_21


<manifest ...>


_21


<!-- ... other tags -->


_21


<application ...>


_21


<activity ...>


_21


<!-- ... other tags -->


_21


_21


<!-- Deep Links -->


_21


<meta-data android:name="flutter_deeplinking_enabled" android:value="true" />


_21


<intent-filter>


_21


<action android:name="android.intent.action.VIEW" />


_21


<category android:name="android.intent.category.DEFAULT" />


_21


<category android:name="android.intent.category.BROWSABLE" />


_21


<data


_21


android:scheme="mfa-app"


_21


android:host="callback" />


_21


</intent-filter>


_21


<!-- END Deep Links -->


_21


_21


</activity>


_21


</application>


_21


</manifest>


`


After, we will add the deep link as one of the redirect URLs in our Supabase dashboard.


Go to` Authentication > URL Configuration` and add` mfa-app://callback/*` as a redirect URL. Make sure you don’t add any extra slashes or anything because if you do, deep linking will not work properly.


Lastly, we will add the` flutter_svg` package. This package will later be used to display a QR code to scan with their authentication app.


`
_10


dart pub add flutter_svg


`


That is all the dependencies that we need. Let’s dive into coding!


### Step 3: Create the signup flow#


Let’s first create a signup flow. Again, the user will register with the app using email and password, and after confirming their email address, they will enroll in MFA using an authenticator app.


The register page contains a form with an email and password field for the user to create a new account. We are just calling the[.signUp()](https://supabase.com/docs/reference/dart/auth-signup) method with it. As you can see in the code below at` emailRedirectTo` option of the` .signUp()` method, upon clicking on the confirmation link sent to the user, they will be taken to MFA enrollment page, which we will implement later.


Create a` lib/pages/auth/signup_page.dart` file and add the following. There will be some errors, but that is because we haven’t created some of the files yet. The errors will go away as we move on, so ignore them for now.


`
_101


import 'package:flutter/material.dart';


_101


import 'package:go_router/go_router.dart';


_101


import 'package:mfa_app/main.dart';


_101


import 'package:mfa_app/pages/auth/login_page.dart';


_101


import 'package:mfa_app/pages/mfa/enroll_page.dart';


_101


import 'package:supabase_flutter/supabase_flutter.dart';


_101


_101


class RegisterPage extends StatefulWidget {


_101


static const route = '/auth/register';


_101


_101


const RegisterPage({super.key});


_101


_101


@override


_101


State<RegisterPage> createState() => _RegisterPageState();


_101


}


_101


_101


class _RegisterPageState extends State<RegisterPage> {


_101


final _emailController = TextEditingController();


_101


final _passwordController = TextEditingController();


_101


_101


bool _isLoading = false;


_101


_101


@override


_101


void dispose() {


_101


_emailController.dispose();


_101


_passwordController.dispose();


_101


super.dispose();


_101


}


_101


_101


@override


_101


Widget build(BuildContext context) {


_101


return Scaffold(


_101


appBar: AppBar(title: const Text('Register')),


_101


body: ListView(


_101


padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 24),


_101


children: \[


_101


TextFormField(


_101


controller: _emailController,


_101


decoration: const InputDecoration(


_101


label: Text('Email'),


_101


),


_101


),


_101


const SizedBox(height: 16),


_101


TextFormField(


_101


controller: _passwordController,


_101


decoration: const InputDecoration(


_101


label: Text('Password'),


_101


),


_101


obscureText: true,


_101


),


_101


const SizedBox(height: 16),


_101


ElevatedButton(


_101


onPressed: () async {


_101


try {


_101


setState(() {


_101


_isLoading = true;


_101


});


_101


final email = _emailController.text.trim();


_101


final password = _passwordController.text.trim();


_101


await supabase.auth.signUp(


_101


email: email,


_101


password: password,


_101


emailRedirectTo:


_101


'mfa-app://callback${MFAEnrollPage.route}', // redirect the user to setup MFA page after email confirmation


_101


);


_101


if (mounted) {


_101


ScaffoldMessenger.of(context).showSnackBar(


_101


const SnackBar(content: Text('Check your inbox.')));


_101


}


_101


} on AuthException catch (error) {


_101


ScaffoldMessenger.of(context)


_101


.showSnackBar(SnackBar(content: Text(error.message)));


_101


} catch (error) {


_101


ScaffoldMessenger.of(context).showSnackBar(


_101


const SnackBar(content: Text('Unexpected error occurred')));


_101


}


_101


if (mounted) {


_101


setState(() {


_101


_isLoading = false;


_101


});


_101


}


_101


},


_101


child: _isLoading


_101


? const SizedBox(


_101


height: 24,


_101


width: 24,


_101


child: Center(


_101


child: CircularProgressIndicator(color: Colors.white)),


_101


)


_101


: const Text('Register'),


_101


),


_101


const SizedBox(height: 16),


_101


TextButton(


_101


onPressed: () => context.push(LoginPage.route),


_101


child: const Text('I already have an account'),


_101


)


_101


\],


_101


),


_101


);


_101


}


_101


}


`


We can then create the enrollment page for MFA. This page is taking care of the following.


- Retrieve the enrollment secret from the server via` supabase.auth.mfa.enroll()` method.
- Displaying the secret and its QR code representation and prompts the user to add the app to their authenticator app
- Verifies the user with a TOTP


The QR code and the secret will be displayed automatically when the page loads. When the user enters the correct 6-digit TOTP, they will be automatically redirected to the home page.


Create` lib/pages/mfa/enroll_page.dart` file and add the following.


`
_135


import 'package:flutter/material.dart';


_135


import 'package:flutter/services.dart';


_135


import 'package:flutter_svg/flutter_svg.dart';


_135


import 'package:go_router/go_router.dart';


_135


import 'package:mfa_app/main.dart';


_135


import 'package:mfa_app/pages/auth/register_page.dart';


_135


import 'package:mfa_app/pages/home_page.dart';


_135


import 'package:supabase_flutter/supabase_flutter.dart';


_135


_135


class MFAEnrollPage extends StatefulWidget {


_135


static const route = '/mfa/enroll';


_135


const MFAEnrollPage({super.key});


_135


_135


@override


_135


State<MFAEnrollPage> createState() => _MFAEnrollPageState();


_135


}


_135


_135


class _MFAEnrollPageState extends State<MFAEnrollPage> {


_135


final _enrollFuture = supabase.auth.mfa.enroll();


_135


_135


@override


_135


Widget build(BuildContext context) {


_135


return Scaffold(


_135


appBar: AppBar(


_135


title: const Text('Setup MFA'),


_135


actions: \[


_135


TextButton(


_135


onPressed: () {


_135


supabase.auth.signOut();


_135


context.go(RegisterPage.route);


_135


},


_135


child: Text(


_135


'Logout',


_135


style: TextStyle(color: Theme.of(context).colorScheme.onPrimary),


_135


),


_135


),


_135


\],


_135


),


_135


body: FutureBuilder(


_135


future: _enrollFuture,


_135


builder: (context, snapshot) {


_135


if (snapshot.hasError) {


_135


return Center(child: Text(snapshot.error.toString()));


_135


}


_135


if (!snapshot.hasData) {


_135


return const Center(child: CircularProgressIndicator());


_135


}


_135


_135


final response = snapshot.data!;


_135


final qrCodeUrl = response.totp.qrCode;


_135


final secret = response.totp.secret;


_135


final factorId = response.id;


_135


_135


return ListView(


_135


padding: const EdgeInsets.symmetric(


_135


horizontal: 20,


_135


vertical: 24,


_135


),


_135


children: \[


_135


const Text(


_135


'Open your authentication app and add this app via QR code or by pasting the code below.',


_135


style: TextStyle(


_135


fontWeight: FontWeight.bold,


_135


),


_135


),


_135


const SizedBox(height: 16),


_135


SvgPicture.string(


_135


qrCodeUrl,


_135


width: 150,


_135


height: 150,


_135


),


_135


const SizedBox(height: 16),


_135


Row(


_135


children: \[


_135


Expanded(


_135


child: Text(


_135


secret,


_135


style: const TextStyle(


_135


fontWeight: FontWeight.bold,


_135


fontSize: 18,


_135


),


_135


),


_135


),


_135


IconButton(


_135


onPressed: () {


_135


Clipboard.setData(ClipboardData(text: secret));


_135


ScaffoldMessenger.of(context).showSnackBar(const SnackBar(


_135


content: Text('Copied to your clip board')));


_135


},


_135


icon: const Icon(Icons.copy),


_135


),


_135


\],


_135


),


_135


const SizedBox(height: 16),


_135


const Text('Enter the code shown in your authentication app.'),


_135


const SizedBox(height: 16),


_135


TextFormField(


_135


decoration: const InputDecoration(


_135


hintText: '000000',


_135


),


_135


style: const TextStyle(fontSize: 24),


_135


textAlign: TextAlign.center,


_135


keyboardType: TextInputType.number,


_135


onChanged: (value) async {


_135


if (value.length != 6) return;


_135


_135


// kick off the verification process once 6 characters are entered


_135


try {


_135


final challenge =


_135


await supabase.auth.mfa.challenge(factorId: factorId);


_135


await supabase.auth.mfa.verify(


_135


factorId: factorId,


_135


challengeId: challenge.id,


_135


code: value,


_135


);


_135


await supabase.auth.refreshSession();


_135


if (mounted) {


_135


context.go(HomePage.route);


_135


}


_135


} on AuthException catch (error) {


_135


ScaffoldMessenger.of(context)


_135


.showSnackBar(SnackBar(content: Text(error.message)));


_135


} catch (error) {


_135


ScaffoldMessenger.of(context).showSnackBar(const SnackBar(


_135


content: Text('Unexpected error occurred')));


_135


}


_135


},


_135


),


_135


\],


_135


);


_135


},


_135


),


_135


);


_135


}


_135


}


`


### Step 4: Creating the login flow#


Now that we have created a registration flow, we can get to the login flow for returning existing users. Again, the login page has nothing fancy going. We are just collecting the user’s email and password, and calling the good old[.signInWithPassword()](https://supabase.com/docs/reference/dart/auth-signinwithpassword) method. Upon signing in, the user will be taken to a verify page where the user will then enter their verification code from their authenticator app.


Create` lib/pages/auth/login_page.dart` and add the following.


`
_75


import 'package:flutter/material.dart';


_75


import 'package:go_router/go_router.dart';


_75


import 'package:mfa_app/main.dart';


_75


import 'package:mfa_app/pages/mfa/verify_page.dart';


_75


import 'package:supabase_flutter/supabase_flutter.dart';


_75


_75


class LoginPage extends StatefulWidget {


_75


static const route = '/auth/login';


_75


_75


const LoginPage({super.key});


_75


_75


@override


_75


State<LoginPage> createState() => _LoginPageState();


_75


}


_75


_75


class _LoginPageState extends State<LoginPage> {


_75


final _emailController = TextEditingController();


_75


final _passwordController = TextEditingController();


_75


_75


@override


_75


void dispose() {


_75


_emailController.dispose();


_75


_passwordController.dispose();


_75


super.dispose();


_75


}


_75


_75


@override


_75


Widget build(BuildContext context) {


_75


return Scaffold(


_75


appBar: AppBar(title: const Text('Login')),


_75


body: ListView(


_75


padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 24),


_75


children: \[


_75


TextFormField(


_75


controller: _emailController,


_75


decoration: const InputDecoration(


_75


label: Text('Email'),


_75


),


_75


),


_75


const SizedBox(height: 16),


_75


TextFormField(


_75


controller: _passwordController,


_75


decoration: const InputDecoration(


_75


label: Text('Password'),


_75


),


_75


obscureText: true,


_75


),


_75


const SizedBox(height: 16),


_75


ElevatedButton(


_75


onPressed: () async {


_75


try {


_75


final email = _emailController.text.trim();


_75


final password = _passwordController.text.trim();


_75


await supabase.auth.signInWithPassword(


_75


email: email,


_75


password: password,


_75


);


_75


if (mounted) {


_75


context.go(MFAVerifyPage.route);


_75


}


_75


} on AuthException catch (error) {


_75


ScaffoldMessenger.of(context)


_75


.showSnackBar(SnackBar(content: Text(error.message)));


_75


} catch (error) {


_75


ScaffoldMessenger.of(context).showSnackBar(


_75


const SnackBar(content: Text('Unexpected error occurred')));


_75


}


_75


},


_75


child: const Text('Login'),


_75


),


_75


\],


_75


),


_75


);


_75


}


_75


}


`


Once a returning user logs in, they are taken to the verification page where they are asked to enter the TOTP from their authenticator app.


This verification page has the same text field as the enrollment page, and upon entering the code, they are taken to the home page.


Create a` lib/pages/mfa/verify_page.dart` file and add the following.


`
_88


import 'package:flutter/material.dart';


_88


import 'package:go_router/go_router.dart';


_88


import 'package:mfa_app/main.dart';


_88


import 'package:mfa_app/pages/auth/register_page.dart';


_88


import 'package:mfa_app/pages/home_page.dart';


_88


import 'package:supabase_flutter/supabase_flutter.dart';


_88


_88


class MFAVerifyPage extends StatefulWidget {


_88


static const route = '/mfa/verify';


_88


const MFAVerifyPage({super.key});


_88


_88


@override


_88


State<MFAVerifyPage> createState() => _MFAVerifyPageState();


_88


}


_88


_88


class _MFAVerifyPageState extends State<MFAVerifyPage> {


_88


@override


_88


Widget build(BuildContext context) {


_88


return Scaffold(


_88


appBar: AppBar(


_88


title: const Text('Verify MFA'),


_88


actions: \[


_88


TextButton(


_88


onPressed: () {


_88


supabase.auth.signOut();


_88


context.go(RegisterPage.route);


_88


},


_88


child: Text(


_88


'Logout',


_88


style: TextStyle(color: Theme.of(context).colorScheme.onPrimary),


_88


),


_88


),


_88


\],


_88


),


_88


body: ListView(


_88


padding: const EdgeInsets.symmetric(


_88


horizontal: 20,


_88


vertical: 24,


_88


),


_88


children: \[


_88


Text(


_88


'Verification Required',


_88


style: Theme.of(context).textTheme.titleLarge,


_88


),


_88


const SizedBox(height: 16),


_88


const Text('Enter the code shown in your authentication app.'),


_88


const SizedBox(height: 16),


_88


TextFormField(


_88


decoration: const InputDecoration(


_88


hintText: '000000',


_88


),


_88


style: const TextStyle(fontSize: 24),


_88


textAlign: TextAlign.center,


_88


keyboardType: TextInputType.number,


_88


onChanged: (value) async {


_88


if (value.length != 6) return;


_88


_88


// kick off the verification process once 6 characters are entered


_88


try {


_88


final factorsResponse = await supabase.auth.mfa.listFactors();


_88


final factor = factorsResponse.totp.first;


_88


final factorId = factor.id;


_88


_88


final challenge =


_88


await supabase.auth.mfa.challenge(factorId: factorId);


_88


await supabase.auth.mfa.verify(


_88


factorId: factorId,


_88


challengeId: challenge.id,


_88


code: value,


_88


);


_88


await supabase.auth.refreshSession();


_88


if (mounted) {


_88


context.go(HomePage.route);


_88


}


_88


} on AuthException catch (error) {


_88


ScaffoldMessenger.of(context)


_88


.showSnackBar(SnackBar(content: Text(error.message)));


_88


} catch (error) {


_88


ScaffoldMessenger.of(context).showSnackBar(


_88


const SnackBar(content: Text('Unexpected error occurred')));


_88


}


_88


},


_88


),


_88


\],


_88


),


_88


);


_88


}


_88


}


`


### Step 5: Add a home page with secure contents#


The home page is where the “secure” contents are displayed. We will create a dummy table with some dummy secure contents for demonstration purposes.


First, we create dummy content. Run the following SQL to create the table and add some content.


`
_14


-- Dummy table that contains "secure" information


_14


create table


_14


if not exists public.private_posts (


_14


id int generated by default as identity primary key,


_14


content text not null


_14


);


_14


_14


-- Dmmy "secure" data


_14


insert into


_14


public.private_posts (content)


_14


values


_14


('Flutter is awesome!'),


_14


('Supabase is awesome!'),


_14


('Postgres is awesome!');


`


Now, we can add some[row security policy](https://supabase.com/docs/guides/auth/row-level-security) to lock those data down so that only users who have signed in using MFA can view them.


Run the following SQL to secure our data from malicious users.


`
_10


-- Enable RLS for private_posts table


_10


alter table


_10


public.private_posts enable row level security;


_10


_10


-- Create a policy that only allows read if they user has signed in via MFA


_10


create policy "Users can view private_posts if they have signed in via MFA" on public.private_posts for


_10


select


_10


to authenticated using ((select auth.jwt() - >> 'aal') = 'aal2');


`


` aal` here stands for[Authenticator Assurance Level](https://pages.nist.gov/800-63-3-Implementation-Resources/63B/AAL/) , and it will be` aal1` for users who have only signed in with 1 sign-in method, and` aal2` for users who have completed the MFA flow. Checking the` aal` inside RLS policy ensures that the data cannot be viewed by users unless they complete the entire MFA flow.


The nice thing about RLS is that it gives us the flexibility to control how users can interact with the data. In this particular example, we are mandating MFA to view the data, but you could easily create layered permissions where for example a user can view the data with 1 factor, but can edit the data when signed in with MFA. You can see more examples in our official MFA guide[here](https://supabase.com/docs/guides/auth/auth-mfa) .


Now that we have the secure data in our Supabase instance, all we need to do is to display them in the HomePage. We can simply query the table and display it using a` FutureBuilder` .


Create a` lib/pages/home_page.dart` file and add the following.


`
_63


import 'package:flutter/material.dart';


_63


import 'package:go_router/go_router.dart';


_63


import 'package:mfa_app/main.dart';


_63


import 'package:mfa_app/pages/auth/register_page.dart';


_63


import 'package:mfa_app/pages/list_mfa_page.dart';


_63


_63


class HomePage extends StatelessWidget {


_63


static const route = '/';


_63


_63


const HomePage({super.key});


_63


_63


@override


_63


Widget build(BuildContext context) {


_63


final privatePostsFuture = supabase.from('private_posts').select();


_63


_63


return Scaffold(


_63


appBar: AppBar(


_63


title: const Text('Home'),


_63


actions: \[


_63


PopupMenuButton(


_63


itemBuilder: (context) {


_63


return \[


_63


PopupMenuItem(


_63


child: const Text('Unenroll MFA'),


_63


onTap: () {


_63


context.push(ListMFAPage.route);


_63


},


_63


),


_63


PopupMenuItem(


_63


child: const Text('Logout'),


_63


onTap: () {


_63


supabase.auth.signOut();


_63


context.go(RegisterPage.route);


_63


},


_63


),


_63


\];


_63


},


_63


)


_63


\],


_63


),


_63


body: FutureBuilder<List<Map<String, dynamic>>>(


_63


future: privatePostsFuture,


_63


builder: (context, snapshot) {


_63


if (snapshot.hasError) {


_63


return Center(child: Text(snapshot.error.toString()));


_63


}


_63


if (!snapshot.hasData) {


_63


return const Center(child: CircularProgressIndicator());


_63


}


_63


_63


// Display the secure private content upon retrieval


_63


final data = snapshot.data!;


_63


return ListView.builder(


_63


itemCount: data.length,


_63


itemBuilder: (context, index) {


_63


return ListTile(title: Text(data\[index\]\['content'\]));


_63


},


_63


);


_63


},


_63


),


_63


);


_63


}


_63


}


`


Because we have set the RLS policy, any user without going through the MFA flow will not see anything on this page.


One final page to add here is the **unenrollment** page. On this page, users can remove any factors that they have added. Once a user removes the factor, the user’s account will no longer be associated with the authenticator app, and they would have to go through the enrollment steps again.


Create` lib/pages/list_mfa_page.dart` file and add the following.


`
_77


import 'package:flutter/material.dart';


_77


import 'package:go_router/go_router.dart';


_77


import 'package:mfa_app/main.dart';


_77


import 'package:mfa_app/pages/auth/register_page.dart';


_77


_77


/// A page that lists the currently signed in user's MFA methods.


_77


///


_77


/// The user can unenroll the factors.


_77


class ListMFAPage extends StatelessWidget {


_77


static const route = '/list-mfa';


_77


ListMFAPage({super.key});


_77


_77


final _factorListFuture = supabase.auth.mfa.listFactors();


_77


_77


@override


_77


Widget build(BuildContext context) {


_77


return Scaffold(


_77


appBar: AppBar(title: const Text('List of MFA Factors')),


_77


body: FutureBuilder(


_77


future: _factorListFuture,


_77


builder: (context, snapshot) {


_77


if (snapshot.hasError) {


_77


return Center(child: Text(snapshot.error.toString()));


_77


}


_77


if (!snapshot.hasData) {


_77


return const Center(child: CircularProgressIndicator());


_77


}


_77


_77


final response = snapshot.data!;


_77


final factors = response.all;


_77


return ListView.builder(


_77


itemCount: factors.length,


_77


itemBuilder: (context, index) {


_77


final factor = factors\[index\];


_77


return ListTile(


_77


title: Text(factor.friendlyName ?? factor.factorType.name),


_77


subtitle: Text(factor.status.name),


_77


trailing: IconButton(


_77


onPressed: () {


_77


showDialog(


_77


context: context,


_77


builder: (context) {


_77


return AlertDialog(


_77


title: const Text(


_77


'Are you sure you want to delete this factor? You will be signed out of the app upon removing the factor.',


_77


),


_77


actions: \[


_77


TextButton(


_77


onPressed: () {


_77


context.pop();


_77


},


_77


child: const Text('cancel'),


_77


),


_77


TextButton(


_77


onPressed: () async {


_77


await supabase.auth.mfa.unenroll(factor.id);


_77


await supabase.auth.signOut();


_77


if (context.mounted) {


_77


context.go(RegisterPage.route);


_77


}


_77


},


_77


child: const Text('delete'),


_77


),


_77


\],


_77


);


_77


});


_77


},


_77


icon: const Icon(Icons.delete_outline),


_77


),


_77


);


_77


},


_77


);


_77


},


_77


),


_77


);


_77


}


_77


}


`


### Step 6: Putting the pieces together with go_router#


Now that we have all the pages, it’s time to put it all together with the help of[go_router](https://pub.dev/packages/go_router) .


go_router, as you may know, is a routing package for Flutter, and its redirect feature is particularly helpful for implementing the complex requirement this app had. Particularly, we wanted to make sure that a user who has not yet set up MFA is redirected to the MFA setup page, and only users who have signed in land on the home page.


Another helpful feature of go_router comes when using deep links, and it automatically redirects the users to the correct path of the deep link. Because of this, we can ensure that user lands on the MFA setup page upon confirming their email address.


We will add the router in our` lib/main.dart` file. Your` main.dart` file should now look like this.


`
_102


import 'package:flutter/material.dart';


_102


import 'package:go_router/go_router.dart';


_102


import 'package:mfa_app/pages/auth/login_page.dart';


_102


import 'package:mfa_app/pages/auth/register_page.dart';


_102


import 'package:mfa_app/pages/home_page.dart';


_102


import 'package:mfa_app/pages/list_mfa_page.dart';


_102


import 'package:mfa_app/pages/mfa/verify_page.dart';


_102


import 'package:supabase_flutter/supabase_flutter.dart';


_102


import 'package:mfa_app/pages/mfa/enroll_page.dart';


_102


_102


void main() async {


_102


await Supabase.initialize(


_102


url: 'YOUR_SUPABASE_URL',


_102


anonKey: 'YOUR_ANON_KEY',


_102


);


_102


runApp(const MyApp());


_102


}


_102


_102


/// Extract SupabaseClient instance in a handy variable


_102


final supabase = Supabase.instance.client;


_102


_102


final _router = GoRouter(


_102


routes: \[


_102


GoRoute(


_102


path: HomePage.route,


_102


builder: (context, state) => const HomePage(),


_102


),


_102


GoRoute(


_102


path: ListMFAPage.route,


_102


builder: (context, state) => ListMFAPage(),


_102


),


_102


GoRoute(


_102


path: LoginPage.route,


_102


builder: (context, state) => const LoginPage(),


_102


),


_102


GoRoute(


_102


path: RegisterPage.route,


_102


builder: (context, state) => const RegisterPage(),


_102


),


_102


GoRoute(


_102


path: MFAEnrollPage.route,


_102


builder: (context, state) => const MFAEnrollPage(),


_102


),


_102


GoRoute(


_102


path: MFAVerifyPage.route,


_102


builder: (context, state) => const MFAVerifyPage(),


_102


),


_102


\],


_102


redirect: (context, state) async {


_102


// Any users can visit the /auth route


_102


if (state.location.contains('/auth') == true) {


_102


return null;


_102


}


_102


_102


final session = supabase.auth.currentSession;


_102


// A user without a session should be redirected to the register page


_102


if (session == null) {


_102


return RegisterPage.route;


_102


}


_102


_102


final assuranceLevelData =


_102


supabase.auth.mfa.getAuthenticatorAssuranceLevel();


_102


_102


// The user has not setup MFA yet, so send them to enroll MFA page.


_102


if (assuranceLevelData.currentLevel == AuthenticatorAssuranceLevels.aal1) {


_102


await supabase.auth.refreshSession();


_102


final nextLevel =


_102


supabase.auth.mfa.getAuthenticatorAssuranceLevel().nextLevel;


_102


if (nextLevel == AuthenticatorAssuranceLevels.aal2) {


_102


// The user has already setup MFA, but haven't login via MFA


_102


// Redirect them to the verify page


_102


return MFAVerifyPage.route;


_102


} else {


_102


// The user has not yet setup MFA


_102


// Redirect them to the enrollment page


_102


return MFAEnrollPage.route;


_102


}


_102


}


_102


_102


// The user has signed invia MFA, and is allowed to view any page.


_102


return null;


_102


},


_102


);


_102


_102


class MyApp extends StatelessWidget {


_102


const MyApp({super.key});


_102


_102


// This widget is the root of your application.


_102


@override


_102


Widget build(BuildContext context) {


_102


return MaterialApp.router(


_102


title: 'MFA App',


_102


debugShowCheckedModeBanner: false,


_102


theme: ThemeData.light().copyWith(


_102


inputDecorationTheme: const InputDecorationTheme(


_102


border: OutlineInputBorder(),


_102


),


_102


),


_102


routerConfig: _router,


_102


);


_102


}


_102


}


`


## Conclusions and future iterations#


We looked at how to incorporate Multi-Factor Authentication into a Flutter app with a complete enrollment and verification flow for new and existing users. We saw how we are able to control how the users can interact with the data using their MFA status.


Another common use case is to make MFA optional and allow the user to opt-in whenever they are ready. Optionally enrolling in MFA will require some tweaks in the code, but might be a fun one to try out.


## More Flutter content#


- [Flutter Tutorial: building a Flutter chat app](https://supabase.com/blog/flutter-tutorial-building-a-chat-app)
- [Flutter Authentication and Authorization with RLS](https://supabase.com/blog/flutter-authentication-and-authorization-with-rls)
- [How to build a real-time multiplayer game with Flutter Flame](https://supabase.com/blog/flutter-real-time-multiplayer-game)
- [Write backend code using Dart Edge](https://supabase.com/docs/guides/functions/dart-edge)
