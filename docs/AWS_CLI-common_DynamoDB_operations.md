## AWS CLI - common DynamoDB operations

###

```
Table name: PetInventory
Primary key:
Partition key: pet_species, String
Check Add sort key
Sort key: pet_id, Number
```

### Create a DynamoDB Table 
```
aws dynamodb\
    create-table\
        --table-name PetInventory\
        --attribute-definitions\
            AttributeName=pet_species,AttributeType=S\
            AttributeName=pet_id,AttributeType=N\
        --key-schema\
            AttributeName=pet_species,KeyType=HASH\
            AttributeName=pet_id,KeyType=RANGE\
        --billing-mode PAY_PER_REQUEST
```

### aws dynamodb list-tables and describe-table

```
aws dynamodb list-tables
```

and

```
aws dynamodb describe-table --table-name PetInventory
```

next adding a query (on table metadata/setup)
```
aws dynamodb describe-table --table-name PetInventory --query 'Table.{PartitionKey:KeySchema[0].AttributeName,PartKeyType:AttributeDefinitions[1].AttributeType,SortKey:KeySchema[1].AttributeName,SortKeyType:AttributeDefinitions[0].AttributeType,BillingMode:BillingModeSummary.BillingMode}'
```
returns
```
{
    "SortKeyType": "N",
    "PartitionKey": "pet_species",
    "BillingMode": "PAY_PER_REQUEST",
    "SortKey": "pet_id",
    "PartKeyType": "S"
}
```


### Delete the table:

```
aws dynamodb delete-table --table-name PetInventory
```


