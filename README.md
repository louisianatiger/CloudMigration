#README#

AWS to OCI Cloud Migration

The Word doc includes screenshots for those who would like the visuals to supplement. 

Exporting/Importing AWS EC2 Instance to Oracle Cloud Infrastructure

Prerequisites
https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html#vmexport-prerequisites

EXPORT RESTRICTIONS ARE AT THE BOTTOM OF THE DOCUMENTATION

Steps (If you have created an EC2 Instance, S3 Bucket, and configured IAM Policies. SKIP to Step 4)
1)	Create EC2 Instance
2)	Create S3 Bucket
3)	Create IAM Groups/Policies 
4)	Add a new Canonical ID to grant access to the Bucket. For all regions in the U.S/Canada your Canonical ID will be (this is also in the documentation linked above): c4d8eabf8db69dbe46bfe0e517100c554f01200b104d59cd408e777ba442a322 
5)	Next you’ll SSH into your EC2 Instance 
6)	Ensure that you’ve installed Pip and Python
7)	Install and Configure AWS CLI 
a)	https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html
b)	https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

Config Example (Ensure you’re in the same region as your instance. Default output format is up to user but for demonstration purposes I chose json)
 
8)	Begin the Export Process
a.	(Overview) https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html#vmexport-limits
b.	(Create Instance Export Task Example Detailed) https://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html
c.	Example code: 
aws ec2 create-instance-export-task --description "RHEL5 instance" --instance-id [instance id] --target-environment vmware --export-to-s3-task DiskImageFormat=vmdk,ContainerFormat=ova,S3Bucket=[bucketname],S3Prefix=RHEL5

9)	Your Instance will reboot. It’ll take about 5 min or so for it to upload to S3
10)	Download your Instance from S3
11)	Log into OCI and Navigate to Object Storage
12)	Create your Object Storage bucket and upload your Object
13)	Copy the Object URL (DO NOT MAKE A PAR)
14)	Navigate to Compute > Custom Images > Import Image
15)	Enter information and URL then Import
16)	Wait for the Image to Import (Takes 5 – 10 minutes depending on size)
17)	Create VCN and ALL related resources 
18)	After the Custom Image is imported you may create an instance then access it via your SSH. However, you will ssh with @ec2-user instead of @opc but will use the OCI Public IP. Also, to begin any programs on there you will need to use the sudo command to launch. For example if you have a web server. Instead of doing apachectl start, you’ll do sudo apachectl start.

Export Restrictions
Exporting instances and volumes is subject to the following limitations:
•	You cannot export a VM if it contains third-party software provided by AWS. For example, VM Export cannot export Windows or SQL Server instances, or any instance created from an image in the AWS Marketplace.
•	You must export your instances and volumes to one of the following image formats that your virtualization environment supports:
o	Open Virtual Appliance (OVA), which is compatible with VMware vSphere versions 4, 5, and 6.
o	Virtual Hard Disk (VHD), which is compatible with Citrix Xen and Microsoft Hyper-V virtualization products.
o	Stream-optimized ESX Virtual Machine Disk (VMDK), which is compatible with VMware ESX and VMware vSphere versions 4, 5, and 6.
•	You can't export Amazon EBS data volumes.
•	You can't export an instance that has more than one virtual disk.
•	You can't export an instance that has more than one network interface.
•	You can't export an instance from Amazon EC2 if you've shared it from another AWS account.
•	You can't have more than five export tasks per Region in progress at the same time.
•	VMs with volumes larger than 1 TiB are not supported.
•	You can export a volume to either an unencrypted S3 bucket or to a bucket encrypted using SSE-S3. You cannot export to an S3 bucket encrypted using SSE-KMS.


