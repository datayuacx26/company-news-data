---
schema_version: "1.0.0"
document_id: "d8612dcf30988e555ac13668e84e87bb0289d9ccca01436b092f94c2aa8bff59"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-3113"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:3c7ea6ea962e9a4f2322977b260f64506956928ca313541b70d03611517d5f59"
---

# mmctl export downloads created world-readable local files (CVE-2026-3113)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-3113


2026


# mmctl export downloads created world-readable local files (CVE-2026-3113)


## Summary


Export files downloaded with mmctl inherited unsafe filesystem permissions


` mmctl export download` and related export download flows write potentially sensitive export archives to the administrator's local filesystem. The shared` downloadFile` helper used` os.Create(path)` when the destination did not exist, which creates files with mode` 0666` before process umask is applied. On a common` 022` umask, the resulting export file is` 0644` and readable by other local users. When the destination already existed but was empty,` os.OpenFile(path, os.O_WRONLY, 0600)` did not change existing permissions, so a pre-created permissive file remained permissive.


The fix creates new files with` os.OpenFile(path, os.O_WRONLY|os.O_CREATE, 0600)` and explicitly` os.Chmod(path, 0600)` before opening existing empty files. The original fix is PR #35182 / commit` 6cd2df33ea12f9aeae4c68994b72656425168b67` ; backports include #35244, #35246, #35247, and #35248.


### CVSS Score


Vector


L


Complexity


L


Privileges


L


User Interaction


R


Scope


U


Confidentiality


H


Integrity


N


Availability


N


CVSS:3.1/AV:L/AC:L/PR:L/UI:R/S:U/C:H/I:N/A:N


### Vulnerability Location


Source


Line 238


server /cmd


/mmctl


/commands


/export.go


downloadFile


Sink


Line 250


server /cmd


/mmctl


/commands


/export.go


os.Create


## Source-to-Sink Analysis


1


server/cmd/mmctl/commands/export.go:238-250


The shared export download helper receives a destination path and checks whether the file exists. Before the fix, the new-file branch used` os.Create` .


GO


```text
case   err !=  nil  :
// file does not exist, we create it
outFile, err = os.Create(path)
createdFile =  true


```


2


server/cmd/mmctl/commands/export.go:251-253


Before the fix, the existing-empty-file branch used` os.OpenFile` with a mode argument, but that mode is ignored unless a file is created. Existing permissive permissions remained unchanged.


GO


```text
default  :
// no error, file exists, we open it
outFile, err = os.OpenFile(path, os.O_WRONLY,  0600  )


```


3


server/cmd/mmctl/commands/export.go:249-258


The patched code creates new files as` 0600` , and fixes existing empty destination files with` os.Chmod(path, 0600)` before writing export contents.


GO


```text
case   err !=  nil  :
outFile, err = os.OpenFile(path, os.O_WRONLY|os.O_CREATE,  0600  )
createdFile =  true
default  :
permErr := os.Chmod(path,  0600  )
if   permErr !=  nil   {
return    ""  , fmt.Errorf( "failed to change permissions on output file: %w"  , permErr)
}


outFile, err = os.OpenFile(path, os.O_WRONLY,  0600  )


```


4


server/cmd/mmctl/commands/export_e2e_test.go:232-236


The regression tests now assert` 0600` permissions for both existing-empty-file and full-download paths.


GO


```text
info, err := os.Stat(downloadPath)
s.Require().Nil(err)
s.Require().Equal(fs.FileMode( 0600  ), info.Mode().Perm(), fmt.Sprintf( "expected %o, got %o"  , fs.FileMode( 0600  ), info.Mode().Perm()))


```


## Impact Analysis


### Critical Impact


The server-side export permission model can be bypassed at rest on the operator's local machine. Sensitive export data that only an administrator should access may become readable by other local users or processes.


### Attack Surface


Administrator workstations or automation hosts where` mmctl export download` writes Mattermost export archives to shared filesystems or machines with multiple local users.


### Preconditions


A privileged operator must download an export file using a vulnerable mmctl build. A local attacker must be able to read files created in the destination directory under the resulting mode.


## Proof of Concept


### Environment Setup


Use a vulnerable mmctl build before PR #35182 on a Unix-like system with a typical` umask 022` .


### Target Configuration


Run an export download into a directory where file mode can be inspected, or pre-create an empty file with permissive permissions and pass it as the download destination.


### Exploit Delivery


Run` umask 022` followed by` mmctl export download <export.zip> ./export.zip` . Then run` stat -c %a export.zip` or the platform equivalent.


### Outcome


Export archives are no longer readable by other local users by default.


**Expected Response:** Vulnerable builds can produce` 644` for new files or preserve permissive permissions on existing empty files. Fixed builds produce` 600` .


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35182)
