Athena.md

### What is Amazon Athena?

Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service (Amazon S3) using standard SQL. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad-hoc queries and get results in seconds.

Amazon Athena also makes it easy to interactively run data analytics using Apache Spark without having to plan for, configure, or manage resources. When you run Apache Spark applications on Athena, you submit Spark code for processing and receive the results directly. Use the simplified notebook experience in Amazon Athena console to develop Apache Spark applications using Python or Athena notebook APIs.

Athena SQL and Apache Spark on Amazon Athena are serverless, so there is no infrastructure to set up or manage, and you pay only for the queries you run. Athena scales automatically—running queries in parallel—so results are fast, even with large datasets and complex queries.



### AWS service integrations with Athena


You can use Athena to query data from the AWS services listed in this section. To see the Regions that each service supports, see Regions and endpoints in the Amazon Web Services General Reference.

AWS services integrated with Athena
AWS CloudFormation
Amazon CloudFront
AWS CloudTrail
Elastic Load Balancing
AWS Glue Data Catalog
AWS Identity and Access Management (IAM)
Amazon QuickSight
Amazon S3 Inventory
AWS Step Functions
AWS Systems Manager Inventory
Amazon Virtual Private Cloud

For information about each integration, see the following sections.

AWS CloudFormation
Capacity reservation
Reference topic: AWS::Athena::CapacityReservation in the AWS CloudFormation User Guide

Specifies a capacity reservation with the provided name and number of requested data processing units. For more information, see Managing query processing capacity in the Amazon Athena User Guide and CreateCapacityReservation in the Amazon Athena API Reference.

Data catalog
Reference topic: AWS::Athena::DataCatalog in the AWS CloudFormation User Guide

Specify an Athena data catalog, including a name, description, type, parameters, and tags. 

Named query
Reference topic: AWS::Athena::NamedQuery in the AWS CloudFormation User Guide

Specify named queries with AWS CloudFormation and run them in Athena. Named queries allow you to map a query name to a query and then run it as a saved query from the Athena console. 

Prepared statement
Reference topic: AWS::Athena::PreparedStatement in the AWS CloudFormation User Guide

Specifies a prepared statement for use with SQL queries in Athena. A prepared statement contains parameter placeholders whose values are supplied at execution time. 

Workgroup
Reference topic: AWS::Athena::WorkGroup in the AWS CloudFormation User Guide

Specify Athena workgroups using AWS CloudFormation. 


Reference.
Amazon CloudFront
Reference topic: Querying Amazon CloudFront logs

Use Athena to query Amazon CloudFront logs. 

AWS CloudTrail
Reference topic: Querying AWS CloudTrail logs

Using Athena with CloudTrail logs is a powerful way to enhance your analysis of AWS service activity. For example, you can use queries to identify trends and further isolate activity by attribute, such as source IP address or user. You can create tables for querying logs directly from the CloudTrail console, and use those tables to run queries in Athena. 

Elastic Load Balancing
Reference topic: Querying Application Load Balancer logs

Querying Application Load Balancer logs allows you to see the source of traffic, latency, and bytes transferred to and from Elastic Load Balancing instances and backend applications. 

Reference topic: Querying Classic Load Balancer logs

Query Classic Load Balancer logs to analyze and understand traffic patterns to and from Elastic Load Balancing instances and backend applications. You can see the source of traffic, latency, and bytes transferred. For more information, see Creating the Table for ELB Logs.

AWS Glue Data Catalog
Reference topic: Integration with AWS Glue

Athena integrates with the AWS Glue Data Catalog, which offers a persistent metadata store for your data in Amazon S3. This allows you to create tables and query data in Athena based on a central metadata store available throughout your Amazon Web Services account and integrated with the ETL and data discovery features of AWS Glue. 

AWS Identity and Access Management (IAM)
Reference topic: Actions for Amazon Athena

You can use Athena API actions in IAM permission policies.

Amazon QuickSight
Reference topic: Connecting to Amazon Athena with ODBC and JDBC drivers

Athena integrates with Amazon QuickSight for easy data visualization. You can use Athena to generate reports or to explore data with business intelligence tools or SQL clients connected with a JDBC or an ODBC driver. For more information about Amazon QuickSight, see What is Amazon QuickSight in the Amazon QuickSight User Guide. 

Amazon S3 Inventory
Reference topic: Querying inventory with Athena in the Amazon Simple Storage Service User Guide

You can use Amazon Athena to query Amazon S3 inventory using standard SQL. You can use Amazon S3 inventory to audit and report on the replication and encryption status of your objects for business, compliance, and regulatory needs. 

AWS Step Functions
Reference topic: Call Athena with Step Functions in the AWS Step Functions Developer Guide

Call Athena with AWS Step Functions. AWS Step Functions can control select AWS services directly using the Amazon States Language. You can use Step Functions with Athena to start and stop query execution, get query results, run ad-hoc or scheduled data queries, and retrieve results from data lakes in Amazon S3. The Step Functions role must have permissions to use Athena. 

Video: Orchestrate Amazon Athena Queries using AWS Step Functions
The following video demonstrates how to use Amazon Athena and AWS Step Functions to run a regularly scheduled Athena query and generate a corresponding report.


AWS Systems Manager Inventory
Reference topic: Querying inventory data from multiple regions and accounts in the AWS Systems Manager User Guide

AWS Systems Manager Inventory integrates with Amazon Athena to help you query inventory data from multiple AWS Regions and accounts. 

Amazon Virtual Private Cloud
Reference topic: Querying Amazon VPC flow logs

Amazon Virtual Private Cloud flow logs capture information about the IP traffic going to and from network interfaces in a VPC. Query the logs in Athena to investigate network traffic patterns and identify threats and risks across your Amazon VPC network. 