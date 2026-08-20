---
schema_version: "1.0.0"
document_id: "a16bbabd9ea57b6e144090f2839d371fa8598dae52e0024aa04e8c04c329fad4"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/offline-first-flutter-apps"
published_at: "2024-10-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:3df36a57ea4be8c5fa5e88937a3c8ae64a5c6a285f229593ceee4caa4adada73"
---

# Building offline-first mobile apps with Supabase, Flutter and Brick

Don’t have time for reading? Skip to[the example](https://github.com/GetDutchie/brick/tree/main/example_supabase) .


Brick is an[all-in-one](https://www.youtube.com/watch?v=2noLcro9iIw) data manager for Flutter that handles querying and uploading between Supabase and local caches like SQLite. Using Brick, developers can focus on implementing the application without[worrying about translating or storing their data](https://www.youtube.com/watch?v=jm5i7e_BQq0) .


Most significantly, Brick focuses on offline-first data parity: an app should function the same with or without connectivity.


### Why Offline?#


The worst version of your app is always the unusable one. People use their phones on subways, airplanes, and on sub-3G connections. Building for offline-first provides the best user experience when you can’t guarantee steady bandwidth.


Even if you’re online-only, Brick’s round trip time is drastically shorter because all data[from Supabase is stored in a local cache](https://getdutchie.github.io/brick/#/offline_first/offline_first_with_supabase_repository) . When you query the same data again, your app retrieves the local copy, reducing the time and expense of a round trip. And, if SQLite isn’t performant enough, Brick also offers a third cache in memory. When requests are made while the app is offline, they’ll be[continually retried until the app comes online](https://getdutchie.github.io/brick/#/offline_first/offline_queue) , ensuring that your local state syncs up to your remote state.


Of course, you can[always opt-out of the cache](https://getdutchie.github.io/brick/#/offline_first/policies) on a request-by-request basis for sensitive or must-be-fresh data.


### Getting Started#


Create a Flutter app:


`
_10


flutter create my_app


`


Add the Brick dependencies to your` pubspec.yaml` :


`
_10


dependencies:


_10


brick_offline_first_with_supabase: ^1.0.0


_10


sqflite: ^2.3.0


_10


brick_sqlite: ^3.1.0


_10


uuid: ^3.0.4


_10


_10


dev_dependencies:


_10


brick_offline_first_with_supabase_build: ^1.0.0


_10


build_runner: ^2.4.0


`


Set up directories for Brick’s generated code:


`
_10


mkdir -p lib/brick/adapters lib/brick/db;


`


Brick synthesizes your remote data to your local data through code generation. From a Supabase table, create Dart fields that match the table’s columns:


`
_26


// Your model definition can live anywhere in lib/**/* as long as it has the .model.dart suffix


_26


// Assume this file is saved at my_app/lib/src/users/user.model.dart


_26


_26


import 'package:brick_offline_first_with_supabase/brick_offline_first_with_supabase.dart';


_26


import 'package:brick_sqlite/brick_sqlite.dart';


_26


import 'package:brick_supabase/brick_supabase.dart';


_26


import 'package:uuid/uuid.dart';


_26


_26


@ConnectOfflineFirstWithSupabase(


_26


supabaseConfig: SupabaseSerializable(tableName: 'users'),


_26


)


_26


class User extends OfflineFirstWithSupabaseModel {


_26


final String name;


_26


_26


// Be sure to specify an index that **is not** auto incremented in your table.


_26


// An offline-first strategy requires distributed clients to create


_26


// indexes without fear of collision.


_26


@Supabase(unique: true)


_26


@Sqlite(index: true, unique: true)


_26


final String id;


_26


_26


User({


_26


String? id,


_26


required this.name,


_26


}) : this.id = id ?? const Uuid().v4();


_26


}


`


When some (or all) of your models have been defined, generate the code:


`
_10


dart run build_runner build


`


This will generate adapters to serialize/deserialize to and from Supabase. Migrations for SQLite are also generated for any new, dropped, or changed columns. Check these migrations after they are generated - Brick is smart, but not as smart as you.


After every model change, run this command to ensure your adapters will serialize/deserialize the way they need to.


### The Repository#


Your application does not need to touch SQLite or Supabase directly. By[interacting with this single entrypoint](https://getdutchie.github.io/brick/#/data) , Brick makes the hard choices under the hood about where to fetch and when to cache while the application code remains consistent in online or offline modes.


Finally, run your app:


`
_54


// Saved in my_app/lib/src/brick/repository.dart


_54


_54


import 'package:brick_sqlite/brick_sqlite.dart';


_54


// This hide is for Brick's @Supabase annotation; in most cases,


_54


// supabase_flutter **will not** be imported in application code.


_54


import 'package:brick_supabase/brick_supabase.dart' hide Supabase;


_54


import 'package:sqflite_common/sqlite_api.dart';


_54


import 'package:supabase_flutter/supabase_flutter.dart';


_54


_54


import 'brick.g.dart';


_54


_54


class Repository extends OfflineFirstWithSupabaseRepository {


_54


static late Repository? _instance;


_54


_54


Repository._({


_54


required super.supabaseProvider,


_54


required super.sqliteProvider,


_54


required super.migrations,


_54


required super.offlineRequestQueue,


_54


super.memoryCacheProvider,


_54


});


_54


_54


factory Repository() => _instance!;


_54


_54


static Future<void> configure(DatabaseFactory databaseFactory) async {


_54


final (client, queue) = OfflineFirstWithSupabaseRepository.clientQueue(


_54


databaseFactory: databaseFactory,


_54


);


_54


_54


await Supabase.initialize(


_54


url: supabaseUrl,


_54


anonKey: supabaseAnonKey,


_54


httpClient: client,


_54


);


_54


_54


final provider = SupabaseProvider(


_54


Supabase.instance.client,


_54


modelDictionary: supabaseModelDictionary,


_54


);


_54


_54


_instance = Repository._(


_54


supabaseProvider: provider,


_54


sqliteProvider: SqliteProvider(


_54


'my_repository.sqlite',


_54


databaseFactory: databaseFactory,


_54


modelDictionary: sqliteModelDictionary,


_54


),


_54


migrations: migrations,


_54


offlineRequestQueue: queue,


_54


// Specify class types that should be cached in memory


_54


memoryCacheProvider: MemoryCacheProvider(),


_54


);


_54


}


_54


}


`


`
_11


import 'package:my_app/brick/repository.dart';


_11


import 'package:sqflite/sqflite.dart' show databaseFactory;


_11


_11


Future<void> main() async {


_11


await Repository.configure(databaseFactory);


_11


// .initialize() does not need to be invoked within main()


_11


// It can be invoked from within a state manager or within


_11


// an initState()


_11


await Repository().initialize();


_11


runApp(MyApp());


_11


}


`


Which` databaseFactory` is used depends on your platform. For unit testing, use` import 'package:sqflite_common_ffi/sqflite_ffi.dart' show databaseFactory` . Please see SQFlite’s docs for specific installation and usage instructions on[web](https://github.com/tekartik/sqflite/tree/master/packages_web/sqflite_common_ffi_web#use-the-proper-factory) ,[Linux](https://github.com/tekartik/sqflite/tree/master/sqflite_common_ffi#linux) , or[Windows](https://github.com/tekartik/sqflite/tree/master/sqflite_common_ffi#windows) .


## Usage#


The fun part.[Brick’s DSL queries](https://getdutchie.github.io/brick/#/supabase/query) are written once and transformed for local and remote integration. For example, to retrieve all users with the name “Thomas”:


`
_10


await Repository().get<User>(query: Query.where('name', 'Thomas'));


`


Or query by association:


`
_10


// Assuming we had a model \`Order\` with a \`user\` association


_10


await Repository().get<Order>(query: Query.where('user', Where.exact('name', 'Thomas'));


`


Queries can be[much more advanced](https://getdutchie.github.io/brick/#/data/query) , leveraging` contains` ,` not` ,` like` operators as well as sub clauses. Please note that, as of writing, not[all Supabase operators are supported](https://getdutchie.github.io/brick/#/supabase/query?id=where) .


**Reactivity**


Beyond async requests, you can subscribe to a stream of updated local data from anywhere in your app (for example, if you pull-to-refresh a list of users, all listeners will be notified of the new data):


`
_10


final Stream<List<User>> usersStream = Repository().subscribe<User>(query: Query.where('name', 'Thomas'));


`


This **does not** leverage Supabase’s channels by default; if Supabase updates, your app will not be notified. This opt-in feature is[currently under active development](https://github.com/GetDutchie/brick/issues/454) .


**Upserting**


After a model has been created, it can uploaded to Supabase without serializing it to JSON first:


`
_10


await Repository().upsert<User>(User(name: 'Thomas'));


`


All attached associations[will be upserted too](https://getdutchie.github.io/brick/#/supabase/models?id=upsert-behavior) .


## Other Tips#


### Foreign Keys/Associations#


Easily connect related models/tables:


`
_25


_25


import 'package:brick_sqlite/brick_sqlite.dart';


_25


import 'package:brick_supabase/brick_supabase.dart';


_25


import 'package:my_app/lib/src/users/user.model.dart';


_25


import 'package:uuid/uuid.dart';


_25


_25


@ConnectOfflineFirstWithSupabase(


_25


supabaseConfig: SupabaseSerializable(tableName: 'orders'),


_25


)


_25


class Order extends OfflineFirstWithSupabaseModel {


_25


// Like Supabase's client, specifying a foreign_key


_25


// is possible but only necessary if there are joins


_25


// with multiple foreign keys


_25


// @Supabase(foreignKey: 'user_id')


_25


final User user;


_25


_25


@Supabase(unique: true)


_25


@Sqlite(index: true, unique: true)


_25


final String id;


_25


_25


Order({


_25


String? id,


_25


required this.user,


_25


}) : this.id = id ?? const Uuid().v4();


_25


}


`


Brick allows very granular[model configuration](https://getdutchie.github.io/brick/#/supabase/models) - you can specify specific tables,[individual columns](https://getdutchie.github.io/brick/#/supabase/fields) , and more.


### Testing#


Quickly mock your Supabase endpoints to add uncluttered[unit testing](https://getdutchie.github.io/brick/#/supabase/testing?id=testing) :


`
_33


import 'package:brick_supabase/testing.dart';


_33


import 'package:test/test.dart'


_33


_33


void main() {


_33


// Pass an instance of your model dictionary to the mock server.


_33


// This permits quick generation of fields and generated responses


_33


final mock = SupabaseMockServer(modelDictionary: supabaseModelDictionary);


_33


_33


group('MyClass', () {


_33


setUp(mock.setUp);


_33


_33


tearDown(mock.tearDown);


_33


_33


test('#myMethod', () async {


_33


// If your request won't exactly match the columns of MyModel, provide


_33


// the query list to the \`fields:\` parameter


_33


final req = SupabaseRequest<MyModel>();


_33


final resp = SupabaseResponse(\[


_33


// mock.serialize converts models to expected Supabase payloads


_33


// but you don't need to use it - any jsonEncode-able object


_33


// can be passed to SupabaseRepsonse


_33


await mock.serialize(MyModel(name: 'Demo 1', id: '1')),


_33


await mock.serialize(MyModel(name: 'Demo 2', id: '2')),


_33


\]);


_33


// This method stubs the server based on the described requests


_33


// and their matched responses


_33


mock.handle({req: resp});


_33


final provider = SupabaseProvider(mock.client, modelDictionary: supabaseModelDictionary);


_33


final retrieved = await provider.get<MyModel>();


_33


expect(retrieved, hasLength(2));


_33


});


_33


});


_33


}


`


## Further Reading#


Brick manages a lot. It can be overwhelming at times. But it’s been used in production across thousands of devices for more than five years, so it’s got a sturdy CV. There’s likely an existing solution to a seemingly novel problem. Please[reach out to the community or package maintainers](https://github.com/GetDutchie/brick/issues) with any questions.


- Example:[Brick with Supabase](https://github.com/GetDutchie/brick/tree/main/example_supabase)
- Video:[Brick Architecture](https://www.youtube.com/watch?v=2noLcro9iIw) . An explanation of Brick parlance with[a supplemental analogy](https://medium.com/flutter-community/brick-your-app-five-compelling-reasons-and-a-pizza-analogy-to-make-your-data-accessible-8d802e1e526e) .
- Video:[Brick Basics](https://www.youtube.com/watch?v=jm5i7e_BQq0) . An overview of essential Brick mechanics.
- [Build a User Management App with Flutter](https://supabase.com/docs/guides/getting-started/tutorials/with-flutter)
