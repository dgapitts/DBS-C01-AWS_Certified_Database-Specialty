### Neptune lab steps

### AWS GUI based setup

'''
Download the neptune-data.rdf file to your local machine.

Create an S3 Bucket and Grant Access
In the AWS Management Console, navigate to S3.

Click + Create bucket.

Set the following values:

Bucket name: "neptune-import<INSERT_CURRENT_DATE_HERE>"
Region: US East (N. Virginia)
Copy the S3 bucket name into a text file for later use.

Select Next.

Under Tags, enter "name" in Key and "neptune-import" in Value.

Select Next > Next > Create bucket.

Click on the bucket's name.

Select Upload.

Select Add files.

Select the neptune-data.rdf file from your machine and click Open.

Select Upload.

At the top left, click Services > IAM

In the lefthand menu, click Roles.

Click Create Role.

Under Choose a use case, select S3.

Click Next: Permissions.

In Filter policies, type "S3".

From the search results, select AmazonS3ReadOnlyAccess.

Click Next: Tags > Next: Review.

For Role name, enter "neptune-import".

Click Create role.

Search for and select neptune-import.

Select the Trust relationships tab.

Click Edit trust policy.

Edit the Service: line to read:

{
  "Version": "2012-10-17",
  "Statement": [
	{
		"Effect": "Allow",
		"Principal": {
			"Service": [
			    "s3.amazonaws.com",
			    "rds.amazonaws.com"
			]
		},
		"Action": "sts:AssumeRole"
	}
  ]
}
Click Update policy.

Click Services > Neptune.

Select the neptune-cluster.

Click Actions > Manage IAM roles.

With the neptune-import role selected, click Add role > Done.

Load the Data
In the AWS Management Console, navigate to VPC.

On the left menu, click Endpoints > Create Endpoint.

In Service category, select Other endpoint services.

In Service Name, enter "com.amazonaws.us-east-1.s3" and click Verify.

In VPC, select the existing VPC from the dropdown menu.

Under Route Table ID, select the ID associated with 2 subnets.

Click Create endpoint > Close.

Navigate to IAM.

On the left menu, click Roles.

Click the neptune-import role.

Under Summary, copy the role ARN into a text file for later use.

Navigate to Neptune.

Select the neptune-cluster.

Under Connectivity & security, copy the cluster endpoint and port number (type = Reader) into a text file for later use.

'''




### Access from sparql cli
'''
[cloud_user@ip-10-0-0-120 eclipse-rdf4j-3.4.1]$ history
    1  date
    2  # arn:aws:iam::205376115077:role/neptune-import
    3  # reader neptune-cluster.cluster-ro-cybpgyhgiedl.us-east-1.neptune.amazonaws.com
    4  # writer neptune-cluster.cluster-cybpgyhgiedl.us-east-1.neptune.amazonaws.com
    5  export NEPTUNE_ENDPOINT=neptune-cluster.cluster-ro-cybpgyhgiedl.us-east-1.neptune.amazonaws.com:8182
    6  sudo yum install libcurl.x86_64 -y
    7  curl -X POST -H 'Content-Type: application/json' \https://$NEPTUNE_ENDPOINT/loader -d '
{
"source": "s3://neptune-import-20230627/neptune-data.rdf",
"format": "ntriples",
"iamRoleArn": "arn:aws:iam::205376115077:role/neptune-import",
"region": "us-east-1",
"failOnError": "FALSE",
"parallelism": "MEDIUM",
"queueRequest": "TRUE"
}'
    8  git clone https://github.com/linuxacademy/content-aws-database-specialty.git
    9  cd content-aws-database-specialty/S06_Additional\ Database\ Services/
   10  tar -xzvf eclipse-rdf4j-3.4.1-nowar.tgz
   11  cd eclipse-rdf4j-3.4.1
   12  bin/console.sh
   13  history
[cloud_user@ip-10-0-0-120 eclipse-rdf4j-3.4.1]$ bin/console.sh
21:34:06.972 [main] DEBUG org.eclipse.rdf4j.common.platform.PlatformFactory - os.name = linux
21:34:06.988 [main] DEBUG org.eclipse.rdf4j.common.platform.PlatformFactory - Detected Posix platform
Connected to default data directory
RDF4J Console 3.4.1+4fae190
Working dir: /home/cloud_user/content-aws-database-specialty/S06_Additional Database Services/eclipse-rdf4j-3.4.1
Type 'help' for help.
> create sparql
Please specify values for the following variables:
SPARQL query endpoint: https://neptune-cluster.cluster-ro-cybpgyhgiedl.us-east-1.neptune.amazonaws.com:8182/sparql
SPARQL update endpoint: https://neptune-cluster.cluster-cybpgyhgiedl.us-east-1.neptune.amazonaws.com:8182/sparql
Local repository ID [endpoint@localhost]: neptune
Repository title [SPARQL endpoint repository @localhost]: Neptune Db Instance
Repository created
> open neptune
Opened repository 'neptune'
neptune> sparql SELECT * where {?s ?p ?o}
Evaluating SPARQL query...
+------------------------+------------------------+------------------------+
| s                      | p                      | o                      |
+------------------------+------------------------+------------------------+
| <http://example.org/#spiderman>| <http://www.perceive.net/schemas/relationship/enemyOf>| <http://example.org/#green-goblin>|
+------------------------+------------------------+------------------------+
1 result(s) (1462 ms)
neptune>
'''
