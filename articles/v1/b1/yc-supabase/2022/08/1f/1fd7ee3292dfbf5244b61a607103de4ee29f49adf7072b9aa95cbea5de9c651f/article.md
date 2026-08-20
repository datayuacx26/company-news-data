---
schema_version: "1.0.0"
document_id: "1fd7ee3292dfbf5244b61a607103de4ee29f49adf7072b9aa95cbea5de9c651f"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-flutter-sdk-1-developer-preview"
published_at: "2022-08-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:25791a265e8a7973422ac760f6fc17778d7dc7130d4b5f651d0b38c88dfe7641"
---

# Supabase Flutter SDK 1.0 Developer Preview

Today, we are releasing of Developer Preview version of v1.0 of[Supabase Flutter SDK](https://pub.dev/packages/supabase_flutter/versions/1.0.0-dev.1) . Flutter has quickly become one of the most popular frameworks for developers to build cross-platform mobile apps. We can attest to that growth, our Flutter SDK is one of the most popular libraries and each day we see more Flutter devs choosing Supabase.


For this release, our main focus is developer experiences. We would love for you to try the SDK and provide your feedback so that we can continue to improve!


Before we dive into the actual updates, I would like to thank all the community contributors who have helped the library to be where it is today.


## Better developer experience#


Until now, there were some disputable implementations in the Flutter SDK. We've made several improvements:


### Automatically handling auth state persistence#


Previously,` supabase-flutter` required a class that extends` SupabaseAuthState` or` SupabaseAuthRequiredState` to persist auth state. With` supabase-flutter` 1.0, you no longer need to include these classes.


All you need to persist the auth state is initialize Supabase and everything else will be automatically taken care of.` SupabaseAuthState` and` SupabaseAuthRequiredState` have been removed from the code base.


`
_16


// Before


_16


await Supabase.initialize(


_16


url: 'SUPABASE_URL',


_16


anonKey: 'SUPABASE_ANON_KEY',


_16


);


_16


...


_16


_16


class AuthState<T extends StatefulWidget> extends SupabaseAuthState<T> {


_16


...


_16


}


_16


_16


// After


_16


await Supabase.initialize(


_16


url: 'SUPABASE_URL',


_16


anonKey: 'SUPABASE_ANON_KEY',


_16


);


`


### Automatically handling deep links#


Deep link handling had similar issues previously, requiring you to implement` SupabaseAuthState` or` SupabaseAuthRequiredState` classes.


With the 1.0 update, you no longer need to use these classes, and deep links will be automatically handled. You can listen to` onAuthStateChange` to handle when a deep link is received to redirect users to a new screen.


`
_10


// Before


_10


void onReceivedAuthDeeplink(Uri uri) {


_10


Supabase.instance.log('onReceivedAuthDeeplink uri: $uri');


_10


}


_10


_10


// After


_10


await Supabase.instance.initialize(


_10


url: 'SUPABASE_URL',


_10


anonKey: 'SUPABASE_ANON_KEY',


_10


);


`


### Throwing errors instead of returning them#


When` supabase-dart` and` supabase-flutter` were created, we wanted to mirror the JavaScript library as much as possible. We soon realized that some syntax does not fit well when written in Dart. Throwing vs returning error is a good example of that. Since Dart does not have object destruction, the code becomes a bit tedious when errors are returned.


With` supabase-flutter` 1.0, we are throwing errors instead of returning them. This is consistent across all features from` auth` ,` postgrest` , and` storage` .


`
_11


// Before


_11


final response = await Supabase.instance.from('messages').select().execute();


_11


final data = response.data;


_11


final error = response.error;


_11


_11


// After


_11


try {


_11


final data = await Supabase.instance.from('messages').select();


_11


} catch(error) {


_11


// Handle error here


_11


}


`


### No more` .execute()` to get the data#


We want this SDK to be as close as possible to the JavaScript SDK to provide consistent developer experience no matter what programming language you are using. Prior to the 1.0 update, whenever you called the` postgrest` endpoints, you had to call` .execute()` at the end of each query.


` .execute()` is now deprecated. You no longer needed it to query data from your Supabase database. This update, along with many many other improvements across the whole library, has been done by[Bruno D'Luka](https://github.com/bdlukaa) , and I would love to give him a special shout out here!


`
_10


// Before


_10


final response = await Supabase.instance.from('messages').select().execute();


_10


final data = response.data;


_10


_10


// After


_10


final data = await Supabase.instance.from('messages').select();


`


## Desktop support for deeplinks#


Ever since` supabase-flutter` was born, it supported only iOS, Android and Web for deep linking. This was a limitation of the deep link library that we were using.


With the 1.0 launch, we are moving to use[app_links](https://pub.dev/packages/app_links) , which will enable us to support MacOS and Windows applications as well! Linux support is being worked on - follow the repo to keep updated.


## Multiplayer support#


[Multiplayer](https://supabase.com/blog/supabase-realtime-with-multiplayer-features) is the next generation Supabase Realtime engine that was announced at the previous launch week.


We want our Flutter developers to experience this new multiplayer feature as well, so are working hard at bringing it to our Flutter SDK. It is not yet included in the developer preview of Supabase Flutter 1.0, but will be part of it when stable launch has been released.


## Supabase Auth UI for Flutter#


Last but not least, we are bringing you another library, the Supabase Auth UI for Supabase! When released, this library will enable you to implement a basic authentication screen without building it yourself. You can just load the library and display a nice looking Auth UI. The library takes your theme settings automatically to match the look and feel of your application.


You can get started with it on[pub.dev](https://pub.dev/packages/supabase_auth_ui) .


I would like to thank[Fatuma](https://twitter.com/XquisiteDreamer) for single-handedly working on bringing us an easier authentication experience.


`
_17


// Email and password signin form


_17


SupaEmailAuth(


_17


authAction: AuthAction.signIn,


_17


redirectUrl: '/home',


_17


),


_17


_17


// Magic Link signin form


_17


SupaMagicAuth(),


_17


_17


// Social Login Buttons


_17


SupaSocialsAuth(


_17


socialProviders: \[


_17


SocialProviders.apple,


_17


SocialProviders.google,


_17


\],


_17


colored: true,


_17


),


`


## Final thoughts#


These updates are just the tip of the iceberg for 1.0. There are been many bug fixes and features constantly being added to the Supabase Flutter SDK. This could not have been possible without the help from the open source community. Here, I would also like to give a shout out to two other developers who have been a major part of the journey of this SDK:[Vinzent](https://twitter.com/Vinzent03_) and[Daniel Mossaband](https://github.com/DanMossa) . They have been a huge part of the Supabase Flutter SDK - not just for the 1.0 release, but throughout the lifetime of the library. For those of you who want to try out the new SDK, you can get the developer preview version from[supabase-flutter](https://pub.dev/packages/supabase_flutter/versions/1.0.0-dev.1) pub.dev page or can simply copy and paste the following into your pubspec.yaml file.


`
_10


supabase_flutter: ^1.0.0-dev.1


`


If you have any feedbacks, please let us know in the issues of the[supabase-flutter](https://github.com/supabase-community/supabase-flutter/issues) repository.


## Flutter Resources#


- [supabase-flutter 1.0 developer preview](https://pub.dev/packages/supabase_flutter)
- [Flutter Tutorial: building a Flutter chat app](https://supabase.com/blog/flutter-tutorial-building-a-chat-app)
- [Flutter Tutorial - Part 2: Authentication and Authorization with RLS](https://supabase.com/blog/flutter-authentication-and-authorization-with-rls)
- [How to build a real-time multiplayer game with Flutter Flame](https://supabase.com/blog/flutter-real-time-multiplayer-game)
- [Build a Flutter app with Very Good CLI and Supabase](https://verygood.ventures/blog/flutter-app-very-good-cli-supabase)
