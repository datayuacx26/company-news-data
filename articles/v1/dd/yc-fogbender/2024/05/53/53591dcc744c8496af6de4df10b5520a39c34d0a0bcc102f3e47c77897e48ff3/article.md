---
schema_version: "1.0.0"
document_id: "53591dcc744c8496af6de4df10b5520a39c34d0a0bcc102f3e47c77897e48ff3"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/postgrex-ssl-with-supabase"
published_at: "2024-05-28T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:1d3808391ef47d4997be246dffa623da5cec891cd5e73d9d4dc0be2227cd6bf7"
---

# SSL for Postgrex and Supabase in Elixir

As sometimes happens, I needed to create a Postgrex connection to a Supabase Postgres instance (hosted on[supabase.com](https://supabase.com/) ) from Elixir. After failing to get it working quickly, I embarked on a super annoying journey I’d love to help others avoid, so I thought I’d write the key findings down.


To find your connection parameters in Supabase, look for Project Settings / Configuration / Database / Connection Parameters.


As a separate minor challenge, I had to get my Supabase certificate from an application configuration variable (fed in via[Doppler](https://www.doppler.com/) ) to a file. To do this, I used the` Application.put_env` approach:


application.ex


```text
case   MyApp.  env  (  :db_ssl_crt  )   do            nil     ->              :ok
pem   ->               {  :ok  , path}   =   Briefly.  create  ()               File.  write!  (path, pem)               Application.  put_env  (  :my_app  ,   :cacertfile_path  , path)          end
```


The certificate is written to a temporary file with[Briefly](https://github.com/CargoSense/briefly) . Note that Briefly will close the temporary file as soon as the temp file creator process exits—in our case, it’s the application process, which means the path to the certificate will remain available as long as the application is running.


```text
def     db  ()   do           ssl   =     case   MyApp.  env  (  :cacertfile_path  )   do            nil     ->               []
path   ->               [  ssl:     true  ,                ssl_opts:   [                  verify:     :verify_peer  ,                  cacertfile:   path,                  server_name_indication:   String.  to_charlist  (MyApp.  env  (  :db_host  )),                  customize_hostname_check:   [                    match_fun:     :public_key  .  pkix_verify_hostname_match_fun  (  :https  )                   ]                 ]               ]          end
opts   =   [            hostname:   MyApp.  env  (  :db_host  ),            port:   MyApp.  env  (  :db_port  ),            username:   MyApp.  env  (  :db_user  ),            database:   MyApp.  env  (  :db_name  ),            password:   MyApp.  env  (  :db_pass  )           ]   ++   ssl
Postgrex.  start_link  (opts)        end
```
