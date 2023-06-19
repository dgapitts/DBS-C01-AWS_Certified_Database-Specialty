


###  DynamoDB supports two types of secondary indexes

This 5 min demo is really good - [DynamoDB Indexes: Making the right choice between GSI and LSI - Amazon DynamoDB Nuggets](https://www.youtube.com/watch?v=BkEu7zBWge8)

Also a few highlights and key options [from the AWS dev guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)

Global secondary index — An index with a partition key and a sort key that can be different from those on the base table. A global secondary index is considered "global" because queries on the index can span all of the data in the base table, across all partitions. A global secondary index is stored in its own partition space away from the base table and scales separately from the base table.

Local secondary index — An index that has the same partition key as the base table, but a different sort key. A local secondary index is "local" in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value.

Characteristic	Global secondary index	Local secondary index
Key Schema	
The primary key of a global secondary index can be either simple (partition key) or composite (partition key and sort key).	
The primary key of a local secondary index must be composite (partition key and sort key).

Key Attributes	
The index partition key and sort key (if present) can be any base table attributes of type string, number, or binary.	
The partition key of the index is the same attribute as the partition key of the base table. The sort key can be any base table attribute of type string, number, or binary.
Size Restrictions Per Partition Key Value	
There are no size restrictions for global secondary indexes.	
For each partition key value, the total size of all indexed items must be 10 GB or less.

Online Index Operations	Global secondary indexes can be created at the same time that you create a table. You can also add a new global secondary index to an existing table, or delete an existing global secondary index. For more information, see Managing Global Secondary Indexes.	Local secondary indexes are created at the same time that you create a table. You cannot add a local secondary index to an existing table, nor can you delete any local secondary indexes that currently exist.
Queries and Partitions	A global secondary index lets you query over the entire table, across all partitions.	A local secondary index lets you query over a single partition, as specified by the partition key value in the query.
Read Consistency	Queries on global secondary indexes support eventual consistency only.	When you query a local secondary index, you can choose either eventual consistency or strong consistency.
Provisioned Throughput Consumption	Every global secondary index has its own provisioned throughput settings for read and write activity. Queries or scans on a global secondary index consume capacity units from the index, not from the base table. The same holds true for global secondary index updates due to table writes. A global secondary index associated with global tables consumes write capacity units.	Queries or scans on a local secondary index consume read capacity units from the base table. When you write to a table its local secondary indexes are also updated, and these updates consume write capacity units from the base table. A local secondary index associated with global tables consumes replicated write capacity units.
Projected Attributes	With global secondary index queries or scans, you can only request the attributes that are projected into the index. DynamoDB does not fetch any attributes from the table.	If you query or scan a local secondary index, you can request attributes that are not projected in to the index. DynamoDB automatically fetches those attributes from the table.


### DynamoDB PointInTimeRecovery highlights

A few highlights and key options [from the AWS dev guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.Tutorial.html)


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

