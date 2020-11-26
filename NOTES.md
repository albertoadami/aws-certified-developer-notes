# Notes for AWS Certified Developer

1. [The Basic of AWS](#introduction)

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
* ***AWS Edge Location:*** AWS datacenter that don't contains AWS services, but instead it's used to deliver content to parts of the world, using for example CloudFront service (CDN).
* ***AWS Region:*** the region contains the components and the services. Each region has a name that represents its area. The region are logically separated.

Each region is composed of multiple ***Availability Zones (AZs)***, that provides high availability and fault tolerance. All the regions are collocated around all the world.

AZs have direct low latency connection between each AZ wihin a region, but each AZ is isolated from others to ensure fault tolerance.

***Virtual Private Cloud (VPC)*** is an isolated piece of network in AWS, that can consist of multiple AZs for be fault tolerance.

For the security AWS uses a ***Shared Security Responsibility Model***. This means that AWS is responsible for the pysical resources and you (as a customer) for the sofware configurations. For example the operating system patches are responsibility of the customer.

In AWS there are some services that are not limitated in a single region, but they are ***Global***. An example of this is IAM service.

The REST APIs of the IAM service don't use the region in the URL because it's global, the other services instead contains the region in the URL.

Not all the services are available in all the regions of the world. When you create your own infrastructure on AWS you should select a region in which operate that's closer to your application users and furthemore the region must contains all the services that you need for your application.