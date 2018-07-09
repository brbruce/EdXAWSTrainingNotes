# EdxAWSTrainingNotes

https://courses.edx.org/courses/course-v1:AWS+OTP-AWSD1+1T2018/course/

# Week 1

AWS Account: bbawsjunk1  
EC2 Dashboard  

__EC2__ - Elastic Computing  

__VM__ - Virtual machines on physical host with hypervisor

__AZ__ - Hosts are in Availability Zones, logical groupings of datacenters, Isolated for failover.  

__Region__ - Multiple AZs per region.  +18 regions globally

Region id: us-west-2  
AZ id: us-west-2a

Edge location services: DNS, Content Delivery Network

__IAM__ - Identity and Access Management.

Scope of services:  EC2 services are scoped by AZ.  Some services like S3 are scoped by region.  VPC is region scope.  IAM is not limited to a region.

### Networking
Services should be isolated from public network.

__VPC__ - Virtual Private Cloud - Multiple VMs working together.  Some public, some private.  Can connect to private corporate network.  Region scope.

Use subnets for easy management.

#### CIDR Notation:
IPV4 = 32 bits (1111 1111.1111 1111.1111 1111.1111 1111 = 0.0.0.0 to 255.255.255.255)

Ex. Public subnet:  
10.0.0.0/24 <-- 24 bits in the prefix, used for network mask.  8 bits for host address.  
XXXX XXXX.XXXX XXXX.XXXX XXXX.#### ####  
0000 1010.0000 0000.0000 0000.0000 0000  
= 10.0.0.0 to 10.0.0.255

Ex. Private subnet:  
10.1.0.0/24 <-- 24 bits in the prefix, used for network mask.  8 bits for host address.  
XXXX XXXX.XXXX XXXX.XXXX XXXX.#### ####  
0000 1010.0000 0001.0000 0000.0000 0000  
= 10.1.0.0 to 10.1.0.255

#### Route Tables
Rules for routing

#### Cloud Formation Template
VPCs are region scope, but subnets are AZ scope.  Must define same subnet in each AZ.

Use Cloud Formation Templates to automate network configurations.

## Exersize 3
edx-build-aws-vpc: vpc-08a0cf9dab862b94d

edx-build-aws-vpc: subnet-03408302057b29cc0

ssh key: AWS\_E3_KeyPair

IPV4 Public IP: 35.164.22.51

To view the log file, type the command below in your instance terminal.  

    cat /var/log/cloud-init-output.log

To view the instance metadata, type the command below:

    curl http://169.254.169.254/latest/meta-data/

Execute the command below to get the instance identity document of your instance:

    curl http://169.254.169.254/latest/dynamic/instance-identity/document

Execute the command below to get the instance public IP address:

    curl http://169.254.169.254/latest/meta-data/public-ipv4

Execute the command below to get the MAC address of the instance:

    curl http://169.254.169.254/latest/meta-data/mac

Execute the command below to get the VPC ID in which the instance resides. Make sure to replace Your-MAC in the command below with the MAC address of your instance:

    curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/Your-MAC/vpc-id

Execute the command below to get the subnet-id in which the instance resides. Make sure to replace Your-MAC in the command below with the MAC address of your instance:

    curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/Your-MAC/subnet-id

Execute the command below to get the instance user data:

    curl http://169.254.169.254/latest/user-data 

### Notes and Resources
https://courses.edx.org/courses/course-v1:AWS+OTP-AWSD1+1T2018/courseware/6f34da6ab7e44b068f5d2d106e8faf61/0fab6dc762ba45e7ac1db99ee2c3ed82/?child=first

#### Shared Responsibility Model
https://aws.amazon.com/compliance/shared-responsibility-model/
Guest OS and higher - Customer responsiblity

#### VPC - Amazon Virtual Private Cloud

#### EC2

Amazon Elastic Compute Cloud allows you to run virtual servers in AWS.
Your virtual server is known as an EC2 Instance. It runs on a physical host that is inside an AWS Availability Zone (AZ). There will be 2 or more AZ within an AWS Region. This design allows you to build applications that are resilient to large scale events that could impact an AZ.

#### CloudFormation

An AWS service that can take in a declarative document called a 'template' and use it to provision AWS resources on your behalf so you don't have to. We used this to create a VPC to the specifications needed for the course.

#### EC2 Metadata service

This is a service that intercepts calls to 169.254.169.254 from your EC2 instance to communicate metadata to the instance. This IP address is in the range for IPv4 Link-Local IP addresses as defined by RFC 3927 and the details about the properties the instance that can be retrieved are documented here in the EC2 User Guide.

#### EC2 User Guide
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html

-------------------------------
# Week 2

### AWS APIs

Programmatic access to services like S3 buckets (load images), Rekognizer, DynamoDB.  
APIs access via SDKs and programming languages.  
__BOTO3__ - AWS SDK for Python, installed using PIP package manager
Create user via IAM, with password, access key and secret key.  Copy and install on the server in a file called "credentials".  

### Create user

AWS services page > IAM > Policies : Create "edXProjectPolicy" with json data.  Permission to multiple amazon services.

NOTE: Always grant "least privileges"  

## Exersize 4

Create user "edXProjectUser" and attach policy "edXProjectPolicy".  
Download credentials.csv file. (Saved in C:\Users\Brian\Documents\_Tech Info & Serial Numbers\PublicPrivateKeys\AWS Training Keys\)  
Send email.  Click Dashboard.  Sign out (as AWS user) and sign in as edXProjectUser.  Copy password from credentials.  Logged in!  

As edXProjectUser user, create EC2 instance:

1) AWS Console > Services > EC2 (EC2 Dashboard) - Check Regions = Oregon (US West  
2) Launch Instance blue button (under Create Instance)  
3) Select AMI = Amazon Linux AMI (2018.03.0 (HVM), SSD Volume Type) - Click Select button  
4) Instance Type = t2.micro and Next: Configure Instance details  
5) Configure Instance Details:  
Network: edx-build-aws-vpc  
Subnet: edx-subnet-public-a  
6) Add Storage: Next: Add Tags  
7) Add Tags:  
Key: Name  
value: Ex4WebServer  
8) Configure Security Group:  
Select existing security group: exercise3-sg  
9) Review and Launch.  Launch.  
10) Select existing or new key pair:  Choose existing key pair: AWS_E3_KeyPair (Created before)

IPv4 Public IP = 52.10.142.199

SSH to "ec2-user@52.10.142.199" with SSH auth keys from C:\Users\Brian\Documents\_Tech Info & Serial Numbers\PublicPrivateKeys\AWS Training Keys\AWS_E3_KeyPair.ppk

__NOTE: The SSH user to use is still the "ec2-user", and not the "edXProjectUser" you created the instance with.__

After SSH, run aws configure.  Enter both keys.  Enter "us-west-2" as the default region.

Run this command to test the access:

    aws ec2 describe-instances

Installed python and boto3.  Run python and create ec2 client:

    import boto3
    client = boto3.client('ec2')
    client.describe_instances()
    client.describe_key_pairs()

Gets json output.

### S3 Object Storage service

Buckets for storage.  Bucket is in a region.

Keys are UIDs.  Bucket+Key = URL to access the object.

### Access control

Default access is private.

* Bucket Policies  
* IAM users and groups  
* Access Control Lists

Signed URLs - Apps can share access for a limited time.  URL has bucket, key, and access key, signature, and expiration time.

### Cloud9 - Web IDE

Automatically create EC2 instance.

Command to list S3 buckets:

    aws s3 ls

Need to install boto3 (sudo pip-3.6 install boto3)

## Exersize 5

Architecture:  
* Amazon EC2 - App server  
* Amazon S3 - Object storage
* MySql DB
* Amazon Cognito - Signup, signin  
* Amazon SNS topic  
* Amazon SQS queue  
* AWS Lambda function - Serverless execution  
* Amazon Rekognition - Image recognition  
* AWS X-Ray - Trace all calls, provides diagnostic info

#### Set up Cloud9 IDE and env.  NOTE: Use the "edXProjectUser" login!!!

Cloud9 Docs: https://docs.aws.amazon.com/console/cloud9/

Create new instance:

* Name: BuildingOnAWS  
* Environment type: EC2  
* Instance type: t2.micro  
* Network (VPC): vpc-08a0cf9dab862b94d  
* Subnet: subnet-03408302057b29cc0  
* Cost-saving settings: After 30 minutes (default)
* IAM role: AWSServiceRoleForAWSCloud9 (generated)

Install boto3:

    sudo pip-3.6 install boto3 

#### Create S3 bucket

S3 page.  Create new bucket.

* Bucket name: bbruceedxbucket  

#### Build S3 uploader

    cd ~/environment

Follow instructions to download zip file, unzip, install requirements.

Set up run configuration with ENV vars PHOTOS_BUCKET and FLASK_SECRET.

Edit security group for Cloud9 EC2 instance, inbound rule, add rule, custom tcp port 5000 from source 0.0.0.0/0.

In Cloud9 IDE, click Share just to check the public IP address. Opens dialog window with IP address.  Copy the IP address:  54.214.132.138

Go to URL http://54.214.132.138:5000

Modified code to sort images by LastModified (See application_sorted_by_lastModified.py)  - Created sort method, and called sorted function while looping over photo content.

    def getLastModified(content):
        return content["LastModified"]
    
    ...
    photos = [s3_client.generate_presigned_url(
            'get_object',
            Params={'Bucket': config.PHOTOS_BUCKET, 'Key': content['Key']}
            # Modified to sort by LastModified
            #) for content in response['Contents']]
            ) for content in sorted(response['Contents'], key=getLastModified)]

https://boto3.readthedocs.io/en/latest/reference/services/s3.html#S3.Client.list_objects

## Exercise 6 - Amazon Rekognition

Same as exercise 5.  Calls Rekognition service with uploaded picture to return labels.

Modified to also print confidence number:

    rek = boto3.client('rekognition')
    response = rek.detect_labels(
    ...
    print("+++++ Response: ")
    pprint.pprint(response)
    # Modified to include confidence number
    #all_labels = [label['Name'] for label in response['Labels']]
    all_labels = [(label['Name']+" ("+str(label['Confidence'])+")") for label in response['Labels']]














NOTE:

At the end of this exercise, you will not be using the access keys again. It is a security best practice to remove IAM user credentials that are not needed. After this exercise, make sure to remove the access keys only (not the AWS Console password) for the IAM user - edXProjectUser. See more IAM Best Practices.  

https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html










