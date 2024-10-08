
# AWS Lambda: Read CSV File from S3 Using Boto3

This project demonstrates how to create an AWS Lambda function that reads a file (e.g., CSV) from an S3 bucket when triggered by an S3 event (e.g., file upload). The Lambda function uses the **Boto3** library to interact with the S3 service and read the contents of the file.

## Summary
```sql

### Instructions for the User:
1. **Create an S3 bucket** if you don't have one.
2. **Create a Lambda function** using the code provided above.
3. **Set up an event notification** in the S3 bucket to trigger the Lambda function upon object uploads.
4. **Test the function** by uploading a file (e.g., CSV) to the S3 bucket. You should see the file content logged in CloudWatch.


```

## Prerequisites

- **AWS Lambda**: You need an AWS Lambda function set up.
- **S3 Bucket**: Your Lambda function should be triggered by an event from an S3 bucket (e.g., file upload).
- **Boto3**: This is the AWS SDK for Python and is pre-installed in AWS Lambda runtime environments.

## IAM Role and Permissions

Ensure that the AWS Lambda execution role has sufficient permissions to access the S3 bucket. Attach the following policy to the Lambda's role:
- **AmazonS3ReadOnlyAccess**

## Lambda Function

This is the Lambda function that handles reading a file from an S3 bucket:

```python
import json
import boto3
from io import StringIO

def lambda_handler(event, context):
    print(f"Event =>{event}")
    
    # Extract bucket name and object key from the event
    bucket_name = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Initialize S3 client
    s3_client = boto3.client('s3')
    
    # Fetch the object from S3
    response = s3_client.get_object(Bucket=bucket_name, Key=key)
    
    # Read file content
    file_content = response['Body'].read().decode('utf-8')
    
    # Log file content
    print("File Contents:")
    print(file_content)
    
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

## Key Points:
- The function is triggered by an S3 event (e.g., file upload).
- Bucket Name and Object Key are extracted from the event payload.
- The Boto3 client is used to read the file's contents from the S3 bucket.
- The file content is logged to the CloudWatch logs.
## Setting Up the Trigger
- Go to your S3 bucket in the AWS Management Console.
- Under Properties, scroll to the Event notifications section.
- Set up an event notification to trigger the Lambda function when a new object is uploaded to the bucket.
  - Select All object create events.
  - Point the event notification to the Lambda function.
## Example Event
Here's an example S3 event payload that triggers the Lambda function:
```sql
{
  "Records": [
    {
      "s3": {
        "bucket": {
          "name": "my-bucket-name"
        },
        "object": {
          "key": "my-folder/my-file.csv"
        }
      }
    }
  ]
}

```
