# Notes for AWS Certified Developer

1. [The Basic of AWS](#introduction)
2. [AWS Identity and Access Management (IAM)](#iam)
3. [Elastic Compute Cloud (EC2)](#ec2)
4. [Amazon Virtual Private Cloud (VPC) Fundamentals](#vpc)
5. [AWS Load Balancing](#load)
6. [AWS Lambda](#lambda)

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

By default, when you create a new IAM user, it will have no-access to any AWS service (DENY-ALL). The access to the various AWS services are given to the IAM users using the ***IAM Policies***.

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

When you define your policies, AWS reccomends that you adopt the principle of ***least privilege*** and grant someone with the minimum permissions they need for their daily tasks.

If to an IAM user we attach an admin policy and also a "deny-all" custom police, the user will not be able to do anything. This because the explicit deny ovverrides the other policy in AWS.

In AWS a ***IAM user*** has:
* Name;
* Password;
* Policies;
* Access key.

The user credenials should never be stored or passed to an EC2 insance for security reasons.

A user can also be part of a ***IAM Group***. Each component of the gruoup has the same policies that are associated with he gruop, plus the personal ones. A user can be part of multiple Groups (N-N relashionship).

A best practive is to create multiple IAM Groups with different policies for the vaious kind of users and associate the users with the correspondings groups.

***IAM Role:*** something than another entity can "assume" and in doing so acquires the specific permissions defined by the role. For example an AWS entity (EC2) needs to access to an other AWS entity (S3).

In AWS an instance can have only one role attached for time. The IAM role is configured with policies for access to the required services/components inside AWS.

When you create a new role you have to select the AWS service for which add the policy versus other AWS services. After you successfully create the role, you need also to associate the role to an instance (an EC2 for example).

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

***Amazon Inspector*** is not under the IAM service. It's a tool that helps you to check if there are some security vulnerabilities. It can be also integrated with your development and deployment pipeline. This allow you to make security tests in a more regular occurrence.

Instead ***Amazon Cognito*** provides authentication, authorization and user management. It's not under IAM but contains a superset of the functionality of web identify federation. 

The users can sign-in directly into Cognito with username and password or use also a 3th party account (Google, Facebook, Amazon) if configurated from the AWS console. Cognito has two main components:
* ***User Pool:*** user directory;
* ***Identity Pool:*** users can obtain temporary access to AWS services, similar to IAM Role.

An Idenitty Pool is associated with a User Pool. Amazon Cognito Sync is a separate module and allows to sync data accross web and mobile platforms.

## Elastic Compute Cloud (EC2) <a name="ec2"></a>
***Amazon Compute Cloud (EC2)*** provides scalable virtual servers in the cloud. An EC2 server is called an "instance" and supports Windows or Linux as Operating System. EC2 can be also configured with different instance types and size, that have also different prices.

The EC2 instances can be commissionated and decomissionated "on-demand" for easily scalability and elasticity.

An EC2 instance is composed of the following components:
* Amazon Machine Image (AMI): operating system and settings;
* Instance Type: hardware (CPU, RAM, network, etc..);
* Network interface: IP adresses;
* Storage: usually EBS (network storage).

Each instance must be placed within a VPC and a subnet. EC2 instance need also to have a ***Security Group*** associated.

To login inside an EC2 machine, AWS use an encrypted key paris with certificate.

***Amazon Machine Image (AMI)*** is a pre-configured package (template) required to launch an EC2 instance. It includes:
* Operating System;
* Software pacakges;
* Other required settings.

AMI can be of different kind:
* ***Communiy AMIs:*** free to use;
* ***AWS Marketplace AMIs:*** pay to use;
* ***My AMIs:*** custom AMIs, created by yourself.

EC2 provides the following purchasing options:
* ***On-demand:*** you can select any instance type and terminate it any time. It's the most expensive and most flexible option. You are charged only for when the instance is running;
* ***Reserved Instances (RI):*** allows you to buy an instance for a set time period (one or three years). This give us some price discount over on-demeand, with this option you pay for the entire period, not for the time usage.
* ***Spot Instances:*** is a way for you to "bid" on an instance type and pay only for the price. This allows Amazon to sell the unused instances with discount. An instance automatically terminate when the spot price is greater than your bid price.

With auto assigning public IP you can configure the EC2 to be accessed outside of the VPC, you can access to it from your PC for example.

By default all the EC2 instances have a private IP adress used for the communication inside the VPC. An instance can have also Elastic IP Addresses(EIP), that are static and public IPV4 addresses designed for cloud computing. Attacching an EIP to an instance, it will replace its default public IP adress.

On EC2 configuration you can customize also the bootstrapping and the user data,  that's a step of EC2 in which you can put some custom scripts.

During the creation process you can also configure the volumes. EBS are network attached storage, they can be attached/dettached to or from an EC2 insance.

EBS are backed up into a snapshot, which can later be restored into an new EBS volume. The volumes created from an EBS snapshot must be initialized, it occurs the first time a storage block on the volume is read.

EBS volume can be of different types:
* ***General Purpose SSD:*** the default one;
* ***Provisioned IOPS SSD:*** for IOPS perfomance applications;
* ***Throughput Optimized HDD and cold HDD:*** cheaper but also less perfomances. Cannot be mounted as a boot volume;
* ***EBS Magnetic:*** old generation. Low storage cost but also less perfomances.

On EC2 you can add multiple volumes of different types during the configuration process.

***Instance Store Volumes*** are virtual devices which hardware is phisically attached to the EC2 instance. Once the instance is stopped or shutdown, the data is erased. Not all types of EC2 instances can use Instance Store Volumes, for example EC2 micro doesn't support it.

***Elastic File System (EFS)*** is a storage option for EC2 that allows for scalable storage option. It's capacity is elastic, it will increase/decrease when files are added/removed. 

It's a separate AWS service and you need to configure a file system before. EFS can be accessed from multiple EC2 instances, you pay for the amount of data you are using. You can also use KMS to encrypt the disk data.

To connect via ssh to the EC2 instance you need to use the .pem file (key paris). The command used to connect to the instance is like the following:

```ssh -i "file.pem" ec2-user@public-ip-of-ec2-instance```

You need to run also the chmod 400 to the pem file before to run the ssh command.

***Elastic Load Balancer (ELB)*** are used to distribute traffic around multiple EC2 instances. The EC2 can be around multiple AZs. ELB can be configured to scale up and down the number of instances. ELB support two different kind of balancer: an Application Load Balancer (HTTP/HTTPS) or a Network Load Balancer (TCP).

On EC2 the customer is responsible for managing the software level security, including Security Groups, Firewall, EBS encryption, SSL certificate to ELB. AWS instead is responsibile for the physical layer of security on EC2. 

AWS provides also a set of commands to manipulate EC2 instances from the AWS CLI.

## Amazon Virtual Private Cloud (VPC) Fundamentals <a name="vpc"></a>
Virtual Private Cloud (VPC) enables you to launch AWS resources into a virtual network that you have defined. VPC represents an isolated part of AWS associated with your account.

In the VPC you can define private and public ***subnets***. You can also extend on premise network to the cloud as if it was part of your network (VPN).

When you define a new VPC you need to select the AWS region associated with it. A VPC spans multiple AZs within a region, in this way the VPC can be configured to be redundant. If you need also to have redundant resources inside multiple regions, you need to define multiple VPC (one for region).

In the VPC creation process, as well as the region, you need to define the IPV4 CIDR, that is the range of IPs used inside the VPC. For example 172.31.0.0/16 contains all the IP adresses starting with 172.31.

By default when you create an AWS account, if comes with a default VPC configured.

Inside a VPC you can define one or more ***subnets*** that can span multiple AZs. For example you can define a Subnet1 with IPs 172.31.1.0/24 and Subnet2 with IPs 172.31.2.0/24.

Inside your VPC you can have public and private subnets, public contains all the resources that are accessible from the public internet. The private subnet instead is not reachable from the internet, but can be accessed from the public subnet.

To configure a public subnet, you need to attach an ***Internet Gateway (IGw)*** to the VPC. Without the IGW the instances are not available from the internet.

***A Route Table*** contains a set of rules, called routes, that are used to determine where network traffic is directed. By default all traffic between he subnets inside a VPC is enabled.

The IGw connection is a rule of the public subnet route table. Each subnet has a route table associated with it.

AWS provides two kind of firewall rules:
* ***NACL:*** applied on subnet level;
* ***Security Group:*** applied on instance level.

NACL protects all the allowed traffic inside a subnet, instead a security group is a firewall at instance level (EC2 for example). The security group rules are applied after the NACL controls. NACL rules are stateless, here you can define ALLOW and DENY rules. The rules are evaluated in order, NACL is an optional layer of security. On NACL you need to define both inbound and outbound rules, this because NACL is stateless.

Usually ELB with auto scaling is used to improve the availability and fault tolerance inside a VPC. ELB performs health-check to understand if instances are up or down.

***Nat Gateway*** is a component that allows to instances in privae subnets (not reachable from the internet) to make request to the internet for download pattches, SW, etc. The Nat Gateway must be inside a public subnet and also be part of the private subnet route table.

A ***Bastian Host*** instead is an EC2 instance that lives in a public subnet and it's used as a "gateway" for traffic destined for private instances. It's a portal to access EC2 instances in the private subnet. This means that for connecting in the private instances we need to connect via ssh inside the Bastian Host and from that we're able to access the various instances in the private subnet.

## AWS Load Balancing <a name="load"></a>
AWS provides different types of Load Balancer:
* ***Application Load Balancer:*** at http/https level;
* ***Network Load Balancer:*** at UDP level;
* ***Classic Load Balancer:*** the old generation.

***Application Load Balancer*** make routing decisions at the application layer (http/https). You can configure rules for your listener that forward requests based on URL request. It supports routing request to multiple applications on a single EC2 instance.

***Network Load Balancer*** instead works on UPD level (Transport layer).
It can handle dynamic port mapping. It's able to handle milions of requests for second. It forwards reuqests without modifying the headers. It supports static IP adressess.

Exists also the ***Classic Load Balancer***, that is the previous generation and it is used when you have EC2 instances in a classic network.

The new generation of load balancers (Application and Network) supports dynamic ports. In a single EC2 instance can be running multiple containers (different ports).

Dynamic ports are facilitated by ***Target Groups***. A Target Group tracks the list of ports that are accepting traffic on each instance and gives the laod balancer a way to distribuite traffic evently across ports. The new generaion of load balancers is ideal for handle containerized services (Docker, ECS).

## AWS Lambda <a name="lambda"></a>
***AWS Lambda*** runs your code without needs to handle any server infrastructure. You need only to upload the function and AWS handle all the stuff for you.

Lambda is a serverless computing platform, you pay only for the computing time that you use. Lambda are composed of two main concepts:
* ***Lambda Functions:*** application code;
* ***Lambda Function Packages:*** dependencies that your code requires (libs, configurations).

Lambda supports Node.js, Java, C#, Python, Go as programming languages and can be integrated with other AWS services like Cloudwatch, API Gateway, KMS, etc.

When you create your Lambda function you need also to provide an AWS Role for the permissions versus other AWS services. AWS Lambda are triggered by events, that can be:
* HTTP API request through API Gateway;
* Cloudwatch events;
* S3 file uploads;
* DynamoDB streams change;
* Direct invocations through AWS CLI or SDKs;
* Many other event sources.

For example we can trigger a function from Cloudwatch one time a day. In the monitor section of Lambda you can see all the invocations and some other metrics about your functions.

All AWS Lambda deal with similar conceps:
* ***Handler files and functions:*** entry point for the invocation;
* ***Events:*** incoming data passed to the function when triggered;
* ***Context:*** a way to get runtime informations;
* ***Logging and Exceptions:*** handled through Cloudwatch logs.

When a Lambda function is called, it can be called synchronous or asynchronous. The handler is composed like `filename.functionname` and it's the function called by the events, we can think of it like the main method in a Java application. The handler receive `event` and `context` as input varialbes when invocated.

You can also configure some setting about the Lambda function like memory allocation, maximum duration, permission to other AWS services, VPC, environment variables (used by your code). We can also use encryption with KMS to encrypt the environment variables.

***Lambda Function Packages*** are zip packages with all code and dependencies required yo your Lambda function when it runs. It includes your handler file, custom libs, any other application code,  3th party pacakge from providers like npm or pip.

Lambda allows you to handle multiple versions of functions with unique ARNS ($LATEST is the last version of a function). Exist also the concept of ***Aliases***, that are pointers to specific versions. Aliases can also be used to split traffic between Lambda versions.

AWS Lambda is available also from the AWS CLI, you are able to create functions, inoke functions, add aliases, etc.

Lambda use cases are:
* ***Data processing;***
* ***Real time file procession;***
* ***Real time stream processing;***
* ***ETL;***
* ***IOT Backends.***