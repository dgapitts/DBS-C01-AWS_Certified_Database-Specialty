

### dynamodb PointInTimeRecovery highlights

A few highlights and key options [from the AWS dev guide] https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.Tutorial.html


You can restore the table to a different AWS Region from where the source table resides.

Note
The sse-specification-override parameter is mandatory for cross-Region restores but optional for restores to the same Region as the source table.
The source-table-arn parameter must be provided for cross-Region restores.
When performing a cross-Region restore from the command line, you must set the default AWS Region to the desired destination Region. To learn more, see Command line options in the AWS Command Line Interface User Guide.

```
aws dynamodb restore-table-to-point-in-time \
    --source-table-arn arn:aws:dynamodb:us-east-1:123456789012:table/Music \
    --target-table-name MusicMinutesAgo \
    --use-latest-restorable-time \
    --sse-specification-override Enabled=true,SSEType=KMS,KMSMasterKeyId=abcd1234-abcd-1234-a123-ab1234a1b234
```  
You can override the billing mode and the provisioned throughput for the restored table.

```
aws dynamodb restore-table-to-point-in-time \
    --source-table-name Music \
    --target-table-name MusicMinutesAgo \
    --use-latest-restorable-time \
    --billing-mode-override PAY_PER_REQUEST
```
You can exclude some or all secondary indexes from being created on the restored table.

Note
Restores can be faster and more cost-efficient if you exclude some or all secondary indexes from being created on the new restored table.


```
aws dynamodb restore-table-to-point-in-time \
    --source-table-name Music \
    --target-table-name MusicMinutesAgo \
    --use-latest-restorable-time \
    --global-secondary-index-override '[]'
```

You can use a combination of different overrides. For example, you can use a single global secondary index and change provisioned throughput at the same time, as follows.

```
aws dynamodb restore-table-to-point-in-time \
    --source-table-name Music \
    --target-table-name MusicMinutesAgo \
    --billing-mode-override PROVISIONED \
    --provisioned-throughput-override ReadCapacityUnits=100,WriteCapacityUnits=100 \
    --global-secondary-index-override IndexName=singers-index,KeySchema=["{AttributeName=SingerName,KeyType=HASH}"],Projection="{ProjectionType=KEYS_ONLY}",ProvisionedThroughput="{ReadCapacityUnits=50,WriteCapacityUnits=50}" \
    --sse-specification-override Enabled=true,SSEType=KMS \
    --use-latest-restorable-time
```

