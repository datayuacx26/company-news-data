---
schema_version: "1.0.0"
document_id: "43c93ff360aa1bd3508950bb4eba6e1dd0031a90594a36787a1d93e71f0e523b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/roles-postgres-hooks"
published_at: "2021-07-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:be4fa291f6f1a84ef657abdb9977a357d3cf66544e2aa7fec742a6855b467383"
---

# Protecting reserved roles with PostgreSQL Hooks

PostgreSQL manages permissions through roles. To create these roles, a database user needs the` CREATEROLE` privilege, which not only allows role creation but also *modification* of any role (except superusers).


At Supabase, we use dedicated roles for each of our customers' backend services. For instance, our[Storage API](https://supabase.com/storage) uses the` supabase_storage_admin` role for connecting to the database. Giving the` CREATEROLE` privilege to our customers would allow them to drop this role and take their own Storage API down.


And yet, we want to give our customers the ability to manage roles the same as they do it in on-premises databases, with the usual` CREATE ROLE` ,` ALTER ROLE` , and` DROP ROLE` statements.


So, how do we grant them the` CREATEROLE` privilege and, at the same time, protect our own roles? In this blog post, we explain how we managed to do this by using PostgreSQL Hooks in our[SupaUtils](https://github.com/supabase/supautils) extension.


## Reserved Roles#


PostgreSQL has a list of[predefined roles](https://www.postgresql.org/docs/14/predefined-roles.html) — all prefixed with "pg_" — that cannot be dropped or altered. Attempting to do so will throw an error mentioning that the role is "reserved".


`
_10


alter role pg_monitor createdb;


_10


ERROR: role name "pg_monitor" is reserved


_10


DETAIL: Cannot alter reserved roles.


`


This mechanism is an internal implementation detail. Unfortunately Postgres doesn't allow us to define our own reserved roles.


### RDS reserved roles#


Amazon RDS has a similar defense mechanism, all of its predefined roles — prefixed with "rds" — cannot be modified.


`
_10


alter role rdsadmin rename to another;


_10


ERROR: The "rdsadmin" role cannot be renamed.


_10


DETAIL: The "rdsadmin" role cannot be renamed because either the source


_10


or the target name refer to an Amazon RDS reserved role name.


_10


LOCATION: handle_rename, rdsutils.c:1534


`


Again, the error mentions that the role is "reserved".


Also note the` rdsutils.c` mention. That's not a stock Postgres source file. This means that the logic comes from an RDS extension. We can confirm this is the case by showing the preloaded libraries.


`
_10


show shared_preload_libraries;


_10


_10


shared_preload_libraries


_10


-----------------------------


_10


rdsutils,pg_stat_statements


`


` rdsutils` can be seen there. Naturally this lead us into thinking we can achieve the same logic with an extension of our own, and thus the SupaUtils idea was born.


## Extending PostgreSQL with Hooks#


PostgreSQL hooks allow us to extend internal functionality. Hooks can modify behavior at different places, including when running SQL statements.


For example, if we wanted to enforce our own password restrictions whenever a user changes passwords, we could use the` check_password_hook` to verify the password. We would write out our own Custom Logic, and raise an error if the password fails the password requirements.


For` SupaUtils` , we're particularly interested in the` ProcessUtility_hook` , which allows us to hook into **utility statements:** every statement except` select` ,` insert` ,` delete` or` update` . They include` alter role` and` drop role` , which are the statements we want to hook on.


### Hooks are global function pointers#


To use hooks, we can override functions pointers that are global. On the Postgres codebase, the` ProcessUtility_hook` is basically used1 like this:


`
_16


// src/backend/tcop/utility.c


_16


_16


// ProcessUtility_hook is NULL by default


_16


ProcessUtility_hook_type ProcessUtility_hook = NULL;


_16


_16


// This function is used for processing all the utility statements


_16


void


_16


ProcessUtility(PARAMS_OMITTED_FOR_BREVITY)


_16


{


_16


// call the ProcessUtility_hook if it's not NULL


_16


if (ProcessUtility_hook)


_16


(*ProcessUtility_hook)(PARAMS_OMITTED_FOR_BREVITY);


_16


// otherwise call the standard function used to process utility statements


_16


else


_16


standard_ProcessUtility(PARAMS_OMITTED_FOR_BREVITY);


_16


}


`


As you can see,` ProcessUtility_hook` is` NULL` by default, so our extension should set it for the hook to run. Also, the` standard_ProcessUtility` function is the one that actually does the job of creating or modifying the roles (among other things) so our hook should also call it.


### Loading and running the hook#


Each extension set in` shared_preload_libraries` will get its` _PG_init` function called. This function will allow us to set our hook onto` ProcessUtility_hook` .


Since hooks are global function pointers, it might be the case that another extension modifies the hook pointer (on its own` _PG_init` ) before us and sets its own hook. So we need to ensure we also run this previously-set hook, before or after our own hook runs.


It's typically2 done like this:


`
_26


// variable to store the previous hook


_26


static ProcessUtility_hook_type prev_hook = NULL;


_26


_26


// initialize our extension


_26


void


_26


_PG_init(void)


_26


{


_26


// ProcessUtility_hook has the global function pointer.


_26


// Store its value in case another extension already set its own hook.


_26


prev_hook = ProcessUtility_hook;


_26


// Now override the ProcessUtility_hook with our hook


_26


ProcessUtility_hook = our_hook;


_26


}


_26


_26


static void


_26


our_hook(PARAMS_OMITTED_FOR_BREVITY)


_26


{


_26


// our hook logic goes here


_26


_26


// If there was a previous hook, run it after our hook


_26


if (prev_hook)


_26


prev_hook(PARAMS_OMITTED_FOR_BREVITY);


_26


// If there's no previous hook, call the standard function


_26


else


_26


standard_ProcessUtility(PARAMS_OMITTED_FOR_BREVITY);


_26


}


`


## Setting up the SupaUtils extension#


We can use the concepts above to build our extension.


First we'll need a Makefile in order to compile the extension code and include it into our PostgreSQL installation.


`
_13


# Makefile


_13


_13


# Our shared library


_13


MODULE_big = supautils


_13


_13


# Our object files to build for the library


_13


OBJS = src/supautils.o


_13


_13


# Tell pg_config to pass us the PostgreSQL extensions makefile(PGXS)


_13


# and include it into our own Makefile through the standard "include" directive.


_13


PG_CONFIG = pg_config


_13


PGXS := $(shell $(PG_CONFIG) --pgxs)


_13


include $(PGXS)


`


For the source file, we'll start with variable definitions and functions declarations.


`
_30


// src/supautils.c


_30


_30


// include common declarations


_30


#include "postgres.h"


_30


_30


// required macro for extension libraries to work


_30


PG_MODULE_MAGIC;


_30


_30


// variable for the previous hook


_30


static ProcessUtility_hook_type prev_hook = NULL;


_30


_30


// variable for our reserved roles configuration parameter


_30


static char *reserved_roles = NULL;


_30


_30


// function declaration for extension initialization


_30


void _PG_init(void);


_30


_30


// function declaration for our hook


_30


static void supautils_hook(


_30


PlannedStmt *pstmt,


_30


const char *queryString,


_30


ProcessUtilityContext context,


_30


ParamListInfo params,


_30


QueryEnvironment *queryEnv,


_30


DestReceiver *dest,


_30


QueryCompletion *completionTag


_30


);


_30


_30


// function declaration for our pure function that will return a reserved role


_30


static char* look_for_reserved_role(Node *utility_stmt, List *reserved_role_list);


`


Up next we'll define each one of these function declarations.


## Initializing the extension#


Let's now` _PG_init` our extension. Besides setting the hook here, we want to define our reserved roles as a configuration parameter, that way they can be modified by editing the` postgresql.conf` file. For this, we can use the` DefineCustomStringVariable` function, which inserts the parameter into Postgres "Grand Unified Configuration"(GUC) system.


`
_22


void


_22


_PG_init(void)


_22


{


_22


// Store the previous hook


_22


prev_hook = ProcessUtility_hook;


_22


// Set our hook


_22


ProcessUtility_hook = supautils_hook;


_22


_22


// Define our "supautils.reserved_roles" parameter


_22


// some arguments are unused so they are left as NULL


_22


DefineCustomStringVariable("supautils.reserved_roles",


_22


"Comma-separated list of roles that cannot be altered or dropped",


_22


NULL,


_22


// It will be assigned to the reserved_roles variable


_22


&reserved_roles,


_22


NULL,


_22


// We should be able to reload this parameter without restarting the server,


_22


// e.g. with "select pg_reload_conf()".


_22


PGC_SIGHUP,


_22


0,


_22


NULL, NULL, NULL);


_22


}


`


## Running the SupaUtils hook#


Now that our hook is set, we'll define what it will do. As specified in the[ProcessUtility_hook_type](https://github.com/postgres/postgres/blob/f807e3410fdfc29ced6590c7c2afa76637e001ad/src/include/tcop/utility.h#L71-L75) , the hook's first parameter is a` PlannedStmt` , this represents the planned statement — the output from the Postgres planner. This is a step before the statement is executed.


We'll look for the presence of a reserved role in the planned statement. If there's one present, we'll report an error and abort the statement execution step.


`
_55


static void


_55


supautils_hook(


_55


// The planned statement


_55


PlannedStmt *pstmt,


_55


// These parameters are here for completion, we'll not use any of them


_55


const char *queryString,


_55


ProcessUtilityContext context,


_55


ParamListInfo params,


_55


QueryEnvironment *queryEnv,


_55


DestReceiver *dest,


_55


QueryCompletion *completionTag


_55


)


_55


{


_55


// Get the utility statement from the planned statement


_55


Node *utility_stmt = pstmt->utilityStmt;


_55


_55


// Only do the logic if supautils.reserved_roles is not NULL


_55


if(reserved_roles){


_55


// The found reserved role, assume none was found by default


_55


char *reserved_role = NULL;


_55


// Temp var for storing the list of reserved roles


_55


List *reserved_role_list;


_55


_55


// split the comma-separated string into a List by using a


_55


// helper function from varlena.h


_55


if (!SplitIdentifierString(pstrdup(reserved_roles), ',', &reserved_role_list))


_55


// abort and report an error if the splitting fails


_55


ereport(ERROR,


_55


(errcode(ERRCODE_INVALID_PARAMETER_VALUE),


_55


errmsg("parameter \\"%s\\" must be a comma-separated list of "


_55


"identifiers", reserved_roles)));


_55


_55


// look for the reserved role in an internal function


_55


reserved_role = look_for_reserved_role(utility_stmt, reserved_role_list);


_55


_55


// we're done with the list so free it from memory


_55


list_free(reserved_role_list);


_55


_55


// abort and report an error if a reserved role was found


_55


if(reserved_role)


_55


ereport(ERROR,


_55


(errcode(ERRCODE_INSUFFICIENT_PRIVILEGE),


_55


errmsg("\\"%s\\" is a reserved role, it cannot be modified", reserved_role)));


_55


}


_55


_55


// Run the previous hook if defined or call the standard function


_55


if (prev_hook)


_55


prev_hook(pstmt, queryString,


_55


context, params, queryEnv,


_55


dest, completionTag);


_55


else


_55


standard_ProcessUtility(pstmt, queryString,


_55


context, params, queryEnv,


_55


dest, completionTag);


_55


}


`


## Looking for the reserved role#


Lastly, we'll define how we look for the reserved role.


At this stage, we already have the utility statement and the reserved role list. All that's left to do is to define if the utility statement is an` ALTER ROLE` or` DROP ROLE` statement, and whether if the role it affects is a reserved one.


`
_70


static char*


_70


look_for_reserved_role(Node *utility_stmt, List *reserved_role_list)


_70


{


_70


// Check the utility statement type


_70


switch (utility_stmt->type)


_70


{


_70


// Matches statements like:


_70


// ALTER ROLE role NOLOGIN


_70


case T_AlterRoleStmt:


_70


{


_70


// cast the utility statement to an alter role statement


_70


AlterRoleStmt *stmt = (AlterRoleStmt *) utility_stmt;


_70


_70


//RoleSpec has the role name plus some attributes


_70


RoleSpec *role = stmt->role;


_70


_70


// postgres defines its own list utilities in pg_list.h


_70


// ListCell is an element of the list that we'll use for iteration


_70


ListCell *role_cell;


_70


_70


// pg_list.h includes the foreach macro for iterating over lists


_70


foreach(role_cell, reserved_role_list)


_70


{


_70


// get the list element


_70


char *reserved_role = (char *) lfirst(role_cell);


_70


_70


// compare the statement role with the reserved role


_70


// get_rolespec_name will get the RoleSpec role name,


_70


// even in cases where the role is the special case of


_70


// "current_user" or "session_user"


_70


if (strcmp(get_rolespec_name(role), reserved_role) == 0)


_70


return reserved_role;


_70


}


_70


_70


break;


_70


}


_70


_70


// Matches statements like:


_70


// DROP ROLE role


_70


case T_DropRoleStmt:


_70


{


_70


// cast the utility statement to a drop role statement


_70


DropRoleStmt *stmt = (DropRoleStmt *) utility_stmt;


_70


ListCell *item;


_70


_70


// the logic is the same as before, iterate over the reserved role list


_70


// and find a match


_70


foreach(item, stmt->roles)


_70


{


_70


RoleSpec *role = lfirst(item);


_70


ListCell *role_cell;


_70


_70


foreach(role_cell, reserved_role_list)


_70


{


_70


char *reserved_role = (char *) lfirst(role_cell);


_70


_70


if (strcmp(get_rolespec_name(role), reserved_role) == 0)


_70


return reserved_role;


_70


}


_70


}


_70


_70


break;


_70


}


_70


_70


default:


_70


break;


_70


}


_70


// Didn't find any reserved role on the statement, so return NULL


_70


return NULL;


_70


}


`


## Testing the extension#


Now that the code is finished, we can test the extension. Since we already have a` Makefile` , the extension can be installed by doing` make && make install` . Then, in postgresql.conf:


`
_10


# set the extension as preloaded, this will require a restart


_10


shared_preload_libraries="supautils"


_10


_10


# the reserved roles


_10


supautils.reserved_roles="supabase_storage_admin, supabase_auth_admin"


`


We'll now try to alter or drop the reserved roles:


`
_10


alter role supabase_storage_admin nologin password 'fake';


_10


ERROR: "supabase_storage_admin" is a reserved role, it cannot be modified


_10


_10


drop role supabase_auth_admin;


_10


ERROR: "supabase_auth_admin" is a reserved role, it cannot be modified


_10


_10


-- Success!!


`


## Wrapping up#


As you can see, PostgreSQL Hooks allow us to intercept SQL statements. There are many types of hooks, you can see unofficial documentation for these at[AmatanHead/psql-hooks](https://github.com/AmatanHead/psql-hooks) .


The full SupaUtils code is in our[GitHub repository](https://github.com/supabase/supautils/) .


By the way, if you like working on PostgreSQL tooling and extensions: we are hiring[PostgreSQL experts](https://supabase.com/docs/careers/postgres-experts) !


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Views](https://supabase.com/blog/postgresql-views)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [Cracking PostgreSQL Interview Questions](https://supabase.com/blog/cracking-postgres-interview)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)


## Footnotes#


1.


You can see the` ProcessUtility_hook` declaration[here](https://github.com/postgres/postgres/blob/11e9caff82bc7326e2bc9782937cb03875050cc4/src/backend/tcop/utility.c#L75-L76) and its usage[here](https://github.com/postgres/postgres/blob/11e9caff82bc7326e2bc9782937cb03875050cc4/src/backend/tcop/utility.c#L515-L527) .↩


2.


This is also done on[pg_stat_statements](https://github.com/postgres/postgres/blob/6991e774e0304f5ef488cf1ae4fa79578b6ae3d5/contrib/pg_stat_statements/pg_stat_statements.c#L1130-L1137) and[sepgsql](https://github.com/postgres/postgres/blob/6991e774e0304f5ef488cf1ae4fa79578b6ae3d5/contrib/sepgsql/hooks.c#L381-L388) .↩
