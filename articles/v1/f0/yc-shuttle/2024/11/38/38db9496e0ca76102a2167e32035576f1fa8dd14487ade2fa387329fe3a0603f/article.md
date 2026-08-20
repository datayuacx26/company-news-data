---
schema_version: "1.0.0"
document_id: "38db9496e0ca76102a2167e32035576f1fa8dd14487ade2fa387329fe3a0603f"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2024/11/20/migrating-to-shuttle"
published_at: "2024-11-20T16:30:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:ee0824545d191857cc22c4a6d212747bb9bde95a01e28f869d7145b69f4eeb30"
---

# Migrating to Shuttle

Hey! Today we're going to have a quick look at migrating a project that uses the Tokio runtime with Axum, to Shuttle.


## Setup #


### Pre-requisites #


Before we start, make sure you have the latest version of[cargo-shuttle](https://docs.shuttle.dev/getting-started/installation) installed.


For this example, we'll assume you are migrating an Axum project that has a database.


## Migrating your project #


### Add your dependencies #


The first step is to add` shuttle-runtime` and` shuttle-axum` to your dependencies to be able to use Shuttle's runtime with the Axum framework.


```text
cargo    add   shuttle-runtime shuttle-axum


```


Any[secrets](https://docs.shuttle.dev/resources/shuttle-secrets) you need to use will be kept in a` Secrets.toml` file (dev secrets in` Secrets.dev.toml` ) which will be placed at the` Cargo.toml` level.


You can also easily get a provisioned database like so (this example will be for a provisioned PostgreSQL instance specifically):


```text
cargo    add   shuttle-shared-db  --features   postgres


```


If you have any database records you'd like to keep, it would be a good idea to export them so that they can be[migrated to the new database.](https://docs.shuttle.dev/guides/migrate-shared-postgres) **You will not need a secrets file if you only need a provisioned Postgres database - this will be automatically be provisioned and given to you in the form of a connection string or an sqlx pool.**


### Migrating your code #


To be able to run your project on Shuttle, you need to make a few changes to your code. Instead of the` tokio::main` macro, you will use the` shuttle_runtime::main` macro and swap out` dotenvy` for Shuttle's Postgres annotation:


This is what your` main.rs` file might look like before:


```text
#[tokio::main]
async    fn    main  (  )    {
dotenvy ::   dotenv  (  )  .  ok  (  )  ;


let   url  =    dotenvy ::   var  (  "DATABASE_URL"  )  .  expect  (  "No database URL was set!"  )  ;


let   pool  =    sqlx ::   Pool  ::  connect  (  &  url )  .  await  .  unwrap  (  )  ;


sqlx ::   migrate!  (  )
.  run  (  &  pool )
.  await
.  expect  (  "Migrations failed :("  )  ;


let   router  =    create_api_router  (  pool )  ;
let   addr  =    SocketAddr  ::  from  (  (  [  0  ,    0  ,    0  ,    0  ]  ,    8000  )  )  ;


Server  ::  bind  (  &  addr )
.  serve  (  router .  into_make_service  (  )  )
.  await
.  unwrap  (  )
}


```


And this is what it looks like after:


```text
#[shuttle_runtime::main]
pub    async    fn    axum    (
#[shuttle_shared_db::Postgres]   pool :    PgPool  ,
#[shuttle_runtime::Secrets]   secrets :    shuttle_runtime ::   SecretStore  ,
)    ->    shuttle_axum ::   ShuttleAxum    {
sqlx ::   migrate!  (  )
.  run  (  &  pool )
.  await
.  expect  (  "Migrations failed :("  )  ;


// Use secrets for anything that needs them


let   router  =    create_api_router  (  pool )  ;


Ok  (  router .  into  (  )  )
}


```


If you need more than a simple router, you'll want to create a custom struct that holds all of your required app state information inside and then create an` impl` for the struct - you can find more about that[here.](https://docs.shuttle.dev/tutorials/custom-service) Anything outside of your entry point function (the function that uses the` shuttle_runtime::main` macro) doesn't need to be changed. If you are using secrets as well as a database connection, you may wish to create a struct that holds both of these values and then pass it into the function that generates the router. Interested? We also have some[additional docs on how to do this](https://www.shuttle.dev/blog/[year]/[month]/[day]/[slug])


### Deploying #


To ensure that you get a unique project name, create a` Shuttle.toml` file at the` Cargo.toml` level to name your project to whatever you like.


```text
name    =    "my-unique-app-name-here"


```


Now all you need to do is to run the following commands:


```text
shuttle project start
shuttle project deploy


```


Your project should now be deployed!


## Finishing up #


Thanks for reading! Hopefully this article has helped you learn what it takes to migrate to Shuttle.


Further reading:


- [How to migrate to Shuttle](https://docs.shuttle.dev/migrations/introduction)
- [Get Started with Axum](https://www.shuttle.dev/blog/2023/12/06/using-axum-rust)
- [Building with AWS S3 using Rust](https://www.shuttle.dev/blog/2024/04/17/using-aws-s3-rust)
