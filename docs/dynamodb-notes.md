## DynamoDB

### Intro

Highlights from [intro](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data. For more information, see DynamoDB encryption at rest.

With DynamoDB, you can create database tables that can store and retrieve any amount of data and serve any level of request traffic. You can scale up or scale down your tables' throughput capacity without downtime or performance degradation. You can use the AWS Management Console to monitor resource utilization and performance metrics.

DynamoDB provides on-demand backup capability. It allows you to create full backups of your tables for long-term retention and archival for regulatory compliance needs. For more information, see Using On-Demand backup and restore for DynamoDB.

You can create on-demand backups and enable point-in-time recovery for your Amazon DynamoDB tables. Point-in-time recovery helps protect your tables from accidental write or delete operations. With point-in-time recovery, you can restore a table to any point in time during the last 35 days. For more information, see Point-in-time recovery: How it works.

DynamoDB allows you to delete expired items from tables automatically to help you reduce storage usage and the cost of storing data that is no longer relevant. For more information, see Expiring items by using DynamoDB Time to Live (TTL).





###  DynamoDB supports two types of secondary indexes

This 5 min demo is really good - [DynamoDB Indexes: Making the right choice between GSI and LSI - Amazon DynamoDB Nuggets](https://www.youtube.com/watch?v=BkEu7zBWge8)

Also a few highlights and key options [from the AWS dev guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)

Global secondary index — An index with a partition key and a sort key that can be different from those on the base table. A global secondary index is considered "global" because queries on the index can span all of the data in the base table, across all partitions. A global secondary index is stored in its own partition space away from the base table and scales separately from the base table.

Local secondary index — An index that has the same partition key as the base table, but a different sort key. A local secondary index is "local" in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value.

Characteristic	Global secondary index	Local secondary index
Key Schema	
The primary key of a global secondary index can be either simple (partition key) or composite (partition key and sort key).	
The primary key of a local secondary index must be composite (partition key and sort key).

#### Key Attributes	
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

####   Read/write capacity mode

Amazon DynamoDB has two read/write capacity modes for processing reads and writes on your tables:
* On-demand 
* Provisioned (default, free-tier eligible)

The read/write capacity mode controls how you are charged for read and write throughput and how you manage capacity. You can set the read/write capacity mode when creating a table or you can change it later.

Secondary indexes inherit the read/write capacity mode from the base table. For more information, see Considerations when changing read/write Capacity Mode.


Amazon DynamoDB on-demand is a flexible billing option capable of serving thousands of requests per second without capacity planning. DynamoDB on-demand offers pay-per-request pricing for read and write requests so that you pay only for what you use.

When you choose on-demand mode, DynamoDB instantly accommodates your workloads as they ramp up or down to any previously reached traffic level. If a workload’s traffic level hits a new peak, DynamoDB adapts rapidly to accommodate the workload. Tables that use on-demand mode deliver the same single-digit millisecond latency, service-level agreement (SLA) commitment, and security that DynamoDB already offers. You can choose on-demand for both new and existing tables and you can continue using the existing DynamoDB APIs without changing code.

On-demand mode is a good option if any of the following are true:
* You create new tables with unknown workloads.
* You have unpredictable application traffic.
* You prefer the ease of paying for only what you use.


####  Read request units and write request units
For on-demand mode tables, you don't need to specify how much read and write throughput you expect your application to perform. DynamoDB charges you for the reads and writes that your application performs on your tables in terms of read request units and write request units.

DynamoDB read requests can be either strongly consistent, eventually consistent, or transactional.
* A strongly consistent read request of an item up to 4 KB requires one read request unit.
* An eventually consistent read request of an item up to 4 KB requires one-half read request unit.
* A transactional read request of an item up to 4 KB requires two read request units.

If you need to read an item that is larger than 4 KB, DynamoDB needs additional read request units. The total number of read request units required depends on the item size, and whether you want an eventually consistent or strongly consistent read. For example, if your item size is 8 KB, you require 2 read request units to sustain one strongly consistent read, 1 read request unit if you choose eventually consistent reads, or 4 read request units for a transactional read request.

Important - If you perform a read operation on an item that does not exist, DynamoDB will still consume read throughput as outlined above.

One write request unit represents one write for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB needs to consume additional write request units. Transactional write requests require 2 write request units to perform one write for items up to 1 KB. The total number of write request units required depends on the item size. For example, if your item size is 2 KB, you require 2 write request units to sustain one write request or 4 write request units for a transactional write request.


####   Peak traffic and scaling properties
DynamoDB tables using on-demand capacity mode automatically adapt to your application’s traffic volume. On-demand capacity mode instantly accommodates up to double the previous peak traffic on a table. For example, if your application’s traffic pattern varies between 25,000 and 50,000 strongly consistent reads per second where 50,000 reads per second is the previous traffic peak, on-demand capacity mode instantly accommodates sustained traffic of up to 100,000 reads per second. If your application sustains traffic of 100,000 reads per second, that peak becomes your new previous peak, enabling subsequent traffic to reach up to 200,000 reads per second.

If you need more than double your previous peak on table, DynamoDB automatically allocates more capacity as your traffic volume increases to help ensure that your workload does not experience throttling. However, throttling can occur if you exceed double your previous peak within 30 minutes. For example, if your application’s traffic pattern varies between 25,000 and 50,000 strongly consistent reads per second where 50,000 reads per second is the previously reached traffic peak, DynamoDB recommends spacing your traffic growth over at least 30 minutes before driving more than 100,000 reads per second.

####  Initial throughput for on-demand capacity mode
If you recently switched an existing table to on-demand capacity mode for the first time, or if you created a new table with on-demand capacity mode enabled, the table has the following previous peak settings, even though the table has not served traffic previously using on-demand capacity mode:

Following are examples of possible scenarios.

A provisioned table configured as 100 WCU and 100 RCU. When this table is switched to on-demand for the first time, DynamoDB will ensure it is scaled out to instantly sustain at least 4,000 write units/sec and 12,000 read units/sec.

A provisioned table configured as 8,000 WCU and 24,000 RCU. When this table is switched to on-demand, it will continue to be able to sustain at least 8,000 write units/sec and 24,000 read units/sec at any time.

A provisioned table configured with 8,000 WCU and 24,000 RCU, that consumed 6,000 write units/sec and 18,000 read units/sec for a sustained period. When this table is switched to on-demand, it will continue to be able to sustain at least 8,000 write units/sec and 24,000 read units/sec. The previous traffic may further allow the table to sustain much higher levels of traffic without throttling.

A table previously provisioned with 10,000 WCU and 10,000 RCU, but currently provisioned with 10 RCU and 10 WCU. When this table is switched to on-demand, it will be able to sustain at least 10,000 write units/sec and 10,000 read units/sec.

####   Table behavior while switching read/write capacity mode
When you switch a table from provisioned capacity mode to on-demand capacity mode, DynamoDB makes several changes to the structure of your table and partitions. This process can take several minutes. During the switching period, your table delivers throughput that is consistent with the previously provisioned write capacity unit and read capacity unit amounts. When switching from on-demand capacity mode back to provisioned capacity mode, your table delivers throughput consistent with the previous peak reached when the table was set to on-demand capacity mode.

####  Pre-warming a table for on-demand capacity mode
With on-demand capacity mode, the requests can burst up to double the previous peak on the table. Note that throttling can occur if the requests spikes to more than double the default capacity or the previously achieved peak request rate within 30 minutes. One solution is to pre-warm the tables to the anticipated peak capacity of the spike.

To pre-warm the table, follow these steps:

Make sure to check your account limits and confirm that you can reach the desired capacity in provisioned mode.

If you're pre-warming a table that already exists, or a new table in on-demand mode, start this process at least 24 hours before the anticipated peak. You can only switch between on-demand and provisioned mode once per 24 hours.

To pre-warm a table that's currently in on-demand mode, switch it to provisioned mode and wait 24 hours. Then go to the next step.

If you want to pre-warm a new table that's in provisioned mode, or has already been in provisioned mode for 24 hours, you can proceed to the next step without waiting.

Set the table's write throughput to the desired peak value, and keep it there for several minutes. You will incur cost from this high volume of throughput until you switch back to on-demand.

Switch to On-Demand capacity mode. This should sustain the provisioned throughput capacity values.

####  Provisioned mode
If you choose provisioned mode, you specify the number of reads and writes per second that you require for your application. You can use auto scaling to adjust your table’s provisioned capacity automatically in response to traffic changes. This helps you govern your DynamoDB use to stay at or below a defined request rate in order to obtain cost predictability.

Provisioned mode is a good option if any of the following are true:

You have predictable application traffic.

You run applications whose traffic is consistent or ramps gradually.

You can forecast capacity requirements to control costs.

Read capacity units and write capacity units
For provisioned mode tables, you specify throughput capacity in terms of read capacity units (RCUs) and write capacity units (WCUs):

One read capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. Transactional read requests require two read capacity units to perform one read per second for items up to 4 KB. If you need to read an item that is larger than 4 KB, DynamoDB must consume additional read capacity units. The total number of read capacity units required depends on the item size, and whether you want an eventually consistent or strongly consistent read. For example, if your item size is 8 KB, you require 2 read capacity units to sustain one strongly consistent read per second, 1 read capacity unit if you choose eventually consistent reads, or 4 read capacity units for a transactional read request. For more information, see Capacity unit consumption for reads.

Note
To learn more about DynamoDB read consistency models, see Read consistency.

One write capacity unit represents one write per second for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB must consume additional write capacity units. Transactional write requests require 2 write capacity units to perform one write per second for items up to 1 KB. The total number of write capacity units required depends on the item size. For example, if your item size is 2 KB, you require 2 write capacity units to sustain one write request per second or 4 write capacity units for a transactional write request. For more information, see Capacity unit consumption for writes.

Important - When calling DescribeTable on an on-demand table, read capacity units and write capacity units are set to 0.

If your application reads or writes larger items (up to the DynamoDB maximum item size of 400 KB), it will consume more capacity units.

For example, suppose that you create a provisioned table with 6 read capacity units and 6 write capacity units. With these settings, your application could do the following:
* Perform strongly consistent reads of up to 24 KB per second (4 KB × 6 read capacity units).
* Perform eventually consistent reads of up to 48 KB per second (twice as much read throughput).
* Perform transactional read requests of up to 12 KB per second.
* Write up to 6 KB per second (1 KB × 6 write capacity units).
* Perform transactional write requests of up to 3 KB per second.


Provisioned throughput is the maximum amount of capacity that an application can consume from a table or index. If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.

Throttling prevents your application from consuming too many capacity units. When a request is throttled, it fails with an HTTP 400 code (Bad Request) and a ProvisionedThroughputExceededException. The AWS SDKs have built-in support for retrying throttled requests (see Error retries and exponential backoff), so you do not need to write this logic yourself. For more information on how to resolve throttling issues, see Why is my Amazon DynamoDB table being throttled? 



####  DynamoDB auto scaling
DynamoDB auto scaling actively manages throughput capacity for tables and global secondary indexes. With auto scaling, you define a range (upper and lower limits) for read and write capacity units. You also define a target utilization percentage within that range. DynamoDB auto scaling seeks to maintain your target utilization, even as your application workload increases or decreases.

With DynamoDB auto scaling, a table or a global secondary index can increase its provisioned read and write capacity to handle sudden increases in traffic, without request throttling. When the workload decreases, DynamoDB auto scaling can decrease the throughput so that you don't pay for unused provisioned capacity.

Note If you use the AWS Management Console to create a table or a global secondary index, DynamoDB auto scaling is enabled by default.

####  Reserved capacity
As a DynamoDB customer, you can purchase reserved capacity in advance for tables that use the DynamoDB Standard table class, as described at Amazon DynamoDB Pricing. With reserved capacity, you pay a one-time upfront fee and commit to a minimum provisioned usage level over a period of time. Your reserved capacity is billed at the hourly reserved capacity rate. By reserving your read and write capacity units ahead of time, you realize significant cost savings on your provisioned capacity costs. Any capacity that you provision in excess of your reserved capacity is billed at standard provisioned capacity rates.

Reserved capacity discounts are applied first to the account that purchased the reserved capacity. Any unused reserved capacity discount is then applied to other accounts in the same AWS organization as the purchasing account. You can turn off Reserved Instance discount sharing on the Preferences page on the Billing and Cost Management console. For more information, see Turning off reserved instances and Savings Plans discount sharing.


###  Global tables: How it works

Global table concepts
* A global table is a collection of one or more replica tables, all owned by a single AWS account.
* A replica table (or replica, for short) is a single DynamoDB table that functions as a part of a global table. Each replica stores the same set of data items. Any given global table can only have one replica table per AWS Region.

When you create a DynamoDB global table, it consists of multiple replica tables (one per Region) that DynamoDB treats as a single unit. Every replica has the same table name and the same primary key schema. When an application writes data to a replica table in one Region, DynamoDB propagates the write to the other replica tables in the other AWS Regions automatically.

You can add replica tables to the global table so that it can be available in additional Regions.

With Version 2019.11.21 (Current), when you create a Global Secondary Index in one Region it is automatically replicated to the other Region(s) as well as automatically backfilled.

#### Common tasks
Common tasks for global tables work as follows.

You can delete a global table’s replica table the same as a regular table. This will stop replication to that Region and delete the table copy kept in that Region. You cannot sever the replication and have copies of the table exist as independent entities. You can instead copy the global table to a local table in that Region, and then delete the global replica for that Region.

Note - You won’t be able to delete a source table until at least 24 hours after it’s used to initiate a new Region. If you try to delete it too soon you will receive an error.

Conflicts can arise if applications update the same item in different Regions at about the same time. To help ensure eventual consistency, DynamoDB global tables use a “last writer wins” method to reconcile between concurrent updates. All the replicas will agree on the latest update and converge toward a state in which they all have identical data.

Note - There are several ways to avoid conflicts, including:
* Only allowing writes to the table in one Region.
* Routing user traffic to different Regions according to your write policies, to ensure there are no conflicts.
* Avoiding the use of non-idempotent updates such as Bookmark = Bookmark + 1, in favor of static updates such as Bookmark=25.
* Keep in mind that when you need to route writes or reads to one Region, it’s up to your application to ensure that flow is enforced.

#### Monitoring global tables
You can use CloudWatch to observe the metric ReplicationLatency. This tracks the elapsed time between when an item is written to a replica table, and when that item appears in another replica in the global table. It’s expressed in milliseconds and is emitted for every source-Region and destination-Region pair. This metric is kept at the source Region. This is the only CloudWatch metric provided by Global Tables v2.

The latencies you will observe depend on the distance between your chosen Regions, as well as other variables. Latencies in the 0.5 to 2.5 second range for Regions can be common within the same geographic area.

#### Time To Live (TTL)
You can use Time To Live (TTL) to specify an attribute name whose value indicates the time of expiration for the item. This value is given as a number in seconds since the start of the Unix epoch. After that time DynamoDB can delete the item without incurring write costs.

With global tables you configure TTL in one Region, and that setting is auto replicated to the other Region(s). When an item is deleted via a TTL rule, that work is performed without consuming Write Units on the source table - but the target table(s) will incur Replicated Write Unit costs.

Be aware that if the source and target table have very low Provisioned write capacity, this may cause throttling as the TTL deletes require write capacity.

#### Streams and transactions with global tables
Each global table produces an independent stream based on all its writes, regardless of the origination point for those writes. You can choose to consume this DynamoDB stream in one Region or in all Regions independently.

If you want processed local writes but not replicated writes, you can add your own region attribute to each item. Then you can use a Lambda event filter to invoke only the Lambda for writes in the local Region.

Transactional operations provide ACID (Atomicity, Consistency, Isolation, and Durability) guarantees ONLY within the Region where the write is made originally. Transactions are not supported across Regions in global tables.

For example, if you have a global table with replicas in the US East (Ohio) and US West (Oregon) Regions and perform a TransactWriteItems operation in the US East (Ohio) Region, you may observe partially completed transactions in US West (Oregon) Region as changes are replicated. Changes will only be replicated to other Regions once they have been committed in the source Region.

Note
Global tables “write around” DynamoDB Accelerator by updating DynamoDB directly. As a result DAX will not be aware it is holding stale data. The DAX cache will only be refreshed when the cache’s TTL expires.

Tags on global tables do not automatically propagate.

Read and write throughput
Global tables manage read and write throughput in the following ways.

The write capacity must be the same on all table instances across Regions.

With Version 2019.11.21 (Current), if the table is set to support autoscaling or is in on-demand mode then the write capacity is automatically kept in sync. This means that a write capacity change to one table replicates to the others.

Read capacity can differ between Regions because reads may not be equal. When adding a global replica to a table, the capacity of the source Region is propagated. After creation you can adjust the read capacity for one replica, and this new setting is not transferred to the other side.

Consistency and conflict resolution
Any changes made to any item in any replica table are replicated to all the other replicas within the same global table. In a global table, a newly written item is usually propagated to all replica tables within a second.

With a global table, each replica table stores the same set of data items. DynamoDB does not support partial replication of only some of the items.

An application can read and write data to any replica table. If your application only uses eventually consistent reads and only issues reads against one AWS Region, it will work without any modification. However, if your application requires strongly consistent reads, it must perform all of its strongly consistent reads and writes in the same Region. DynamoDB does not support strongly consistent reads across Regions. Therefore, if you write to one Region and read from another Region, the read response might include stale data that doesn't reflect the results of recently completed writes in the other Region.

If applications update the same item in different Regions at about the same time, conflicts can arise. To help ensure eventual consistency, DynamoDB global tables use a last writer wins reconciliation between concurrent updates, in which DynamoDB makes a best effort to determine the last writer. This is performed at the item level. With this conflict resolution mechanism, all the replicas will agree on the latest update and converge toward a state in which they all have identical data.

Availability and durability
If a single AWS Region becomes isolated or degraded, your application can redirect to a different Region and perform reads and writes against a different replica table. You can apply custom business logic to determine when to redirect requests to other Regions.

If a Region becomes isolated or degraded, DynamoDB keeps track of any writes that have been performed but have not yet been propagated to all of the replica tables. When the Region comes back online, DynamoDB resumes propagating any pending writes from that Region to the replica tables in other Regions. It also resumes propagating writes from other replica tables to the Region that is now back online.




### In-memory acceleration with DynamoDB Accelerator (DAX)

Amazon DynamoDB is designed for scale and performance. In most cases, the DynamoDB response times can be measured in single-digit milliseconds. However, there are certain use cases that require response times in microseconds. For these use cases, DynamoDB Accelerator (DAX) delivers fast response times for accessing eventually consistent data.

DAX is a DynamoDB-compatible caching service that enables you to benefit from fast in-memory performance for demanding applications. DAX addresses three core scenarios:

As an in-memory cache, DAX reduces the response times of eventually consistent read workloads by an order of magnitude from single-digit milliseconds to microseconds.

DAX reduces operational and application complexity by providing a managed service that is API-compatible with DynamoDB. Therefore, it requires only minimal functional changes to use with an existing application.

For read-heavy or bursty workloads, DAX provides increased throughput and potential operational cost savings by reducing the need to overprovision read capacity units. This is especially beneficial for applications that require repeated reads for individual keys.

DAX supports server-side encryption. With encryption at rest, the data persisted by DAX on disk will be encrypted. DAX writes data to disk as part of propagating changes from the primary node to read replicas. For more information, see DAX encryption at rest.

DAX also supports encryption in transit, ensuring that all requests and responses between your application and the cluster are encrypted by transport level security (TLS), and connections to the cluster can be authenticated by verification of a cluster x509 certificate. For more information, see DAX encryption in transit.




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


###  Expiring items by using DynamoDB Time to Live (TTL)

#### Intro to Time to Live
A few highlights and key options [from the AWS dev guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)

Amazon DynamoDB Time to Live (TTL) allows you to define a per-item timestamp to determine when an item is no longer needed. Shortly after the date and time of the specified timestamp, DynamoDB deletes the item from your table and only consumes throughput when the deletion is replicated to additional Regions. TTL is provided at no extra cost as a means to reduce stored data volumes by retaining only the items that remain current for your workload’s needs.

TTL is useful if you store items that lose relevance after a specific time. The following are example TTL use cases:
* Remove user or sensor data after one year of inactivity in an application.
* Archive expired items to an Amazon S3 data lake via Amazon DynamoDB Streams and AWS Lambda.
* Retain sensitive data for a certain amount of time according to contractual or regulatory obligations.

#### How it works: DynamoDB Time to Live (TTL)


Higlights from [How it works: DynamoDB Time to Live (TTL)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/howitworks-ttl.html)


The scanner background process compares the current time, in Unix epoch time format in seconds, to the value stored in the user-defined attribute of an item. If the attribute is a Number data type, the attribute’s value is a timestamp in Unix epoch time format in seconds, and the timestamp value is older than the current time but not five years older or more (in order to avoid a possible accidental deletion due to a malformed TTL value), then the item is set to expired. For specifics about formatting TTL attributes, see Formatting an item’s TTL attribute. A second background process scans for expired items and deletes them. Both processes take place automatically in the background, do not affect read or write traffic to the table, and do not have a monetary cost.

As items are deleted from the table, two background operations happen simultaneously:

Items are removed from any local secondary index and global secondary index in the same way as a DeleteItem operation. This operation comes at no extra cost.

A delete operation for each item enters the DynamoDB Stream, but is tagged as a system delete and not a regular delete. For more information about how to use this system delete, see DynamoDB Streams and Time to Live.

Important
1 TTL typically deletes expired items within a few days. Depending on the size and activity level of a table, the actual delete operation of an expired item can vary. Because TTL is meant to be a background process, the nature of the capacity used to expire and delete items via TTL is variable (but free of charge).
2 Items that have expired, but haven’t yet been deleted by TTL, still appear in reads, queries, and scans. If you do not want expired items in the result set, you must filter them out. To do this, use a filter expression that returns only items where the Time to Live expiration value is greater than the current time in epoch format. For more information, see Filter expressions for scan.
3 Items that are past their expiration, but have not yet been deleted can still be updated, and successful updates to change or remove the expiration attribute will be honored.




#### DynamoDB Streams and Time to Live

You can back up, or otherwise process, items that are deleted by Time to Live (TTL) by enabling Amazon DynamoDB Streams on the table and processing the streams records of the expired items.

The streams record contains a user identity field Records[<index>].userIdentity.

Items that are deleted by the Time to Live process after expiration have the following fields:

Records[<index>].userIdentity.type

"Service"

Records[<index>].userIdentity.principalId

"dynamodb.amazonaws.com"

The following JSON shows the relevant portion of a single streams record.


"Records": [
    {
        ...

        "userIdentity": {
            "type": "Service",
            "principalId": "dynamodb.amazonaws.com"
        }

        ...

    }
]
Using DynamoDB Streams and Lambda to archive TTL deleted items

Combining DynamoDB Time to Live (TTL), DynamoDB Streams, and AWS Lambda can help simplify archiving data, reduce DynamoDB storage costs, and reduce code complexity. Using Lambda as the stream consumer provides many advantages, most notably the cost reduction compared to other consumers such as Kinesis Client Library (KCL).


