


### Notes on VPC Pairing


Hightlights from [what is VPC pairing](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)

AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection, and does not rely on a separate piece of physical hardware. There is no single point of failure for communication or a bandwidth bottleneck.

A VPC peering connection helps you to facilitate the transfer of data. For example, if you have more than one AWS account, you can peer the VPCs across those accounts to create a file sharing network. You can also use a VPC peering connection to allow other VPCs to access resources you have in one of your VPCs.

When you establish peering relationships between VPCs across different AWS Regions, resources in the VPCs (for example, EC2 instances and Lambda functions) in different AWS Regions can communicate with each other using private IP addresses, without using a gateway, VPN connection, or network appliance. The traffic remains in the private IP space. All inter-Region traffic is encrypted with no single point of failure, or bandwidth bottleneck.


#### Pricing for a VPC peering connection
There is no charge to create a VPC peering connection. All data transfer over a VPC Peering connection that stays within an Availability Zone (AZ) is free. Charges apply for data transfer over a VPC Peering connections that cross Availability Zones and Regions.


### Direct Connect

A VPC VPN Connection utilizes IPSec to establish encrypted network connectivity between your intranet and Amazon VPC over the Internet. VPN Connections can be configured in minutes and are a good solution if you have an immediate need, have low to modest bandwidth requirements, and can tolerate the inherent variability in Internet-based connectivity. 

AWS Direct Connect does not involve the Internet; instead, it uses dedicated, private network connections between your intranet and Amazon VPC.


### Gateway endpoints for Amazon DynamoDB
 
You can access Amazon DynamoDB from your VPC using gateway VPC endpoints. After you create the gateway endpoint, you can add it as a target in your route table for traffic destined from your VPC to DynamoDB.

There is no additional charge for using gateway endpoints.

Considerations
* A gateway endpoint is available only in the Region where you created it. Be sure to create your gateway endpoint in the same Region as your DynamoDB tables.
* If you're using the Amazon DNS servers, you must enable both DNS hostnames and DNS resolution for your VPC. If you're using your own DNS server, ensure that requests to DynamoDB resolve correctly to the IP addresses maintained by AWS.
* The outbound rules for the security group for instances that access DynamoDB through the gateway endpoint must allow traffic to DynamoDB. You can use the prefix list ID for DynamoDB as the destination in the outbound rule.
* DynamoDB does not support resource-based policies (for example, on tables). Access to DynamoDB is controlled through the endpoint policy and policies for individual users and roles.
* If you use AWS CloudTrail to log DynamoDB operations, the log files contain the private IP addresses of the EC2 instances in the service consumer VPC and the ID of the gateway endpoint for any requests performed through the endpoint.
* Gateway endpoints support only IPv4 traffic.




### What is AWS Site-to-Site VPN?

By default, instances that you launch into an Amazon VPC can't communicate with your own (remote) network. You can enable access to your remote network from your VPC by creating an AWS Site-to-Site VPN (Site-to-Site VPN) connection, and configuring routing to pass traffic through the connection.

Although the term VPN connection is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on-premises network. Site-to-Site VPN supports Internet Protocol security (IPsec) VPN connections.
The following are the key concepts for Site-to-Site VPN:
* VPN connection: A secure connection between your on-premises equipment and your VPCs.
* VPN tunnel: An encrypted link where data can pass from the customer network to or from AWS.
* Each VPN connection includes two VPN tunnels which you can simultaneously use for high availability.
* Customer gateway: An AWS resource which provides information to AWS about your customer gateway device.
* Customer gateway device: A physical device or software application on your side of the Site-to-Site VPN connection.
* Target gateway: A generic term for the VPN endpoint on the Amazon side of the Site-to-Site VPN connection.
* Virtual private gateway: A virtual private gateway is the VPN endpoint on the Amazon side of your Site-to-Site VPN connection that can be attached to a single VPC.
* Transit gateway: A transit hub that can be used to interconnect multiple VPCs and on-premises networks, and as a VPN endpoint for the Amazon side of the Site-to-Site VPN connection.



### Provide access to your DB instance in your VPC by creating a security group

VPC security groups provide access to DB instances in a VPC. They act as a firewall for the associated DB instance, controlling both inbound and outbound traffic at the DB instance level. DB instances are created by default with a firewall and a default security group that protect the DB instance.

(Optional) In Outbound rules, add rules for outbound traffic. By default, all outbound traffic is allowed.


### Amazon Route 53 weighted record sets

You can use Amazon Route 53 weighted record sets to distribute requests across your read replicas. Within a Route 53 hosted zone, create individual record sets for each DNS endpoint associated with your read replicas. Then, give them the same weight, and direct requests to the endpoint of the record set.