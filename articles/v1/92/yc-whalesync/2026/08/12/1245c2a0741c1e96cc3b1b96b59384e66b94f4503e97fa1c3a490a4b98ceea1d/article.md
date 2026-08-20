---
schema_version: "1.0.0"
document_id: "1245c2a0741c1e96cc3b1b96b59384e66b94f4503e97fa1c3a490a4b98ceea1d"
company_key: "yc-whalesync"
company: "Whalesync"
source_id: "yc-whalesync-news-import-4fcd9b7a082a"
canonical_url: "https://www.whalesync.com/blog/how-to-get-a-google-sheets-api-key"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T08:25:37.902095+00:00"
fetched_at: "2026-08-12T08:25:38.438144+00:00"
content_hash: "sha256:36ce65f66d6ba5b3d85db81dd607e3dec64632e68bd24a6361e7a9f023e9a74d"
---

# How to Get a Google Sheets API Key

A Google Sheets API key lets you read and write a spreadsheet from the command line or from code. This guide follows the video above step by step. You'll enable the Sheets API in Google Cloud, create a service account with write access, mint an access token with a short shell script, and call the API with curl to read and update a sheet.


### **Step 1: Create a test sheet**


Create a spreadsheet in Google Drive to test against. In the video the sheet is named "YouTube API key demo" and one cell holds "hello, YouTube."


### **Step 2: Open the Google Cloud console**


Google Sheets has no settings page with a ready-made key. API access runs through Google Cloud, so go to[console.cloud.google.com](https://console.cloud.google.com/) . Your first visit may ask you to accept the terms. The API access itself costs nothing or pennies.


### **Step 3: Create a project**


Create a new project and select it. The video names it "Google Sheets API Key Project."


### **Step 4: Enable the Google Sheets API**


Search for "Google Sheets" in the bar at the top and open the Google Sheets API, which lives in the Marketplace. Click Enable.


### **Step 5: Create a service account**


A read-only path with a plain API key exists, but write access needs a service account. Search for "service accounts," click Create service account, and name it. The video uses "Google Sheets API Service Account." Click through Create and continue, then Continue, then Done.


### **Step 6: Create a JSON key**


On the service account, open Manage keys, then Add key, Create new key, and choose JSON. The key file downloads to your computer. You can't paste this file into a request as a key. The script in step 8 reads it to mint one.


### **Step 7: Share the sheet with the service account**


This is the step everyone misses. Copy the service account's email address, which looks like name@project-id.iam.gserviceaccount.com, and share the target sheet with it as an Editor. That share is what gives the API access to the sheet.


### **Step 8: Mint an access token**


Save the script below as mint_token.sh and make it executable. It uses only jq, openssl, and curl. No Python, no gcloud. Tokens expire after one hour; run the script again for a fresh one.


```text
#!/usr/bin/env bash
# Mint a Google OAuth access token from a service account key.
# Pure shell: needs only jq, openssl, and curl. No Python, no gcloud.
#
#   ./mint_token.sh <key.json> [scope]
set -euo pipefail


KEY="$1"
SCOPE="${2:-https://www.googleapis.com/auth/spreadsheets}"


b64url() { openssl base64 -A | tr '+/' '-_' | tr -d '='; }


EMAIL=$(jq -r .client_email "$KEY")
NOW=$(date +%s)


HEADER=$(printf '{"alg":"RS256","typ":"JWT"}' | b64url)
CLAIMS=$(jq -nc \
--arg iss "$EMAIL" --arg scope "$SCOPE" \
--argjson iat "$NOW" --argjson exp "$((NOW + 3600))" \
'{iss:$iss, scope:$scope, aud:"https://oauth2.googleapis.com/token",
iat:$iat, exp:$exp}' | b64url)


# Sign "<header>.<claims>" with the key's RSA private key.
SIG=$(printf '%s.%s' "$HEADER" "$CLAIMS" \
| openssl dgst -sha256 -sign <(jq -r .private_key "$KEY") | b64url)


curl -s https://oauth2.googleapis.com/token \
-d grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer \
--data-urlencode "assertion=${HEADER}.${CLAIMS}.${SIG}" \
| jq -r .access_token
```


### **Step 9: Call the API with curl**


Set three variables, then run any of the commands below. The sheet ID is the long string in the middle of the spreadsheet URL.


```text
export KEY=~/Downloads/your-service-account-key.json
export SHEET=your_sheet_id_from_the_url
export TOKEN=$(./mint_token.sh $KEY)


# Read a range
curl -s -H "Authorization: Bearer $TOKEN" "https://sheets.googleapis.com/v4/spreadsheets/$SHEET/values/Sheet1!A1:D10"


# Sheet metadata (title + tab names)
curl -s -H "Authorization: Bearer $TOKEN" "https://sheets.googleapis.com/v4/spreadsheets/$SHEET?fields=properties.title,sheets.properties.title"


# Read several ranges in one request
curl -s -H "Authorization: Bearer $TOKEN" "https://sheets.googleapis.com/v4/spreadsheets/$SHEET/values:batchGet?ranges=Sheet1!A1:B5&ranges=Sheet1!D1:D5"


# Overwrite a range
curl -s -X PUT -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' "https://sheets.googleapis.com/v4/spreadsheets/$SHEET/values/Sheet1!B1?valueInputOption=USER_ENTERED" -d '{"values":[["hello","from curl"]]}'


# Append a row after the last populated row
curl -s -X POST -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' "https://sheets.googleapis.com/v4/spreadsheets/$SHEET/values/Sheet1!A1:append?valueInputOption=USER_ENTERED" -d '{"values":[["appended","row"]]}'


```


## **Two-way sync without the shell script**


[Whalesync](https://www.whalesync.com/) syncs Google Sheets two-way with Webflow, Notion, Supabase, Salesforce, HubSpot, and more.[Scratch](https://scratch.md/) downloads your data locally so AI can edit it, then publishes it back safely. This video was recorded by Curtis, founder of Whalesync.
