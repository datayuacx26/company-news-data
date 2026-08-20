---
schema_version: "1.0.0"
document_id: "70eb47b5f5b75c2d48481ed45218a46d3afcc648cac0ecd35568646749a67384"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/secure-aws-account-iam-setup/"
published_at: "2017-09-20T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:d9b865cccc97114dd7252e35ee8b20341e7fa5facd1aed2d74ed6c45d4e944d0"
---

# Secure (and usable) multi-AWS account IAM setup

Alexis Le-Quoc


## From one to many: Account sprawl


With an email and a credit card anyone can sign up for AWS. And everyone did to the point that, if you are part of the team managing the AWS infrastructure at your organization, you’ve had to wrestle with this for some time now. Multiple accounts, even when you have assembled the definitive list of what’s in use by your colleagues, create complexity in many areas: billing, compliance, security. It is through the lens of security that we are looking at managing multiple accounts, with the hope to provide a pattern that you can apply or tweak to your own case.


## The benefits of multiple AWS Accounts


While having multiple AWS accounts adds operational complexity, especially when they are managed in an ad hoc way, the principle is defensible from a security standpoint. This model offers multiple natural security boundaries and isolation between your workloads.


1.


At the network level, accounts are isolated from one another unless their VPCs are explicitly peered.


2.


At the API level, a compromised account has by default no access to other accounts, unless, again, API calls have been explicitly allowed with role delegation.


3.


At the compute level, if one account gets turned into a bitcoin-mining field, billing alerts or careful CloudTrail monitoring will alert you right away. And you can set per-account spend limits that make taking over a limited account, a much less palatable target.


From a security standpoint all of this is appealing, which leads to the question: how to make it work without adding significant overhead with each new account that is added to the mix.


Caveat lector: what’s about to be presented will only work with up to 10 different accounts, based on current limitations in IAM groups. It will thus apply to production environments much more so than to development environments—especially if every single developer gets her own account.


## The qualities of a secure and usable setup


There is plenty of detailed documentation on AWS Identity and Access Management (IAM), but very limited guidance on how to put everything together. The[AWS IAM best practices documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) only covers very general topics e.g. “never have root API credentials” so they are good guidelines but not enough to come up with a design.


To help come up with a solution, we are spelling out a number of requirements which we will then figure out how to implement.


1.


IAM users can only exist in one account.


2.


By default IAM users have very limited API access to any account.


3.


Access to privileged API calls requires a user to assume a role in the target account.


4.


To assume a role, a user needs an active Multi-factor Authentication (MFA) with a limited time-to-live.


5.


Privileged API calls are grouped together by topic, e.g. VPC, DNS, IAM.


Let’s now look at each requirement and its rationale in detail.


### IAM users can only exist in one account


This simply keeps user management tractable. When someone joins and needs access to the AWS infrastructure, or when someone leaves, you want to only have to look into one spot. By the same token, if you need to apply password complexity rules, you’re in much better spot if you need only apply them in one account.


Conversely, by having IAM users only in one account, you can monitor all other accounts and alert if an IAM user gets created there. It should simply never happen.


### Why have IAM users at all?


Managing users independently in AWS IAM sounds error prone and dangerous. Why would an administrator not choose to use Single Sign On (SSO) and their existing central identity provider? The primary reason not to use SSO is because of one very important tradeoff. When an AWS account uses SSO for authentication it loses the ability to protect tasks with MFA. Much of this strategy requires that the administrator is able to use MFA protection with fine grained precision.


### By default IAM users have very limited API access to any account


By “very limited” we mean that a user should be able to:


1.


Look at her own record.


2.


Update her console password.


3.


Create, update or delete her API credentials.


4.


Create her own multi-factor authentication device.


And that’s it. So a user in the main account only allows authentication self-service but cannot do much beyond that. This helps limit the “blast radius” if the user’s credentials are compromised (as long as the MFA is not also compromised): an attacker with stolen username/password could at most look at the IAM user record and any attempt to user API credentials would trigger` 401 Unauthorized` responses from AWS’ APIs.


### Access to privileged API calls requires a user to assume a role in the target account


This is how a user can actually get something done in AWS. Since the user operates under a very restrictive policy by default, she needs to (temporarily) elevate her privileges in a controlled fashion. This pattern is very similar to sudo in Unix land: she requests from the API elevated privileges by calling` sts:AssumeRole` with her own API credentials. If the call succeeds, she gets temporary API credentials and a session token to call the API and do something useful.


These credentials have a short time-to-live (maximum 1 hour by default) so even if they get lost, the time window to exploit them is short.


### To assume any role, a user needs an active MFA with a limited time-to-live


This again, follows the sudo pattern, but rather than expecting the user to re-enter her unix password, we force an MFA to be present for the` sts:AssumeRole` to even have a chance to succeed. Thus the 2 conditions for privilege escalations are:


1.


User has an active MFA and, of course the MFA with her.


2.


User has the right to assume the role she wants to assume (spoiler alert: this will be set by a group policy).


The MFA requirement, like any security measure, does not make an exploit impossible, just more expensive. It limits the damage that misplaced API credentials could have since any user API credentials without the MFA only provide read-only access to the user’s IAM record.


### Privileged API calls are grouped together by topic, e.g. VPC, DNS, IAM


This is the final piece: to keep mapping users to activities reasonably easy to manage, we group them by scope. For instance, let’s assume you want a group of people to manage the network and DNS but not EC2 instances, and vice-versa, and a third group to manage S3. In this case you create 3 roles:


1.


A` vpc` role whose policy allows network configuration to be changed.


2.


An` ec2` role whose policy allows all EC2 calls except VPC.


3.


An` s3` role whose policy allows all S3 calls.


## Tying it all together: an example


Now that we have all the requirements spelled out, we can get into the details of the implementation. The design is simple and relies solely on simple IAM constructs: users, groups and roles.


Let’s assume that you need 3 accounts named A, B and M and let’s start with 3 users, Alice, Bob and Charlie.


1.


Alice is the network engineer responsible for managing VPCs across both accounts.


2.


Bob is a developer who needs control over B’s EC2 infrastructure only (excluding VPC).


3.


Charlie is an SRE who needs full administrative access over A and B.


At this point you also have full administrative access to all accounts, which you’ll be able to relinquish when you are done.


### Step 1: create a users group in the M(anagement) account


That` users` group will host all users and needs to have a policy that lets its members look at their own record and update their credentials and control their MFA, nothing more.


Here is an example of policy,` users.json`


```text
1   {    2         "Version"  :   "2012-10-17"  ,    3         "Statement"  : [    4              {    5                 "Sid"  :   "AllowUsersAllActionsForCredentials"  ,    6                 "Effect"  :   "Allow"  ,    7                 "Action"  : [    8                     "iam:ListAttachedUserPolicies"  ,    9                     "iam:GenerateServiceLastAccessedDetails"  ,    10                     "iam:*LoginProfile"  ,    11                     "iam:*AccessKey*"  ,    12                     "iam:*SigningCertificate*"    13                  ],    14                 "Resource"  : [    15                     "arn:aws:iam::__M__:user/${aws:username}"    16                  ]    17              },    18              {    19                 "Sid"  :   "AllowUsersToSeeStatsOnIAMConsoleDashboard"  ,    20                 "Effect"  :   "Allow"  ,    21                 "Action"  : [    22                     "iam:GetAccount*"  ,    23                     "iam:ListAccount*"    24                  ],    25                 "Resource"  : [    26                     "*"    27                  ]    28              },    29              {    30                 "Sid"  :   "AllowUsersToListUsersInConsole"  ,    31                 "Effect"  :   "Allow"  ,    32                 "Action"  : [    33                     "iam:ListUsers"    34                  ],    35                 "Resource"  : [    36                     "arn:aws:iam::__M__:user/*"    37                  ]    38              },    39              {    40                 "Sid"  :   "AllowUsersToListOwnGroupsInConsole"  ,    41                 "Effect"  :   "Allow"  ,    42                 "Action"  : [    43                     "iam:ListGroupsForUser"    44                  ],    45                 "Resource"  : [    46                     "arn:aws:iam::__M__:user/${aws:username}"    47                  ]    48              },    49              {    50                 "Sid"  :   "AllowUsersToCreateTheirOwnVirtualMFADevice"  ,    51                 "Effect"  :   "Allow"  ,    52                 "Action"  : [    53                     "iam:CreateVirtualMFADevice"  ,    54                     "iam:EnableMFADevice"  ,    55                     "iam:ResyncMFADevice"  ,    56                     "iam:DeleteVirtualMFADevice"    57                  ],    58                 "Resource"  : [    59                     "arn:aws:iam::__M__:mfa/${aws:username}"  ,    60                     "arn:aws:iam::__M__:user/${aws:username}"    61                  ]    62              },    63              {    64                 "Sid"  :   "AllowUsersToListVirtualMFADevices"  ,    65                 "Effect"  :   "Allow"  ,    66                 "Action"  : [    67                     "iam:ListMFADevices"  ,    68                     "iam:ListVirtualMFADevices"    69                  ],    70                 "Resource"  : [    71                     "arn:aws:iam::__M__:*"    72                  ]    73              }    74          ]    75   }
```


### Step 2: create Alice, Charlie and Bob in the M(anagement) account as regular IAM users


This part is easy:


1.


Create all 3 users in the` M` account


2.


Add all 3 to the` users` group


3.


Share the console credentials and instruct all 3 users to set up an MFA for their account.


### Step 3: create 3 roles in A and B accounts


For symmetry reasons it’s best to create the 3 roles in all accounts even if, in our example, no-one needs just EC2 access to the A account.


Thus you need to create a total of 6 roles:


1.


A` vpc` role in A and B, with a policy that gives control over VPC


2.


An` ec2` role in A and B, with a policy that gives control over EC2 minus VPC


3.


An` admin` role in A and B, with a policy that gives full control


Here are some example of policies that would make sense for each role:


-


Admin policy for the` admin` role


```text
1   {    2         "Version"  :   "2012-10-17"  ,    3         "Statement"  : [    4              {    5                 "Effect"  :   "Allow"  ,    6                 "Action"  :   "*"  ,    7                 "Resource"  :   "*"    8              }    9          ]    10   }
```


-


EC2 policy for the` ec2` role


```text
1   {    2         "Version"  :   "2012-10-17"  ,    3         "Statement"  : [    4          {    5           "Sid"  :   "AllowEC2"  ,    6           "Action"  : [    7             "ec2:*"    8            ],    9           "Effect"  :   "Allow"  ,    10           "Resource"  :   "*"    11          },    12          {    13           "Sid"  :   "DenyVPC"  ,    14           "Action"  : [    15             "ec2:AcceptVpcPeeringConnection"  ,    16             "ec2:AllocateAddress"  ,    17             "ec2:AssignPrivateIpAddresses"  ,    18             "ec2:AssociateAddress"  ,    19             "ec2:AssociateDhcpOptions"  ,    20             "ec2:AssociateRouteTable"  ,    21             "ec2:AttachClassicLinkVpc"  ,    22             "ec2:AttachInternetGateway"  ,    23             "ec2:AttachNetworkInterface"  ,    24             "ec2:AttachVpnGateway"  ,    25             "ec2:AuthorizeSecurityGroupEgress"  ,    26             "ec2:AuthorizeSecurityGroupIngress"  ,    27             "ec2:CreateCustomerGateway"  ,    28             "ec2:CreateDhcpOptions"  ,    29             "ec2:CreateFlowLogs"  ,    30             "ec2:CreateInternetGateway"  ,    31             "ec2:CreateNatGateway"  ,    32             "ec2:CreateNetworkAcl"  ,    33             "ec2:CreateNetworkAcl"  ,    34             "ec2:CreateNetworkAclEntry"  ,    35             "ec2:CreateNetworkInterface"  ,    36             "ec2:CreateRoute"  ,    37             "ec2:CreateRouteTable"  ,    38             "ec2:CreateSecurityGroup"  ,    39             "ec2:CreateSubnet"  ,    40             "ec2:CreateVpc"  ,    41             "ec2:CreateVpcEndpoint"  ,    42             "ec2:CreateVpcPeeringConnection"  ,    43             "ec2:CreateVpnConnection"  ,    44             "ec2:CreateVpnConnectionRoute"  ,    45             "ec2:CreateVpnGateway"  ,    46             "ec2:DeleteCustomerGateway"  ,    47             "ec2:DeleteDhcpOptions"  ,    48             "ec2:DeleteFlowLogs"  ,    49             "ec2:DeleteInternetGateway"  ,    50             "ec2:DeleteNatGateway"  ,    51             "ec2:DeleteNetworkAcl"  ,    52             "ec2:DeleteNetworkAclEntry"  ,    53             "ec2:DeleteNetworkInterface"  ,    54             "ec2:DeleteRoute"  ,    55             "ec2:DeleteRouteTable"  ,    56             "ec2:DeleteSecurityGroup"  ,    57             "ec2:DeleteSubnet"  ,    58             "ec2:DeleteVpc"  ,    59             "ec2:DeleteVpcEndpoints"  ,    60             "ec2:DeleteVpcPeeringConnection"  ,    61             "ec2:DeleteVpnConnection"  ,    62             "ec2:DeleteVpnConnectionRoute"  ,    63             "ec2:DeleteVpnGateway"  ,    64             "ec2:DetachClassicLinkVpc"  ,    65             "ec2:DetachInternetGateway"  ,    66             "ec2:DetachNetworkInterface"  ,    67             "ec2:DetachVpnGateway"  ,    68             "ec2:DisableVgwRoutePropagation"  ,    69             "ec2:DisableVpcClassicLink"  ,    70             "ec2:DisassociateAddress"  ,    71             "ec2:DisassociateRouteTable"  ,    72             "ec2:EnableVgwRoutePropagation"  ,    73             "ec2:EnableVpcClassicLink"  ,    74             "ec2:ModifyNetworkInterfaceAttribute"  ,    75             "ec2:ModifySubnetAttribute"  ,    76             "ec2:ModifyVpcAttribute"  ,    77             "ec2:ModifyVpcEndpoint"  ,    78             "ec2:MoveAddressToVpc"  ,    79             "ec2:RejectVpcPeeringConnection"  ,    80             "ec2:ReleaseAddress"  ,    81             "ec2:ReplaceNetworkAclAssociation"  ,    82             "ec2:ReplaceNetworkAclEntry"  ,    83             "ec2:ReplaceRoute"  ,    84             "ec2:ReplaceRouteTableAssociation"  ,    85             "ec2:ResetNetworkInterfaceAttribute"  ,    86             "ec2:RestoreAddressToClassic"  ,    87             "ec2:RevokeSecurityGroupEgress"  ,    88             "ec2:RevokeSecurityGroupIngress"  ,    89             "ec2:UnassignPrivateIpAddresses"    90            ],    91           "Effect"  :   "Deny"  ,    92           "Resource"  :   "*"    93          }    94        ]    95   }
```


-


VPC policy for the` vpc` role


```text
1   {    2       "Version"  :   "2012-10-17"  ,    3       "Statement"  : [    4          {    5           "Effect"  :   "Allow"  ,    6           "Action"  : [    7             "ec2:AcceptVpcPeeringConnection"  ,    8             "ec2:AllocateAddress"  ,    9             "ec2:AssignPrivateIpAddresses"  ,    10             "ec2:AssociateAddress"  ,    11             "ec2:AssociateDhcpOptions"  ,    12             "ec2:AssociateRouteTable"  ,    13             "ec2:AttachClassicLinkVpc"  ,    14             "ec2:AttachInternetGateway"  ,    15             "ec2:AttachNetworkInterface"  ,    16             "ec2:AttachVpnGateway"  ,    17             "ec2:AuthorizeSecurityGroupEgress"  ,    18             "ec2:AuthorizeSecurityGroupIngress"  ,    19             "ec2:CreateCustomerGateway"  ,    20             "ec2:CreateDhcpOptions"  ,    21             "ec2:CreateFlowLogs"  ,    22             "ec2:CreateInternetGateway"  ,    23             "ec2:CreateNatGateway"  ,    24             "ec2:CreateNetworkAcl"  ,    25             "ec2:CreateNetworkAcl"  ,    26             "ec2:CreateNetworkAclEntry"  ,    27             "ec2:CreateNetworkInterface"  ,    28             "ec2:CreateRoute"  ,    29             "ec2:CreateRouteTable"  ,    30             "ec2:CreateSecurityGroup"  ,    31             "ec2:CreateSubnet"  ,    32             "ec2:CreateTags"  ,    33             "ec2:CreateVpc"  ,    34             "ec2:CreateVpcEndpoint"  ,    35             "ec2:CreateVpcPeeringConnection"  ,    36             "ec2:CreateVpnConnection"  ,    37             "ec2:CreateVpnConnectionRoute"  ,    38             "ec2:CreateVpnGateway"  ,    39             "ec2:DeleteCustomerGateway"  ,    40             "ec2:DeleteDhcpOptions"  ,    41             "ec2:DeleteFlowLogs"  ,    42             "ec2:DeleteInternetGateway"  ,    43             "ec2:DeleteNatGateway"  ,    44             "ec2:DeleteNetworkAcl"  ,    45             "ec2:DeleteNetworkAclEntry"  ,    46             "ec2:DeleteNetworkInterface"  ,    47             "ec2:DeleteRoute"  ,    48             "ec2:DeleteRouteTable"  ,    49             "ec2:DeleteSecurityGroup"  ,    50             "ec2:DeleteSubnet"  ,    51             "ec2:DeleteTags"  ,    52             "ec2:DeleteVpc"  ,    53             "ec2:DeleteVpcEndpoints"  ,    54             "ec2:DeleteVpcPeeringConnection"  ,    55             "ec2:DeleteVpnConnection"  ,    56             "ec2:DeleteVpnConnectionRoute"  ,    57             "ec2:DeleteVpnGateway"  ,    58             "ec2:Describe*"  ,    59             "ec2:DetachClassicLinkVpc"  ,    60             "ec2:DetachInternetGateway"  ,    61             "ec2:DetachNetworkInterface"  ,    62             "ec2:DetachVpnGateway"  ,    63             "ec2:DisableVgwRoutePropagation"  ,    64             "ec2:DisableVpcClassicLink"  ,    65             "ec2:DisassociateAddress"  ,    66             "ec2:DisassociateRouteTable"  ,    67             "ec2:EnableVgwRoutePropagation"  ,    68             "ec2:EnableVpcClassicLink"  ,    69             "ec2:ModifyNetworkInterfaceAttribute"  ,    70             "ec2:ModifySubnetAttribute"  ,    71             "ec2:ModifyVpcAttribute"  ,    72             "ec2:ModifyVpcEndpoint"  ,    73             "ec2:ModifyVpcPeeringConnectionOptions"  ,    74             "ec2:MoveAddressToVpc"  ,    75             "ec2:RejectVpcPeeringConnection"  ,    76             "ec2:ReleaseAddress"  ,    77             "ec2:ReplaceNetworkAclAssociation"  ,    78             "ec2:ReplaceNetworkAclEntry"  ,    79             "ec2:ReplaceRoute"  ,    80             "ec2:ReplaceRouteTableAssociation"  ,    81             "ec2:ResetNetworkInterfaceAttribute"  ,    82             "ec2:RestoreAddressToClassic"  ,    83             "ec2:RevokeSecurityGroupEgress"  ,    84             "ec2:RevokeSecurityGroupIngress"  ,    85             "ec2:UnassignPrivateIpAddresses"    86            ],    87           "Resource"  :   "*"    88          }    89        ]    90   }
```


You also need to let the` M` account assume each role so you’ll need to attach the following role delegation stanza to each role in all accounts. Note the requirement for the MFA.


```text
1   {    2       "Version"  :   "2012-10-17"  ,    3       "Statement"  : [    4          {    5           "Sid"  :   ""  ,    6           "Effect"  :   "Allow"  ,    7           "Principal"  : {    8             "AWS"  :   "arn:aws:iam::__M__:root"    9            },    10           "Action"  :   "sts:AssumeRole"  ,    11           "Condition"  : {    12             "Bool"  : {    13               "aws:MultiFactorAuthPresent"  :   "true"    14              }    15            }    16          }    17        ]    18   }
```


If you store the policy above in` assume-role-from-m.json` you can create the roles with the following:


Terminal window


```text
1   # Create the admin role with the appropriate policy and role delegation    2   aws     iam     create-role     --role-name     admin     --assume-role-policy-document     file://assume-role-from-m.json    3   aws     iam     put-role-policy     --role-name     admin     --policy-name     admin     --policy-document     file://admin.json    4
5   # Create the vpc role with the appropriate policy and role delegation    6   aws     iam     create-role     --role-name     vpc       --assume-role-policy-document     file://assume-role-from-m.json    7   aws     iam     put-role-policy     --role-name     vpc     --policy-name     vpc     --policy-document     file://vpc.json    8
9   # Create the ec2 role with the appropriate policy and role delegation    10   aws     iam     create-role     --role-name     ec2       --assume-role-policy-document     file://assume-role-from-m.json    11   aws     iam     put-role-policy     --role-name     ec2     --policy-name     ec2     --policy-document     file://ec2.json
```


You need to do so in all accounts,` A` ,` B` and` M` .


### Step 4: create the corresponding groups in M


These are the IAM constructs that binds users, the right to assume roles and the target roles. You’ll need 6 groups in the M account.


-


` vpc-A` : can assume the` vpc` role in` A`


-


` vpc-B` : same in` B`


-


` ec2-A` : can assume the` ec2` role in` A`


-


` ec2-B` : same in` B`


-


` admin-A` : can assume the` admin` role


-


` admin-B` : same in` B`


Here is how you bind the group to the right to assume a given role. Start with a policy to let the members of the group` admin-A` call` sts:AssumeRole` on the` admin` role in` A` , here saved in` admin-A-delegation.json` . The only moving pieces are in the` Resource` section: the account ID and the role name.


```text
1   {    2         "Version"  :   "2012-10-17"  ,    3         "Statement"  : [    4              {    5                 "Sid"  :   "AllowGroupToAssumeAdminARole"  ,    6                 "Effect"  :   "Allow"  ,    7                 "Action"  : [    8                     "sts:AssumeRole"    9                  ],    10                 "Condition"  : {    11                     "Bool"  : {    12                         "aws:MultiFactorAuthPresent"  :   "true"    13                      }    14                  },    15                 "Resource"  : [    16                     "arn:aws:iam::__A__:role/admin"    17                  ]    18              }    19          ]    20   }
```


Then simply inline the policies above to their respective groups:


Terminal window


```text
1   # Let members of admin-A become admin in A    2   aws     iam     put-group-policy     --group-name     admin-A     --policy-document     file://admin-A-delegation.json     --policy-name     admin-A    3   # Let members of admin-B become admin in B    4   aws     iam     put-group-policy     --group-name     admin-B     --policy-document     file://admin-B-delegation.json     --policy-name     admin-B    5
6   # Let members of vpc-A become vpc in A    7   aws     iam     put-group-policy     --group-name     vpc-A     --policy-document     file://vpc-A-delegation.json     --policy-name     vpc-A    8   # Let members of vpc-B become vpc in B    9   aws     iam     put-group-policy     --group-name     vpc-B     --policy-document     file://vpc-B-delegation.json     --policy-name     vpc-B    10
11   # Let members of ec2-A become ec2 in A    12   aws     iam     put-group-policy     --group-name     ec2-A     --policy-document     file://ec2-A-delegation.json     --policy-name     ec2-A    13   # Let members of ec2-B become ec2 in B    14   aws     iam     put-group-policy     --group-name     ec2-B     --policy-document     file://ec2-B-delegation.json     --policy-name     ec2-B
```


Now any member of` vpc-A` group can call` sts:AssumeRole` on the` vpc` role in account A. The MFA needs to be present for the` sts:AssumeRole` call to succeed.


### Step 5: add users to groups


Now that the machinery is in place, you just need to assign users to groups:


1.


Alice belongs to` vpc-A` and` vpc-B` .


2.


Bob belongs to` ec2-B` .


3.


Charlie belongs to` admin-A` and` admin-B` .


And you are done.


## 10, and closing thoughts


If you have followed thus far, you may have spotted a limitation of this scheme: an IAM user cannot be part of more than 10 groups at once. Until that limit is raised, you’ll need to use group membership sparingly. How to make this work for more than 10 accounts at once is left as an exercise to the reader (to riff on Fermat’s famous and most nagging ever margin note).


IAM is a complex topic, with relatively little that’s shared beyond the official documentation. This is my modest attempt at reversing the trend so I more than welcome feedback, critiques and (civil) comments.


-
-
-
