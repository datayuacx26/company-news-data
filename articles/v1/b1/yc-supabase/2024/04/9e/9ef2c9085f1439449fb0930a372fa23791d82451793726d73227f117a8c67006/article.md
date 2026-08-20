---
schema_version: "1.0.0"
document_id: "9ef2c9085f1439449fb0930a372fa23791d82451793726d73227f117a8c67006"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/anonymous-sign-ins"
published_at: "2024-04-17T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:f0015af256d35eea80571813ac15ca5d629fd70766f733cd807d8bfb50ef9ebf"
---

# Supabase Auth now supports Anonymous Sign-ins

Supabase Auth now supports[anonymous sign-ins](https://supabase.com/docs/guides/auth/auth-anonymous) , one of our[most-requested features](https://github.com/supabase/auth/issues/68) by the community.


Anonymous sign-ins can be used to create **temporary users** who haven’t signed up for your application yet. This lowers the friction for new users to try out your product since they don’t have to provide any signup credentials.


## Enabling Anonymous sign-ins#


You can[enable anonymous sign-ins](https://supabase.com/dashboard/project/_/auth/providers) for your project today from the dashboard:


For local development, upgrade your Supabase CLI and add the config to the` config.toml` file:


`
_10


\[auth\]


_10


enable_anonymous_sign_ins = true


`


You can create an anonymous user through the[Javascript](https://supabase.com/docs/reference/javascript/auth-signinanonymously) ,[Flutter](https://supabase.com/docs/reference/dart/auth-signinanonymously) or[Swift](https://supabase.com/docs/reference/swift/auth-signinanonymously) SDKs today. Here’s how you can create an anonymous user using` supabase-js` .


`
_10


const { data, error } = await supabase


_10


.auth


_10


.signInAnonymously()


`


## Terminology#


Profiles created with anonymous sign-ins are also` authenticated` !


Once you call` .signInAnonymously()` you have moved the user into an authentication flow, and we treat them like a signed in user:


## Restricting access for anonymous users#


Like a permanent user, anonymous users are persisted in the` auth.users` table:


id role email is_anonymous


e053e470-afa1-4625-8963-37adb862fd11 authenticated NULL true


5563108e-ac81-4063-9288-4f3db068efa1 authenticatedluke@starwars.com false


An anonymous user can be identified by the` is_anonymous` claim returned in the user’s JWT, which is accessible from your Row Level Security policies (RLS). This is helpful if you want to limit access to certain features in your application.


For example, let’s say that we have an online forum where users can create and read posts.


Given this table to store the posts:


`
_10


create table public.posts (


_10


id serial primary key,


_10


name text not null,


_10


description text


_10


);


`


If we only want to allow permanent users to create posts, we can check if the user is anonymous by inspecting the JWT` select auth.jwt() ->> 'is_anonymous'` .


Using this function in an RLS policy:


`
_10


create policy "Only permanent users can create posts"


_10


on public.posts


_10


for insert


_10


to authenticated -- Note: user is still authenticated


_10


with check (


_10


(select auth.jwt() ->> 'is_anonymous')::boolean is false


_10


);


`


RLS gives us full flexibility to create a variety of rules.


For example, to allow read access for permanent users for all posts and limit anonymous users to posts created today:


`
_12


create policy "Limit access to anonymous users"


_12


on public.posts


_12


for select


_12


to authenticated -- Note: user is still authenticated


_12


using (


_12


case


_12


when (select (auth.jwt() ->> 'is_anonymous'))::boolean is true


_12


then (created_at >= current_date)


_12


else


_12


true


_12


end


_12


);


`


## Convert an anonymous user to a permanent user#


At some point, an anonymous user may decide they want to create a post. This is where we prompt them to sign up for an account which converts them to a permanent user.


An anonymous user is considered a permanent user when they have an identity associated to them.


After they have been converted, the user id remains the same, which means that any data associated with the user’s id would be carried over.


Supabase Auth provides 2 ways to achieve this:


1. Link an email or phone identity
2. Link an OAuth identity


### Link an email or phone identity#


To link an email or phone identity:


`
_10


const { data, error } = await supabase


_10


.auth


_10


.updateUser({ email })


`


### Link an OAuth identity#


To link an OAuth identity to an anonymous user, you need to[enable manual linking](https://supabase.com/dashboard/project/_/auth/providers) for your project. Learn about how[identity linking](https://supabase.com/docs/guides/auth/auth-identity-linking) works with Supabase Auth.


Once enabled, you can call the` linkIdentity()` method:


`
_10


const { data, error } = await supabase


_10


.auth


_10


.linkIdentity({ provider: 'google' })


`


## Impersonating an anonymous user#


When creating RLS policies to differentiate access for an anonymous user, you can leverage the[user impersonation feature](https://supabase.com/blog/studio-introducing-assistant) in the SQL editor to test out your policies:


The[user management screen](https://supabase.com/dashboard/project/_/auth/users) provides an option to filter by anonymous users, which can help to know how many anonymous users have been created.


## What’s next#


Managing anonymous users can be tricky, especially when you have a lot of visitors to your site. We’re working on an “automatic clean-up” option to delete anonymous users that have been inactive for more than 30 days. In the meantime, since anonymous users are stored in the auth schema in your database, you can clean up orphaned anonymous users by running the following query:


`
_10


-- deletes anonymous users created more than 30 days ago


_10


delete from auth.users


_10


where is_anonymous is true and created_at < now() - interval '30 days';


`


We are also working on a[linter](https://github.com/supabase/splinter/pull/28) to check your RLS policies and highlight those that allow anonymous users access - stay tuned for updates later this month!


## Getting started#


- Docs:[Anonymous sign-ins](https://supabase.com/docs/guides/auth/auth-anonymous)
- API method references:[Javascript](https://supabase.com/docs/reference/javascript/auth-signinanonymously) ,[Flutter](https://supabase.com/docs/reference/dart/auth-signinanonymously) ,[Swift](https://supabase.com/docs/reference/swift/auth-signinanonymously)
