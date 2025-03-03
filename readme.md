AWS Inventory Collection Solution

Overview

This solution automates the collection of AWS resources across multiple AWS accounts and regions. It retrieves data from services such as EC2, EBS, RDS, Lambda, S3, IAM, VPC, Security Groups, Load Balancers, Route 53, DynamoDB, SNS, and SQS. The collected inventory is stored as a CSV file in an S3 bucket for further analysis.

The solution is implemented using AWS CloudFormation, Lambda, IAM, S3, and EventBridge (CloudWatch Events) to automate execution.

Architecture & Components

AWS Lambda: A Python-based function that collects inventory across all AWS regions.

Amazon S3: Stores the collected inventory as a CSV file.

AWS IAM Role: Grants Lambda permissions to access AWS services.

EventBridge (CloudWatch Events): Triggers Lambda execution on a schedule (e.g., daily).

CloudFormation Template: Automates the deployment of all resources across AWS accounts.

How It Works

Lambda Function Execution:

Automatically detects all available AWS regions.

Collects inventory from EC2, RDS, Lambda, S3, IAM, VPC, Security Groups, Load Balancers, Route 53, DynamoDB, SNS, and SQS.

Saves the inventory to a CSV file in S3.

Storage in S3:

The collected inventory is formatted as a CSV file.

The file is stored in a centralized S3 bucket under the naming format:

s3://<inventory-bucket>/<account-id>/aws_inventory_YYYYMMDDHHMMSS.csv


Currently you can only run lambda through manual trigger .

Deployment Instructions

Prerequisites

AWS CLI installed and configured with admin credentials.

AWS CloudFormation enabled.

S3 bucket to store inventory.

Deploy Using CloudFormation

1️⃣ Create the Stack

Save the CloudFormation YAML file as aws_inventory.yaml, then run:

aws cloudformation create-stack --stack-name AWSInventoryStack \
    --template-body file://aws_inventory.yaml \
    --capabilities CAPABILITY_NAMED_IAM \
    --parameters ParameterKey=S3BucketName,ParameterValue=my-inventory-bucket

2️⃣ Wait for Stack Creation

aws cloudformation wait stack-create-complete --stack-name AWSInventoryStack

3️⃣ Verify Lambda Deployment

aws lambda list-functions | grep AWSInventoryCollector

4️⃣ Check S3 for Inventory Data

aws s3 ls s3://my-inventory-bucket/

Execution Methods

Manual Execution (Run Lambda Function on Demand)

aws lambda invoke --function-name AWSInventoryCollector output.json


Limitations

🔹 Cross-Account Inventory Requires Setup

If using multiple AWS accounts, IAM roles must be configured with cross-account access.

🔹 Permissions Must Be Correct

Lambda requires and permissions.

Ensure IAM permissions allow Lambda to access AWS services.

🔹 Large AWS Environments May Exceed Limits

If AWS has thousands of resources, Lambda may hit execution time limits.

Solution: Use Step Functions or split execution per region.

🔹 Global vs. Regional Services

Some AWS services (IAM, S3, Route 53) are global and are only queried once.

Other services (EC2, RDS, Lambda) are regional and queried per region.

🔹 List of regions
currently we set list of regions in the code , need to list of regions that need to work on it 

