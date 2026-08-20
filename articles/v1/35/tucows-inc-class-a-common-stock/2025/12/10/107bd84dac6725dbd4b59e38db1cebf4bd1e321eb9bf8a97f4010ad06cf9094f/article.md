---
schema_version: "1.0.0"
document_id: "107bd84dac6725dbd4b59e38db1cebf4bd1e321eb9bf8a97f4010ad06cf9094f"
company_key: "tucows-inc-class-a-common-stock"
company: "Tucows Inc."
source_id: "tucows-inc-class-a-common-stock-rss-e99aab447650"
canonical_url: "https://storiesfromtheherd.com/bringing-oidc-to-atlantis-a-secure-workflow-for-temporary-aws-credentials-c9c7b93e0c25"
published_at: "2025-12-09T20:58:47+00:00"
first_seen_at: "2026-07-20T23:20:12.168287+00:00"
fetched_at: "2026-07-28T22:25:03.887977+00:00"
content_hash: "sha256:d3de95f074b2e132192a9bb628169bff3949846be56c25ba319755e2f2ddcf65"
---

# Bringing OIDC to Atlantis: A Secure Workflow for Temporary AWS Credentials

# Bringing OIDC to Atlantis: A Secure Workflow for Temporary AWS Credentials


[Team Tucows](https://tucows.medium.com/?source=post_page---byline--c9c7b93e0c25---------------------------------------)


8 min read


·


Dec 9, 2025


--


***Disclaimer**
*The information, examples, and approaches described in this post are intended for illustrative or educational purposes only. They do not necessarily reflect the exact methods, configurations, or practices used in our production environments. Our live systems may use different architectures, tools, or security measures optimized for scalability, reliability, and compliance.*


**Introduction:** Atlantis doesn’t have built-in support for using OpenID Connect (OIDC) to get short-lived cloud credentials. That means it can’t directly use OIDC to log in to AWS. In this guide, we walk through how to work around this limitation by combining GitHub Actions, HashiCorp Vault, and a few helper scripts so Atlantis can still manage AWS infrastructure using temporary, secure OIDC-based credentials.


**Problem Statement:** Atlantis cannot connect with AWS using OpenID Connect (OIDC), which means we either have to use static credentials or find an alternative solution to manage Terraform orchestration. OIDC improves credential management by using short-lived tokens.


Using a combination of common CI/CD tools like Harshcorp Vault, Github Actions, and some scripts, we can use short-lived AWS credentials in Atlantis


### **Why the insistence on Atlantis and OpenID Connect (OIDC)?**


**Atlantis:**


Atlantis is an open-source Terraform Automation and Collaboration Software (TACOS) designed to automate and enhance the management of Terraform infrastructure within pull requests (PR). Some of the benefits of Atlantis include enhanced collaboration, automatic planning, locking state, logging and auditing, to name a few. Furthermore, our infrastructure is already being managed using Atlantis and switching to another software because it does not integrate with OpenID Connect (OIDC) is a great task. Instead, we found a way to generate and pass the short-lived temporary credentials securely.


**OpenID Connect (OIDC):**


OpenID Connect is a standardized protocol for proving an identity and sharing minimal, verifiable information about that identity. The key here is that the “identity” doesn’t have to be a person. It can be an individual user, an application, a service, or an ephemeral process (for example, a GitHub workflow).


Essentially, OIDC provides a secure, trust-based way for one system (the relying party, like AWS or Vault) to verify that a request is truly coming from a specific, trusted identity (like a GitHub Actions workflow).


Instead of relying on long-lived secrets such as passwords or access keys, OIDC uses a secure token. This token acts like a digital certificate of authenticity. It’s signed and issued by an identity provider (like GitHub) and contains claims (verified attributes) about the identity, such as:


- Who the identity is: e.g., “This request is from the main branch of the my-org/my-repo repository.”
- When the identity was verified: e.g., “This token was issued at 12:30 PM.”
- Who issued it: e.g., “This token was issued by GitHub.”


The relying party (AWS/Vault in our case) validates the token’s signature and claims. If everything checks out, it grants access or provides credentials to the identity, but only for a very limited time.


When OIDC is set up between Vault/AWS and GitHub, your GitHub Action’s workflow identity is acting as the SSO, not your personal GitHub login. The workflow is given a unique, temporary identity that the external service (AWS or Vault) trusts.


This allows the workflow to authenticate and obtain credentials without storing any long-lived secrets in GitHub. The process works like this:


Press enter or click to view image in full size


1. A GitHub Actions workflow is triggered.
2. GitHub creates a secure, verifiable token for that specific workflow run, which contains information like the repository name and the branch.
3. The workflow sends this token to AWS or Vault.
4. AWS or Vault is pre-configured to trust tokens coming from your specific GitHub repository. It validates the token’s signature and the claims within it (e.g., that it came from your repo).
5. If the token is valid, AWS or Vault issues temporary credentials to the workflow.
6. The workflow then uses those short-lived credentials to perform its tasks (e.g., upload the AWS credentials to Hashicorp Vault for use by Terraform).


The OIDC authentication is based on the machine identity of the workflow run and not your user account. This is a key security feature that ensures secrets are granted to a specific, temporary process, rather than to a person.


### **Tools needed:**


- Atlantis
- AWS
- OIDC — AWS
- OIDC — Vault
- Github actions
- Terraform


### **Steps:**


1. Create a PR or comment atlantis plan or atlantis apply on an existing PR
2. Trigger a GitHub workflow to refresh the AWS credentials in Vault
3. Retrieve Temporary AWS credentials and upload to Vault
4. Run Terraform using credentials in Vault


### **Prerequisites:**


- ATLANTIS_GH_TOKEN — A token to be used by Atlantis to connect to GitHub
- $TF_VAR_vault_token — A token used by Atlantis and Terraform to authenticate with Vault
- OIDC configured between AWS and GitHub — Used to generate temporary AWS credentials
- OIDC configured between Harshicorp Vault and GitHub — Used to generate temporary Vault token
- vault-cli and aws-cli are installed on both Atlantis and your GitHub action runner; otherwise, you can add the installation to your workflow. Installing when running the workflow will add to the time it takes to generate a new AWS credential.
- The Atlantis webhook is configured in GitHub


### **Flow Diagram:**


Press enter or click to view image in full size


### **Components:**


**Github PR:**


## Get Team Tucows’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


This is where we send commands to Atlantis, such as


```text
atlantis plan  atlantis apply
```


**Github workflow:**


This workflow is triggered from Atlantis when there are no AWS credentials or the available AWS credentials are invalid/expired. Its job is to authenticate with AWS using OIDC, generate a new token, and upload the token to Vault


Code:


```text
name: Get temporary aws credentials and send to vault  run-name: Get temporary aws credentials and send to vault  on:  workflow_dispatch:  permissions:  id-token: write # To request the JWT  contents: read # To request the JWT  env:  run_dc: us-east-1  vault_url: <vault url>  vault_namespace: <vault namespace name>  jobs:  retrieve_aws_creds_example:  runs-on: [ self-hosted, <runner name> ]  name: retrieve_aws_creds_example  env:  # github repo secrets  GITHUB_USER: ${{ github.actor }}  GITHUB_TOKEN: ${{ github.token }}  steps:  # To use this, you need to have set up OIDC between Github and Vault (JWT token)  - name: "Fetch vault token"  uses: hashicorp/vault-action@v2.7.4  id: vault-cert  with:  method: jwt  url: ${{ env.vault_url }}  namespace: ${{ env.vault_namespace }}  caCertificate: ${{ secrets.<cert_for_vault_jwt> }} # this needs to be added in github secrets  role: <vault jwt role name>  path: <your jwt method name>  exportToken: true  outputToken: true  - name: Check out repository code  uses: actions/checkout@v4  # To use this, you need to set up OIDC between Github and AWS  - name: configure-aws-credentials-oidc  uses: aws-actions/configure-aws-credentials@v4  with:  role-to-assume: <aws role to assume>  role-session-name: <role session name>  aws-region: ${{ env.run_dc }}  - name: configure-aws-credentials-bot-role-lab  uses: aws-actions/configure-aws-credentials@v4  with:  role-to-assume: <aws role to assume>  role-session-name: <role session name>  aws-region: ${{ env.run_dc }}  role-chaining: true  # Upload the aws credentials to vault  - name: Update Vault secret - Lab  env:  VAULT_ADDR: ${{ env.vault_url }}  VAULT_TOKEN: ${{ steps.vault-cert.outputs.vault_token }}  VAULT_NAMESPACE: ${{ env.vault_namespace }}  AWS_ACCESS_KEY_ID: ${{ env.AWS_ACCESS_KEY_ID }}  AWS_SECRET_ACCESS_KEY: ${{ env.AWS_SECRET_ACCESS_KEY }}  AWS_SESSION_TOKEN: ${{ env.AWS_SESSION_TOKEN }}  LOCATION: lab  run: |  sh ./repo_scripts/update_secret.sh
```


**Atlantis:**


Atlantis manages the Terraform code. When a PR is created or any of the following commands is used, Atlantis is triggered using a webhook.


```text
atlantis plan  atlantis apply
```


Code:


```text
version: 3  automerge: true  projects:  # LAB  - name: lab-example  workflow: lab-example  dir: aws-infra  terraform_version: v1.9.8  apply_requirements: [mergeable]  autoplan:  when_modified: [  "./*.tf",  "./environments/lab-example.tfvars"  ]  workflows:  # LAB  lab-example:  plan:  steps:  - run: rm -rf .terraform  - run: LOCATION=lab sh ../repo_scripts/aws_credential_manager.sh  - init:  extra_args:  ["-backend-config=key=<path to terraform state in s3 bucket>/terraform_state"]  - plan:  extra_args:  [  "-var-file=./environments/example.tfvars"  ]  apply:  steps:  - run: LOCATION=lab sh ../repo_scripts/aws_credential_manager.sh  - apply
```


**Terraform:**


Terraform is the infrastructure-as-code (IaC) tool used to manage our AWS infrastructure. Below is the code needed to integrate the AWS provider while using


lab-example.tfvars


```text
vault_namespace = "<vault namespace name>"  run_dc = "us-east-1"  run_stage = "lab-example"
```


base.tf


```text
# Terraform needs to connect to Vault  provider "vault" {  token = var.vault_token  #address is set in atlantis as VAULT_ADDR  namespace = var.vault_namespace  skip_tls_verify = true  }  # passing the temporary credentials to the aws provider  provider "aws" {  region = var.run_dc  access_key = data.vault_generic_secret.aws_creds.data.AWS_ACCESS_KEY_ID  secret_key = data.vault_generic_secret.aws_creds.data.AWS_SECRET_ACCESS_KEY  token = data.vault_generic_secret.aws_creds.data.AWS_SESSION_TOKEN  }  # Obtaining the temporary AWS credentials from vault  locals {  environment_configs = {  "lab-example" = {  aws_creds_path = "<credentials path>" } }  }  current_env_config = lookup(  local.environment_configs, var.run_stage,{  aws_creds_path = "aws/default",  eip_state_key = "wavelo-e2e-c-terraform/default/eip/placeholder"  }  )  }  data "vault_generic_secret" "aws_creds" {  path = local.current_env_config.aws_creds_path  }
```


**What glues all these together? Scripts:**


Before atlantis plan or apply is completed, a series of pre-initiation steps are carried out to ensure Terraform gets fresh working AWS temporary credentials. These steps start with the aws_credential_manager.sh script.


```text
steps:     - run: LOCATION=lab sh ../repo_scripts/aws_credential_manager.sh
```


The job of the aws_credential_manager.sh script is to determine if the AWS credentials in Vault are valid; if they are, the atlantis init, plan, or apply steps are carried out. If not, it runs the aws_credential_refresh.sh script that triggers a workflow dispatch to run the GitHub action workflow in GitHub. Thereafter, it waits and validates that the new credentials are working before handing over to the next Atlantis steps.


**aws_credential_refresh.sh**


```text
#!/bin/sh  # Set variables  GITHUB_REPO="<repo where github action workflow is located>"  GITHUB_TOKEN="$ATLANTIS_GH_TOKEN"  # Use the current branch from Atlantis environment  REF="$HEAD_BRANCH_NAME"  WORKFLOW_FILENAME="refresh_aws_credentials_in_vault.yml"   # Validate that REF is set  if [ -z "$REF" ]; then    echo "ERROR: HEAD_BRANCH_NAME environment variable is not set"    exit 1  fi   echo "Triggering workflow '$WORKFLOW_FILENAME' on branch: $REF"   # dispatch the github action  curl -L \    -X POST \    -H "Accept: application/vnd.github+json" \    -H "Authorization: Bearer $GITHUB_TOKEN" \    -H "X-GitHub-Api-Version: 2022-11-28" \    https://api.github.com/repos/$GITHUB_REPO/actions/workflows/$WORKFLOW_FILENAME/dispatches \    -d "{\"ref\":\"$REF\"}"  RTN=$?  # Check if curl command was successful  if [ $RTN -ne 0 ]; then    echo "ERROR: AWS credential refresh workflow was not triggered"    exit 1  fi   echo "Successfully triggered credential refresh workflow!"
```


**update_secret.sh**


If the AWS credentials are invalid or do not exist in vault, and the GitHub action is triggered, it uses the update_secret.sh script to write the credentials into vault.


```text
# upload secrets to vault  echo "Writing credentials to $LOCATION vault..."  vault kv put -namespace=$VAULT_NAMESPACE <credentials path> \  AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \  AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \  AWS_SESSION_TOKEN=$AWS_SESSION_TOKEN  VAULT_RTN=$?   # test the credentials  # Confirm that the secrets were successfully uploaded  if [ $VAULT_RTN = 0 ]; then    echo "SUCCESS: $LOCATION credentials successfully written"  else    echo "ERROR: $LOCATION credentials could not be written"    exit 1  fi   # Test the newly updated secret  # pull new secret from vault and add to env variables  export AWS_ACCESS_KEY_ID=$(vault kv get -field=AWS_ACCESS_KEY_ID -namespace=$VAULT_NAMESPACE <credentials path>)  export AWS_SECRET_ACCESS_KEY=$(vault kv get -field=AWS_SECRET_ACCESS_KEY -namespace=$VAULT_NAMESPACE <credentials path>)  export AWS_SESSION_TOKEN=$(vault kv get -field=AWS_SESSION_TOKEN -namespace=$VAULT_NAMESPACE <credentials path>)   # test aws credentials  echo "Testing New AWS Credentials"  AWS_RESPONSE=$(aws sts get-caller-identity 2>&1)  AWS_RTN=$?
```


**Conclusion:**


This article details a robust integration of GitHub Actions, Atlantis, AWS, and Vault. By leveraging OpenID Connect (OIDC) and custom scripts, the workflow securely passes temporary, short-lived AWS credentials directly to Atlantis, enabling it to perform Terraform provisioning without using static secrets.


**References:**


[https://www.runatlantis.io/](https://www.runatlantis.io/)


[https://spacelift.io/blog/atlantis-terraform-tutorial](https://spacelift.io/blog/atlantis-terraform-tutorial)


[https://www.pingidentity.com/en/openid-connect.html](https://www.pingidentity.com/en/openid-connect.html)


Press enter or click to view image in full size


## About Tucows


We do a lot, but at our core, we’re in the business of keeping people connected and keeping the Internet open. We’re made up of three companies: Tucows Domains, Ting, and Wavelo.


- As[Tucows Domains](http://tucowsdomain.com/) , we help people find their place online as the world’s largest domain name wholesaler and the third-largest domain registrar globally.
- As[Ting Internet](http://tinginternet.com/) , we deliver high-speed fiber internet service to communities across the United States.
- As[Wavelo](http://wavelo.com/) , we believe the future of telecom is event driven. We build telecom billing and operations software for Mobile Virtual Network Operators and Fiber Internet Services.


#JoinTheHerd at[https://www.tucows.com/careers/](https://www.tucows.com/careers/overview)
