# AWS EC2 Fundamentals Notes I

This is first blog in a series designated to learning AWS and AWS Development using C++.
Please refer to the [concepts](#Concepts) section for this note and see [Links](#Links)
for additional information.

# Concepts
* [EC2 Instances](#EC2-Instances)
* [IAM](#IAM)
* [Security Groups](#Security-Groups)
* [ENI](#ENI)

# EC2 Instances
Amazon Elastic Compute(EC) provides a scalable computing approach on Amazon Web Services(AWS) cloud.
EC2 instance represents a virtual computing envirnoment e.g. virtual machine.
To create EC2 instance a several approaches can be used:

* Aws Web Console
* Aws CLI tool
* Programming Language Library e.g. Aws C++ SDK

All these approaches will be covered in a future posts.

Here are important facts about EC2 instances(hereafter instance) that you need to remember:

* To create an instance you need to use Amazon Machine Image(AMI). There a lot of different images 
  provided by different authors(e.g. Amazon). In these blogposts we will be mainly concentrated on
  the Amazon Linux 2 AMI (HVM). This images is provided by the Amazon itself and contains some
  preinstalled packages. This image is not based on any other distribution and being suppoted
  by the Amazon.

* There several types of instances supported. All their differ in their configuration(vCPUs, Memory, Storage Type, Network Performance, IPv6 Support, etc...). The **only** instance type that is eligible for free tier is a `t2.micro`. During these notes this instance type will be used. It has
** 1 vCPU
** 1 GiB of Memory
** EBS Storage(More on this later)
** Low to Moderate Network Performace
** IPv6 Support

* During instance you can configure your instance options. The options that are under interest are the following:
** Number of Instances
** Subnet - You creating your instance in some Availability Zone(AZ)(e.g. eu-central-1) and usually 2 or more machines are being connected to the specific availability zone. So via `subnet` option you can select a specific machine.
** More about options later...

* You can attach a storage to your instance.

* You can assign a [security group](##Security-Groups) to your instance.

* Also, at the final step when 'Launch' button being clicked you will be asked to create new key-pair or use an existing one by selecting it from list. The second option is appliable only when you have the .pem file. If you lost your .pem file then you can't connect to that instance.

# IAM
Identity and Access Management(IAM) is a security core of Aws. The whole security is here:
- Users
- Groups
- Roles.

At high level, **User - is a physical person** and every User should get an IAM. Groups is a collection of Users. Roles are only used for machines.
There are policies that are described in JSON.

You need to remember:
- One IAM **User** per PHYSICAL PERSON
- One IAM **Role** per Application
- IAM credentials should NEVER BE SHARED
- NEVER, ever, ever, ever write your IAM credentials in code. EVER.

# Security Groups
Security Group is a collection of inbound/outbound network rules.

## Network Rules
Two types network rules are supported:
- Inbound Network Rules - Describes which **incoming** network traffic should be acceppted. For example you can allow SSH over TCP on port 22 from the all sources.
- Outbound Network Rules - Describes which **outgoing** network traffic should be acceppted. Usually **all** outgoing network traffic is acceppted by **I'm not sure if it's secure**.

# ENI
Elastic Network Interface(ENI) is a logical component inside Virtual Private Network(VPC) that represents a **virtual network card**.
Each ENI has the following parameters:
- Primate private IPv4 and one or more secondary private IPv4
- One (Elastic IP)[##Elastic IP] per private IPv4
- One public IPv4
- One or more [security groups](##Security-Groups)
- A MAC address

ENI can be attached to any instance on the fly(being moved) for failover. Each ENI is bound to a specific AZ. So you can attach it to the instances that are in a same AZ.

# Links
- [What is Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
- [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
