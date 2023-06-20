redis.md



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