---
schema_version: "1.0.0"
document_id: "9c4938ab53270b3afe5bef2793ccdb18daf98056db213cdf4ceda525e0266ecb"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-3114"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:26b3c0c0940c3e85de09d76f29305ab1a13ed8a75451cf4a7ee573be6b4424ba"
---

# Zip bomb memory exhaustion in recursive document extraction (CVE-2026-3114)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-3114


2026


# Zip bomb memory exhaustion in recursive document extraction (CVE-2026-3114)


## Summary


Archive extraction limited compressed upload size but not decompressed entry size


Mattermost enforces` FileSettings.MaxFileSize` on uploaded files, but the document extraction service previously did not apply that same limit to each decompressed archive entry. When file-content extraction and archive recursion were enabled,` archiveExtractor.Extract` mounted an uploaded archive, walked entries with` fs.WalkDir` , opened each entry, and called` io.ReadAll(file)` before handing the decompressed bytes to sub-extractors. A small compressed archive could therefore expand into very large in-memory entry data and exhaust server memory during content extraction.


The fix threads` MaxFileSize` through` docextractor.ExtractSettings` and the` Extractor` interface, passes the configured file-size limit from` App.ExtractContentFromFileInfo` , and wraps archive entry readers with` utils.NewLimitedReaderWithError` before` io.ReadAll` . The original fix is PR #35200 / commit` b947f1c38a675688c5fc9ade696d0f0f2bad430a` ; backports include #35220, #35279, and #35282.


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


N


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H


### Vulnerability Location


Source


Line 1625


server /channels


/app


/file.go


App.ExtractContentFromFileInfo


Sink


Line 100


server /platform


/services


/docextractor


/archive.go


archiveExtractor.Extract


## Source-to-Sink Analysis


1


server/channels/app/upload.go:349-352


When file content extraction is enabled, uploaded file metadata can trigger` ExtractContentFromFileInfo` immediately after storage. The extract-content job can also process stored files later.


GO


```text
if   *a.Config().FileSettings.ExtractContent {
infoCopy := info
err := a.ExtractContentFromFileInfo(rctx, &infoCopy)
}


```


2


server/channels/app/file.go:1636-1638


The patched app layer now passes both` ArchiveRecursion` and the configured` MaxFileSize` into the document extractor.


GO


```text
text, err := docextractor.Extract(rctx.Logger(), fileInfo.Name, file, docextractor.ExtractSettings{
ArchiveRecursion: *a.Config().FileSettings.ArchiveRecursion,
MaxFileSize:      *a.Config().FileSettings.MaxFileSize,
})


```


3


server/platform/services/docextractor/docextractor.go:36-48


When archive recursion is enabled, the archive extractor is configured with a sub-extractor chain and receives the` MaxFileSize` value through the shared extractor interface.


GO


```text
if   settings.ArchiveRecursion {
enabledExtractors.Add(&archiveExtractor{SubExtractor: enabledExtractors})
}  else   {
enabledExtractors.Add(&archiveExtractor{})
}


if   enabledExtractors.Match(filename) {
return   enabledExtractors.Extract(filename, r, settings.MaxFileSize)
}


```


4


server/platform/services/docextractor/archive.go:87-105


Before the fix, each archive entry was read with an unbounded` io.ReadAll(file)` . The patched code wraps the entry in a limited reader before reading decompressed bytes into memory.


GO


```text
file, err := fsys.Open(path)
if   err !=  nil   {
return   err
}
defer   file.Close()


var   reader io.Reader = file
if   maxFileSize >  0   {
reader = utils.NewLimitedReaderWithError(file, maxFileSize)
}


data, err := io.ReadAll(reader)
if   err !=  nil   {
return   fmt.Errorf( "error reading archive entry %s: %w"  , path, err)
}


subtext, extractErr := ae.SubExtractor.Extract(filename, bytes.NewReader(data), maxFileSize)


```


## Impact Analysis


### Critical Impact


A small authenticated upload can cause disproportionate server memory allocation during content extraction. Repeated uploads or a sufficiently large decompressed entry can exhaust memory, destabilize extraction workers, and degrade or crash the Mattermost process.


### Attack Surface


Mattermost servers with file uploads enabled and file-content extraction configured. The decompressed-entry risk specifically depends on recursive archive extraction (` FileSettings.ArchiveRecursion` ) because sub-extraction reads archive entries into memory.


### Preconditions


The attacker needs an account or integration capable of uploading files to a channel where the server will process the file for text extraction. The compressed archive must fit within the configured upload size limit while expanding to very large entry data.


## Proof of Concept


### Environment Setup


Use a vulnerable build before PR #35200 with` FileSettings.ExtractContent` enabled and` FileSettings.ArchiveRecursion` enabled. Keep` FileSettings.MaxFileSize` at a normal value such as 100 MB.


### Target Configuration


The vulnerable path is the document extraction service, not the upload size check. The archive itself must be accepted by upload limits while containing an entry that decompresses to a much larger size.


### Exploit Delivery


Upload a crafted ZIP archive with a highly compressed large text entry. The server stores the uploaded file and invokes extraction either immediately or through the extract-content job.


### Outcome


The patch turns decompressed-entry overrun into a bounded extraction error instead of unbounded memory growth.


**Expected Response:** On vulnerable builds, extraction attempts to allocate the full decompressed entry via` io.ReadAll(file)` . On fixed builds, reading the entry fails once the configured max file size is exceeded.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35200)
