---
schema_version: "1.0.0"
document_id: "f9947bd4965808364450ad5f7a54e50d0d371240b464340e0c4afeaa2a5061b3"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/dr-investigating-aws-persistence-mechanisms"
published_at: "2026-07-15T13:00:00+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:8adb5ada2fd2943f8f0dd16db1dac333bf4d6bc40732da79443278f82a220327"
---

# Investigating Persistence Mechanisms in AWS

## Overview


In the cloud, your infrastructure may be short-lived, but an attacker’s persistence doesn't have to be. While your environment scales and changes in seconds, adversaries are embedding themselves into your IAM policies, Lambda functions, and federated sessions, creating invisible footholds that survive long after you believe an incident is closed.


Persistence in AWS is not just a technical oversight; it is a fundamental business risk. If you cannot see how an attacker has rooted themselves in your environment, you cannot contain them. This article moves beyond theory to provide the critical detection logic, investigation workflows, and actionable response steps required to hunt down hidden persistence and reclaim your AWS environment. This reference enables Rapid7[Incident Command](https://www.rapid7.com/products/siem) customers to investigate and understand AWS alert behaviors.


## Persistence technique: IAM user


One of the most common persistence techniques is maintaining access by creating or modifying Identity and Access Management (IAM) users. An attacker can issue the


iam:CreateUser


API call to create a new IAM user. In addition to establishing persistence, threat actors may use this API call to create a separate user for each collaborator, allowing them to divide work and perform activities independently.


During incident investigations, we have observed that malicious


iam:CreateUser


actions are usually simple and often include only the


userName


of the newly created user. Example request and response parameters for this API call are shown in Listing 1, where an attacker creates a new IAM user named


malicious-user


*.*


```text
"requestParameters": {
"userName": "malicious-user"
},
"responseElements": {
"user": {
"path": "/",
"userName": "malicious-user",
"userId": "AIDAS7R4L4RPRYBWCIXXX",
"arn": "arn:aws:iam::123456789012:user/malicious-user",
"createDate": "Mar 9, 2026, 9:16:35 AM"
}
},
```


*Listing 1: Example request and response parameters of the*


*iam:CreateUser*


*API call*


**


Creating an IAM user does not, by itself, provide threat actors with a particularly effective persistence mechanism, because the newly created user has no credentials for authentication and no identity-based policies assigned. Therefore, several follow-up actions usually occur. These actions typically focus on adding credentials and assigning permissions to the newly created user. Specific examples include:


#### Credential addition:


-


iam:CreateAccessKey


— Creates a long-term credential for the target IAM user. This may also be used for lateral movement when the source user differs from the target user.


-


iam:CreateConsoleProfile


****


— Creates credentials that allow the user to authenticate through the AWS Console interface. Like the previous API call, this may also be used for lateral movement when performed on a different IAM user.


#### Permission addition:


-


iam:AttachUserPolicy


— Attaches the specified managed policy to the user.


-


iam:PutUserPolicy


— Adds or updates an inline policy document embedded in the specified IAM user.


-


iam:AddUserToGroup


— Adds the user to the specified group.


All of these API calls use standardized request parameters, which makes it possible to investigate actions performed on the newly created user with the following LEQL query:


```text
where(service="cloudtrail" and source_json.requestParameters.userName = "malicious-user")
```


*Listing 2: LEQL query for investigating actions performed on an IAM user*


**


Excluding the source user who originally created the malicious IAM user can help reveal other compromised accounts involved in the activity.


To get an overview of the most important actions performed on the malicious entity, the following query can be used:


```text
where(service="cloudtrail" and source_json.requestParameters.userName = "malicious-user" and not source_json.eventName ISTARTS-WITH-ANY ["Get", "List", "Describe"] and source_json.errorCode != /.+/)groupby(source_json.userIdentity.arn, source_json.eventName)
```


*Listing 3: LEQL query to get an overview of the most important actions performed on the user*


**


The query in Listing 3 displays a table of successful actions performed by user identities targeting the compromised user. It filters out common read operations that may occur regularly in the environment and also excludes unsuccessful actions.


Incident Command parses the source user into a separate field, which makes it easy to examine all actions performed by IAM users. To get a list of actions performed by the newly created IAM user, the following LEQL query can be used:


```text
where(service="cloudtrail" and source_account = "malicious-user")groupby(source_json.eventName)
```


*Listing 4: LEQL query for actions performed by the user*


### Recommended steps for newly created IAM users


When investigating and remediating persistence involving newly created IAM users, Rapid7 recommends the following steps:


-


Review the actions performed by both the newly created IAM user and the user that initiated its creation to understand the scope and intent of the activity.


-


Examine authentication activity for unusual locations or patterns, and identify any additional resources that may have been accessed by the same threat actor.


-


Where possible, apply a deny-all IAM policy to all compromised entities to immediately prevent further malicious actions.


-


Rotate credentials for all compromised accounts to prevent further unauthorized access.


-


Remove any unknown or unauthorized IAM users to fully remediate persistence.


## Persistence technique: Modifying assume role policies


An IAM role is an entity that has specific permissions that can be assumed to whoever needs it and has necessary permissions to do so. Roles are intended to provide access to resources to users, applications, and services that normally don’t have access to the required AWS resources. Unlike IAM users, roles do not have long-term access keys so they provide only short-term credentials when they are assumed.


During an attack, threat actors can establish persistence by modifying a role's assume role policy. By altering this policy, they can allow users from an attacker-controlled AWS account to assume the role within the victim’s account.This form of persistence can be achieved by creating a fresh new role using


iam:CreateRole


with already backdoored assume role policy, or via editing an assume role policy that already exists using


iam:UpdateAssumeRolePolicy


API call. Listing 5 shows an example of an assumed role policy document that allows access from external AWS accounts.


```text
{
"Version": "2012-10-17",
"Id": "...",
"Statement": [
{
"Sid": "Statement1",
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam::111111111111:root"
},
"Action": "sts:AssumeRole"
},
{
"Sid": "Statement2",
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam::222222222222:root"
},
"Action": "sts:AssumeRole"
}
]
}


```


*Listing 5: Assume role policy allows external access*


The document contains two external account IDs,


111111111111


and


222222222222


, and allows anyone with necessary permissions in the attacker's account to assume the role.


In addition to investigating the user who performed the action to confirm its compromise, there are additional queries that could reveal other potentially malicious activity. The LEQL query in Listing 6 shows all actions performed on the


malicious-role


that has a suspicious assume role policy statement. The query also filters our common noise in AWS environments.


```text
where(service = "cloudtrail" and source_json.requestParameters.roleName = "malicious-role" and not source_json.userIdentity.invokedBy IIN ["resource-explorer-2.amazonaws.com", "access-analyzer.amazonaws.com"])
```


*Listing 6: LEQL query to show actions performed on the suspicious role*


When this persistence technique is observed, it’s recommended to search for activity originating from malicious accounts. When


iam:AssumeRole


action is observed, the returned temporary key can be extracted and its associated activity can be further examined.


```text
where(service = "cloudtrail" and source_json.userIdentity.accountId IN ["111111111111", "222222222222"])
```


*Listing 7: LEQL query showing actions from the suspicious AWS accounts*


Also, it’s recommended to search for other potentially backdoored policies that may have been created within the environment. The LEQL query in Listing 8 shows a table of principal IDs that wrote the previously identified malicious AWS accounts into specific roles.


```text
where(service = "cloudtrail"  and source_json.eventName IIN ["CreateRole", "UpdateAssumeRolePolicy"] and source_json.eventSource = NOCASE("iam.amazonaws.com") and source_json.requestParameters.assumeRolePolicy, source_json.requestParameters.policyDocument ICONTAINS-ANY ["111111111111", "222222222222"])groupby(source_json.userIdentity.principalId, source_json.requestParameters.roleName)
```


*Listing 8: LEQL query showing roles with assume role referring to the suspicious AWS accounts*


## Persistence technique: Lambda abuse


AWS Lambda is a serverless compute service that allows users to execute code without managing servers. Lambda functions contain code that can be triggered by various AWS services, such as API Gateway, CodeCommit, Config, and others.


Threat actors may abuse Lambda functions to upload malicious code that maintains access to the environment when invoked. The code inside a Lambda function can perform any operation, as long as the function has the necessary permissions assigned to it. However, a common malicious use case is provisioning new privileged IAM users.


```text
import string
import boto3
import uuid
import json
import random


def lambda_handler(event, context):
iam = boto3.client('iam')


user_name = f"user-{uuid.uuid4().hex[:8]}"
password = ''.join(random.choices(string.ascii_letters + string.digits + string.punctuation, k=10))


try:
response = iam.create_user(UserName=user_name)
print(f"User {user_name} created successfully")


iam.create_login_profile(
UserName=user_name,
Password=password,
PasswordResetRequired=False
)


iam.attach_user_policy(
UserName=user_name,
PolicyArn='arn:aws:iam::aws:policy/AdministratorAccess'
)


account_id = context.invoked_function_arn.split(":")[4]
iam_login_url = f"https://{account_id}.signin.aws.amazon.com/console"


return {
'statusCode': 200,
'body': json.dumps({
'message': f'User {user_name} created successfully',
'login_url': iam_login_url,
'username': user_name,
'password': password
})
}
except Exception as e:
return {
'statusCode': 500,
'body': json.dumps({'error': error_message})
}
```


*Listing 9: Backdoor Python Lambda code*


The code in Listing 9 creates a new IAM user with a login profile and attaches the AdministratorAccess policy to it. The login credentials are returned to the attacker in the response from the Lambda function. To execute, the Lambda function must be triggered. Threat actors may create various triggers depending on how the malicious code operates. In scenarios like the example above, the Lambda function is usually assigned a public URL that a threat actor can call to invoke it.


One way the function can be invoked via a public URL is by using the


lambda:CreateFunctionUrlConfig


and


lambda:AddPermission


sequence. The


lambda:CreateFunctionUrlConfig


API call takes the function name as an argument and returns the function URL. This URL can then be used by threat actors to invoke the function. The second API call,


*lambda:AddPermission*


, assigns permission that allows the function to be invoked from the URL.


```text
"requestParameters": {
"functionName": "backdoor_function",
"authType": "NONE",
"cors": {
"allowHeaders": [
"*"
],
"allowMethods": [
"GET",
"POST"
],
"allowOrigins": [
"*"
]
}
},
"responseElements": {
"functionUrl": "https://uniqueaddress.lambda-url.us-east-1.on.aws/",
"functionArn": "arn:aws:lambda:us-east-1:123456789012:function:backdoor_function",
"authType": "NONE",
"cors": {
"allowHeaders": [
"*"
],
"allowMethods": [
"GET",
"POST"
],
"allowOrigins": [
"*"
]
}
}
```


*Listing 10: Example request and response elements of the* lambda:CreateFunctionUrlConfig


*function* ** Another way to trigger a Lambda function via a URL is to create an API Gateway endpoint and use


apigateway:CreateIntegration


or


apigateway:PutIntegration


to set the destination to a Lambda function. The action logged in Listing 10 creates an integration to trigger version 1 of a Lambda function named


backdoor_lambda_function


. When investigating, it is important to check the content of the version of the Lambda function being triggered, as there may be legitimate-looking code in later versions used to hide malicious code.


```text
"eventSource": "apigateway.amazonaws.com",
"eventName": "CreateIntegration",
"awsRegion": "us-east-1",
"requestParameters": {
"integrationMethod": "GET",
"integrationType": "AWS_PROXY",
"payloadFormatVersion": "2.0",
"integrationUri": "arn:aws:lambda:us-east-1:123456789012:function:backdoor_lambda_function:1",
"apiId": "xxxxxxx"
},
```


*Listing 11: Part of* apigateway:CreateIntegration


*CloudTrail log*


There are various other ways the backdoor function may be implemented. For example, threat actors may use


events:PutRule


to set up event-driven execution and then use


events:PutTargets


to assign the Lambda function as a target. The function may then establish a backdoor and send credentials to attacker-controlled C2 servers.


### Suspicious Lambda function activity: Next steps


This section contains recommended actions and investigation steps to take whenever Incident Command highlights activity originating from a Lambda function as suspicious. During investigations, focus on answering the following questions:


-


Is the Lambda function known and authorized?


-


What code invoked the suspicious activity?


-


Who created the Lambda function?


-


How was the Lambda function triggered?


-


What actions were performed by the function?


The LEQL query shown in Listing 12 provides an example that displays successful actions performed by a Lambda function named


malicious-function


, grouped by event source.


```text
where(service = "cloudtrail" and source_json.userIdentity.arn ICONTAINS "/malicious-function" and source_json.errorCode != /.+/)groupby(source_json.eventSource, source_json.eventName)
```


*Listing 12: LEQL query showing an overview of actions performed by the Lambda function*


Malicious activity performed by Lambda functions can originate from malicious code within the function or from the exploitation of a legitimate application. If malicious code is identified, the user who inserted it is likely to be compromised as well. The query in Listing 13 displays principal IDs and their associated API calls affecting the Lambda function, including the techniques described in this section and function invocation events (


lambda:Invoke


API call).


```text
where(service = "cloudtrail" and source_json.requestParameters.functionName,source_json.requestParameters.putIntegrationInput.uri, source_json.requestParameters.integrationUri, source_json.requestParameters.targets.arn ICONTAINS "malicious-function" and not source_json.userIdentity.invokedBy IIN ["resource-explorer-2.amazonaws.com", "config.amazonaws.com"])groupby(source_json.userIdentity.principalId, source_json.eventSource, source_json.eventName)
```


*Listing 13: LEQL query showing actions performed on the Lambda function*


## Persistence technique: Federated user session creation


Threat actors may use the Security Token Service (STS) API call to create a federated user session and maintain access to an AWS environment even after some standard containment actions have been completed. GetFederationToken returns a set of temporary security credentials for a federated user principal. The API call must be made using long-term IAM user credentials, which means activity from a federated user should always be investigated together with the IAM user that created the session.


This technique is especially important during incident response because disabling or deleting the original access key does not automatically invalidate temporary credentials that have already been issued. Those credentials remain usable until they expire, unless their effective permissions are blocked. As a result, responders should treat the federated session as a separate active identity and investigate both the session activity and the source IAM user activity.


The effective permissions of a federated user are based on the permissions available to the IAM user that requested the token and any session policies passed in the


GetFederationToken


request. A session policy cannot grant permissions that the source IAM user does not already have. However, if the compromised IAM user is highly privileged, the resulting federated session may still provide broad access to the environment.


When Incident Command alerts on suspicious activity performed by a federated user, the userIdentity field in CloudTrail may look similar to the example below:


```text
"userIdentity": {
"type": "FederatedUser",
"principalId": "123456789012:None",
"arn": "arn:aws:sts::123456789012:federated-user/None",
"accountId": "123456789012",
"accessKeyId": "ASIAS8T6L4RPJJGXXXX",
"sessionContext": {
"sessionIssuer": {
"type": "IAMUser",
"principalId": "AIDAIT67N6AB4IH6XXXXX",
"arn": "arn:aws:iam::123456789012:user/compromisedUser",
"accountId": "123456789012",
"userName": "compromised_user"
},
"attributes": {
"creationDate": "2026-04-11T09:13:11Z",
"mfaAuthenticated": "false"
}
}
},
```


Listing 14: userIdentity


field of an event performed by a federated user


In this example, the federated user name is


None


, which comes from the name parameter supplied to STS. The


sessionContext.sessionIssuer


field identifies the IAM user that created the federated session. This is the most important pivot point during the investigation because the source IAM user is likely to be compromised.


To review successful actions performed by the federated user, defenders can use the following LEQL query:


```text
where(service = "cloudtrail" and source_json.userIdentity.arn = "arn:aws:sts::123456789012:federated-user/None" and source_json.errorCode != /.+/)groupby(source_json.eventSource, source_json.eventName)
```


*Listing 15: LEQL query showing all successful actions performed by the federated user*


To focus on higher-signal activity, defenders can exclude common enumeration actions:


```text
where(service = "cloudtrail" and source_json.userIdentity.arn = "arn:aws:sts::123456789012:federated-user/None" and source_json.errorCode != /.+/ and not source_json.eventName ISTARTS-WITH-ANY ["Get", "List", "Describe"])groupby(source_json.eventSource, source_json.eventName)
```


*Listing 16: LEQL query showing successful non-enumeration actions performed by the federated user*


When reviewing actions performed by federated users, pay close attention to activity involving IAM, CloudTrail, GuardDuty, Organizations, KMS, Secrets Manager, S3, Lambda, and EC2. IAM activity is particularly important. Federated user credentials cannot call IAM APIs via AWS CLI and AWS API, but this limitation does not apply to AWS Management Console sessions. Therefore, successful IAM activity associated with a federated user may indicate that the threat actor generated console access by using the


signin:GetSigninToken


and


signin:ConsoleLogin


API sequence.


Defenders can review


sts:GetFederationToken


calls to review federated tokens creations performed by the source user. The API calls may be further scoped down by adding


source_json.responseElements.credentials.accessKeyId = “malicious_access_key”


, which will display the exact API call that was used to obtain the temporary token. This may be useful when determining Initial Access Vector, as the API call may contain the initially leaked long-term credentials.


```text
where(service = "cloudtrail" and action = "GetFederationToken" and source_json.eventSource = "sts.amazonaws.com" and source_json.requestParameters.name = "None" and source_json.userIdentity.userName = "compromised_user")
```


*Listing 17: LEQL query showing the* GetFederationToken


*event that created the federated user credentials*


During the investigation, responders should focus on answering the following questions:


-


Which IAM user created the federated session?


-


What actions did the federated user perform after the token was issued?


-


Did the actor use the federated session to access the AWS Management Console?


-


Did the federated user create or modify additional persistence mechanisms?


-


What other suspicious activities were performed?


When compromise is confirmed, Rapid7 recommends the following steps:


-


Apply a deny-all policy to the IAM user that created the federated session. Keep the deny in place until the federated credentials have expired.


-


Rotate or delete all affected access keys associated with the compromised IAM user.


-


Remove any additional persistence that might have been created.


## Summary


AWS persistence often relies on abusing legitimate identity and automation features such as IAM users, access keys, assume role policies, Lambda functions, and federated user sessions. Many malicious activities are made possible by overly permissive policies, so organizations should regularly review IAM permissions, trust policies, and resource-based policies, and use Service Control Policies to enforce preventative guardrails across AWS accounts.


Effective detection and response requires pivoting from the alerted activity to related identities, credentials, sessions, policies, and resources to determine whether additional persistence exists.


[Rapid7 MDR](https://www.rapid7.com/services/managed-detection-and-response-mdr/) provides comprehensive detection and incident response services to help organizations identify suspicious AWS activity, contain compromised identities, and harden cloud environments against repeat abuse.
