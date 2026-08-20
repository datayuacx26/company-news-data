---
schema_version: "1.0.0"
document_id: "d01ca904bbf69f1dd4920146cdcea235c38259421e2d6b8d90977d9780620804"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/elastic-cloud-azure-key-vault"
published_at: "2024-08-20T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:f79ba69b7dd3f05d8dba08dabb7c95997f224949145dffe95cb5e5b15ba73606"
---

# Encryption at rest in Elastic Cloud: Bring your own key with Azure Key Vault

# Encryption at rest in Elastic Cloud: Bring your own key with Azure Key Vault


By


[Alex Chalkias](https://www.elastic.co/blog/author/alex-chalkias)[Greg Crist](https://www.elastic.co/blog/author/greg-crist)[Gheorghe Pucea](https://www.elastic.co/blog/author/gheorghe-pucea)


August 20, 2024


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


In the first


[blog](https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-enterprise-security) in this series, we unpacked the foundational concepts of encryption at rest and introduced you to Elastic Cloud’s “bring your own key” (BYOK) feature, which allows you to do encryption at rest with encryption keys managed by the KMS service of your cloud provider. The second


[blog](https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-aws-kms) of this series dives into the technical nuances of implementing encryption at rest with AWS KMS keys. Building on that knowledge, this blog dives into the technical nuances of implementing encryption at rest with Azure Key Vault keys and Elastic Cloud deployments, providing you with a secure and compliant environment for your sensitive data.


First, let’s walk through the architecture of the solution and its prerequisites, and then we’ll explore how to create an Azure Key Vault key and apply it to an Elastic Cloud Hosted deployment for encrypting data and snapshots at rest. We’ll also show you how to validate your setup and implement additional security policies, such as encryption key rotation and revocation.


## The Elastic Cloud and Azure Key Vault integration


### Architecture


The following diagram showcases how Elastic Cloud integrates with Azure Key Vault to provide your application with Hosted Stack deployments encrypted with your own Key Vault keys.


### Prerequisites


**1. Get your own key:**


To use BYOK, you need a key that you control. You set this up in your Azure Key Vault account. Create an RSA asymmetric key. The key must be available in each region you have deployments to encrypt. You can use the same key to encrypt multiple deployments, although security best practices recommend using a different one per deployment. Later, you'll need to provide the Key Vault Key URI and the key name to Elastic Cloud.


**2. Upgrade to Enterprise:**


BYOK is available for the Enterprise subscription level. This means if you're using Elastic on Azure, you can encrypt your data using your own key.


Keep in mind, i


f you're using a custom snapshot repository instead of Elastic Cloud's default one, your snapshots won't automatically be encrypted with your key. However, the file system itself will still encrypt data being stored on disk.


**3. Access control:**


Create Azure Identity and Access Management (IAM) policies to control access to the Elastic cluster.


-


The following are required permissions on Azure:


-


Permissions to create an RSA key in the Azure Key Vault where you want to store your key


-


Membership in the Application Administrator role (This is required to create a new service principal for Elastic Cloud in your Azure tenant.)


-


Permissions to assign roles in your Key Vault using access control (IAM) (This is required to grant the service principal access to your key.)


-


The Azure Key Vault where the RSA key will be stored must have purge protection enabled to support the encryption of snapshots.


## Creating and configuring an Azure Key Vault key


1. To start the key creation process, go to the Key Vault service in the Azure portal. Click


**Keys**


.


2. In the key creation process, select


**Generate/Import**


and specify the key type and key size.


3. Provide a name for the key and click


**Create**


.


4. Add the necessary principal (Elastic service principal) to the access policy and click


**Save**


. The service principal grants Elastic Cloud access to interact with your RSA key.


In your Azure Portal, view the key that you created. In the Access control (IAM) settings for the key, grant the service principal the role


**Key Vault Crypto User**


.


5. Copy the


**Key Identifier**


and the key name from the Overview tab. Save them in a safe place for use in a later step.


## Integrating an Azure Key Vault key with Elastic Cloud


1. Now you can create a new Elastic deployment that uses the Azure Key Vault key you just created. Start by signing in to the Elastic Cloud console.


2. After you’ve signed in to the console, click the


**Create deployment**


button.


3. Enter a Name for your deployment and select


**Microsoft Azure**


as your Cloud provider. Then expand the


**Advanced setting**


**s** section and enable the


**Use a customer-managed encryption key option**


. Paste in the Azure Key Vault RSA Key Identifier (URI) and key name that you copied in the previous step.


4. Click


**Create deployment**


. The deployment is now created and encrypted using the specified key.


## Verification and troubleshooting


1. In the Elastic Cloud Console, you can check that your hosted deployment is correctly encrypted with the key you specified. To do that, go to the deployment’s


**Security**


page by selecting


**Security**


from the left navigation menu.


2. Select


**Manage encryption key**


in the Encryption at rest section.


3. You should see your Azure Key Vault URI and key name listed in the Azure


**Key Vault RSA key identifier**


field.


## Key rotation and revocation


Elastic Cloud Hosted deployments encrypted with Azure Key Vault keys benefit from Azure's security policies and features, such as key rotation and revocation. Key rotation helps reduce the risk of data breaches due to compromised keys, while key revocation ensures that access to encrypted data via a compromised key is terminated. This can be done by disabling, deleting the key, or altering the key’s access policy.


Azure Key Vault keys can be rotated manually when necessary. Elastic automatically manages these key rotations, ensuring that your Elastic Cloud deployment remains encrypted and accessible with the most current Azure Key Vault key.


If a key is compromised, you can manually revoke it in Azure Key Vault. This emergency operation, intended for security breaches, locks the deployment’s data directories within 30 minutes and prompts you to delete the deployment. If the revocation is accidental, the key can be restored, allowing the deployment to resume normal operations.


## Try it out


You now understand the process of using your own key for encrypting an Elastic Deployment on Azure. Initially, an Azure Key Vault key is created and configured with the necessary policy settings for Elastic to manage and rotate the key’s credentials. You can create an Elastic Cloud deployment using this key to encrypt the deployment’s data.


Find more information about Elastic Cloud and AWS KMS in the


[product documentation](https://www.elastic.co/docs/deploy-manage/security/encrypt-deployment-with-customer-managed-encryption-key) .


Give it a try today! Create an Elastic Cloud deployment with your Azure Key Vault key to enhance the security of your Elastic Cloud deployment. Sign up for a


[free 14-day trial](https://cloud.elastic.co/registration) to get started. In the[last blog of this series](https://www.elastic.co/blog/encryption-at-rest-elastic-cloud-google-cloud) , we will walk you through the steps to encrypt your deployment data and snapshots with GCP KMS managed keys.


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
