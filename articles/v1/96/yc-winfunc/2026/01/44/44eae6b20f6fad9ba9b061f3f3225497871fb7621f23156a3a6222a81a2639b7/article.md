---
schema_version: "1.0.0"
document_id: "44eae6b20f6fad9ba9b061f3f3225497871fb7621f23156a3a6222a81a2639b7"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-24458"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:cce847c38887274158a660f098d123774a63ecfc034e3f2b7802e2ebd12d4faa"
---

# Oversized password login DoS in legacy password comparison (CVE-2026-24458)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


High


CVE-2026-24458


2026


# Oversized password login DoS in legacy password comparison (CVE-2026-24458)


## Summary


Login password comparison paths accepted attacker-supplied passwords without enforcing the maximum password length first


Mattermost validates password length when passwords are created or changed, but affected login comparison paths did not consistently apply the same maximum-length guard before invoking password verification. In modern 11.x branches,` App.checkUserPassword` parses the stored hash and dispatches to the matching hasher. PBKDF2 already enforced` PasswordMaxLengthBytes` , but the legacy bcrypt hasher's` CompareHashAndPassword` did not check input length before calling` bcrypt.CompareHashAndPassword` . In the 10.11 line, the older` users.ComparePassword` helper similarly called` bcrypt.CompareHashAndPassword` before checking` model.PasswordMaximumLength` .


The fix adds explicit maximum-length checks in the comparison path before expensive password verification is attempted: PR #35092 / commit` 7201f42d955f1bc44719b862132546626b60a180` for the 11.x legacy hasher path, with backports #35117, #35126, and #35127; and PR #35062 / commit` 0655a633546037107a1e0f3243c542fbaea0901f` for the 10.11 comparison helper.


### CVSS Score


Vector


N


Complexity


L


Privileges


N


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


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H


### Vulnerability Location


Source


Line 2060


server /channels


/api4


/user.go


login


Sink


Line 75


server /channels


/app


/password


/hashers


/bcrypt.go


BCrypt.CompareHashAndPassword


## Source-to-Sink Analysis


1


server/channels/api4/user.go:2060-2096


The unauthenticated login endpoint parses the attacker-supplied JSON body, including` password` , and passes it into authentication.


GO


```text
props := model.MapFromJSON(r.Body)
loginId := props[ "login_id"  ]
password := props[ "password"  ]
mfaToken := props[ "token"  ]


user, err = c.App.AuthenticateUserForLogin(c.AppContext, id, loginId, password, mfaToken,  ""  , ldapOnly)


```


2


server/channels/app/authentication.go:65-78


` checkUserPassword` obtains the hasher from the stored hash and compares the attacker-supplied plaintext password with it.


GO


```text
hasher, phc, err := hashers.GetHasherFromPHCString(user.Password)
if   err !=  nil   {
return   model.NewAppError( "checkUserPassword"  ,  "api.user.check_user_password.invalid_hash.app_error"  ,  nil  ,  "user_id="  +user.Id, http.StatusUnauthorized)
}


err = hasher.CompareHashAndPassword(phc, password)


```


3


server/channels/app/password/hashers/bcrypt.go:75-85


The 11.x fix adds a maximum-length check to the legacy bcrypt comparison wrapper before calling` bcrypt.CompareHashAndPassword` .


GO


```text
func    (b BCrypt)    CompareHashAndPassword(hash phcparser.PHC, password  string  )  error   {
if    len  (password) > PasswordMaxLengthBytes {
return   ErrPasswordTooLong
}


err := bcrypt.CompareHashAndPassword([] byte  (hash.Hash), [] byte  (password))
if   errors.Is(err, bcrypt.ErrMismatchedHashAndPassword) {
return   ErrMismatchedHashAndPassword
}


return   err
}


```


4


server/channels/app/users/password.go:23-31 (release-10.11 fix)


The 10.11 fix adds the same fail-fast length check to the older` ComparePassword` helper before the bcrypt comparison call.


GO


```text
func    ComparePassword  (hash  string  , password  string  )     error   {
if   password ==  ""   || hash ==  ""   {
return   errors.New( "empty password or hash"  )
}


if    len  (password) > model.PasswordMaximumLength {
return   NewErrInvalidPassword( "model.user.is_valid.pwd_max_length.app_error"  )
}


return   bcrypt.CompareHashAndPassword([] byte  (hash), [] byte  (password))
}


```


## Impact Analysis


### Critical Impact


Oversized login attempts can impose disproportionate work on the server's authentication path, resulting in denial of service for legitimate login attempts. The fix makes oversized values fail closed before hash comparison.


### Attack Surface


The login API (` POST /api/v4/users/login` ) for accounts whose stored hashes use the affected comparison path. The 11.x fix is specifically the legacy bcrypt hasher path; the 10.11 fix is the older bcrypt comparison helper.


### Preconditions


Network access to the login endpoint. The attacker can submit oversized password values for an existing login ID. No valid password is required for the resource-exhaustion attempt.


## Proof of Concept


### Environment Setup


Use a vulnerable release before the relevant fix for the release line: before #35092 for 11.x legacy bcrypt accounts or before #35062 for 10.11.


### Target Configuration


Have a reachable Mattermost login endpoint and a known existing login ID. For 11.x, the clearest affected path is an account with a legacy bcrypt-formatted stored password.


### Exploit Delivery


Send repeated` POST /api/v4/users/login` requests with a multi-megabyte` password` value and the target` login_id` .


### Outcome


The maximum password length is enforced consistently during comparison, not only during password creation or update.


**Expected Response:** Vulnerable builds spend expensive verification work on the oversized password before returning an authentication error. Fixed builds return through the password-length error path before invoking the hasher comparison.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35092)
