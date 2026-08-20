---
schema_version: "1.0.0"
document_id: "487d9dba25123eea07bcc006ad5e387279ad6e346f3ccf2ebe12bca4d1318371"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/event-triggers-wo-superuser"
published_at: "2025-05-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:6380c225898134c6c79b5a3e740c68245e67d796dd80f4af5026772d930e3718"
---

# PostgreSQL Event Triggers without superuser access

Event Triggers in Postgres are powerful, but only a superuser can create them. In cloud environments, granting superuser access isn't an option.


But thanks to Postgres' extensibility, we can allow regular users to create Event Triggers, in a safe way.


In this blog post, we’ll explain how we do this in the` supautils` extension, using a combination of the Utility Hook and the Function Manager Hook.


## Privileged Role#


The core of` supautils` is the “privileged role”, which is a role that serves as proxy to superuser. It provides a safe subset of superuser capabilities and it’s accessible to regular users.


When the privileged role does a` create event trigger` , we intercept the statement with a Utility Hook (` ProcessUtility_hook` ). Here we elevate the role to a superuser, continuing the usual flow and allowing the creation on Postgres core. As a last step, we downgrade to the privileged role and make it the event trigger owner1 .


Creating an event trigger like this is not safe though, as it would allow privilege escalation.


## The privilege escalation problem#


Here, a problem arises. Once an Event Trigger is created:


- It targets every role.
- It runs using the target role privileges2 .


This means that a malicious user can create an Event Trigger like:


`
_11


create or replace function become_super()


_11


returns event_trigger


_11


language plpgsql as


_11


$$


_11


begin


_11


alter role malicious SUPERUSER;


_11


end;


_11


$$;


_11


_11


create event trigger bad_event_trigger on ddl_command_end


_11


execute procedure become_super();


`


And once a superuser trips on the event trigger, it will fire with its privileges. Making the malicious user a superuser.


## Skipping Event Triggers#


A solution would be skipping user Event Triggers for superusers.


The Function Manager hook (` fmgr_hook` ) allows us to intercept and modify functions’ execution.


We can intercept the Event Trigger function and replace it with a “noop”. Postgres doesn’t provide a noop function, but we can use the existing` version()` function for the same purpose.


Besides superusers, we also want to skip user event triggers for “reserved roles”3 . These are used for managed services (like` pgbouncer` ).


## User Event Triggers#


This now allows users to safely create Event Triggers, without superuser access:


`
_26


-- use the privileged role, which is configured to be "postgres"


_26


set role postgres;


_26


select current_setting('is_superuser'); -- prove it's not a superuser


_26


current_setting


_26


-----------------


_26


off


_26


(1 row)


_26


_26


-- now create the event trigger


_26


create function show_current_user()


_26


returns event_trigger as $$


_26


begin


_26


raise notice 'the event trigger is executed for %', current_user;


_26


end;


_26


$$ language plpgsql;


_26


_26


create event trigger myevtrig on ddl_command_end


_26


execute procedure show_current_user();


_26


_26


-- check it succeeds


_26


create table foo();


_26


NOTICE: the event trigger is executed for postgres


_26


_26


set role myrole;


_26


create table bar();


_26


NOTICE: the event trigger is executed for myrole


`


## Future in Postgres core#


We would also like to allow regular user event triggers in Postgres core. To this end, we’ve submitted[some patches](https://www.postgresql.org/message-id/flat/CAGRrpzbtYDkg7_xwfzrqByYgCJQbbL38tADyuN%2B6tAkbA-Pnkg%40mail.gmail.com) which are already generating fruitful discussion.


Note that user Event Triggers in Postgres core will likely be more restricted than the` supautils` version.


## Try it out#


User Event Triggers are now available for new projects on the Supabase platform.


You can also git clone the[supautils repo](https://github.com/supabase/supautils/) and` make install` it in your own deployment.


Finally, we want to give a special shout out to the[Zero Sync](https://zero.rocicorp.dev/) team, who pushed us to release this feature.


---


## Footnotes#


1.


This is so the event trigger can be altered or dropped by end users.↩


2.


This is not true if you mark the event trigger function as` security definer` , then it will run with the privileges of the function owner. But this is not a usual practice on event triggers, as they usually want to preserve the context of the current user.↩


3.


These are configurable. You can read more about reserved roles[here](https://supabase.com/blog/roles-postgres-hooks) .↩
