---
schema_version: "1.0.0"
document_id: "6d2229e9ce0a4db30e3461b76a32c7a6bbf8ec9c45054cf9d63425243a9bf8ef"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/how-to-deploy-airflow-environments-in-scale-using-aws-mwaa-9c35606a7a5e"
published_at: "2023-03-29T08:44:56+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:d736d6a1cbe01edb74632c7e60942dd4a3bd9aaa4fe636622ba1e8a960a99e12"
---

# Deploy AWS MWAA (Airflow) environments in scale using Terraform

Press enter or click to view image in full size


# Deploy AWS MWAA (Airflow) environments in scale using Terraform


[Eliezer Yaacov](https://medium.com/@eliezerj8?source=post_page---byline--9c35606a7a5e---------------------------------------)


6 min read


·


Mar 28, 2023


--


For years now, Airflow become the standard for using a platform for developing and scheduling batch workflows.


If you ever tried to manage the infrastructure for airflow, you probably had to tailor your solution for creating the MetaStore DB, queue or key-value storage for the scheduler, servers to host the web servers, workers and scheduler components, and maybe other component to create a fully, functional, production-ready Airflow environment.


While it might sounds complex, we used to run Airflow on a vary infrastructures such as Nomad and Kubernetes and it was good enough. The actual problem started when we wanted to scale up Airflow environments creation. The ability to create environment in a hour, with all its component, gave us the ability to develop, test and deploy changes quickly to production, work on few parallel data sets with separated environments and more.


If you are looking to create airflow environments in scale, quickly, the following solution worked for us.


**Amazon Managed Workflows for Apache Airflow (MWAA) is a fully managed service that makes it easy to run open-source versions of Apache Airflow on AWS. With AWS MWAA, you can easily build, run, and scale your workflows without having to manage the underlying infrastructure. In this article, we’ll guide you through the steps to create an AWS MWAA environment in Terraform, including an IAM execution role and an S3 bucket.**


The entire infrastructure in Similarweb managed in Terraform code. We created a Terraform module with the following parts:


- MWAA environment
- S3 bucket
- IAM role (execution role)
- IAM user (CI user)


[The S3 bucket](https://docs.aws.amazon.com/mwaa/latest/userguide/mwaa-s3-bucket.html) purpose is to contain all code relevant for Airflow env (DAGs, requirements, plugins, etc).


[The execution role](https://docs.aws.amazon.com/mwaa/latest/userguide/mwaa-create-role.html) is required by MWAA env to invoke actions on the different services in the environment such as logs and metrics, access to the bucket, etc.


The IAM user purpose is to provide the full solution with automatic code deployment. Whether managing the code in GitHub, Gitlab or any other version control system, we’ll want a process of code deployment, when the code is verified, and upload it to the S3 bucket. For that, we’ll use awscli with the user, to manage the code sync.


Baseline for Terraform — All code tested on Terraform 0.14.11 and up, AWS provider 4.9 and up.


> MWAA Environment —


```text
resource "aws_mwaa_environment" "managed_airflow" {    airflow_version                = "2.2.2"    airflow_configuration_options  = {      .....      "core.dag_file_processor_timeout"           = 150      "core.dagbag_import_timeout"                = 90      ....    }    dag_s3_path                    = "dags/"    execution_role_arn             = module.execution_role.role_arn    name                           = "airflow-env-name"    environment_class              = "mw1.small"     network_configuration {      security_group_ids = [aws_security_group.managed_airflow_sg.id]      subnet_ids         = ["A", "B"] # 2 subnets required for high availability    }     source_bucket_arn               = aws_s3_bucket.managed-airflow-bucket.arn    weekly_maintenance_window_start = "SUN:19:00"     logging_configuration {      dag_processing_logs {        enabled   = true        log_level = "WARNING"      }       scheduler_logs {        enabled   = true        log_level = "WARNING"      }       task_logs {        enabled   = true        log_level = "WARNING"      }       webserver_logs {        enabled   = true        log_level = "WARNING"      }       worker_logs {        enabled   = true        log_level = "WARNING"      }    }     tags = {      name = "airflow-env-name"      .....    }     lifecycle {      ignore_changes = [        requirements_s3_object_version,        plugins_s3_object_version,      ]    }   }    ##### Security group  resource "aws_security_group" "managed_airflow_sg" {    name        = "managed_airflow-sg"    vpc_id      = <required-vpc-id>     tags = {      Name          = "managed-airflow-sg"    }  }   ##### Security group rules  resource "aws_security_group_rule" "allow_all_out_traffic_managed_airflow" {    type              = "egress"    from_port         = 0    to_port           = 0    protocol          = -1    cidr_blocks       = ["0.0.0.0/0"]    security_group_id = aws_security_group.managed_airflow_sg.id  }   resource "aws_security_group_rule" "allow_inbound_internal_traffic" {    type              = "ingress"    from_port         = 443    to_port           = 443    protocol          = "tcp"    cidr_blocks       = data.terraform_remote_state.network_remote_state.outputs.internal_subnet_cidrs    security_group_id = aws_security_group.managed_airflow_sg.id  }   resource "aws_security_group_rule" "self_reference_sgr" {    type              = "ingress"    from_port         = 0    to_port           = 65535    protocol          = "tcp"    self              = true    security_group_id = aws_security_group.managed_airflow_sg.id  }
```


> S3 Bucket —


```text
resource "aws_s3_bucket" "managed-airflow-bucket" {    bucket        = "airflow-bucket-sw"    force_destroy = "false"     tags = {      Name          = "airflow-bucket-sw"      .....    }  }   resource "aws_s3_bucket_versioning" "managed-airflow-bucket-versioning" {    bucket = aws_s3_bucket.managed-airflow-bucket.id    versioning_configuration {      status = "Enabled"    }  }
```


Bucket must be defined with versioning as a requirement by MWAA environment. The version of the files helps to manage changes in production environments for requirements, plugins and more.


> IAM execution role and policy —


```text
data "aws_iam_policy_document" "execution_role_policy" {    version = "2012-10-17"    statement {      effect = "Allow"      actions = [        "airflow:PublishMetrics"      ]      resources = [        "arn:aws:airflow:${var.region}:${var.account_id}:environment/${var.name}*"      ]    }    statement {      effect  = "Deny"      actions = ["s3:ListAllMyBuckets"]      resources = [        "arn:aws:s3:::${var.bucket_name}",        "arn:aws:s3:::${var.bucket_name}/*"      ]    }    statement {      effect = "Allow"      actions = [        "s3:GetObject*",        "s3:GetBucket*",        "s3:List*"      ]      resources = [        "arn:aws:s3:::${var.bucket_name}",        "arn:aws:s3:::${var.bucket_name}/*"      ]    }    statement {      effect = "Allow"      actions = [        "s3:GetAccountPublicAccessBlock"      ]      resources = ["*"]    }    statement {      effect = "Allow"      actions = [        "logs:CreateLogStream",        "logs:CreateLogGroup",        "logs:PutLogEvents",        "logs:GetLogEvents",        "logs:GetLogRecord",        "logs:GetLogGroupFields",        "logs:GetQueryResults"      ]      resources = [        "arn:aws:logs:${var.region}:${var.account_id}:log-group:airflow-${var.name}-*"      ]    }    statement {      effect = "Allow"      actions = [        "logs:DescribeLogGroups"      ]      resources = [        "*"      ]    }    statement {       effect = "Allow"      actions = [        "cloudwatch:PutMetricData"      ]      resources = [        "*"      ]    }    statement {      effect = "Allow"      actions = [        "sqs:ChangeMessageVisibility",        "sqs:DeleteMessage",        "sqs:GetQueueAttributes",        "sqs:GetQueueUrl",        "sqs:ReceiveMessage",        "sqs:SendMessage"      ]      resources = [        "arn:aws:sqs:${var.region}:*:airflow-celery-*"      ]    }    statement {      effect = "Allow"      actions = [        "kms:Decrypt",        "kms:DescribeKey",        "kms:GenerateDataKey*",        "kms:Encrypt"      ]      resources = var.kms_key_arn != null ? [        var.kms_key_arn      ] : []      not_resources = var.kms_key_arn == null ? [        "arn:aws:kms:*:${var.account_id}:key/*"      ] : []      condition {        test = "StringLike"        values = var.kms_key_arn != null ? [          "sqs.${var.region}.amazonaws.com",          "s3.${var.region}.amazonaws.com"        ] : [          "sqs.${var.region}.amazonaws.com"        ]        variable = "kms:ViaService"      }    }  }    resource "aws_iam_role" "role" {    name                 = "airflow-execution-role"    path                 = "/"    tags                 = local.tag_list  }   resource "aws_iam_role_policy" "role-policy" {    name   = "airflow-execution-role-policy"    role   = aws_iam_role.role.id    policy = data.aws_iam_policy_document.execution_role_policy.json  }
```


The execution role has access to KMS for a given key. We can extend this policy of course if we need airflow to access other services, but this is the baseline, recommended by AWS.


> IAM User (CI user) —


```text
data "aws_iam_policy_document" "ci_user_policy" {    statement {      effect = "Allow"      actions = [        "s3:GetObject*",        "s3:GetBucket*",        "s3:List*",        "s3:PutObject",        "s3:DeleteObject",        "s3:GetEncryptionConfiguration",      ]      resources = [        "arn:aws:s3:::${var.bucket_name}",        "arn:aws:s3:::${var.bucket_name}/*"      ]    }  }  resource "aws_iam_user" "app_user" {    name = "appusr_airflow_ci"    path = "/"    tags = local.tag_list      }   resource "aws_iam_access_key" "access_key" {    user = aws_iam_user.app_user.name  }   resource "aws_iam_user_policy" "user_policy" {    name   = "airflow_ci_policy"    user   = aws_iam_user.app_user.name    policy = data.aws_iam_policy_document.ci_user_policy.json  }   output "ci_access_key" {    value = aws_iam_access_key.access_key.id  }   output "ci_secret_key" {    value = aws_iam_access_key.access_key.secret  }
```


The user will have permissions to sync the source code containing the DAGs, requirements and plugins to an S3 bucket. The user needs permission to add/update and delete files if necessary, and its actions limited to the bucket only. Warning: apply the code like that, will expose the secret key to the output.


Eventually, the CI user will be used to sync the code to the S3 bucket, as followed:


```text
AWS_ACCESS_KEY_ID=<CI-USER-ACCESS-KEY> \  AWS_SECRET_ACCESS_KEY=<CI-USER-SECRET-KEY> \  aws s3 sync ./ s3://$AWS_S3_BUCKET/ \              --delete \              --exclude '.git/*' \              --exclude <EXTRA-FILES-NEEDED> \
```


Just to emphasize how simple is it to create Airflow env, using the suggested solution here, the following is a call for a Terraform module which contains all the above parts:


```text
locals {    airflow_configuration_options = {      ....      "core.dag_file_processor_timeout" = 150      "core.dagbag_import_timeout" = 90      ....    }  }   module "managed_airflow_web_platform" {    source  = "terraform-registry-url/airflow-managed/aws"    version = "~> 2.0"     subnet_cidrs                         = ["10.10.X.Y/27", "10.10.X.Z/27"]    dag_s3_path                          = "dags/"    name                                 = "sw-airflow"    bucket_name                          = "sw-airflow"    cicd_user                            = "sw-airflow-ci-user"    requirements_s3_path                 = "requirements.txt"    plugins_s3_path                      = "plugins.zip"    environment_class                    = "mw1.large"    airflow_configuration_options        = local.airflow_configuration_options    dag_processing_logs_level            = "INFO"  }
```


All we have to do now is run Terraform apply on this example state, connect a git repository to it which manages the DAGs and airflow code and we have full Airflow environment setup in no time. :)
