


### How can I distribute read requests across multiple Amazon RDS read replicas?

Highlights from [AWS Knowledge Center](https://repost.aws/knowledge-center/requests-rds-read-replicas)

You can use Amazon Route 53 weighted record sets to distribute requests across your read replicas. Within a Route 53 hosted zone, create individual record sets for each DNS endpoint associated with your read replicas. Then, give them the same weight, and direct requests to the endpoint of the record set.


### Storage autoscaling 

With storage autoscaling enabled, when Amazon RDS detects that you are running out of free database space it automatically scales up your storage. Amazon RDS starts a storage modification for an autoscaling-enabled DB instance when these factors apply:
1 Free available space is less than or equal to 10 percent of the allocated storage.
2 The low-storage condition lasts at least five minutes.
3 At least six hours have passed since the last storage modification, or storage optimization has completed on the instance, whichever is longer.

Storage auto scaling is checked by default when creating a new Amazon RDS DB instance, but it can be customized.


The default storage setting for an Amazon RDS MySQL instance is set to 1000, but the maximum is 16,384 GiB.

To enable storage autoscaling for a new DB instance, use the AWS CLI command create-db-instance. Set the following parameter:

> --max-allocated-storage – Turns on storage autoscaling and sets the upper limit on storage size, in gibibytes.







### Backup retention period

You can set the backup retention period when you create a DB instance or Multi-AZ DB cluster. If you don't set the backup retention period, the default backup retention period is one day if you create the database using the Amazon RDS API or the AWS CLI. The default backup retention period is seven days if you create the database using the console.

After you create a DB instance or cluster, you can modify the backup retention period. You can set the backup retention period of a DB instance to between 0 and 35 days. Setting the backup retention period to 0 disables automated backups. You can set the backup retention period of a multi-AZ DB cluster to between 1 and 35 days. Manual snapshot limits (100 per Region) don't apply to automated backups.



### Fast failover with Amazon Aurora PostgreSQL

Following, you can learn how to make sure that failover occurs as fast as possible. To recover quickly after failover, you can use cluster cache management for your Aurora PostgreSQL DB cluster. For more information, see Fast recovery after failover with cluster cache management for Aurora PostgreSQL.

Some of the steps that you can take to make failover perform fast include the following:
* Set Transmission Control Protocol (TCP) keepalives with short time frames, to stop longer running queries before the read timeout expires if there's a failure.
* Set timeouts for Java Domain Name System (DNS) caching aggressively. Doing this helps ensure the Aurora read-only endpoint can properly cycle through read-only nodes on later connection attempts.
* Set the timeout variables used in the JDBC connection string as low as possible. Use separate connection objects for short- and long-running queries.
* Use the read and write Aurora endpoints that are provided to connect to the cluster.
* Use RDS API operations to test application response on server-side failures. Also, use a packet dropping tool to test application response for client-side failures.
* Use the AWS JDBC Driver for PostgreSQL to take full advantage of the failover capabilities of Aurora PostgreSQL.



### Working with Amazon RDS event notification

Amazon RDS uses the Amazon Simple Notification Service (Amazon SNS) to provide notification when an Amazon RDS event occurs. These notifications can be in any notification form supported by Amazon SNS for an AWS Region, such as an email, a text message, or a call to an HTTP endpoint.

RDS resources eligible for event subscription
* DB instance
* DB snapshot
* DB parameter group
* DB security group
* RDS Proxy
* Custom engine version



## Enabling cross-Region automated backups

You can enable backup replication on new or existing DB instances using the Amazon RDS console. You can also use the start-db-instance-automated-backups-replication AWS CLI command or the StartDBInstanceAutomatedBackupsReplication RDS API operation. You can replicate up to 20 backups to each destination AWS Region for each AWS account.


aws rds start-db-instance-automated-backups-replication \
--region us-east-1 \
--source-db-instance-arn "arn:aws:rds:us-west-2:123..:db:mydatabase" \
--kms-key-id "arn:aws:kms:us-east-1:123..:key/AKa.." \
--backup-retention-period 7



Note in the acloud.guru test exam

> Manually create an RDS snapshot and copy it to the secondary region: Because the RTO is not aggressive, the manual backup will be fine. Additionally, unlike automated backups, manual snapshots aren't subject to the backup retention period. Snapshots don't expire.


### Amazon RDS Database Deletion Protection

When you request the deletion of a database instance with deletion protection in the AWS Console, you are blocked and may not continue without first modifying the instance and disabling deletion protection.


### IAM database authentication for MariaDB, MySQL, and PostgreSQL

An authentication token is a unique string of characters that Amazon RDS generates on request. Authentication tokens are generated using AWS Signature Version 4. Each token has a lifetime of 15 minutes. 


### Overview of Database Activity Streams

As an Amazon RDS database administrator, you need to safeguard your database and meet compliance and regulatory requirements. One strategy is to integrate database activity streams with your monitoring tools. In this way, you monitor and set alarms for auditing activity in your database.

Security threats are both external and internal. To protect against internal threats, you can control administrator access to data streams by configuring the Database Activity Streams feature. Amazon RDS DBAs don't have access to the collection, transmission, storage, and processing of the streams.


### Elastic Beanstalk and DB decoupling

 At this point you have the option to decouple the database from your Elastic Beanstalk environment to move towards a configuration that offers greater flexibility. The decoupled database can remain operational as an external Amazon RDS database instance. The health of the environment isn't affected by decoupling the database. If you need to terminate the environment, you can do so and also choose the option to keep the database available and operational outside of Elastic Beanstalk.

