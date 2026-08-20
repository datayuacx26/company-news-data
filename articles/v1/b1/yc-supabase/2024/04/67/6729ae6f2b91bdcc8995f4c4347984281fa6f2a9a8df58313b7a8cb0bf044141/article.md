---
schema_version: "1.0.0"
document_id: "6729ae6f2b91bdcc8995f4c4347984281fa6f2a9a8df58313b7a8cb0bf044141"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-roles-and-privileges"
published_at: "2024-04-11T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:f03262d834251dface79eeebb81352692b60c7173975add82c1dbfc5f508bb2c"
---

# Postgres Roles and Privileges

Controlling access to data in Postgres is paramount for data security. Postgres provides a robust and flexible permissions model for users to manage access to their data. The permissions model is based on the familiar object, privilege, role model but has subtleties which must be understood by a database administrator to create airtight access. In this post we will take a detailed look at how roles and permissions work in Postgres.


# Basic Concepts#


Let's first understand some basic concepts which will be used throughout the rest of the post.


## Database Object#


A database object is any entity created in the database. Tables, foreign tables, views, materialized views, types, domains, operators, functions, triggers etc. are database objects. Objects allow operations on them which vary for each object. For example, you can *select* data from a table and you can *execute* a function.


## Privilege#


A privilege controls what operation is allowed to be run on a database object. For example, the` select` privilege on a table controls the ability to read data from the table. Similarly, the` execute` privilege controls the ability to execute a function. Privileges are assigned to roles. A role must have the permission for the operation it is performing on an object.


## Role#


A role is a user or a group. A user is someone who can login to the database. A group is a collection of users to make it easier to manage privileges for users. Unlike a user, a group can't login to the database. The distinction between a user and a group doesn’t matter to Postgres for the most part as they are both roles, but it is still useful to think of them as separate concepts for ease of understanding.


## Owner#


Every database object has an owner. The owner has complete control over the object. They can modify or delete the object or grant privileges to other users and groups. When a user creates a new object, they become the owner of the object. An owner can also transfer the ownership of objects to other roles. A role cannot be deleted before all its owned objects’ ownership is transferred to another role.


With these basic terms defined, let's take a look at the permissions model in Postgres in depth. The rest of the post will be more like a tutorial, so you can follow along. I'll be using a hosted Supabase project, but you are free to use any Postgres installation.


# Setting Up#


Create a new Supabase project (or use an existing one) and copy its connection string URI from the[Database Settings page](https://supabase.com/dashboard/project/_/database/settings) . The URI looks like the following:


`
_10


postgres://\[USER\].\[YOUR-PROJECT-REF\]:\[YOUR-PASSWORD\]@\[REGION-SUBDOMAIN\].pooler.supabase.com:5432/postgres


`


Where` USER` is the user to connect as.` YOUR-PROJECT-REF` is a string uniquely identifying your project.` YOUR-PASSWORD` is the database password for the` USER` user and` REGION-SUBDOMAIN` is the subdomain where your database is hosted.


Use the[psql command line tool](https://www.postgresql.org/download/) to connect to the database:


`
_10


➜ psql postgres://postgres.\[YOUR-PROJECT-REF\]:\[YOUR-PASSWORD\]@\[REGION-SUBDOMAIN\].pooler.supabase.com:5432/postgres


`


Once connected, confirm that you are connected as the` postgres` user by running` select current_role` command:


`
_10


# as postgres


_10


postgres=> select current_role;


_10


┌──────────────┐


_10


│ current_role │


_10


├──────────────┤


_10


│ portgres │


_10


└──────────────┘


_10


(1 row)


`


## Creating Roles#


Now, let's create two users named` junior_dev` and` senior_dev` . A database role can be created with the` create role` command. Since a user is a role that can login, use the` login` parameter:


`
_10


# as postgres


_10


postgres=> create role junior_dev login password 'a long and secure password';


_10


CREATE ROLE


_10


postgres=> create role senior_dev login password 'another long and secure password';


_10


CREATE ROLE


`


You can now confirm that the` junior_dev` and` senior_dev` users can login to the database:


`
_21


➜ psql postgres://junior_dev.\[YOUR-PROJECT-REF\]:\[YOUR-PASSWORD\]@\[REGION-SUBDOMAIN\].pooler.supabase.com:5432/postgres


_21


_21


postgres=> select current_role;


_21


┌──────────────┐


_21


│ current_role │


_21


├──────────────┤


_21


│ junior_dev │


_21


└──────────────┘


_21


(1 row)


_21


_21


postgres=> exit


_21


_21


➜ psql postgres://senior_dev.\[YOUR-PROJECT-REF\]:\[YOUR-PASSWORD\]@\[REGION-SUBDOMAIN\].pooler.supabase.com:5432/postgres


_21


_21


postgres=> select current_role;


_21


┌──────────────┐


_21


│ current_role │


_21


├──────────────┤


_21


│ senior_dev │


_21


└──────────────┘


_21


(1 row)


`


For the rest of the post, open three terminals and login each with` junior_dev` ,` senior_dev` and` postgres` to easily switch between them. Each executed command will list at the beginning the user it should be executed as, for example:


`
_10


# as junior_dev


_10


postgres=> this command should be executed from the junior_dev's terminal


`


## Creating Objects and Assigning Privileges#


Let's now try to create a table from as` junior_dev` :


`
_10


# as junior_dev


_10


postgres=> create table public.apps(id serial primary key, name text);


_10


ERROR: permission denied for schema public


_10


LINE 1: create table public.apps(id serial primary key, name text);


_10


^


`


What happened? The error` permission denied for schema public` tells us that` junior_dev` doesn't have some permission on the` public` schema. We can check existing permissions on a schema using the` dn+ <schema>` command in` psql` :


`
_14


# as junior_dev


_14


postgres=> \\dn+ public


_14


List of schemas


_14


┌────────┬───────────────────┬────────────────────────────────────────┬────────────────────────┐


_14


│ Name │ Owner │ Access privileges │ Description │


_14


├────────┼───────────────────┼────────────────────────────────────────┼────────────────────────┤


_14


│ public │ pg_database_owner │ pg_database_owner=UC/pg_database_owner↵│ standard public schema │


_14


│ │ │ =U/pg_database_owner ↵│ │


_14


│ │ │ postgres=U/pg_database_owner ↵│ │


_14


│ │ │ anon=U/pg_database_owner ↵│ │


_14


│ │ │ authenticated=U/pg_database_owner ↵│ │


_14


│ │ │ service_role=U/pg_database_owner │ │


_14


└────────┴───────────────────┴────────────────────────────────────────┴────────────────────────┘


_14


(1 row)


`


Indeed, the` Access privileges` column doesn’t list` junior_dev` role anywhere, which means it doesn’t have any permission on the` public` schema. How do we fix this? The` postgres` user in Supabase hosted databases is a powerful role with more privileges than many other roles. Think of the` postgres` role as an admin role, although it is not a superuser. We can use this role to grant appropriate permissions.


So, let’s switch to the` postgres` user connection and grant` junior_dev` the permission to create objects in the` public` schema. The general format of the` grant` command is` grant <privilege> on <object> to <role>` . You can consult the[privileges page in Postgres documentation](https://www.postgresql.org/docs/current/ddl-priv.html) to find out the correct privilege name.


`
_10


# as postgres


_10


postgres=> grant create on schema public to junior_dev;


_10


GRANT


`


Let’s check the permissions again:


`
_15


# as junior_dev


_15


postgres=> \\dn+ public


_15


List of schemas


_15


_15


│ Name │ Owner │ Access privileges │ Description │


_15


_15


_15


│ │ │ =U/pg_database_owner ↵│ │


_15


│ │ │ postgres=U/pg_database_owner ↵│ │


_15


│ │ │ anon=U/pg_database_owner ↵│ │


_15


│ │ │ authenticated=U/pg_database_owner ↵│ │


_15


│ │ │ service_role=U/pg_database_owner ↵│ │


_15


│ │ │ junior_dev=C/pg_database_owner │ │


_15


_15


(1 row)


`


This time we see a new line in the access privileges column:


`
_10


# as junior_dev


_10


postgres=> create table public.apps(id serial primary key, name text);


_10


CREATE TABLE


`


Let’s insert some data in it:


`
_10


# as junior_dev


_10


postgres=> insert into public.apps(name) values ('next app');


_10


INSERT 0 1


_10


postgres=> select * from public.apps;


_10


┌────┬──────────┐


_10


│ id │ name │


_10


├────┼──────────┤


_10


│ 1 │ next app │


_10


└────┴──────────┘


_10


(1 row)


`


Now switch to` senior_dev` and try to select data from the table:


`
_10


# as senior_dev


_10


postgres=> select * from public.apps;


_10


ERROR: permission denied for table apps


`


` senior_dev` can’t select data from the` public.apps` table. Let’s debug the permissions error as before. The command in` psql` to view table permissions is` \\dp <tablename>` :


`
_10


# as senior_dev


_10


postgres=> \\dp public.apps


_10


Access privileges


_10


┌────────┬──────┬───────┬───────────────────┬───────────────────┬──────────┐


_10


│ Schema │ Name │ Type │ Access privileges │ Column privileges │ Policies │


_10


├────────┼──────┼───────┼───────────────────┼───────────────────┼──────────┤


_10


│ public │ apps │ table │ │ │ │


_10


└────────┴──────┴───────┴───────────────────┴───────────────────┴──────────┘


_10


(1 row)


`


No access privileges are present at all. As we did before, let’s now switch to the` postgres` user and fix the permissions. The[privileges page](https://www.postgresql.org/docs/current/ddl-priv.html) tells us that we need to grant the` select` privilege to` senior_dev` for them to select data from the` public.apps` table:


`
_10


# as postgres


_10


postgres=> grant select on table public.apps to senior_dev;


_10


ERROR: permission denied for table apps


`


Why can’t` postgres` grant the` select` privilege? Because it is neither an owner, nor has it any access privileges on the table. But then how was` junior_dev` able to select data from the table? That is because` junior_dev` is the owner of the table:


`
_10


# as postgres


_10


postgres=> \\dt public.apps


_10


List of relations


_10


┌────────┬──────┬───────┬────────────┐


_10


│ Schema │ Name │ Type │ Owner │


_10


├────────┼──────┼───────┼────────────┤


_10


│ public │ apps │ table │ junior_dev │


_10


└────────┴──────┴───────┴────────────┘


_10


(1 row)


`


Since an owner has all the privileges on an object,` junior_dev` can select the data.` junior_dev` can also grant privileges on the owned objects to other roles. Let’s fix the permissions with` junior_dev` :


`
_10


# as junior_dev


_10


postgres=> grant select on public.apps to senior_dev;


_10


GRANT


`


Now` senior_dev` can select the data:


`
_10


# as senior_dev


_10


postgres=> select * from public.apps;


_10


┌────┬──────────┐


_10


│ id │ name │


_10


├────┼──────────┤


_10


│ 1 │ next app │


_10


└────┴──────────┘


_10


(1 row)


`


Another option in the above example would have been for` junior_dev` to grant the *privilege to grant the` select` privilege* to the` postgres` role. The` postgres` role would then have been able to grant the` select` privilege to` senior_dev` . To try this, let’s revoke the previously granted privilege to` senior_dev` first:


`
_10


# as junior_dev


_10


postgres=> revoke select on public.apps from senior_dev;


_10


REVOKE


`


And then grant the` select` privilege` with grant option` to` postgres` :


`
_10


# as junior_dev


_10


postgres=> grant select on public.apps to postgres with grant option;


_10


GRANT


`


Now, if we view the permissions on the` public.apps` table:


`
_10


# as_junior_dev


_10


postgres=> \\dp public.apps


_10


Access privileges


_10


┌────────┬──────┬───────┬───────────────────────────────┬───────────────────┬──────────┐


_10


_10


├────────┼──────┼───────┼───────────────────────────────┼───────────────────┼──────────┤


_10


│ public │ apps │ table │ junior_dev=arwdDxt/junior_dev↵│ │ │


_10


│ │ │ │ postgres=r*/junior_dev │ │ │


_10


└────────┴──────┴───────┴───────────────────────────────┴───────────────────┴──────────┘


_10


(1 row)


`


Notice the` *` after the` r` in` postgres=r*/junior_dev` . which indicates that the` select` permission was granted` with grant option` . Now` postgres` can grant the` select` privilege to` senior_dev` :


`
_10


# as postgres


_10


postgres=> grant select on table public.apps to senior_dev;


_10


GRANT


`


And` senior_dev` has the` select` privilege and can select from the table again:


`
_19


# as senior_dev


_19


postgres=> \\dp public.apps


_19


Access privileges


_19


_19


_19


_19


│ public │ apps │ table │ junior_dev=arwdDxt/junior_dev↵│ │ │


_19


│ │ │ │ postgres=r*/junior_dev ↵│ │ │


_19


│ │ │ │ senior_dev=r/postgres │ │ │


_19


_19


(1 row)


_19


_19


postgres=> select * from public.apps;


_19


┌────┬──────────┐


_19


│ id │ name │


_19


├────┼──────────┤


_19


│ 1 │ next app │


_19


└────┴──────────┘


_19


(1 row)


`


A` grant` command only adds privileges for existing objects. What if we want to grant certain privileges to objects as soon as they are created? That’s where default access privileges come in.


### Default Access Privileges#


If` junior_dev` now creates another table, it has to grant the privileges again to` senior_dev` . To avoid doing this each time` junior_dev` creates a new table, we can alter` junior_dev` 's default access privileges. First let’s see the current default privileges on the` public` schema:


`
_32


# as junior_dev


_32


postgres=> \\ddp public


_32


Default access privileges


_32


┌────────────────┬────────┬──────────┬──────────────────────────────────────┐


_32


│ Owner │ Schema │ Type │ Access privileges │


_32


├────────────────┼────────┼──────────┼──────────────────────────────────────┤


_32


│ postgres │ public │ function │ postgres=X/postgres ↵│


_32


│ │ │ │ anon=X/postgres ↵│


_32


│ │ │ │ authenticated=X/postgres ↵│


_32


│ │ │ │ service_role=X/postgres │


_32


│ postgres │ public │ sequence │ postgres=rwU/postgres ↵│


_32


│ │ │ │ anon=rwU/postgres ↵│


_32


│ │ │ │ authenticated=rwU/postgres ↵│


_32


│ │ │ │ service_role=rwU/postgres │


_32


│ postgres │ public │ table │ postgres=arwdDxt/postgres ↵│


_32


│ │ │ │ anon=arwdDxt/postgres ↵│


_32


│ │ │ │ authenticated=arwdDxt/postgres ↵│


_32


│ │ │ │ service_role=arwdDxt/postgres │


_32


│ supabase_admin │ public │ function │ postgres=X/supabase_admin ↵│


_32


│ │ │ │ anon=X/supabase_admin ↵│


_32


│ │ │ │ authenticated=X/supabase_admin ↵│


_32


│ │ │ │ service_role=X/supabase_admin │


_32


│ supabase_admin │ public │ sequence │ postgres=rwU/supabase_admin ↵│


_32


│ │ │ │ anon=rwU/supabase_admin ↵│


_32


│ │ │ │ authenticated=rwU/supabase_admin ↵│


_32


│ │ │ │ service_role=rwU/supabase_admin │


_32


│ supabase_admin │ public │ table │ postgres=arwdDxt/supabase_admin ↵│


_32


│ │ │ │ anon=arwdDxt/supabase_admin ↵│


_32


│ │ │ │ authenticated=arwdDxt/supabase_admin↵│


_32


│ │ │ │ service_role=arwdDxt/supabase_admin │


_32


└────────────────┴────────┴──────────┴──────────────────────────────────────┘


_32


(6 rows)


`


Neither` junior_dev` nor` senior_dev` are listed. Let’s alter` junior_dev` 's default privileges:


`
_10


# as junior_dev


_10


postgres=> alter default privileges in schema public grant select on tables to senior_dev;


_10


ALTER DEFAULT PRIVILEGES


`


Here we are altering default privileges such that whenever` junior_dev` creates a new table in the` public` schema,` senior_dev` should be granted` select` privilege on it. Let’s check the privileges again:


`
_33


# as junior_dev


_33


postgres=> \\ddp public


_33


Default access privileges


_33


_33


│ Owner │ Schema │ Type │ Access privileges │


_33


_33


│ junior_dev │ public │ table │ senior_dev=r/junior_dev │


_33


│ postgres │ public │ function │ postgres=X/postgres ↵│


_33


│ │ │ │ anon=X/postgres ↵│


_33


│ │ │ │ authenticated=X/postgres ↵│


_33


│ │ │ │ service_role=X/postgres │


_33


│ postgres │ public │ sequence │ postgres=rwU/postgres ↵│


_33


│ │ │ │ anon=rwU/postgres ↵│


_33


│ │ │ │ authenticated=rwU/postgres ↵│


_33


│ │ │ │ service_role=rwU/postgres │


_33


│ postgres │ public │ table │ postgres=arwdDxt/postgres ↵│


_33


│ │ │ │ anon=arwdDxt/postgres ↵│


_33


│ │ │ │ authenticated=arwdDxt/postgres ↵│


_33


│ │ │ │ service_role=arwdDxt/postgres │


_33


│ supabase_admin │ public │ function │ postgres=X/supabase_admin ↵│


_33


│ │ │ │ anon=X/supabase_admin ↵│


_33


│ │ │ │ authenticated=X/supabase_admin ↵│


_33


│ │ │ │ service_role=X/supabase_admin │


_33


_33


│ │ │ │ anon=rwU/supabase_admin ↵│


_33


│ │ │ │ authenticated=rwU/supabase_admin ↵│


_33


│ │ │ │ service_role=rwU/supabase_admin │


_33


_33


│ │ │ │ anon=arwdDxt/supabase_admin ↵│


_33


│ │ │ │ authenticated=arwdDxt/supabase_admin↵│


_33


│ │ │ │ service_role=arwdDxt/supabase_admin │


_33


_33


(7 rows)


`


The first line now indicates the default access privilege we just added. Let’s now create a new table and insert a row in it:


`
_10


# as junior_dev


_10


postgres=> create table public.users(id serial primary key, name text);


_10


CREATE TABLE


_10


postgres=> insert into public.users(name) values ('john doe');


_10


INSERT 0 1


`


Now try to select data in` public.users` from` senior_dev` :


`
_10


# as senior_dev


_10


postgres=> select * from public.users;


_10


┌────┬──────────┐


_10


│ id │ name │


_10


├────┼──────────┤


_10


│ 1 │ john doe │


_10


└────┴──────────┘


_10


(1 row)


`


Note that we were immediately able to select data from` public.users` without explicit grants from` junior_dev` .


It is clear from above that the owner has all the privileges on an object which they can grant to other roles. But it can become cumbersome for the owner to keep granting the same privileges to every new role. There is a better way. We can ensure that objects are owned by a group and then any users which need access to those objects are assigned membership to the group. Let’s see how this works.


## Creating Groups#


We want to create a new` developers` group which will own the` public.apps` table. Then we will make` junior_dev` and` senior_dev` members of the` developers` group. This will ensure that they both have the same kind of access, without explicitly granting privileges after creating a new object.


First, let’s drop the` public.apps` table:


`
_10


# as junior_dev


_10


postgres=> drop table public.apps;


_10


DROP TABLE


`


Let’s also revoke the` create` privilege from` junior_dev` on the` public` schema:


`
_10


# as postgres


_10


postgres=> revoke create on schema public from junior_dev;


_10


REVOKE


`


Let’s create a` developers` group. Since a group is a role that is not allowed to login, use the` nologin` parameter:


`
_10


# as postgres


_10


postgres=> create role developers nologin;


_10


CREATE ROLE


`


You can't login with the` developers` role because we set the` nologin` parameter. The` login` /` nologin` parameters control the` login` attribute of a role. Earlier we also set the` password` attribute of the` junior_dev` and` senior_dev` roles. There are many other[role attributes](https://www.postgresql.org/docs/current/role-attributes.html) which we will talk about later in the post.


Let’s give the` create` privilege to the` developers` group:


`
_10


# as postgres


_10


postgres=> grant create on schema public to developers;


_10


GRANT


`


Since` junior_dev` and` senior_dev` users do not have` create` privilege on the` public` schema, they can’t create objects in it. The` developers` group can, but we can’t login with it. So how do we create` public.apps` owned by` developers` ? Well, a user can temporarily impersonate a group if they are a member of the group. So let’s ensure` junior_dev` and` senior_dev` are members of the` developers` group:


`
_10


# as postgres


_10


postgres=> grant developers to junior_dev;


_10


GRANT ROLE


_10


postgres=> grant developers to senior_dev;


_10


GRANT ROLE


`


The` grant <group> to <user>` is another variant of the` grant` command but should be mentally read as` add <user> to <group>` .


Now` junior_dev` (or` senior_dev` ) can impersonate` developers` :


`
_10


# as junior_dev


_10


postgres=> set role developers;


_10


SET


_10


postgres=> select current_role;


_10


┌──────────────┐


_10


│ current_role │


_10


├──────────────┤


_10


│ developers │


_10


└──────────────┘


_10


(1 row)


`


And create the` public.apps` table:


`
_10


# as junior_dev


_10


postgres=> create table public.apps(id serial primary key, name text);


_10


CREATE TABLE


`


Which is owned by the` developers` group:


`
_10


# as junior_dev


_10


postgres=> \\dt public.apps


_10


List of relations


_10


_10


│ Schema │ Name │ Type │ Owner │


_10


_10


│ public │ apps │ table │ developers │


_10


_10


(1 row)


`


Now if you stop impersonation:


`
_10


# as junior_dev


_10


postgres=> reset role;


_10


RESET


_10


postgres=> select current_role;


_10


┌──────────────┐


_10


│ current_role │


_10


├──────────────┤


_10


│ junior_dev │


_10


└──────────────┘


_10


(1 row)


`


And try to insert or select data from` public.apps` it works:


`
_10


# as junior_dev or senior_dev


_10


postgres=> insert into public.apps(name) values ('next app');


_10


INSERT 0 1


_10


postgres=> select * from public.apps;


_10


┌────┬──────────┐


_10


│ id │ name │


_10


├────┼──────────┤


_10


│ 1 │ next app │


_10


└────┴──────────┘


_10


(1 row)


`


The reason` junior_dev` and` senior_dev` are able to insert and select data is because they are part of the` developers` group. If a new developer is created later, they are just a` grant developers to <new dev>` away from having the same access as every other developer. Contrast this with the previous method in which the new user would have to ask the owner of every object to grant them permissions.


## Grant Options#


Making a user part of another group might grant it three abilities:


1. The ability to impersonate the group.
2. The ability to inherit the permissions from the group.
3. The ability to add or remove other users from the group.


All of these abilities can be controlled independently while running the` grant <group> to <user>` command by using the` with <option name> true/false` suffixed to it. The names of each of the above options are` set` ,` inherit` , and` admin` . For example, to disallow a user from impersonating a group run` grant <group> to <user> with set false` .


To demonstrate, if we enable admin option on` junior_dev` :


`
_10


# as postgres


_10


postgres=> grant developers to junior_dev with admin option;


_10


GRANT ROLE


`


It will be able to remove` senior_dev` from the` developers` group:


`
_10


# as junior_dev


_10


postgres=> revoke developers from senior_dev;


_10


REVOKE ROLE


`


Without the` admin` option,` junior_dev` wouldn’t have been able to do this.


## Role Attributes#


Every role has some attributes associated with it which control the behavior of the role. Some of the common ones are listed below. For the full list and their details, refer to the[Postgres role attributes documentation](https://www.postgresql.org/docs/current/role-attributes.html) .


- ` login` - controls the role’s ability to login.
- ` superuser` - controls whether the role is a superuser or not. See next section for details.
- ` createdb` - controls whether the role will be able to create databases.
- ` createrole` - controls whether the role will be able to create other roles.
- ` replication` - controls whether the role can be used to initiate replication.
- ` bypassrls` - controls whether the role can bypass row level security.
- ` connection limit` - limits the maximum number of connections that the role can make to the database.
- ` inherit` - controls whether the role can inherit permissions from roles it is a member of.


## Special Roles#


There are two special roles which play an important part in how roles and privileges are managed.


### Superuser#


A` superuser` is a role with the` superuser` attribute set. A` superuser` is like a root user on the *nix OSes. It is very powerful and bypasses all privilege checks except authentication during login. For this reason, you should avoid working with this role as much as possible. Only superusers can create other` superuser` roles.


### Public#


` public` is a group role which every other role is automatically a part of. There is only one` public` role. So unlike` superuser` , there’s no` public` role attribute. The` public` role is used to provide privileges which are considered to be so common that every role should have them. These privileges are:


- ` connect` - ability to connect to the database.
- ` temporary` - ability to create temporary tables.
- ` execute` - ability to execute functions.
- ` usage` - ability to use an object like a domain, language or type.


The` public` role can’t be deleted, but its privileges can be revoked.


Privileges of a role are union of three sets of privileges:


1. Those granted to the role directly.
2. Those inherited from the roles this role is an explicit member of.
3. Those inherited from the` public` role, which every role is implicitly a member of.


Privileges inherited from the` public` role are a common source of confusion when working with roles in Postgres. Imagine that we want to disallow` junior_dev` from executing functions. Let’s first create a function:


`
_10


# as postgres


_10


postgres=> create function add(integer, integer)


_10


returns integer


_10


as 'select $1 + $2;'


_10


language sql;


_10


CREATE FUNCTION


`


` junior_dev` is currently able to execute this function:


`
_10


# as junior_dev


_10


postgres=> select add(1, 2);


_10


┌─────┐


_10


│ add │


_10


├─────┤


_10


│ 3 │


_10


└─────┘


_10


(1 row)


`


Now let’s revoke` junior_dev` 's` execute` permission:


`
_10


# as postgres


_10


postgres=> revoke execute on function add(integer, integer) from junior_dev;


_10


REVOKE


`


But` junior_dev` is still able to execute the function:


`
_10


# as junior_dev


_10


postgres=> select add(1, 2);


_10


┌─────┐


_10


│ add │


_10


├─────┤


_10


│ 3 │


_10


└─────┘


_10


(1 row)


`


How? Let’s check` add` function’s privileges:


`
_12


# as postgres


_12


postgres=> \\df+ add


_12


┌────────┬──────┬──────────────────┬─────────────────────┬──────┬────────────┬──────────┬──────────┬──────────┬──────────────────────────┬──────────┐


_12


│ Schema │ Name │ Result data type │ Argument data types │ Type │ Volatility │ Parallel │ Owner │ Security │ Access privileges │ Language │


_12


├────────┼──────┼──────────────────┼─────────────────────┼──────┼────────────┼──────────┼──────────┼──────────┼──────────────────────────┼──────────│


_12


│ public │ add │ integer │ integer, integer │ func │ volatile │ unsafe │ postgres │ invoker │ =X/postgres ↵│ sql │


_12


│ │ │ │ │ │ │ │ │ │ postgres=X/postgres ↵│ │


_12


│ │ │ │ │ │ │ │ │ │ anon=X/postgres ↵│ │


_12


│ │ │ │ │ │ │ │ │ │ authenticated=X/postgres↵│ │


_12


│ │ │ │ │ │ │ │ │ │ service_role=X/postgres │ │


_12


└────────┴──────┴──────────────────┴─────────────────────┴──────┴────────────┴──────────┴──────────┴──────────┴──────────────────────────┴──────────┘


_12


(1 row)


`


` junior_dev` doesn’t have any privilege, but the missing role name in the` =X/postgres` line means the` public` role. Let’s revoke` execute` from` public` :


`
_14


# as postgres


_14


postgres=> revoke execute on function add(integer, integer) from public;


_14


REVOKE


_14


postgres=> \\df+ add


_14


_14


_14


_14


│ public │ add │ integer │ integer, integer │ func │ volatile │ unsafe │ postgres │ invoker │ postgres=X/postgres ↵│ sql │


_14


│ │ │ │ │ │ │ │ │ │ anon=X/postgres ↵│ │


_14


│ │ │ │ │ │ │ │ │ │ authenticated=X/postgres↵│ │


_14


│ │ │ │ │ │ │ │ │ │ service_role=X/postgres │ │


_14


│ │ │ │ │ │ │ │ │ │ │ │


_14


_14


(1 row)


`


Now` junior_dev` can not longer execute the` add` function:


`
_10


# as junior_dev


_10


postgres=> select add(1, 2);


_10


ERROR: permission denied for function add


`


Another thing to note here is that when we revoked` execute` privilege on` add` from` junior_dev` , there was actually nothing to revoke. But Postgres did not show us any warning. So it is important to always explicitly check the permissions, especially after a` revoke` command.


## Summary#


To summarize:


- Every database object has an owner.
- Operations on database objects are controlled by privileges.
- Owners can grant privileges on owned objects to other roles.
- Roles can be either users or groups.
- Roles can inherit permissions from roles they are a member of.
- ` public` role is a role which every other role is implicitly a member of. It can’t be deleted, but its privileges can be revoked.
- ` superuser` roles are all powerful roles that bypass all privilege checks and should be used with care.
- ` grant` command only grants privileges on existing objects.
- Default privileges control privileges to be granted to objects created in the future.


## Conclusion#


Postgres permissions follow the traditional objects, roles, privileges model but it has its subtleties which can surprise users unless they understand it in detail. In this post we experimented with this model to understand it in depth. Hope this understanding will allow you to manage and protect your Postgres database more effectively.
