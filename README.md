# AWS Certified Developer Prep

> This repo will be used as I prepare for my AWS Certified Developer certification. I will be committing all notes and code I write over the course of my preparation.
* I have actually provisioned my first fully self-deployed webserver in order to apply some of this knowledge.
> www.louiswillcock.com

* I am using Udemy to prepare.
> https://www.udemy.com/aws-certified-developer-associate

## EC2

* EC2 gives you access to VMs on demand, with which you can run whatever you wish, they come in varying capacities and sizes. Scaling in price.

#### Pricing

* On Demand - Whenever needed
> Testing of ideas, only need a server for an hour or two at random times.
* Reserved  - Capacity reservation for period of time.
> Applications with steady and predictable use.
* Spot 	    - Bid on excess capacity.
> Predictable but infrequent use. Genomics companies need massive capacity that would be expensive, spot allows them to bid and schedule their apps to run at strange times.
* Dedicated - Normal dedicated hosting.
> When you are mandated for use of physical servers and have predictable use.

* Exam Tip
> If the spot instance is terminated by Amazon, you will not be charged for a partial hour of usage. However, if you teriminate the instance yourself, you will be charged for any hour in which the instance ran.

#### EC2 Instance Types

* EC2 Instances are separated into families with different uses.

* D2 - Dense Storage - Fileservers/DataWarehousing/Hadoop
* R4 - RAM Optimised - Memory Intensive Apps/DBs
* M4 - Main Instance - General purpose App Servers.
* C4 - Compute Optimised - CPU Intensive Apps/DBs
* G2 - Graphics Intensive - Vid Encoding/3D App Streaming
* I2 - IOperations - NoSQL DBs, Data WareHousing
* F1 - Field Programmable Gate Array - Hardware Accel for Code
* T2 - Trivial Tasks - Low Cost/Gen Purpose Web Servers/Small DBs
* P2 - Powerful Graphics - ML/Bitcoin Mining
* X1 - Extreme RAM - SAP HANA/Apache Spark

>DIRTMCGX

## EBS - Elastic Block Store

* EBS creates storage volumes that can be attached to EC2 instances in the same way you would add a HDD to a computer. They are auto replicated to avoid failure and data corruption.

#### EBS Types

* GP2 - General Purpose (SSD)
> 3 IOPS per GB with upto 10,000 IOPS per volume.
* IO1 - Provisioned IOPS (SSD)
> NoSQL and I/O intensive apps. Use if you need >10,000IOPS. Upto 20,000IOPS per volume.
* ST1 - Throughput Optimised (HDD)
> Big Data, Data WareHousing, Log Processing. Can't be a boot volume.
* SC1 - Cold Storage (HDD)
> Lowest cost storage. Infrequently accessed. File servers. Can't be a boot volume.
* Magnetic Standard (HDD)
> Lowest cost bootable drive. Applications where lower storage cost is importnat.

### EC2 Exam Tips

* Know the differences between pricing tiers.

* Remember about teriminating spot instances.

* Understand differences of EBS types.

* You can't mount 1 EBS volume to multiple EC2 instances, use EFS instead.

* DRMCGIFTPX - EC2 Instance Types

## Configuring instances

#### Subnets, VPC and Avail.Zones.

* Subnets can only be used for one availability zone, ie: sydney-1a or sydney-1b. You can't have subnets across multiple availability zones.

#### Tags
* Tags should be used in as fine grain way possible. They allow labelling of instances and location of costs in bigger organisations. You use tags to highlight the employee procuring the service, or department budget to charge to.

#### Security Groups
* Security Groups are effectively firewalls that we control.


> Setting up the apache server was actually ridiculously easy. Wow. Superb. Will get my website hosted by the end of the night most likely.

### Exam Tips

* Termination Protection is turned off by default, you must turn it on.

* On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated.

* Default AMI root volumes cannot be encrypted, you must create an image of a built VM, then encrypt that. Or you can use bitlocker.

## Security Groups

* A security group is just a virtual firewall.

* 1 instance can have multiple security groups. The benefit of this is that you can create boilerplate rulesets for different applications, this allows us to leverage effective mental modelling. We know what a standard DB instance's rules will look like, and what a applications rules too.

* Changes to security group (firewall) rules, are effected immediately, a change to http inbound will be felt straight away, most likely in the form of a timeout.

* Inbound/Outbound is stateful, in that, if you have inbound http rules, you don't require outbound http rules, it will automatically allow traffic back out.

* AWS EC2 instances implement a "trust no one, authenticate few" model of security. In that, by default, ports and protocols are treated as malicious, and are required to be given access case by case. A great way of doing security.

* If you wish to block IP addresses, you cannot do it using SecGroups, you do it with Network Access Control Lists.

## Volumes VS Snapshots

* When first building an instance and you get to the select EBS or Volume, some choices won't be available. After this, if you decide to add a volume, they will become available. These are the non-root volumes.

* Cannot attach volumes from outside the availablity zone of your instance.
> IE: If you're instance is in Chicago1A you can't attach a volume from Chicago1B.

* There is a long process with a few steps involved in creating a new volume to add onto an AWS instance. You must provision it from AWS. Attach it to an instance. Then SSH into the instance. From here, run lsblk to find the thing. cd into the / root dir. mkdir whatever you want to call it, myfileserver is a good example.
Then from the / root dir. run mkfs -t ext4 /dev/xvdf or the fileextension of the volume.

* umount to unmount, then unassociate it from AWS. You can then make a snapshot of the volume, which will hold all the data on the volume. Which you can spin up whenever you need.

* Volumes exist on EBS. They are virtual hard disks.

* Snapshots exist on S3.

* Snapshots are point in time copies of Volumes.

* Snapshots are incremental. Only records changes.

## EC2 User-Roles

* Use roles instead of storing your access key and secret access key on individual instances. This is because these values are stored in plain text config files, should your ssh keys be stolen or you otherwise lose access. You are pretty much fucked.

* Roles are your friend, much easier to handle and manage than the aws configure command.

* Roles are dictated at the start of an instance. You cannot add roles once the instance is spawned.

* You can however edit the policies that are inside that role, these changes occur immediately.

* If you delete the one and only role on your ec2 instance. You are fucked, you are looking at a terminate and new start.

## CLI Commands for the Dev Exam

* aws ec2 describe-instances
> Instances available to us
* aws ec2 describe-images
> Images available to us to provision instances from
* aws s3 ls
> Lists all s3 buckets.
* start-instances VS run-instances
> aws ec2 start-instances is used to start, stopped instances
> aws ec2 run-instances is used to boot new instances

## Bash Scripting

* Setting up an S3 bucket, just learnt that the s3 bucket namespace is shared, so you have to pick something unique.

* Key pairs are region specific too, they do not track globally, which is a good thing really, but requires management of a host of keys.

* Pretty cool little lab really, speeds up the process even more. So if you were running a web dev firm, you'd be able to provision standard servers with your details and whatnot super quickly.
