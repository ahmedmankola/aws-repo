AWS Inventory Collection Solution

Overview

This solution automates the collection of AWS inventory across multiple AWS accounts and regions. It retrieves information about various AWS resources, such as EC2 instances, Lambda functions, and RDS instances, and stores the data in an S3 bucket as a CSV file. The solution is implemented using AWS CloudFormation and utilizes AWS Lambda, EventBridge, and S3.

Components

AWS Lambda Function: A Python-based function that collects inventory details from multiple AWS regions.

Amazon S3 Bucket: Stores the inventory data as CSV files.

AWS EventBridge Rule: Triggers the Lambda function on a scheduled interval (e.g., daily).

IAM Role and Policies: Grants necessary permissions to the Lambda function to access AWS resources.

CloudFormation Template: Automates the deployment of all components.

How It Works

The CloudFormation template provisions all required resources.

The EventBridge rule triggers the Lambda function at the defined schedule (e.g., once per day).

The Lambda function collects resource inventory from specified AWS regions.

The inventory data is formatted as a CSV file and uploaded to the designated S3 bucket.

The collected inventory data can be accessed from the S3 bucket.

Deployment Instructions

Prerequisites

An AWS account with appropriate permissions to create IAM roles, Lambda functions, S3 buckets, and EventBridge rules.

AWS CLI installed and configured with the necessary credentials.

Steps to Deploy

Create the CloudFormation stack:

aws cloudformation create-stack --stack-name AWSInventoryStack \
    --template-body file://aws_inventory.yml \
    --capabilities CAPABILITY_NAMED_IAM \
    --parameters ParameterKey=S3BucketName,ParameterValue=my-aws-inventory-bucket

Wait for stack creation to complete:

aws cloudformation wait stack-create-complete --stack-name AWSInventoryStack

Verify the deployment:

Check the AWS Lambda function in the AWS console.

Verify that an EventBridge rule exists.

Ensure the S3 bucket is created.

Executing the Solution Manually

To manually trigger the Lambda function:

aws lambda invoke --function-name AWSInventoryCollector output.json

Retrieving the Inventory Data

After execution, the inventory data will be stored in the S3 bucket. You can download the CSV file using:

aws s3 cp s3://my-aws-inventory-bucket/aws_inventory_<timestamp>.csv ./

Replace <timestamp> with the appropriate timestamp of the file.

Limitations

Region Restriction: The script only collects data from specified AWS regions.

Supported Resources: Currently supports EC2, Lambda, and RDS. Additional services may need custom modifications.

IAM Permissions: Ensure IAM policies grant required permissions; otherwise, some resources may not be retrieved.

S3 Storage Costs: The collected inventory files are stored in S3, which may incur costs based on storage size and access frequency.

Future Enhancements

Expand support for more AWS services (e.g., S3, IAM, DynamoDB, etc.).

Add error handling and logging to CloudWatch for debugging.

Implement AWS Organizations integration for multi-account inventory collection.

Cleanup

To remove all deployed resources, delete the CloudFormation stack:

aws cloudformation delete-stack --stack-name AWSInventoryStack

Author
