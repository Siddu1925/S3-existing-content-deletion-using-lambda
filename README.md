# S3-existing-content-deletion-using-lambda
AWS Lambda function in Python using Boto3 that deletes objects older than 30 days from a specified S3 bucket and logs the deleted object keys

Steps:
1. Create S3 Bucket bucket.
2. Ensure your Lambda IAM role has AmazonS3FullAccess.
3. Deploy this function in AWS Lambda and manually invoke it.
   
   import boto3
   
from datetime import datetime, timezone, timedelta

def lambda_handler(event, context):
    bucket_name = 'your-bucket-name'  # Replace with your bucket name
    s3 = boto3.client('s3')
    days_threshold = 30
    deleted_objects = []

    # Calculate cutoff date
    cutoff = datetime.now(timezone.utc) - timedelta(days=days_threshold)

    # List objects in the bucket
    response = s3.list_objects_v2(Bucket=bucket_name)
    if 'Contents' in response:
        for obj in response['Contents']:
            last_modified = obj['LastModified']
            if last_modified < cutoff:
                s3.delete_object(Bucket=bucket_name, Key=obj['Key'])
                deleted_objects.append(obj['Key'])

    print(f"Deleted objects older than {days_threshold} days: {deleted_objects}")
    
4. Check the S3 bucket to confirm only files newer than 30 days remain.
