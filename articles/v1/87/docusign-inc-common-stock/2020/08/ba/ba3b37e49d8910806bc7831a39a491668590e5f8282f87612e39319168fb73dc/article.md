---
schema_version: "1.0.0"
document_id: "ba3b37e49d8910806bc7831a39a491668590e5f8282f87612e39319168fb73dc"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-aebf23be7403"
canonical_url: "https://www.docusign.com/blog/developers/automating-health-questionnaires"
published_at: "2020-08-06T00:00:00+00:00"
first_seen_at: "2026-07-25T01:48:46.315991+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:8aa99235273736a4235b0d20e517126d832a2f401f2ffdfbc415e87821996d5a"
---

# Automating Health Questionnaires with PowerForms

Reopening an office during the pandemic requires many new procedures. For many businesses, asking employees to complete and sign a health questionnaire each day will be part of the new normal.


Docusign offers many product features to help with this task.


## Using a PowerForm for the health questionnaire


A Docusign PowerForm is a public web address that automatically creates a Docusign envelope and enables the user to fill out and sign the form. PowerForms are ideal for health questionnaires, since they are easy to set up from the Docusign eSignature web app (no programming needed), and easy for employees or visitors to complete.


PowerForm responses are encrypted and stored within Docusign’s servers. They can only be accessed by authorized users on your Docusign account.


You can try an[example employer health questionnaire PowerForm opens in a new tab](https://demo.docusign.net/Member/PowerFormSigning.aspx?PowerFormId=23377688-69f9-45e3-b677-1be3ee353054&env=demo&acct=7a9312b8-a9ba-45c0-98a0-2322ef460e32&v=2) based on the[recommendations of the New York City Department of Health opens in a new tab](https://www1.nyc.gov/assets/doh/downloads/pdf/imm/covid-19-symptom-screening-businesses.pdf) .


### Make the form responsive and mobile-friendly


Docusign offers an optional[responsive signing feature](https://support.docusign.com/s/document-item?bundleId=gbo1643332197980&topicId=cbv1578456473989.html&_LANG=enus&language=en_US&rsc_301) that automatically reformats documents for easier viewing on mobile devices. To see how this feature works, try filling in the example health questionnaire using a mobile phone.


### Simplify the form with conditional fields


The example questionnaire uses[conditional fields](https://support.docusign.com/s/document-item?language=en_US&rsc_301&bundleId=gbo1643332197980&topicId=hew1578456460132.html&_LANG=enus) to simplify the form. If the **No** checkbox is checked for question 1, then the **No** and **Yes** checkboxes for question 2 are shown. In the same way, a conditional rule for the question 2 **No** checkbox causes the question 3 checkboxes to be shown. (A field that is conditionally shown can itself have a rule for conditionally showing other fields.)


If a required field is conditionally shown, then it will only be required when it is shown. In the example form, the later questions have required checkbox groups, but they’re only shown conditionally.


### Use checkbox groups with validation


For each set of No/Yes checkboxes, the[checkbox group validation feature](https://support.docusign.com/s/document-item?language=en_US&rsc_301&bundleId=gbo1643332197980&topicId=srj1578456517556.html&_LANG=enus) is set to **Select exactly 1** . This rule ensures that either the No or Yes checkbox will be checked (but not both) before the envelope can be signed and submitted.


### Skip the PowerForm splash page


PowerForms normally display a **Signer Information Page** before the document is shown. By including **Name** and **Email** fields on the document itself, the signer information page can be skipped, providing a smoother user experience. See[Bypassing the PowerForm Signer Information Page](https://support.docusign.com/s/document-item?language=en_US&bundleId=jbx1643062255110&topicId=mwf1578456643729.html&_LANG=enus) for more information.


## No need to fill in your name everyday: customized URLs


Most health questionnaires include fields for the employee’s name, email address, and employee number. With Docusign, there’s no need to fill in that information manually each day. Instead, it is easy to distribute customized URLs to your employees that will open the questionnaire with their name, email, and employee number already filled in. For example,[this URL opens in a new tab](https://demo.docusign.net/Member/PowerFormSigning.aspx?PowerFormId=23377688-69f9-45e3-b677-1be3ee353054&env=demo&acct=7a9312b8-a9ba-45c0-98a0-2322ef460e32&v=2&signer_UserName=Pat%20Jones&signer_Email=pat.jones@example.com&EmployeeID=1234) will open the demonstration form with an example employee’s information already filled in. The[Programming PowerForms video opens in a new tab](https://www.youtube.com/watch?v=7BKfyDSMoSE) discusses how to create a personalized URL that will fill in PowerForm fields.


To create personalized URLs and email them to your employees in bulk:


1.


Create an Excel spreadsheet with the employees’ names, email addresses, and employee IDs.


2.


Create a new column that will hold the employee’s names with any spaces changed to “%20”. This step is needed because URLs with spaces can be problematic. Here’s an example formula: =SUBSTITUTE(A1," ","%20")


3.


Create a new column that will hold the complete URL for the employee. See the video or[Populate Custom Document Fields in a Web PowerForm](https://docs.docusign.com/supportdocs/ndse-help/Content/populate-custom-fields.htm) for more information on creating the personalized URL.


4.


Test some of the resulting URLs (in an incognito browser window) to make sure they work properly. Remember to test employee names that include accented characters, apostrophes, and other non-ASCII characters.


5.


Use the[Word Email mail merge feature opens in a new tab](https://support.microsoft.com/en-us/office/use-mail-merge-to-send-bulk-email-messages-0f123521-20ce-4aa8-8b62-ac211dedefa4) to email the URLs to your employees as part of an explanatory email. Result: each employee will receive a personalized email that includes their personalized PowerForm URL for completing the questionnaire each day.


## Did every employee complete a form?


Some businesses will want to positively ensure that all employees in the office have completed a daily health form. Docusign offers features for your IT department to develop this type of application.


An application can use[Docusign’s webhook system, Connect](https://www.docusign.com/blog/dsdev-webhook-listeners-part-1) , to receive a notification each time a health questionnaire is completed. The form’s employee number field (or email field) can be used to update a master list with the fact that the employee has completed the form for the day. At a preset time (for example, 11am), any employee that has not yet completed a form can be emailed, asking them to complete a form.


In this use case, the form might be expanded to enable an employee to record that they won’t be in the office that day because they’re on vacation, or working from home.


To enable the application to determine easily if a Docusign envelope is being used for the health questionnaire or for a different purpose, an[Envelope Custom Field](https://support.docusign.com/s/document-item?language=en_US&rsc_301&bundleId=pik1583277475390&topicId=qor1583277385137.html&_LANG=enus) can be used. Envelope Custom Fields enable metadata to be stored with an envelope.


Docusign webhook notifications can be received from behind a firewall, with no changes to the firewall. To do so, an intermediate Platform as a Service (PaaS) system is used to queue and distribute the notifications. This[blog post](https://www.docusign.com/blog/dsdev-webhook-listeners-part-4) provides more information and example software.


## PowerForm limits


If you expect that your employees will submit 1,000 or more PowerForms an hour, please discuss your use case with your Docusign Account Manager or Technical Customer Service manager. This will help Docusign plan adequate system capacity.


**Reporting:** PowerForms reporting is limited to 1,000 PowerForms at a time.


## Summary


Docusign’s product features enable businesses to quickly spin up new applications that include document signatures and authentication. Administering, recording, and tracking health questionnaires is just one example of Docusign’s flexibility for businesses and organizations of all sizes.


## Additional resources


-


[Docusign Developer Center](https://developers.docusign.com/)


-


[Docusign Developers YouTube channel opens in a new tab](https://www.youtube.com/channel/UCJSJ2kMs_qeQotmw4-lX2NQ)


-


[@DocuSignAPI on Twitter opens in a new tab](https://twitter.com/docusignapi)


-


[Docusign for Developers on LinkedIn opens in a new tab](https://www.linkedin.com/showcase/docusign-for-developers/)


-


[Docusign Developer Newsletter](https://developers.docusign.com/#newsletter-signup)


---------------------------------------------------------


*Larry Kluger is the Lead Developer Advocate for Docusign. He has many years of tech industry experience as a software developer, product manager, and evangelist. An award-winning speaker, he enjoys giving talks and meeting members of the developer community.*
