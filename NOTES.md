# Notes for AWS Certified Developer

1. [The Basic of AWS](#introduction)
2. [AWS Identity and Access Management (IAM)](#iam)

## The Basic of AWS <a name = "introduction"></a>
AWS provided three different ways to interact with their services:
* AWS Console: web interface;
* AWS CLI: terminal/shell;
* AWS SDKs: libraries for varius programming languages.

All the actions that are performed using the SDKs are all REST API calls to the AWS services.

When you create a new AWS account by default the new user created is the ***root*** user with full administrative rights. The best practice is to not use this user directly, but instead create an other administrative user.

An other good practice is to configure the root user wih the MFA authentication.

***IAM*** is the service provided by AWS to manage the users. In IAM you can create one ore more users, who will have various degrees of access to the various AWS services.

All the AWS physical resources contain the following concepts:
* ***AWS Edge Location:*** AWS datacenter that doesn't contain AWS services, but instead it's used to deliver content to parts of the world, using for example CloudFront service (CDN).
* ***AWS Region:*** the region contains the components and the services. Each region has a name that represents its area. The region are logically separated.

Each region is composed of multiple ***Availability Zones (AZs)***, that provides high availability and fault tolerance. All the regions are collocated around all the world.

AZs have direct low latency connection between each AZ wihin a region, but each AZ is isolated from others to ensure fault tolerance.

***Virtual Private Cloud (VPC)*** is an isolated piece of network in AWS, that can consist of multiple AZs for be fault tolerance.

For the security AWS uses a ***Shared Security Responsibility Model***. This means that AWS is responsible for the pysical resources and you (as a customer) for the sofware configurations. For example the operating system patches are responsibility of the customer.

In AWS there are some services that are not limitated in a single region, but they are ***Global***. An example of this is IAM service.

The REST APIs of the IAM service don't use the region in the URL because it's global, the other services instead contains the region in the URL.

Not all the services are available in all the regions of the world. When you create your own infrastructure on AWS you should select a region in which operate that's closer to your application users and furthemore the region must contains all the services that you need for your application.

## AWS Identity and Access Management (IAM) <a name="iam"></a>
Like said before IAM is the AWS service used to manage the users and their roles and policies inside of AWS. In general the common use of IAM is to manage:
* Users;
* Groups;
* Roles;
* Access Policies;
* API Keys;
* Specify password policy.

By default, when you create a new IAM user, it will have no-access to any AWS service (DENY-ALL). The access to the various AWS services are gicen to the IAM users using the ***IAM Policies***.

The best practive, when we create a new account, is to:
1. Delete read access to root keys;
2. Activate MFA on your root account;
3. Create IAM user;
4. Create IAM Groups to assign permissions;
5. Apply a password policy.

***IAM Policy*** is a document that states one or more permissions. More that one or more policy can be attached to a single user or to a group. AWS provides pre-build policies, but you can create your custom ones. The most relevants policies are:
* ***Administrator access:*** full-acess;
* ***Power user access:*** admin without user management;
* ***Read only access:*** only view to AWS services.

If to an IAM user we attach an admin policy and also a "deny-all" custom police, the user will not be able to do anything. This because the explicit deny ovverrides the other policy in AWS.

In AWS a ***IAM user*** has:
* Name;
* Password;
* Policies;
* Access key.

A user can also be part of a ***IAM Group***. Each component of the gruoup has the same policies that are associated with he gruop, plus the personal ones. 

The user credenials should never be stored or passed to an EC2 insance for security reasons.

A best practive is to create multiple IAM Groups with different policies for tthe vaious kind of users and associate the users with the correspondings groups.

***IAM Role:*** something than another entity can "assume" and in doing so acquires the specific permissions defined by the role. For example an AWS entitty (EC2) needs to access to an other AWS entity (S3).

In AWS an instance can have only one roled attached for time. The IAM role is configured with policies for access to the required services/components inside AWS.

When you create a new role you have to select the AWS service for which tadd the policy versus other AWS services. After you successfully create the role, you need also to associated the rolo to an instance (an EC2 for example).

***IAM Security Token Service (STS)*** allows you to create temporary security credentials that grant trusted users access to your AWS resources. These credentials are for short time usage (from some minutes to several hours).

When for example you associate a role to an EC2 instance to access an S3 bucket, the STS service is used.

The AWS credentials are composed of:
* Security token;
* An access Key Id;
* A secret access key.

***API Access Keys*** are required to make calls to AWS from:
* AWS CLI;
* AWS SDKs;
* HTP Calls.

The API Keys are only available when a new user is created. The access key is associatted with a user. If you need new credentials you need to deactivate the current ones and generate new credentials after that.

***AWS Key Management Service (KNS)*** is a managed service to create and control the encryption keys to encrypt your data. It's used to encrypt data inside AWS services.
The data key can be used to encrypt the plain text data.

***Amazon Inspector*** is not under the IAM service. It's a tool tthat helps you to check if there are some security vulnerabilities. If can be also integrated with your development and deployment pipeline. This allow to make security tests in a more regular occurrence.

Instead ***Amazon Cognito*** provides authentication, authorization and user management. It's not under IAM but contains a supersett of the functionality of web identify federation. 

The users can sign-in directly into Cognito with username and password or use also a 3th party account (Google, Facebook, Amazon) if configurated from the AWS console. Cognito has two main components:
* ***User Pool:*** userr directory;
* ***Identity Pool:*** users can obtain temporary access to AWS services, similar to IAM Role.

An Idenitty Pool is associated witht a User Pool. Amazon Cognito Sync is a separate module and allows to sync data accross web and mobile platforms.