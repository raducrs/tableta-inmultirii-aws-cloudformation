
Named IAM policies have special conditions when beeing build using ```Cloudformation```, namely that they can not be updated or follow the drift. We will build them from scratch.

**Warning: you must create policy names that are at least 20 characters longs (the -Policy suffix was added for this reason)**

**You must have deployed the [s3 stack](../s3) before**

Run command

```
aws cloudformation create-change-set \
    --stack-name TI-S3-Policies --change-set-name CreateChangeSet \
    --change-set-type CREATE \
    --template-body file://policies-s3-import.json \
    --capabilities  CAPABILITY_NAMED_IAM


aws cloudformation execute-change-set --change-set-name CreateChangeSet --stack-name TI-S3-Policies
```



