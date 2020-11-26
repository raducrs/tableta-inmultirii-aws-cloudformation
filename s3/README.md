Follow the instructions below, either building from scratch or importing existing resources into the stack

# Existing resources

## Import existing resources into stack

Run the command

```
aws cloudformation create-change-set \
    --stack-name TIS3 --change-set-name ImportChangeSet \
    --change-set-type IMPORT \
    --resources-to-import file://import-resources-s3.json \
    --template-body file://s3-import.yaml

aws cloudformation execute-change-set --change-set-name ImportChangeSet --stack-name TIS3

```

**Since the import does not allow for outputs to be defined** we need to create a new change set with them.

Also ** Note: The following resource types are not supported for resource import: AWS::S3::BucketPolicy [Learn more](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-supported-resources.html?icmpid=docs_cfn_console)**

You can construct it in the AWS console using ```dynamodb.yaml``` or use ```--change-set-type UPDATE``` from the AWS cli.

```
aws cloudformation create-change-set \
    --stack-name TIS3 --change-set-name UpdateChangeSet \
    --change-set-type UPDATE \
    --template-body file://s3-post.yaml

aws cloudformation execute-change-set --change-set-name UpdateChangeSet --stack-name TIS3

```

** If the bucket policy does not exist uncomment the lines regarding the bucket policy in the ```s3-post.yaml```

# New resources

Run command

```
aws cloudformation create-change-set \
    --stack-name TIS3 --change-set-name UpdateChangeSet \
    --change-set-type UPDATE \
    --template-body file://s3.yaml

aws cloudformation execute-change-set --change-set-name UpdateChangeSet --stack-name TIS3

```

## Notifications



**Note [User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket-notificationconfig.html)**

If you create the target resource and related permissions in the same template, you might have a circular dependency.

For example, you might use the ```AWS::Lambda::Permission``` resource to grant the bucket permission to invoke an AWS Lambda function. However, AWS CloudFormation can't create the bucket until the bucket has permission to invoke the function (AWS CloudFormation checks whether the bucket can invoke the function). If you're using Refs to pass the bucket name, this leads to a circular dependency.

To avoid this dependency, you can create all resources without specifying the notification configuration. Then, update the stack with a notification configuration.

Example of usage:

```
     NotificationConfiguration:
       LambdaConfigurations:
         Event: 's3:ObjectCreated:*'
         Function: 'arn:aws:lambda:<region>:<account>:function:<lamba-function-name>'
```


