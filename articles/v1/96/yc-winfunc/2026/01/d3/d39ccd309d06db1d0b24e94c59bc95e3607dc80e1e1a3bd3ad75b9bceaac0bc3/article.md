---
schema_version: "1.0.0"
document_id: "d39ccd309d06db1d0b24e94c59bc95e3607dc80e1e1a3bd3ad75b9bceaac0bc3"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-3108"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:5bf2a5c3202c408a72240e9f20cbfff0feaea290b9acae14da3e6b7c4ad5ded7"
---

# mmctl terminal escape injection via unsanitized server-controlled output (CVE-2026-3108)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


High


CVE-2026-3108


2026


# mmctl terminal escape injection via unsanitized server-controlled output (CVE-2026-3108)


## Summary


mmctl rendered user-controlled Mattermost content to administrator terminals without stripping ANSI, OSC, DCS, or other control sequences


Mattermost's` mmctl` plain-text output path accumulated values from API responses and templates in` printer.Lines` , then wrote each line directly to stdout in` Printer.linesToBytes` . The report-posts command also bypassed the printer and wrote` post.Message` directly with` fmt.Fprintf(os.Stdout, ...)` . Because post messages and many other displayed fields can be controlled by ordinary users on the server being administered, a malicious value containing ANSI CSI sequences or OSC sequences could be replayed into an administrator's terminal when they inspected data with` mmctl` .


The root cause was a missing terminal-output trust boundary: the CLI treated server data as display-safe text. The fix added` printer.SanitizeForTerminal` , applied it to the central plain-output sink in` linesToBytes` , and separately sanitized the direct` report` command message print. The original fix is PR #35191 / commit` 22e4e9c171dcf9b447a56c7d9a45e5714fb24536` ; release-line backports include #35208, #35276, #35334, and #35335.


### CVSS Score


Vector


N


Complexity


L


Privileges


L


User Interaction


R


Scope


C


Confidentiality


H


Integrity


H


Availability


L


CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:C/C:H/I:H/A:L


### Vulnerability Location


Source


Line 111


server /cmd


/mmctl


/commands


/report.go


reportPostsCmdF


Sink


Line 275


server /cmd


/mmctl


/printer


/printer.go


Printer.linesToBytes


## Source-to-Sink Analysis


1


server/cmd/mmctl/commands/report.go:111-118


The administrator runs` mmctl report posts` , which fetches post objects from the Mattermost API and passes every returned post into` printReportPost` . The` post.Message` field is user-controlled content stored on the server.


GO


```text
response, _, err := c.GetPostsForReporting(context.TODO(), options, cursorObj)
if   err !=  nil   {
return   fmt.Errorf( "failed to get posts for reporting: %w"  , err)
}


for   _, post :=  range   response.Posts {
printReportPost(post)
}


```


2


server/cmd/mmctl/commands/report.go:147-152


Before the fix, report output wrote` post.Message` directly to stdout. The patched code applies` printer.SanitizeForTerminal` before rendering the message.


GO


```text
fmt.Fprintf(os.Stdout,  "Post ID: %s\n"  , post.Id)
fmt.Fprintf(os.Stdout,  "User ID: %s\n"  , post.UserId)
fmt.Fprintf(os.Stdout,  "Channel ID: %s\n"  , post.ChannelId)
fmt.Fprintf(os.Stdout,  "Message: %s\n"  , printer.SanitizeForTerminal(post.Message))


```


3


server/cmd/mmctl/printer/printer.go:103-115


Most mmctl commands render API-derived values through templates and append the rendered string into` printer.Lines` .


GO


```text
tpl := template.Must(template.New( ""  ).Funcs(printer.templateFuncs).Parse(templateString))
sb := &strings.Builder{}
if   err := tpl.Execute(sb, v); err !=  nil   {
PrintError( "Can't print the message using the provided template: "   + templateString)
return
}
printer.Lines =  append  (printer.Lines, sb.String())


```


4


server/cmd/mmctl/printer/printer.go:272-280


The central plain-text sink now converts each line to a string and strips terminal control sequences before writing to the output buffer.


GO


```text
case   FormatPlain:
var   buf bytes.Buffer
for   i :=  range   p.Lines {
line := fmt.Sprintf( "%s"  , p.Lines[i])
fmt.Fprintf(&buf,  "%s%s"  , SanitizeForTerminal(line), newline)
}
b = buf.Bytes()


```


5


server/cmd/mmctl/printer/printer_helpers.go:27-61


` SanitizeForTerminal` removes ANSI CSI, OSC, DCS, APC/PM, single-character escape sequences, and remaining control characters except tab, newline, and carriage return.


GO


```text
func    SanitizeForTerminal  (s  string  )     string   {
result := csiRegex.ReplaceAllString(s,  ""  )
result = oscRegex.ReplaceAllString(result,  ""  )
result = dcsRegex.ReplaceAllString(result,  ""  )
result = otherEscRegex.ReplaceAllString(result,  ""  )


var   cleaned strings.Builder
cleaned.Grow( len  (result))
for   _, r :=  range   result {
switch   {
case   r ==  '\t'   || r ==  '\n'   || r ==  '\r'  :
cleaned.WriteRune(r)
case   r <  0x20   || r ==  0x7F  :
continue
default  :
cleaned.WriteRune(r)
}
}
return   cleaned.String()
}


```


## Impact Analysis


### Critical Impact


The bug crosses the trust boundary between untrusted server data and the administrator's local terminal. Depending on terminal behavior, payloads can alter terminal state, hide or rewrite displayed output, set misleading prompts or window titles, or abuse OSC features such as clipboard writes. This is not server-side code execution, but it is a high-impact operator-side terminal injection primitive against privileged users.


### Attack Surface


Administrators or operators running` mmctl` against a Mattermost instance containing attacker-controlled content, especially commands that print post messages, user fields, channel names, webhook data, or other server-originated strings in plain format.


### Preconditions


The attacker must be able to place content that later appears in mmctl output. A privileged user must run an affected mmctl command in a terminal that interprets escape sequences. JSON output is less exposed because JSON encoding escapes control bytes, while plain output was vulnerable.


## Proof of Concept


### Environment Setup


Use a vulnerable Mattermost build before PR #35191 and a terminal that interprets ANSI/OSC sequences. Configure mmctl for an administrator account.


### Target Configuration


Any server where an ordinary user can create a post that an administrator later reviews with` mmctl report posts` or another plain-output mmctl command.


### Exploit Delivery


Create a post containing terminal control bytes such as CSI screen-clear sequences or an OSC clipboard sequence. Then run` mmctl report posts` as an administrator and include the malicious post in the report results.


### Outcome


The fix prevents terminal interpretation by sanitizing both the central printer path and the direct report-post message output.


**Expected Response:** On vulnerable builds, the terminal receives the raw escape bytes from` post.Message` or from` printer.Lines` . On fixed builds, the same output is rendered with the escape/control sequences removed.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35191)
