
Follow the instructions below, either building from scratch or importing existing resources into the stack

# Existing resources

## How to build CloudFormation stack from existing resources

List the available tables ```aws dynamodb list-tables```

List configuration with ```aws dynamodb describe-table --table-name=<Table-name>```

Join all configurations into ```dynamodb-import.json```

## Import existing resources into stack

Create a list of resources to import like in ```import-resources-db.json``` from the definitions in ```dynamodb-import.json```

Run command

```
aws cloudformation create-change-set \
    --stack-name TIDynamoDB --change-set-name ImportChangeSet \
    --change-set-type IMPORT \
    --resources-to-import file://import-resources-db.json \
    --template-body file://dynamodb-import.json

aws cloudformation execute-change-set --change-set-name ImportChangeSet --stack-name TIDynamoDB

```

**Since the import does not allow for outputs to be defined** we need to create a new change set with them.

You can construct it in the AWS console using ```dynamodb.yaml``` or use ```--change-set-type UPDATE``` from the AWS cli.

# New resources

Run command

```
aws cloudformation create-change-set \
    --stack-name TIDynamoDB --change-set-name CreateChangeSet \
    --change-set-type CREATE \
    --template-body file://dynamodb.yaml

aws cloudformation execute-change-set --change-set-name CreateChangeSet --stack-name TIDynamoDB