## AWS python boto3 - common DynamoDB operations

### Spec

```
Table name: PetInventory
Primary key:
Partition key: pet_species, String
Check Add sort key
Sort key: pet_id, Number
```

### Create a DynamoDB Table 
```
import boto3
ddb = boto3.client('dynamodb')
createResponse = ddb.create_table(
    AttributeDefinitions=[
        {
            'AttributeName':'pet_species',
            'AttributeType': 'S',
        },
        {
            'AttributeName':'pet_id',
            'AttributeType':'N'
        }
    ],
    KeySchema=[
        {
            'AttributeName':'pet_species',
            'KeyType':'HASH'
        },
        {
            'AttributeName':'pet_id',
            'KeyType':'RANGE'
        },
    ],
    BillingMode = 'PAY_PER_REQUEST',
    TableName='PetInventory'
)

print(createResponse)
createResponse['TableDescription']
createResponse['TableDescription']['AttributeDefinitions']
createResponse['TableDescription']['KeySchema']
createResponse['TableDescription']['TableStatus']
createResponse['TableDescription']['BillingModeSummary']
createResponse['TableDescription']['TableName']
```

### describe_table
```
statusResponse = ddb.describe_table(TableName='PetInventory')
statusResponse['Table']['TableStatus']
statusResponse['Table']
```

### list_tables
```
listResponse = ddb.list_tables()
listResponse['TableNames']
```

### delete_table
```
deleteResponse = ddb.delete_table(TableName = 'PetInventory')
deleteResponse['TableDescription']
```


