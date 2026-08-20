---
schema_version: "1.0.0"
document_id: "5cc0284e753470b943f7dd7ed56c4b3c3d7f7c72ada08aa0607db65db47561ca"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-audit"
published_at: "2022-03-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:5615d6c6c2605cc7077d49b963d6c8619b3db56bb513cb7ce34c11ebe520f9d3"
---

# Postgres Auditing in 150 lines of SQL

Data auditing is a system that tracks changes to tables' contents over time. PostgreSQL has a robust set of features which we can leverage to create a generic auditing solution in 150 lines of SQL.


Auditing is particularly useful for historical analysis. To demonstrate, imagine you have a` users` table that tracks when a user is online. You might add a` status` column which can have one of two values:` online` and` offline` . How would you track how long a user is online for throughout an entire month? An auditing system would track every change with timestamps, and so you can measure the difference between each timestamp and sum them up for the entire month.


The goals of our auditing solution are:


- low maintenance
- easy to use
- fast to query


To demonstrate what we're working towards, the following example shows what we'll have at the end of the blog post:


`
_10


-- create a table


_10


create table public.members (


_10


id int primary key,


_10


name text not null


_10


);


_10


_10


-- Enable auditing on the new table


_10


select audit.enable_tracking('public.members');


`


Produce some records to audit


`
_13


-- create a new record


_13


insert into public.members


_13


(id, name)


_13


values


_13


(1, 'foo');


_13


_13


-- edit the record


_13


update public.members


_13


set name = 'bar'


_13


where id = 1;


_13


_13


-- delete the record


_13


delete from public.members;


`


Review the audit log


`
_10


select * from audit.record_history;


`


`
_10


id | record_id | old_record_id | op | ts | table_oid | table_schema | table_name | record | old_record


_10


----+--------------------------------------+--------------------------------------+--------+-------------------------------------+-----------+--------------+------------+--------------------------+--------------------------


_10


2 | 1ecd5ff0-1b6b-5bc2-ad80-1cb19769c081 | | INSERT | Mon Feb 28 18:13:52.698511 2022 PST | 16452 | public | members | {"id": 1, "name": "foo"} |


_10


3 | 1ecd5ff0-1b6b-5bc2-ad80-1cb19769c081 | 1ecd5ff0-1b6b-5bc2-ad80-1cb19769c081 | UPDATE | Mon Feb 28 18:13:52.698511 2022 PST | 16452 | public | members | {"id": 1, "name": "bar"} | {"id": 1, "name": "foo"}


_10


4 | | 1ecd5ff0-1b6b-5bc2-ad80-1cb19769c081 | DELETE | Mon Feb 28 18:13:52.698511 2022 PST | 16452 | public | members | | {"id": 1, "name": "bar"}


_10


(3 rows)


`


Notice that our` record_id` and` old_record_id` stayed constant as we updated the row so we can easily query for a single row's history over time!


## Lets get building#


### Namespace#


To quote a tenet from[the zen of python](https://www.python.org/dev/peps/pep-0020/) :


> Namespaces are one honking great idea -- let's do more of those!


So first things first, we'll create a separate schema named` audit` to house our auditing entities.


`
_10


create schema if not exists audit;


`


### Storage#


Next, we need a table to track inserts, updates and deletes.


Classically, an audit table's schema mirrors the table being audited and appends some metadata columns like the commit's timestamp. That solution has a few maintenance challenges:


- enabling auditing on a table requires a database migration
- when the source table's schema changes, the audit table's schema must also change


So instead, we'll lean on PostgreSQL's schema-less[JSONB data type](https://www.postgresql.org/docs/current/datatype-json.html) to store each record's data in a single column. That approach has the added benefit of allowing us to store multiple tables' audit history in a single audit table.


`
_15


create table audit.record_version (


_15


id bigserial primary key,


_15


-- auditing metadata


_15


record_id uuid, -- identifies a new record by it's table + primary key


_15


old_record_id uuid, -- ^


_15


op varchar(8) not null, -- INSERT/UPDATE/DELETE/TRUNCATE


_15


ts timestamptz not null default now(),


_15


-- table identifiers


_15


table_oid oid not null, -- pg internal id for a table


_15


table_schema name not null, -- audited table's schema name e.g. 'public'


_15


table_name name not null, -- audited table's table name e.g. 'account'


_15


-- record data


_15


record jsonb, -- contents of the new record


_15


old_record jsonb -- previous record contents (for UPDATE/DELETE)


_15


);


`


Postgres version compatibility


The table above uses PostgreSQL's built-in[uuid functionality](https://www.postgresql.org/docs/14/functions-uuid.html) , which is available from version 14. For backwards compatibility you can use the uuid-ossp extension.


` create extension if not exists "uuid-ossp";`


### Query Patterns#


An audit log doesn't do us much good if its too slow to query! There are 2 query patterns we think are table stakes (😉) for an audit system:


**Changes to a Table in a Time Range**


For time slices, we need an index on the` ts` column. Since the table is append-only and the` ts` column is populated by insertion date, our values for` ts` are naturally in ascending order.


PostgreSQL's builtin[BRIN index](https://www.postgresql.org/docs/current/brin-intro.html) can leverage that correlation between value and physical location to produce an index that, at scale, is many hundreds of times smaller than the default (BTREE index) with faster lookup times.


`
_10


-- index ts for time range filtering


_10


create index record_version_ts


_10


on audit.record_version


_10


using brin(ts);


`


For table filtering, we've included a` table_oid` column which tracks PostgreSQL's internal numeric table identifier. We can add an index to this column instead of the` table_schema` and` table_name` columns, minimizing the index size and offering better performance.


`
_10


-- index table_oid for table filtering


_10


create index record_version_table_oid


_10


on audit.record_version


_10


using btree(table_oid);


`


**Changes to a Record Over Time**


One of the downsides to storing each row's data as` jsonb` is that filtering based on a column's value becomes very inefficient. If we want to look up a row's history quickly, we need to extract and index a unique identifier for each row.


For the globally unique identifier, we'll use the following structure


`
_10


\[table_oid, primary_key_value_1, primary_key_value_2, ...\]


`


and hash that array as a UUID v5 to get an efficiently indexable UUID type to identify the row that is robust to data changes.


We'll use one utility function to lookup a record's primary key column names:


`
_21


create or replace function audit.primary_key_columns(entity_oid oid)


_21


returns text\[\]


_21


stable


_21


security definer


_21


language sql


_21


as $$


_21


-- Looks up the names of a table's primary key columns


_21


select


_21


coalesce(


_21


array_agg(pa.attname::text order by pa.attnum),


_21


array\[\]::text\[\]


_21


) column_names


_21


from


_21


pg_index pi


_21


join pg_attribute pa


_21


on pi.indrelid = pa.attrelid


_21


and pa.attnum = any(pi.indkey)


_21


where


_21


indrelid = $1


_21


and indisprimary


_21


$$;


`


and another to consume the` table_oid` and primary key, converting the result into the record's UUID.


`
_28


create or replace function audit.to_record_id(


_28


entity_oid oid,


_28


pkey_cols text\[\],


_28


rec jsonb


_28


)


_28


returns uuid


_28


stable


_28


language sql


_28


as $$


_28


select


_28


case


_28


when rec is null then null


_28


-- if no primary key exists, use a random uuid


_28


when pkey_cols = array\[\]::text\[\] then gen_random_uuid()


_28


else (


_28


select


_28


uuid_generate_v5(


_28


'fd62bc3d-8d6e-43c2-919c-802ba3762271',


_28


(


_28


jsonb_build_array(to_jsonb($1))


_28


|| jsonb_agg($3 ->> key_)


_28


)::text


_28


)


_28


from


_28


unnest($2) x(key_)


_28


)


_28


end


_28


$$;


`


Finally, we index the` record_id` and` old_record_id` columns that contain these unique identifiers for fast querying.


`
_10


-- index record_id for fast searching


_10


create index record_version_record_id on audit.record_version (record_id)


_10


where record_id is not null;


_10


_10


-- index old_record_id for fast searching


_10


create index record_version_old_record_id on audit.record_version (record_id)


_10


where old_record_id is not null;


`


### Enrollment#


Okay, so we have a home for our audit data that we're confident it can be queried efficiently. Now how do we populate it?


We need the audit table to populate without end-users making any changes to their transactions. So we'll set up a[trigger](https://www.postgresql.org/docs/current/sql-createtrigger.html) to fire when the data changes. In this case, we'll fire the trigger once for every inserted/updated/deleted row.


`
_36


create or replace function audit.insert_update_delete_trigger()


_36


returns trigger


_36


security definer


_36


language plpgsql


_36


as $$


_36


declare


_36


pkey_cols text\[\] = audit.primary_key_columns(TG_RELID);


_36


record_jsonb jsonb = to_jsonb(new);


_36


record_id uuid = audit.to_record_id(TG_RELID, pkey_cols, record_jsonb);


_36


old_record_jsonb jsonb = to_jsonb(old);


_36


old_record_id uuid = audit.to_record_id(TG_RELID, pkey_cols, old_record_jsonb);


_36


begin


_36


_36


insert into audit.record_version(


_36


record_id,


_36


old_record_id,


_36


op,


_36


table_oid,


_36


table_schema,


_36


table_name,


_36


record,


_36


old_record


_36


)


_36


select


_36


record_id,


_36


old_record_id,


_36


TG_OP,


_36


TG_RELID,


_36


TG_TABLE_SCHEMA,


_36


TG_TABLE_NAME,


_36


record_jsonb,


_36


old_record_jsonb;


_36


_36


return coalesce(new, old);


_36


end;


_36


$$;


`


## Public API#


Finally, we'll wrap up the trigger creation and removal process behind a clean, idempotent, user facing API.


The API we'll expose for enabling auditing on a table is


`
_10


select audit.enable_tracking('<schema>.<table>'::regclass);


`


and for disabling tracking


`
_10


select audit.disable_tracking('<schema>.<table>'::regclass);


`


Under the hood, those functions register our auditing trigger against the requested table.


`
_43


create or replace function audit.enable_tracking(regclass)


_43


returns void


_43


volatile


_43


security definer


_43


language plpgsql


_43


as $$


_43


declare


_43


statement_row text = format('


_43


create trigger audit_i_u_d


_43


before insert or update or delete


_43


on %I


_43


for each row


_43


execute procedure audit.insert_update_delete_trigger();',


_43


$1


_43


);


_43


_43


pkey_cols text\[\] = audit.primary_key_columns($1);


_43


begin


_43


if pkey_cols = array\[\]::text\[\] then


_43


raise exception 'Table % can not be audited because it has no primary key', $1;


_43


end if;


_43


_43


if not exists(select 1 from pg_trigger where tgrelid = $1 and tgname = 'audit_i_u_d') then


_43


execute statement_row;


_43


end if;


_43


end;


_43


$$;


_43


_43


create or replace function audit.disable_tracking(regclass)


_43


returns void


_43


volatile


_43


security definer


_43


language plpgsql


_43


as $$


_43


declare


_43


statement_row text = format(


_43


'drop trigger if exists audit_i_u_d on %I;',


_43


$1


_43


);


_43


begin


_43


execute statement_row;


_43


end;


_43


$$;


`


And we're done with 2 lines of code to spare!


### Performance#


Auditing tables always reduces throughput of inserts, updates, and deletes. In cases where throughput is less than 1000 writes per second the overhead is typically negligible. For tables with a higher write frequency, consider logging changes outside of SQL with a tool like[pgAudit](https://www.pgaudit.org/) .


### Do I really expect you to copy/paste all that?#


Nope, for a turnkey solution to auditing in PostgreSQL, we've packaged this script into an extension with some extra goodies like` TRUNCATE` support. Check it out at[https://github.com/supabase/supa_audit](https://github.com/supabase/supa_audit) .
