# aws-terraform-project
This project demonstrates a fully serverless architecture using Amazon API Gateway, AWS Step Functions, and Amazon DynamoDB — all provisioned and connected using Terraform. It avoids using AWS Lambda entirely, which is typically the default compute option in serverless projects.
# 🚀 Serverless API Gateway to Step Functions to DynamoDB (No Lambda)

This project demonstrates a **serverless architecture** using:

- **Amazon API Gateway (REST API)**
- **AWS Step Functions**
- **Amazon DynamoDB**
- **Terraform** for infrastructure as code (IaC)

✅ **No Lambda functions were used** — we used **Step Functions directly** from API Gateway to orchestrate DynamoDB actions.

---

## 🔍 Project Overview

This serverless app exposes an HTTP POST endpoint (`/tasks`) via **API Gateway**, which triggers a **Step Function**, which in turn could write to **DynamoDB** or perform other tasks.

### 🧠 Why Step Functions Instead of Lambda?

- **No Runtime to Manage**: No code to deploy or maintain.
- **Cost-Effective**: No Lambda invocation costs; ideal for lightweight tasks.
- **Built-in Observability**: Visual workflow UI and native retries.
- **Simplified Security**: API Gateway assumes an IAM role to invoke Step Functions directly.
- **Infrastructure-Only Focus**: All behavior is defined in Terraform.

| Issue                                                                      | Solution                                                                                               |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| ❌ `No valid credential sources found`                                      | Fixed by configuring AWS credentials using `aws configure`                                             |
| ❌ `BadRequestException: Operation AWS::DynamoDB::PutItem is not supported` | This happened because we mistakenly used a Lambda-style integration; we switched to Step Functions     |
| ❌ `Missing credentials_arn`                                                | Added an IAM role allowing API Gateway to invoke Step Functions                                        |
| ❌ `var.aws_region not declared`                                            | Declared `variable "aws_region"` in `main.tf`                                                          |
| ❌ `AWS_PROXY only supports Lambda`                                         | Switched to using `aws_api_gateway_rest_api` instead of `v2` HTTP APIs                                 |
| ⚠️ `stage_name is deprecated` warning                                      | Not blocking, but for future: switch to `aws_api_gateway_stage` instead of `stage_name` in deployments |

