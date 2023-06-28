### Defining a parameter in a template
The following example declares a parameter named InstanceTypeParameter. This parameter lets you specify the Amazon EC2 instance type for the stack to use when you create or update the stack.

Note that InstanceTypeParameter has a default value of t2.micro. This is the value that AWS CloudFormation uses to provision the stack unless another value is provided.


YAML
'''
Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - m1.small
      - m1.large
    Description: Enter t2.micro, m1.small, or m1.large. Default is t2.micro.
'''

JSON can also be used


### StackSets
A stack set lets you create stacks in AWS accounts across regions by using a single CloudFormation template. A stack set's CloudFormation template defines all the resources in each stack. As you create the stack set, specify the template to use, in addition to any parameters and capabilities that template requires.

After you've defined a stack set, you can create, update, or delete stacks in the target accounts and AWS Regions you specify. When you create, update, or delete stacks, you can also specify operation preferences. For example, include the order of Regions you want to perform the operation, the failure tolerance threshold before stack operations stop, and the number of accounts performing stack operations concurrently.

A stack set is a regional resource. If you create a stack set in one AWS Region, you can only see or change it when viewing that Region.


### Stack Termination Protection

AWS CloudFormation now allows you to protect a stack from being accidently deleted. You can enable termination protection on a stack when you create it. If you attempt to delete a stack with termination protection enabled, the deletion fails and the stack, including its status, will remain unchanged. To delete a stack you need to first disable termination protection. 

### dynamic references

CloudFormation currently supports the following dynamic reference patterns:
* ssm, for plaintext values stored in AWS Systems Manager Parameter Store.
* ssm-secure, for secure strings stored in AWS Systems Manager Parameter Store.
* secretsmanager, for entire secrets or secret values stored in AWS Secrets Manager.


As specified, CloudFormation will use version 2 of the S3AccessControl parameter for stack and change set operations.
'''
  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      AccessControl: '{{resolve:ssm:S3AccessControl:2}}' 
'''
