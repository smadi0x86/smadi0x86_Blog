---
description: >-
  Essential AWS CLI commands for setting up your AWS account and managing AWS
  resources.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# AWS CLI

## <mark style="color:red;">AWS CLI Configuration</mark>

Before starting, you must configure the AWS CLI with your credentials.&#x20;

#### <mark style="color:purple;">Run the following command and provide your Access Key, Secret Key, preferred AWS Region, and default output format:</mark>

```bash
aws configure
```

## <mark style="color:red;">Identifying the Current User</mark>

To confirm the identity being used by your AWS CLI commands, use `aws iam get-user`.&#x20;

This command returns details about the IAM user or role whose credentials are used to call the operation.

```bash
aws iam get-user
```

## <mark style="color:red;">Creating an IAM Role</mark>

AWS services like AWS Lambda need to assume an IAM role to work with other AWS services under your purview.

#### &#x20;<mark style="color:purple;">Use the following command to create a new IAM role:</mark>

{% code overflow="wrap" %}
```bash
aws iam create-role --role-name <role-name> --assume-role-policy-document file://<trust-policy-file>
```
{% endcode %}

Make sure to replace `<role-name>` and `<trust-policy-file>` with your preferred role name and the location of your trust policy JSON file, respectively.

## <mark style="color:red;">Attaching IAM Policy to a Role</mark>

Once you have created an IAM role, you can attach policies to it.&#x20;

#### <mark style="color:purple;">The following command attaches a predefined AWS policy to an IAM role:</mark>

{% code overflow="wrap" %}
```bash
aws iam attach-role-policy --role-name <role-name> --policy-arn <policy-arn>
```
{% endcode %}

Replace `<role-name>` with your role name and `<policy-arn>` with the ARN of the policy you want to attach.

## <mark style="color:red;">Working with AWS Lambda</mark>

AWS Lambda allows you to run your code without provisioning or managing servers. Let's discuss the key commands to manage your AWS Lambda functions.

### <mark style="color:yellow;">Creating a Lambda Function</mark>

You can create an AWS Lambda function using the `create-function` command.&#x20;

#### <mark style="color:purple;">Below is an example command:</mark>

{% code overflow="wrap" %}
```bash
aws lambda create-function --function-name <function-name> --role <role-arn> --runtime provided --timeout 15 --memory-size 128 --handler <handler> --zip-file fileb://<zip-file>
```
{% endcode %}

Make sure to replace `<function-name>`, `<role-arn>`, `<handler>`, and `<zip-file>` with your preferred function name, the ARN of the IAM role, the function handler, and the location of your ZIP file, respectively.

### <mark style="color:yellow;">Updating a Lambda Function</mark>

#### <mark style="color:purple;">If you want to update the code of an existing Lambda function, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`update-function-code`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">command:</mark>

{% code overflow="wrap" %}
```bash
aws lambda update-function-code --function-name <function-name> --zip-file fileb://<zip-file>
```
{% endcode %}

Replace `<function-name>` and `<zip-file>` with your function name and the location of your ZIP file, respectively.

### <mark style="color:yellow;">Invoking a Lambda Function</mark>

#### <mark style="color:purple;">You can invoke a Lambda function synchronously using the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`invoke`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">command:</mark>

{% code overflow="wrap" %}
```bash
aws lambda invoke --function-name <function-name> --payload '<JSON-payload>' <output-file>
```
{% endcode %}

Replace `<function-name>`, `<JSON-payload>`, and `<output-file>` with your function name, the JSON payload your function expects, and the file where you want to save the function response, respectively.

### <mark style="color:yellow;">Deleting a Lambda Function</mark>

#### <mark style="color:purple;">To delete a specific Lambda function, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`delete-function`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">command:</mark>

```bash
aws lambda delete-function --function-name <function-name>
```

Replace `<function-name>` with the name of the function you want to delete.

### <mark style="color:yellow;">Listing All Lambda Functions</mark>

#### <mark style="color:purple;">To see a list of all your Lambda functions, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`list-functions`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">command:</mark>

```bash
aws lambda list-functions
```

## <mark style="color:red;">**Working with S3 Buckets**</mark>

#### <mark style="color:purple;">**Creating a bucket**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">- To create a bucket in S3, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`mb`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(make bucket) command:</mark>

```bash
aws s3 mb s3://<bucket-name>
```

#### <mark style="color:purple;">**List all buckets**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">- To list all the buckets in S3, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`ls`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(list) command:</mark>

```bash
aws s3 ls
```

#### <mark style="color:purple;">**Upload file to bucket**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">- To upload a file to an S3 bucket, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`cp`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(copy) command:</mark>

```bash
aws s3 cp <file-to-upload> s3://<bucket-name>/<destination-key>
```

#### <mark style="color:purple;">**Delete a bucket**</mark> <mark style="color:purple;"></mark><mark style="color:purple;">- To delete a bucket, use the</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`rb`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(remove bucket) command:</mark>

```bash
aws s3 rb s3://<bucket-name> --force
```

## <mark style="color:red;">**Working with DynamoDB**</mark>

#### <mark style="color:purple;">**Create a table**</mark><mark style="color:purple;">:</mark>

{% code overflow="wrap" %}
```bash
aws dynamodb create-table --table-name <table-name> --attribute-definitions AttributeName=<attribute-name>,AttributeType=<attribute-type> --key-schema AttributeName=<key-attribute-name>,KeyType=HASH --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```
{% endcode %}

#### <mark style="color:purple;">**List all tables**</mark><mark style="color:purple;">:</mark>

```bash
aws dynamodb list-tables
```

#### <mark style="color:purple;">**Delete a table**</mark><mark style="color:purple;">:</mark>

```bash
aws dynamodb delete-table --table-name <table-name>
```

## <mark style="color:red;">**Working with EC2 Instances**</mark>

#### <mark style="color:purple;">**Launch an instance**</mark><mark style="color:purple;">:</mark>

{% code overflow="wrap" %}
```bash
aws ec2 run-instances --image-id <ami-id> --count <number-of-instances> --instance-type <instance-type> --key-name <key-pair-name> --subnet-id <subnet-id> --security-group-ids <security-group-id>
```
{% endcode %}

#### <mark style="color:purple;">**Stop an instance**</mark><mark style="color:purple;">:</mark>

```bash
aws ec2 stop-instances --instance-ids <instance-id>
```

#### <mark style="color:purple;">**Terminate an instance**</mark><mark style="color:purple;">:</mark>

```bash
aws ec2 terminate-instances --instance-ids <instance-id>
```

#### <mark style="color:purple;">**Describe instances**</mark><mark style="color:purple;">:</mark>

```bash
aws ec2 describe-instances
```



{% embed url="https://www.bluematador.com/learn/aws-cli-cheatsheet" %}
