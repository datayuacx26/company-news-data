---
schema_version: "1.0.0"
document_id: "4048f088a7fec875780e000fdb486671d41c3cd5ecbf274419aa55585fa11326"
company_key: "yc-better-auth"
company: "Better Auth"
source_id: "yc-better-auth-rss-ab6aa45cffa8"
canonical_url: "https://better-auth.com/blog/0-supabase-auth-to-planetscale-migration"
published_at: "2025-08-25T00:00:00+00:00"
first_seen_at: "2026-07-24T18:52:17.785032+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:bfdebf635cda9dcb2eae052b6b3be48a63574ada4c518155c63bb3a720776f0f"
---

# Migrate from Supabase Auth to Better Auth + PlanetScale PostgreSQL

# Supabase Auth to Better Auth + PlanetScale PostgreSQL Migration Guide


Recently,[PlanetScale announced](https://planetscale.com/blog/planetscale-for-postgres) support for PostgreSQL. This is exciting news for developers and a big step forward for the database industry.


We’ve noticed that some users are migrating from Supabase to PlanetScale PostgreSQL, but facing challenges because they also rely on Supabase Auth. This guide will help you migrate your authentication from Supabase Auth to Better Auth on PlanetScale PostgreSQL.


## 1. Setup a PlanetScale Database


Open the PlanetScale[dashboard](https://app.planetscale.com/)


Create a[new database](https://app.planetscale.com/new?org=better-auth)


Get your connection string (PostgreSQL URI)


```text
postgresql  :  //<username>:<password>@<host>/postgres?sslmode=verify-full
```


Save the database URL in your` .env` file for later use with Better Auth:


.env


```text
DATABASE_URL =
postgresql://<username>:<password>@<host>/postgres?sslmode=verify-full
```


This is what will be in the` database` field of our auth config


## 2. Install Better Auth


Install Better Auth


```text
npm   install   better-auth
```


Follow and complete the[basic setup](https://www.better-auth.com/docs/installation)


Make sure to set up all required environment variables as per the docs.


## 3. Install PostgreSQL Client


Install the` pg` package and its types:


```text
npm   install   pg
npm   install   --save-dev   @types/pg
```


## 4. Generate & Migrate Better Auth Schema


Run this cli command to generate all the schema needed to setup Better Auth:


```text
npx   auth   generate
```


Then run this command to apply the generated schema to your PlanetScale database:


```text
npx   auth   migrate
```


You should now have the required auth tables in PlanetScale.


### 5. Quick Check


Your auth config should be like this:


```text
import   { Pool }   from   "pg"  ;
import   { betterAuth }   from   "better-auth"  ;


export   const   auth   =   betterAuth  ({
baseURL:   "http://localhost:3000"  ,
database:   new   Pool  ({
connectionString: process.env.  DATABASE_URL  ,
}),
emailAndPassword: {
enabled:   true  ,
},
});
```


### 6. The Fun Part


Now comes the fun part. You are now all setup to move your auth from Supabase Auth to Better Auth and all you have to do is go through the instances you've used Supabase Auth client and replace it with Better Auth client. We are going to see a few examples here.


```text
// Supabase Auth
await   supabase.auth.  signUp  ({
email,
password,
});


// Better Auth
await   authClient.signUp.  email  ({
email,
password,
name:   "John"  ,
});
```


### 7. Migrate your users from Supabase Auth


This migration will invalidate all active sessions. While this guide doesn't currently cover migrating two-factor (2FA) or Row Level Security (RLS) configurations, both should be possible with additional steps.


For a more detailed guide checkout[this guide](https://www.better-auth.com/docs/guides/supabase-migration-guide) we made.


Essentially you should be able to copy the following code into` migration.ts` and run it.


migration.ts


```text
import   { Pool }   from   "pg"  ;
import   { auth }   from   "./lib/auth"  ;
import   { User   as   SupabaseUser }   from   "@supabase/supabase-js"  ;


type   User   =   SupabaseUser   &   {
is_super_admin  :   boolean  ;
raw_user_meta_data  :   {
avatar_url  :   string  ;
};
encrypted_password  :   string  ;
email_confirmed_at  :   string  ;
created_at  :   string  ;
updated_at  :   string  ;
is_anonymous  :   boolean  ;
identities  :   {
provider  :   string  ;
identity_data  :   {
sub  :   string  ;
email  :   string  ;
};
created_at  :   string  ;
updated_at  :   string  ;
};
};


const   migrateFromSupabase   =   async   ()   =>   {
const   ctx   =   await   auth.$context;
const   db   =   ctx.options.database   as   Pool  ;
const   users   =   await   db
.  query  (
`
SELECT
u.*,
COALESCE(
json_agg(
i.* ORDER BY i.id
) FILTER (WHERE i.id IS NOT NULL),
'[]'::json
) as identities
FROM auth.users u
LEFT JOIN auth.identities i ON u.id = i.user_id
GROUP BY u.id
`
)
.  then  ((  res  )   =>   res.rows   as   User  []);
for   (  const   user   of   users) {
if   (  !  user.email) {
continue  ;
}
await   ctx.adapter
.  create  ({
model:   "user"  ,
data: {
id: user.id,
email: user.email,
name: user.email,
role: user.is_super_admin   ?   "admin"   :   user.role,
emailVerified:   !!  user.email_confirmed_at,
image: user.raw_user_meta_data.avatar_url,
createdAt:   new   Date  (user.created_at),
updatedAt:   new   Date  (user.updated_at),
isAnonymous: user.is_anonymous,
},
})
.  catch  (()   =>   {});
for   (  const   identity   of   user.identities) {
const   existingAccounts   =   await   ctx.internalAdapter.  findAccounts  (user.id);


if   (identity.provider   ===   "email"  ) {
const   hasCredential   =   existingAccounts.  find  (
(  account  :   {   providerId  :   string   })   =>
account.providerId   ===   "credential"
);
if   (  !  hasCredential) {
await   ctx.adapter
.  create  ({
model:   "account"  ,
data: {
userId: user.id,
providerId:   "credential"  ,
accountId: user.id,
password: user.encrypted_password,
createdAt:   new   Date  (user.created_at),
updatedAt:   new   Date  (user.updated_at),
},
})
.  catch  (()   =>   {});
}
}
const   supportedProviders   =   Object.  keys  (ctx.options.socialProviders   ||   {});
if   (supportedProviders.  includes  (identity.provider)) {
const   hasAccount   =   existingAccounts.  find  (
(  account  :   {   providerId  :   string   })   =>
account.providerId   ===   identity.provider
);
if   (  !  hasAccount) {
await   ctx.adapter.  create  ({
model:   "account"  ,
data: {
userId: user.id,
providerId: identity.provider,
accountId: identity.identity_data?.sub,
createdAt:   new   Date  (identity.created_at   ??   user.created_at),
updatedAt:   new   Date  (identity.updated_at   ??   user.updated_at),
},
});
}
}
}
}
};
migrateFromSupabase  ();
```


Run the migration script


Terminal


```text
bun   migration.ts   # or use node, ts-node, etc.
```


### 8. Migrate the Rest of Your Data


If you have additional user-related data in Supabase, you can use the[Supabase to PlanetScale migration tool](https://planetscale.com/docs/postgres/imports/supabase) .


### 9. Clean up all the Supabase Auth code from your codebase


You now own your auth, you should start removing all the Supabase Auth related code.


### 10. Done! 🎉


You've successfully migrated from Supabase Auth to Better Auth on PlanetScale.


### Tips


- Double-check that all environment variables are set in production.
- Test all auth flows (sign-up, login, password reset, session refresh) before going live.
- Remember that this is just the basics and if you've integrated Supabase Auth's auth functions in a lot of placed you'd have to find the suitable[Better Auth replacements](https://www.better-auth.com/docs) .
- Have fun!


### Learn More!


[Better Auth Setup Get started with installing Better Auth](https://www.better-auth.com/docs/introduction)[PlanetScale Quick Start Get started on PlanetScale here](https://planetscale.com/docs/vitess/tutorials/planetscale-quick-start-guide)[PlanetScale Migration Guides Use this guide to move your data from Supabase and many more services](https://planetscale.com/docs/postgres/imports/postgres-imports)[Supabase Auth Migration Move your auth from Supabase Auth to your own DB](https://www.better-auth.com/docs/guides/supabase-migration-guide)
