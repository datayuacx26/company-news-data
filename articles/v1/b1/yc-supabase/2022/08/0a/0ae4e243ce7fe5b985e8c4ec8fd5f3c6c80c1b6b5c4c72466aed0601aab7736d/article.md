---
schema_version: "1.0.0"
document_id: "0ae4e243ce7fe5b985e8c4ec8fd5f3c6c80c1b6b5c4c72466aed0601aab7736d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-vault"
published_at: "2022-08-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:31.203349+00:00"
content_hash: "sha256:0d3318c824636fbc8c8fd260c3735cb59b1e3c4e6a1abad2a551c6ae1ab99c9b"
---

# Supabase Vault

Today we're announcing[Supabase Vault](https://supabase.com/docs/guides/database/vault) , a Postgres extension for managing secrets and encryption inside your database. Vault is a thin usability-layer on top of[pgsodium](https://github.com/michelp/pgsodium) .


❇️ UPDATE JUNE 2023 ❇️


Vault is now available on every Supabase project.[Check it out](https://supabase.com/dashboard/project/_/settings/vault/secrets)


[Transparent Column Encryption with Postgres](https://supabase.com/blog/transparent-column-encryption-with-postgres) is a blog post that describes the technology behind Vault -[libsodium](https://doc.libsodium.org/) and[pgsodium](https://doc.libsodium.org/) . Now, we will go through a quick example of storing a secret, like a service access token, into the Vault.


## Background#


Until now, the industry-standard for PostgreSQL encryption is a built-in extension called[pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html) . Like most cloud providers, Supabase offers` pgcrypto` for developers to use in their applications.` pgcrypto` has been around for a long time, and while it supports some basic encryption and decryption abilities, it lacks features like public key signing, key derivation APIs, streaming encryption, and other modern features required by security-first applications.


### Problems with raw keys#


Databases often store sensitive information, and they need tools that guarantee this data is stored and backed-up in an encrypted form. A fundamental issue with` pgcrypto` is that it lacks the ability to derive keys from outside of SQL. Instead you must have the raw encryption key for the algorithm you wish to use:


`
_12


create extension pgcrypto;


_12


_12


create table users (


_12


id serial primary key,


_12


email varchar not null unique


_12


);


_12


_12


insert into users


_12


(email)


_12


values


_12


(pgp_sym_encrypt('alice@supabase.io', 's3kr3t_k3y')),


_12


(pgp_sym_encrypt('bob@supabase.io', 's3kr3t_key'));


`


pgcrypto works with *raw* keys. In order to encrypt the data with pgcrypto you must pass the key directly to the encryption function. Leaking those raw keys is all too easy - logs, files, clients, tables, replication streams - you name it. Wouldn't it be great if you could encrypt data, but instead of specifying the raw key you reference it indirectly, like with a key ID?


## Supabase Vault#


Supabase Vault allows you to store secrets without leaking any sensitive information.


The Vault is a good place to put things like API keys, access tokens, and other secrets from external services that you need to access within your database. The core of the Supabase Vault is a table with some metadata and an encrypted text column where you can put your secrets and any metadata related to them.


We take the pain out of key management by pre-generating a unique, per-database key that is used by default - a “root” key - which is stored outside of the SQL language, accessibly only internally in the Postgres server by the libsodium library. This root key is managed by the[pgsodium](https://github.com/michelp/pgsodium) Postgres extension when the server boots using[Server Key Management.](https://github.com/michelp/pgsodium#server-key-management)


pgsodium provides an Encryption and Key Derivation API based on the[libsodium library](https://libsodium.gitbook.io/doc/) and can get it's root key from a variety of sources depending on how you configure it. Supabase generates and preserves your project's root key behind the scenes, so you don't have to worry about it. If you install pgsodium locally the default is to generate a random root key from the` /dev/urandom` device which is then saved in a file in your Postgres data directory.


Installing the` vault` extension is the same as any other Postgres extension:


`
_10


create extension supabase_vault with schema vault;


`


Once enabled, you can insert secrets into the` vault.secrets` table:


`
_10


insert into vault.secrets


_10


(secret, associated)


_10


values


_10


('s3kr3t_k3y', 'This is the secret API service key.');


`


Now when you look in the` vault.secrets` table, the secret is encrypted:


`
_10


select * from vault.secrets;


`


hideCopy


`
_10


-\[ RECORD 1 \]--------------------------------------------------------


_10


id | f6a2fe0a-3471-4eea-a581-75c4d2be396b


_10


secret | /eT9bb96POTJ7L2gYrluTZ3r3pG5IMwPSQo6pQP0xdZTarpRrpWPXTWQ


_10


key_id | caabfc28-2ab3-48f5-8978-1b3d4b659911


_10


associated | This is the secret API service key.


_10


nonce | \\x77c7381c523630ba72f1f137626a9f9a


_10


created_at | 2022-08-18 19:33:15.312651+00


`


Notice how the row has a` key_id` column. This is the *ID* of the internally derived key that is used to encrypt the secret, not the key itself. The actual raw key is not available to you in SQL, it is managed entirely outside of the SQL language in the Postgres server.


At Supabase, we manage this key for your project automatically and generate a unique default Key ID for you in the` secrets` table. For self-hosting, pgsodium[supports a variety of ways](https://github.com/michelp/pgsodium#server-key-management) to place the root key into Postgres.


To see the decrypted data, there is a special view created called` vault.decrypted_secrets` :


`
_10


select * from vault.decrypted_secrets;


`


hideCopy


`
_10


-\[ RECORD 1 \]----+---------------------------------------------------------


_10


id | f6a2fe0a-3471-4eea-a581-75c4d2be396b


_10


secret | /eT9bb96POTJ7L2gYrluTZ3r3pG5IMwPSQo6pQP0xdZTarpRrpWPXTWQ


_10


decrypted_secret | s3kr3t_k3y


_10


key_id | caabfc28-2ab3-48f5-8978-1b3d4b659911


_10


associated | This is the secret API service key.


_10


nonce | \\x77c7381c523630ba72f1f137626a9f9a


_10


created_at | 2022-08-18 19:33:15.312651+00


`


Now you can see a new` decrypted_secret` column that contains the decrypted secret we originally inserted into the table. This` vault.decrypted_secrets` view automatically decrypts rows in the` vault.secrets` table “on-the-fly” as you query them, but the secret is stored on disk in *encrypted* form. If you take a backup, or pause your project, that data remains encrypted. We will keep your hidden root key safe in our backend systems for when you need to restore or un-pause your projects.


If you wish to use your own Key ID for different secrets, instead of the default Key ID we've generated, you can create one using the` pgsodium.create_key()` function:


`
_10


select * from pgsodium.create_key('This is a comment for the new key');


`


hideCopy


`
_10


-\[ RECORD 1 \]-------------------------------------


_10


id | f9f176eb-7069-4743-9403-582c04354ffc


_10


status | valid


_10


created | 2022-08-18 22:31:50.331792


_10


expires |


_10


key_type | aead-det


_10


key_id | 2


_10


key_context | \\x7067736f6469756d


_10


comment | This is the comment for the new key


_10


user_data |


`


Now you can encrypt table secrets with this new key by inserting its ID explicitly:


`
_10


insert into vault.secrets


_10


(secret, associated, key_id)


_10


values


_10


(


_10


'apikey_XaYrurzcquqhEdBjzfTzfwAZqpd',


_10


'This is some different associated data.',


_10


'f9f176eb-7069-4743-9403-582c04354ffc'


_10


)


_10


returning *;


`


hideCopy


`
_10


-\[ RECORD 1 \]------------------------------------------------------------


_10


id | 9c58a0f3-aa40-4789-b683-6db48b241f9e


_10


secret | YWxuTnWdF55MuRrZ7xneBvaz2uH59U1dJV/7CCZjSn5B5jELOoy/csq8x/s=


_10


key_id | f9f176eb-7069-4743-9403-582c04354ffc


_10


associated | This is some different associated data.


_10


nonce | \\xd39808b07c9ae52c8f02c33a7f87595c


_10


created_at | 2022-08-18 22:34:07.219941+00


`


The type of encryption used by the Vault is called[Authenticated Encryption with Associated Data](https://en.wikipedia.org/wiki/Authenticated_encryption) . The data you insert into the` associated` column, which is up to you, is combined with the encrypted text when libsodium creates the authentication signature for the secret. This means that when you read the secret, you know that the associated data is also authentic. The associated data could be an account ID or some information that ties your system to the secret. And as always, you can refer to rows in the secrets table by their primary key UUID.


If you only want to store secrets that you know are encrypted on disk and in backups, then all you need to know is shown above. Just insert secrets into the table, optionally creating new keys, and select them from the view when you want to use them.


## Going Beyond the Vault#


The Vault is good for a reasonable amount of secure data, like API keys, access tokens, or environment variables. But if you have a lot more sensitive information, like personally Identifiable Information (PII), you may want to break them out into side-tables using pgsodium's[Transparent Column Encryption](https://supabase.com/blog/transparent-column-encryption-with-postgres) which we will describe soon in a follow-up blog post. Stay tuned!
