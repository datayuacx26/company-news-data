---
schema_version: "1.0.0"
document_id: "f2fd451ece295668ba36a7eb24e0e70b61b860a98f50f39860db822092ae51b0"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-google-cloud"
published_at: "2024-09-25T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:72977bb10a8354923af9ce19734129797357aaaeb3d0cbdc80bdbf76dd1ee5d6"
---

# Encryption at rest in Elastic Cloud: Bring your own key with Google Cloud

# Encryption at rest in Elastic Cloud: Bring your own key with Google Cloud


By


[Jonathan Simon](https://www.elastic.co/blog/author/jonathan-simon)[Alex Chalkias](https://www.elastic.co/blog/author/alex-chalkias)


September 25, 2024


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Now that we’ve introduced


[Elastic Cloud encryption at rest](https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-enterprise-security) and walked you through setting it up in


[AWS](https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-aws-kms) and


[Azure](https://www.elastic.co/blog/elastic-cloud-azure-key-vault) , it’s time to get you set up in Google Cloud.


In this final blog of the series, we will explain how encryption at rest works with Google Cloud Key Management Service (KMS) and then show you how to apply a Google Cloud KMS key to an Elastic Cloud Hosted deployment for encrypting data and snapshots at rest. We’ll also show you how to validate your setup and implement additional security policies, such as encryption key rotation and revocation.


## The Elastic Cloud and Google Cloud Key Management integration


### Architecture


The following diagram shows how Elastic Cloud integrates with Google Cloud to provide your application with Hosted Elastic Cloud Hosted deployments encrypted with your own key.


### Prerequisites


1.


**Get your own key:**


Creating an Elastic deployment with a customer provided encryption key is also known as


*Bring Your Own Key*


(BYOK). To create an Elastic deployment with BYOK, you need to have Google Identity and Access Management (IAM) permissions to create a Google Cloud key using the Cloud KMS. The key must be created on a Google Cloud key ring in the same


[region](https://www.elastic.co/guide/en/cloud/current/ec-regions-templates-instances.html) as the Elastic deployment that you’re going to encrypt.


2.


**Upgrade to Enterprise:**


An Enterprise license is required for BYOK.


3. **Access control:**


You also need permissions to


[manage access to your new key resource using Google IAM](https://cloud.google.com/kms/docs/iam) . This is required to grant the service principals used by Elastic to access your key.


**Note:**


If your GCP organization enforces


iam.allowedPolicyMemberDomains


, go to our product documentation in the


[Create a deployment encrypted with your key](https://www.elastic.co/docs/deploy-manage/security/encrypt-deployment-with-customer-managed-encryption-key#ec_create_a_deployment_encrypted_with_your_key) section, select the Google Cloud tab, and follow the steps to allow granting access to Elastic service principals; otherwise, reach out to Elastic Support.


## Elastic deployment initialization


Begin by logging in to the Elastic Cloud console.


After you’ve logged in to the console, click the


**Create deployment**


button.


Enter a name for your deployment and select Google Cloud as your cloud provider. Expand the


**Advanced settings**


section and enable the


**Use a customer-managed encryption key**


option. Copy the


**Elastic service account**


and the


**Google Cloud Platform cloud storage service agent**


to save these values somewhere handy for a later step.


For now, we’ll leave the create deployment page as it is and open a new browser tab, where we’ll create a Google Cloud key that we’ll use to encrypt the deployment.


## Creating and configuring a Google Cloud key


To start the key creation process, go to


**Key Management**


in the


[Google Cloud console](https://console.cloud.google.com/) .


Select the


**Key Ring**


, which will contain the key that you will create.


Click


**Create Key**


.


Enter a


**Key Name**


for the key to be created and click


**Create.**


Select the newly created key to see its details.


Select the key’s


**Permissions**


tab.


Select


**Grant**


**Access**


.


Paste in the


Elastic service account in the


**New Principals**


field and assign it the roles


**Cloud KMS CryptoKey Encrypter/Decrypter**


and


**Cloud KMS Viewer**


. Click


**Save**


.


Select the key’s


**Grant**


**Access**


button again.


Paste in the


Google Cloud Platform cloud storage agent in the


**New Principals**


field and assign it the role


**Cloud KMS CryptoKey Encrypter/Decrypter**


. Click


**Save**


.


Click on the


**Back to key ring details**


button.


Click the


**Action**


button for the key and select


**Copy resource name**


.


## Elastic deployment creation completion


Return to the Elastic Cloud portal to complete the deployment creation that you started at the outset of this blog post. Within the


**Advanced Settings**


,


****


under


**Encryption at rest**


,


****


paste in the


**Google Cloud Key resource name**


.


****


It should be in the following format:


```text
projects/PROJECT_ID/locations/LOCATION/keyRings/KEY_RING/cryptoKeys/KEY_NAME
```


Click


**Create deployment**


.


The deployment is now created and encrypted using the specified Google Cloud key.


## Verification and troubleshooting


In the


[Elasticsearch Service console](https://cloud.elastic.co/?page=docs&placement=docs-body) , you can check that your hosted deployment is correctly encrypted with the key you specified. To do that, go to the deployment’s security page by selecting


**Security**


from the left navigation menu.


Select


**Manage encryption key**


in the Encryption at rest


****


section.


You should see your Google Cloud key resource name.


## Key rotation and revocation


Key rotations are managed in the Google Cloud Key Management service. You can manually rotate keys or set up automatic rotation. Key rotation operations made in Google Cloud KMS will take effect in Elastic Cloud within a day.


Revoking a key in the Google Cloud KMS is a break-glass procedure in case of a security breach. Elastic Cloud will receive an error within a 30-minute period if an encryption key is disabled or deleted, or if the assigned role is removed from the IAM permissions.


The revocation can be rolled back if the action was unintended. Otherwise, Elastic Cloud locks the directories in which your deployment data live and prompts you to delete your deployment as an increased security measure.


## Enhance your security today


You’ve now seen how BYOK can be used for encryption of an Elastic deployment running on Google Cloud. First, a Google Cloud KMS key needs to be created and set up with the necessary policy settings required for Elastic to manage and rotate the key’s credentials. Then, an Elastic Cloud deployment can be created, and you can use that very same key for encryption of the data contained within the deployment.


Try it out for yourself today.


[Create an Elastic Cloud deployment](https://cloud.elastic.co/registration) with your own Google Cloud KMS key to enhance the overall security of your Elastic Cloud deployment.


You can find more information about Elastic Cloud and AWS KMS in the


[product documentation](https://www.elastic.co/docs/deploy-manage/security/encrypt-deployment-with-customer-managed-encryption-key) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
