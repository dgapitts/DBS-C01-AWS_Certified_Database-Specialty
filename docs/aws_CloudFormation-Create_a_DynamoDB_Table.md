
## Create a DynamoDB Table with CloudFormation

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "PetTable": {
            "Type": "AWS::DynamoDB::Table",
            "Properties": {
                "AttributeDefinitions" : [
                    {
                        "AttributeName": "pet_species",
                        "AttributeType": "S"
                    },
                    {
                        "AttributeName": "pet_id",
                        "AttributeType": "N"
                    }
                ],
                "KeySchema" : [
                    {
                        "AttributeName": "pet_species",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "pet_id",
                        "KeyType": "RANGE"
                    }
                ],
                "TableName": "PetInventory",
                "BillingMode": "PAY_PER_REQUEST"
            }
        }
    }
}
```

