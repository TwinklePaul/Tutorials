# CROSS ACCOUNT TRANSFER OF S3 FILES

## Objective:
This documentation deals with the file transfer from one S3 bucket to other across different accounts.

## Pre-Requisite:
Multiple Accounts (Atleast 2).

## Process:

### Step 1: Setup Source S3
1. Sign in to source AWS account. Create a bucket in S3. 

2. Attach the following policy to the bucket. 
```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "DelegateS3Access",
                "Effect": "Allow",
                "Principal": {
                    "AWS":"arn:aws:iam::account1:role/Lambda-List-S3-Role"
                },
                "Action": [
                    "s3:ListBucket",
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::SOURCE_BUCKET_NAME/*",
                    "arn:aws:s3:::SOURCE_BUCKET_NAME"
                ]
            }
        ]
    }
```

3. Upload some test files which are meant to be copied automatically to the destination bucket.


### Step 2: Setup Destination S3
Sign in to destination AWS account. Create a bucket in S3.

### Step 3: Attach policy to IAM user in destination AWS account.
Attach the following policy to the IAM user created previously in the destination AWS account.
```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket",
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::SOURCE_BUCKET_NAME",
                    "arn:aws:s3:::SOURCE_BUCKET_NAME/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket",
                    "s3:PutObject",
                    "s3:PutObjectAcl"
                ],
                "Resource": [
                    "arn:aws:s3:::DESTINATION_BUCKET_NAME",
                    "arn:aws:s3:::DESTINATION_BUCKET_NAME/*"
                ]
            }
        ]
    }
```


### Step 4: Create Lambda Policy for Source Account
Create the following Policy for Lambda:
```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "S3ListBucket",
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket"
                ],
                "Resource": "arn:aws:s3:::bucketname"
            },
            {
                "Sid": "logsstreamevent",
                "Effect": "Allow",
                "Action": [
                    "logs:CreateLogStream",
                    "logs:PutLogEvents"
                ],
                "Resource": "arn:aws:logs:us-east-1:account1:log-group:/aws/lambda/Lambda-List-S3*/*"
            },
            {
                "Sid": "logsgroup",
                "Effect": "Allow",
                "Action": "logs:CreateLogGroup",
                "Resource": "*"
            }
        ]
    }
```

### Step 5: Create Lambda for Source Account.
Create the following lambda function for source account.
```
    import json
    import boto3
    import os
    import uuid

    def lambda_handler(event, context):
        try:

            # Create an S3 client
            s3 = boto3.client('s3')

            # Call S3 to list current buckets
            objlist = s3.list_objects(
                            Bucket='bucketname',
                            MaxKeys = 10)

            print (objlist['Contents'])
            return str(objlist['Contents'])


        except Exception as e:
            print(e)
            raise e
```