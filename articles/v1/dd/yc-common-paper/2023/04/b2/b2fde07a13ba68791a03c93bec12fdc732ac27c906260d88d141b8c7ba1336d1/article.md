---
schema_version: "1.0.0"
document_id: "b2fde07a13ba68791a03c93bec12fdc732ac27c906260d88d141b8c7ba1336d1"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/introducing-the-common-paper-api/"
published_at: "2023-04-20T12:42:16+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:e18af7f6030764d3d454278d24285019bcf745285a3e8101b429b0b01d3a4284"
---

# Introducing the Common Paper API

In the first post on our blog we laid out our vision that[contracts should be APIs](https://commonpaper.com/blog/contracts-should-be-apis/) . Today, we’re excited to announce an important step to making that vision a reality: the official release of the[Common Paper API](https://api.commonpaper.com/docs) .


Contracts represent interfaces between companies. They contain data and prescribe actions. When this information is locked up in unstructured PDF and DOCX files, deals slow down and contracts can’t power downstream decisions and automation.


The Common Paper API allows you to create and send new contracts while channeling alerts to the people and systems who need them. The structure built into all of our standard agreements enables programmatic access and a consistent data model. This opens up a world of possibilities for automation, compliance, and integration.


Let’s explore some sample use cases:


## NDA check before sharing documents


With the Common Paper API, you can automatically check whether a[Mutual NDA](https://commonpaper.com/standards/mutual-nda/) is in place between your company and a recipient before sending sensitive documents, such as the results of a security test or your SOC 2 report. By integrating the API with your document management or email system, you can prevent unauthorized sharing of confidential information and ensure that all parties involved are legally protected.


A curl request to get the data:


```text
curl --request GET \
--url "https://api.commonpaper.com/v1/agreements?filter\[agreement_type_eq\]=NDA&filter\[status_eq\]=signed&filter\[recipient_email_eq\]=mynewcustomer@example.com&filter\[effective_date_lt\]=2023-04-19&filter\[end_date_gt\]=2023-04-19" \
--header 'Authorization: Bearer <YOUR_KEY_HERE>'
```


Or a wrap it in a python script:


```text
import requests
from datetime import datetime


def can_send_report(email):
headers = {
"Authorization": "Bearer YOUR_API_KEY",
"Accept": "application/json"
}


# URL to get signed NDAs with this email that are in effect as of today
today = datetime.now().strftime('%Y-%m-%d')
url = f"https://api.commonpaper.com/v1/agreements?filter[agreement_type_eq]=NDA&filter[status_eq]=signed&filter[recipient_email_eq]={email}&filter[effective_date_lt]={today}&filter[end_date_gt]={today}"
try:
response = requests.get(url, headers=headers)
response.raise_for_status()  # Raise an exception if we get an error response code
data = response.json()
if 'data' in data and len(data['data']) > 0:
return True
else:
return False
except (requests.exceptions.RequestException, ValueError):
return False


email='mynewcustomer@example.com'
print(f"Can we send this to {email}?")
print(can_send_report(email))


```


## Real-time alerts for closed deals


Sales teams can integrate the Common Paper API with their customer relationship management (CRM) or chat systems to receive real-time notifications when contracts are viewed, signed, or declined. This enables faster communication and collaboration between teams, reducing the time it takes to act on new business opportunities and ensuring that everyone stays on the same page.


## Integration with analytics tools


The Common Paper API allows you to replicate contract data into your data warehouse and analyze it with your in house analytics and business intelligence tools. You can create custom reports and dashboards to track your company’s contract performance, trends, and pinpoint terms that slow down your deals. You can also download data about your agreements into a csv file as shown below.


```text
curl --request GET \
--url https://api.commonpaper.com/v1/agreements \
--header 'Authorization: Bearer <YOUR_KEY_HERE>'
```


Python script to download some agreement data to a csv file.


```text
import requests
import json
import csv


# Set up the API request headers
headers = {
'Authorization': 'Bearer YOUR_API_KEY',
'Content-Type': 'application/json'
}


# Make the API request to the agreements endpoint
response = requests.get('https://api.commonpaper.com/v1/agreements', headers=headers)


# Parse the JSON response
data = json.loads(response.text)


# Extract the list of agreements from the response data
agreements = data['data']


# Set up the CSV output file
with open('agreements.csv', 'w', newline='') as csvfile:
writer = csv.writer(csvfile)


# Write the header row
writer.writerow(['id', 'recipient_email', 'agreement_type', 'status', 'sent_date', 'effective_date'])


# Loop through the list of agreements and write each one to the CSV file
for agreement in agreements:
agreement_id = agreement['id']
agreement_recipient_email = agreement['attributes']['recipient_email']
agreement_type = agreement['attributes']['agreement_type']
agreement_status = agreement['attributes']['status']
agreement_sent_date = agreement['attributes']['sent_date']
agreement_effective_date = agreement['attributes']['effective_date']
writer.writerow([agreement_id, agreement_recipient_email, agreement_type, agreement_status, agreement_sent_date, agreement_effective_date])


print('Agreements data has been saved to agreements.csv')


```


## Automated contract compliance monitoring


By integrating the Common Paper API with your compliance monitoring tools, you can automate the process of tracking adherence to contractual obligations and ensure that your company remains compliant with all relevant regulations. This not only saves time and resources but also minimizes the risk of non-compliance, protecting your business from potential legal and financial penalties.


The Common Paper API represents a significant leap forward in the world of contract management, paving the way for unprecedented automation, integration, and efficiency. As we continue to develop and expand our API capabilities, we look forward to helping businesses of all sizes transform their contracting processes and unlock the full potential of contracts as APIs.


Ready to take your contract management to the next level? Get started with the Common Paper API today.[https://api.commonpaper.com/docs](https://api.commonpaper.com/docs) . Special thanks to[Zuplo](https://zuplo.com/) for their help in setting up and powering our API.
