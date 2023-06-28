


### Aurora Auto Scaling

You define and apply a scaling policy to an Aurora DB cluster. The scaling policy defines the minimum and maximum number of Aurora Replicas that Aurora Auto Scaling can manage. Based on the policy, Aurora Auto Scaling adjusts the number of Aurora Replicas up or down in response to actual workloads, determined by using Amazon CloudWatch metrics and target values.


#### Aurora Auto Scaling has the following components:

A service-linked role
A target metric
Minimum and maximum capacity
A cooldown period

#### Enable or disable scale-in activities
You can enable or disable scale-in activities for a policy. Enabling scale-in activities allows the scaling policy to delete Aurora Replicas. When scale-in activities are enabled, the scale-in cooldown period in the scaling policy applies to scale-in activities. Disabling scale-in activities prevents the scaling policy from deleting Aurora Replicas.

Note: Scale-out activities are always enabled so that the scaling policy can create Aurora Replicas as needed.



### Backtracking limitations

The following limitations apply to backtracking:

Backtracking is only available for DB clusters that were created with the Backtrack feature enabled. You can enable the Backtrack feature when you create a new DB cluster or restore a snapshot of a DB cluster. For DB clusters that were created with the Backtrack feature enabled, you can create a clone DB cluster with the Backtrack feature enabled. Currently, you can't perform backtracking on DB clusters that were created with the Backtrack feature disabled.

The limit for a backtrack window is 72 hours.

Backtracking affects the entire DB cluster. For example, you can't selectively backtrack a single table or a single data update.

Backtracking isn't supported with binary log (binlog) replication. Cross-Region replication must be disabled before you can configure or use backtracking.

You can't backtrack a database clone to a time before that database clone was created. However, you can use the original database to backtrack to a time before the clone was created. For more information about database cloning, see Cloning a volume for an Amazon Aurora DB cluster.

Backtracking causes a brief DB instance disruption. You must stop or pause your applications before starting a backtrack operation to ensure that there are no new read or write requests. During the backtrack operation, Aurora pauses the database, closes any open connections, and drops any uncommitted reads and writes. It then waits for the backtrack operation to complete.

You can't restore a cross-Region snapshot of a backtrack-enabled cluster in an AWS Region that doesn't support backtracking.

If you perform an in-place upgrade for a backtrack-enabled cluster from Aurora MySQL version 2 to version 3, you can't backtrack to a point in time before the upgrade happened.

Backtrack is not available for Aurora PostgreSQL.


### Cluster cache management
The cluster cache management (CCM) feature improves the performance of the new primary/writer instance after failover occurs. The replica preemptively reads frequently accessed buffers cached from the  primary/writer instance. With CCM, you can designate a specific Aurora PostgreSQL replica as the failover target. CCM ensures that data in the designated replica’s cache is synchronized with the data in the primary DB instance’s cache.



### Load balancing with the reader endpoint

Because the reader endpoint contains all Aurora Replicas, it can provide DNS-based, round robin load balancing for new connections. Every time you resolve the reader endpoint, you'll get an instance IP that you can connect to, chosen in round robin fashion.

