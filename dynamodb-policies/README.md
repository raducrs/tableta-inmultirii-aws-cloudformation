
Named IAM policies have special conditions when beeing build using ```Cloudformation```, namely that they can not be updated or follow the drift. We will build them from scratch.

**Warning: you must create policy names that are at least 20 characters longs (the -Policy suffix was added for this reason)**

**You must have deployed the [dynamodb stack](../dynamodb) before**

Run command

```
aws cloudformation create-change-set \
    --stack-name TI-DB-Policies --change-set-name CreateChangeSet \
    --change-set-type CREATE \
    --template-body file://policies-db-import.json \
    --capabilities  CAPABILITY_NAMED_IAM


aws cloudformation execute-change-set --change-set-name CreateChangeSet --stack-name TI-DB-Policies
```

---
You can use this command to list managed policies ```aws iam list-policies --scope Local```



