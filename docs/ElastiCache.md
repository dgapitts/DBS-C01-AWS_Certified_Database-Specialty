### Overview of ElastiCache for Redis

Highlights from [Redis User Guide - Overview](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/WhatIs.html)


Existing applications that use Redis can use ElastiCache with almost no modification. Your applications simply need information about the host names and port numbers of the ElastiCache nodes that you have deployed.

ElastiCache for Redis has multiple features that help make the service more reliable for critical production deployments:

Automatic detection of and recovery from cache node failures.

Multi-AZ for a failed primary cluster to a read replica, in Redis clusters that support replication.

Redis (cluster mode enabled) supports partitioning your data across up to 500 shards.

For Redis version 3.2 and later, all versions support encryption in transit and encryption at rest encryption with authentication. This support helps you build HIPAA-compliant applications.

Flexible Availability Zone placement of nodes and clusters for increased fault tolerance.

Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS. This integration helps provide a managed in-memory caching solution that is high-performance and highly secure.

ElastiCache for Redis manages backups, software patching, automatic failure detection, and recovery.

You can have automated backups performed when you need them, or manually create your own backup snapshot. You can use these backups to restore a cluster. The ElastiCache for Redis restore process works reliably and efficiently.

You can get high availability with a primary instance and a synchronous secondary instance that you can fail over to when problems occur. You can also use read replicas to increase read scaling.

You can control access to your ElastiCache for Redis clusters by using AWS Identity and Access Management to define users and permissions. You can also help protect your clusters by putting them in a virtual private cloud (VPC).

By using the Global Datastore for Redis feature, you can work with fully managed, fast, reliable, and secure replication across AWS Regions. Using this feature, you can create cross-Region read replica clusters for ElastiCache for Redis to enable low-latency reads and disaster recovery across AWS Regions.

Data tiering provides a price-performance option for Redis workloads by utilizing lower-cost solid state drives (SSDs) in each cluster node in addition to storing data in memory. It is ideal for workloads that access up to 20 percent of their overall dataset regularly, and for applications that can tolerate additional latency when accessing data on SSD. For more information, see Data tiering.



### Redis - Preparation


Highlights from [Redis User Guide - Preparation](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/cluster-create-determine-requirements.html)




Knowing the answers to the following questions helps make creating your cluster go smoother:
1 Which node instance type do you need? For guidance on choosing an instance node type, see Choosing your node size.

2 Will you launch your cluster in a virtual private cloud (VPC) based on Amazon VPC?

Important
If you're going to launch your cluster in a VPC, make sure to create a subnet group in the same VPC before you start creating a cluster. For more information, see Subnets and subnet groups.

ElastiCache is designed to be accessed from within AWS using Amazon EC2. However, if you launch in a VPC based on Amazon VPC and your cluster is in an VPC, you can provide access from outside AWS. For more information, see Accessing ElastiCache resources from outside AWS.

3 Do you need to customize any parameter values?

If you do, create a custom parameter group. For more information, see Creating a parameter group.

If you're running Redis, consider setting reserved-memory or reserved-memory-percent. For more information, see Managing Reserved Memory.

4 Do you need to create your own security group or VPC security group?

For more information, see Security groups: EC2-Classic and Security in Your VPC.

5 How do you intend to implement fault tolerance?

For more information, see Mitigating Failures.


### Understanding Redis replication




Highlights from [Redis User Guide - Replication](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Replication.Redis-RedisCluster.html)




Redis implements replication in two ways:

With a single shard that contains all of the cluster's data in each node—Redis (cluster mode disabled)

With data partitioned across up to 500 shards—Redis (cluster mode enabled)

Each shard in a replication group has a single read/write primary node and up to 5 read-only replica nodes. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed.





### Redis cluster mode what should I choose?




More highlights from [Redis User Guide - Replication](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Replication.Redis-RedisCluster.html)





When choosing between Redis (cluster mode disabled) or Redis (cluster mode enabled), consider the following factors:

Scaling v. partitioning – Business needs change. You need to either provision for peak demand or scale as demand changes. Redis (cluster mode disabled) supports scaling. You can scale read capacity by adding or deleting replica nodes, or you can scale capacity by scaling up to a larger node type. Both of these operations take time. For more information, see Scaling Redis (Cluster Mode Disabled) clusters with replica nodes.

 

Redis (cluster mode enabled) supports partitioning your data across up to 500 node groups. You can dynamically change the number of shards as your business needs change. One advantage of partitioning is that you spread your load over a greater number of endpoints, which reduces access bottlenecks during peak demand. Additionally, you can accommodate a larger data set since the data can be spread across multiple servers. For information on scaling your partitions, see Scaling clusters in Redis (Cluster Mode Enabled).

 

Node size v. number of nodes – Because a Redis (cluster mode disabled) cluster has only one shard, the node type must be large enough to accommodate all the cluster's data plus necessary overhead. On the other hand, because you can partition your data across several shards when using a Redis (cluster mode enabled) cluster, the node types can be smaller, though you need more of them. For more information, see Choosing your node size.

 

Reads v. writes – If the primary load on your cluster is applications reading data, you can scale a Redis (cluster mode disabled) cluster by adding and deleting read replicas. However, there is a maximum of 5 read replicas. If the load on your cluster is write-heavy, you can benefit from the additional write endpoints of a Redis (cluster mode enabled) cluster with multiple shards.



###   Improving backup performance
The following are guidelines for improving backup performance.
* Set the reserved-memory-percent parameter – To mitigate excessive paging, we recommend that you set the reserved-memory-percent parameter. This parameter prevents Redis from consuming all of the node's available memory, and can help reduce the amount of paging. You might also see performance improvements by simply using a larger node. For more information about the reserved-memory and reserved-memory-percent parameters, see Managing Reserved Memory.
* Create backups from a read replica – If you are running Redis in a node group with more than one node, you can take a backup from the primary node or one of the read replicas. Because of the system resources required during BGSAVE, we recommend that you create backups from one of the read replicas. While the backup is being created from the replica, the primary node remains unaffected by BGSAVE resource requirements. The primary node can continue serving requests without slowing down.


### Backup constraints
Consider the following constraints when planning or making backups:
* At this time, backup and restore are supported only for clusters running on Redis.
* For Redis (cluster mode disabled) clusters, backup and restore aren't supported on cache.t1.micro nodes. All other cache node types are supported.
* For Redis (cluster mode enabled) clusters, backup and restore are supported for all node types.
* During any contiguous 24-hour period, you can create no more than 20 manual backups per node in the cluster.
* Redis (cluster mode enabled) only supports taking backups on the cluster level (for the API or CLI, the replication group level). Redis (cluster mode enabled) doesn't support taking backups at the shard level (for the API or CLI, the node group level).
* During the backup process, you can't run any other API or CLI operations on the cluster.
* If using clusters with data tiering, you cannot export a backup to Amazon S3.
* You can restore a backup of a cluster using the r6gd node type only to clusters using the r6gd node type.


### Redis vs Memcached

Sub-millisecond latency
Both Redis and Memcached support sub-millisecond response times. By storing data in-memory they can read data more quickly than disk based databases.

Developer ease of use
Both Redis and Memcached are syntactically easy to use and require a minimal amount of code to integrate into your application.

Data partitioning
Both Redis and Memcached allow you to distribute your data among multiple nodes. This allows you to scale out to better handle more data when demand grows.

Support for a broad set of programming languages
Both Redis and Memcached have many open-source clients available for developers. Supported languages include Java, Python, PHP, C, C++, C#, JavaScript, Node.js, Ruby, Go and many others.

Advanced data structures
In addition to strings, Redis supports lists, sets, sorted sets, hashes, bit arrays, and hyperloglogs. Applications can use these more advanced data structures to support a variety of use cases. For example, you can use Redis Sorted Sets to easily implement a game leaderboard that keeps a list of players sorted by their rank.

Multithreaded architecture
Since Memcached is multithreaded, it can make use of multiple processing cores. This means that you can handle more operations by scaling up compute capacity.

Snapshots
With Redis you can keep your data on disk with a point in time snapshot which can be used for archiving or recovery.

Replication
Redis lets you create multiple replicas of a Redis primary. This allows you to scale database reads and to have highly available clusters.

Transactions
Redis supports transactions which let you execute a group of commands as an isolated and atomic operation.

Pub/Sub
Redis supports Pub/Sub messaging with pattern matching which you can use for high performance chat rooms, real-time comment streams, social media feeds, and server intercommunication.

Lua scripting
Redis allows you to execute transactional Lua scripts. Scripts can help you boost performance and simplify your application.

Geospatial support
Redis has purpose-built commands for working with real-time geospatial data at scale. You can perform operations like finding the distance between two elements (for example people or places) and finding all elements within a given distance of a point.