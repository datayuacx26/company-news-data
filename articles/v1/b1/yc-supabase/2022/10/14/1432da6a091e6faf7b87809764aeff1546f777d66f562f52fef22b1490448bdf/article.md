---
schema_version: "1.0.0"
document_id: "1432da6a091e6faf7b87809764aeff1546f777d66f562f52fef22b1490448bdf"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-flutter-sdk-v1-released"
published_at: "2022-10-21T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:fadd638ebfa02416b7255384fa607b3a930aebff03b4e2d40dbb503596bb17bd"
---

# supabase-flutter v1 Released

A few months ago, we announced a[developer preview version of supabase-flutter SDK](https://supabase.com/blog/supabase-flutter-sdk-1-developer-preview) . Since then, we have heard a lot of amazing feedback from the community, and have been improving it. Today, we are happy to announce the stable v1 of[supabase-flutter](https://pub.dev/packages/supabase_flutter) . You can also find the updated[quick start guide](https://supabase.com/docs/guides/with-flutter) ,[documentation](https://supabase.com/docs/reference/dart) and a[migration guide from v0](https://supabase.com/docs/reference/dart/v0/upgrade-guide) .


## What is new in v1?#


supabase-flutter v1 focuses on improved developer experience. The new version requires far less boiler plate code as well as it provides more intuitive APIs. Here are some highlights of the update.


### No more` .execute()`#


Previously, for` .select()` ,` .insert()` ,` .update()` ,` .delete()` and` .stream()` required` execute()` to be called at the end. On top of that, errors are thrown, not returned, so you can be sure that you have the query results in the returned value.


`
_10


// Before


_10


final response = await supabase.from('messages').select().execute();


_10


final data = response.data;


_10


_10


// After


_10


final data = await supabase.from('messages').select();


`


### More predictable auth methods#


Names of the auth methods are more descriptive about what they do. Here are some examples of the new methods:


`
_10


await supabase.auth.signInWithPassword(email: email, password: password);


_10


_10


await supabase.auth.signInWithOAuth(Provider.github)


`


Also,` onAuthStateChange` returns stream, which feels more natural for anyone coding in Dart.


`
_10


supabase.auth.onAuthStateChange.listen((data) {


_10


final AuthChangeEvent event = data.event;


_10


final Session? session = data.session;


_10


});


`


### Realtime Multiplayer edition support#


During the last launch week, we announced the[general availability of Realtime Multiplayer](https://supabase.com/blog/supabase-realtime-multiplayer-general-availability) . supabase-flutter now has first class support for the two newly introduced realtime methods, broadcast and presence. Broadcast can be used to share realtime data to all connected clients with low latency. Presence is a way to let other connected clients know the status of the client. You can visit[multiplayer.dev](http://multiplayer.dev/) to see a quick demo of the feature.


`
_31


final channel = Supabase.instance.client.channel('my_channel');


_31


_31


// listen to \`location\` broadcast events


_31


channel.on(


_31


RealtimeListenTypes.broadcast,


_31


ChannelFilter(


_31


event: 'location',


_31


), (payload, \[ref\]) {


_31


// Do something exciting with the broadcast event


_31


});


_31


_31


// send \`location\` broadcast events


_31


channel.send(


_31


type: RealtimeListenTypes.broadcast,


_31


event: 'location',


_31


payload: {'lat': 1.3521, 'lng': 103.8198},


_31


);


_31


_31


// listen to presence states


_31


channel.on(RealtimeListenTypes.presence, ChannelFilter(event: 'sync'),


_31


(payload, \[ref\]) {


_31


// Do something exciting with the presence state


_31


});


_31


_31


// subscribe to the above changes


_31


channel.subscribe((status) async {


_31


if (status == 'SUBSCRIBED') {


_31


// if subscribed successfully, send presence event


_31


final status = await channel.track({'user_id': myUserId});


_31


}


_31


});


`


These are just tip of the iceberg of all the updates that we shipped in v1. Check out the[documentation](https://supabase.com/docs/reference/dart/) to see the full list.


## Acknowledgements#


It required massive support from the community to bring the supabase-flutter to where it is today. I would like to thank everyone who has contributed to the library, and a special thanks to[Bruno](https://github.com/bdlukaa) and[Vinzent](https://github.com/Vinzent03) , who have been key for this release. We really could not have done it without you!


## Resources#


- [Install supabase-flutter v1.0](https://pub.dev/packages/supabase_flutter)
- [supabase-flutter documentation](https://supabase.com/docs/reference/dart/)
- [v0 to v1 migration guide](https://supabase.com/docs/reference/dart/v0/upgrade-guide)
- [Flutter Tutorial: building a Flutter chat app](https://supabase.com/blog/flutter-tutorial-building-a-chat-app)
- [Flutter Tutorial - Part 2: Authentication and Authorization with RLS](https://supabase.com/blog/flutter-authentication-and-authorization-with-rls)
- [How to build a real-time multiplayer game with Flutter Flame](https://supabase.com/blog/flutter-real-time-multiplayer-game)
- [Build a Flutter app with Very Good CLI and Supabase](https://verygood.ventures/blog/flutter-app-very-good-cli-supabase)
