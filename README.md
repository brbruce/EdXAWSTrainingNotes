# EdxAWSTrainingNotes

https://courses.edx.org/courses/course-v1:AWS+OTP-AWSD1+1T2018/course/

----------------
# Week 1 - 6/28-7/1/2018

Create AWS Account: bbawsjunk1  (This is the Root AWS account)

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

__NOTE: The actual user for the EC2 Instance is "ec2-user".__

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
# Week 2 - 7/1-7/6

### AWS APIs

Programmatic access to services like S3 buckets (load images), Rekognizer, DynamoDB.  
APIs access via SDKs and programming languages.  
__BOTO3__ - AWS SDK for Python, installed using PIP package manager
Create user via IAM, with password, access key and secret key.  Copy and install on the server in a file called "credentials".  

### Create AWS IAM user (Safer than using root user)

AWS services page > IAM > Policies : Create "edXProjectPolicy" with json data.  Permission to multiple amazon services.

NOTE: Always grant "least privileges"  

## Exersize 4

Create user "edXProjectUser3" and attach policy "edXProjectPolicy".  
Download credentials.csv file. (Saved in C:\Users\Brian\Documents\\_Tech Info & Serial Numbers\PublicPrivateKeys\AWS Training Keys)  
Send email.  Click Dashboard.  Sign out (as AWS root user) and sign in as edXProjectUser.  Copy password from credentials.  Logged in!  

__IMPORTANT NOTE:__ Do not save access key info (csv file or the public/private keys) in the github folder.  Amazon checks github for any checkins with the access keys, and will suspend the IAM user account until you delete the access keys, and it may take a while to unsuspend it.  Save in an offline folder (\_TechInfo/PublicPrivateKeys)

As edXProjectUser3 user, create EC2 instance:

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

Select existing or new key pair:  Choose existing key pair: AWS_E3_KeyPair (Created before)

IPv4 Public IP = 52.10.142.199

SSH to "ec2-user@52.10.142.199" with SSH auth keys from C:\Users\Brian\Documents\\_Tech Info & Serial Numbers\PublicPrivateKeys\AWS Training Keys\AWS_E3_KeyPair.ppk

__NOTE: The SSH user to use is still the "ec2-user", and not the "edXProjectUser3" you created the instance with.__

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

#### Set up Cloud9 IDE and env.  NOTE: Use the "edXProjectUser3" login!!!

Cloud9 Docs: https://docs.aws.amazon.com/console/cloud9/

Create new instance:

* Name: BuildingOnAWS  
* Environment type: EC2  
* Instance type: t2.micro  
* Network (VPC): vpc-08a0cf9dab862b94d  
* Subnet: subnet-03408302057b29cc0  
* Cost-saving settings: After 30 minutes (default)
* IAM role: AWSServiceRoleForAWSCloud9 (generated)

__NOTE: The user in the bash shell is "ec2-user".__

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

At the end of this exercise, you will not be using the access keys again. It is a security best practice to remove IAM user credentials that are not needed. After this exercise, make sure to remove the access keys only (not the AWS Console password) for the IAM user - edXProjectUser3. See more IAM Best Practices.  

https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html

(Deleted access keys)

-------------------------------
# Week 3 - 7/11 - 

Objectives: Add MySQL db instance and use application load balancers.

## IAM Roles Review

Users, groups, roles - Can all have Permissions and Policies attached, which provide authorization.

Roles - Trusted Entity assumes a role.
* Time limited security credentials
* Provided by __AWS Security Token Service__

Trusted Entity - Can be an AWS Service (EC2, etc) or AWS Account.

AWS Services and Roles - An AWS service gets a temporary security credential from __Instance Metadata Service__.

## Amazon Relational Database Service (RDS)

Features for reliablity:
* Multi-AZ deployments
* Read Replicas

RDS instances can use MySQL, Oracle, SQL Server, Amazon Aurora, etc.

DB Instance in the cloud - Each instance can have multiple databases.

Need to create an endpoint to connect to the db instance.

## Exercise 7

Create a RDS MySql instance:

Name of DB instance: edx-photos-db  
Master username: master  
Master user password: Type a master user password and write it down for later use.  1W4&IK%oDoKQpKGQ
VPC: edx-build-aws-vpc  
Database name: Photos  

Important: Make a note of the database endpoint.   
Endpoint:  edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com

To allow access from my Cloud9 dev server:

Security and Network:   
Security Group: rds-launch-wizard (sg-0965722cdbe774e58)
Security Group ID: sg-0965722cdbe774e58

Inbound rule > Source: "sg" -> Pick Cloud9 env name: EdXWeek5Cloud9 = sg-0d9149b51c7ed7186

Start Cloud9 server EdXWeek5Cloud9, get zip file, unzip.

Run database_create_tables.py:

    edXProjectUser3:~/environment $ python3 exercise-rds/SetupScripts/database_create_tables.py 
    This script will drop and recreate the photo table, and the web_user user.

    Database host> edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com
    Database user> master
    Database password> 1W4&IK%oDoKQpKGQ
    Database name> Photos
    web_user password> 1W4&IK%oDoKQpKGQ

    Dropping / creating photo table
    Creating the web_user
    Granting access to web_user

web_user_password: 1W4&IK%oDoKQpKGQ

Go to the Python3RunConfiguration tab (or from the file menu: Run > Run Configurations > Python3RunConfig)

Click on the ENV button on the right corner of the tab header.  Enter the database env settings.

Update the command in the run config to database-rds and start.

In browser, go to http://34.215.202.123:5000

(To find the IP, go to EC2 dashboard and look at the public ip setting for the running Cloud9 instance)

MySql CLI command:

    mysql -h edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com -u web_user -p
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | Photos             |
    +--------------------+
    2 rows in set (0.00 sec)

    mysql> use Photos
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    mysql> show tables;
    +------------------+
    | Tables_in_Photos |
    +------------------+
    | photo            |
    +------------------+
    1 row in set (0.00 sec)

    mysql> select * from Photos.photo where labels like '%Toy%';                                                        
    +-----------------------------+---------------+-------------+------------------+---------------------+
    | object_key                  | labels        | description | cognito_username | created_datetime    |
    +-----------------------------+---------------+-------------+------------------+---------------------+
    | photos/4be2887ab515830a.png | Toy, Figurine | NULL        | NULL             | 2018-07-11 19:12:41 |
    | photos/ecdd28b026fa3b67.png | Toy           | NULL        | NULL             | 2018-07-11 19:12:14 |
    +-----------------------------+---------------+-------------+------------------+---------------------+
    2 rows in set (0.00 sec)

Stop the RDS Instance.  Instance actions -> Stop.

## Connecting Github and Cloud9 instance (Sidenote)

https://docs.aws.amazon.com/cloud9/latest/user-guide/sample-github.html#sample-github-install-git

Check if git is intalled in Cloud9 instance.  git --version.  It is.

Set git config vars:

    git config --global user.name "Brian Bruce"
    git config --global user.email public1@brianbruce.org

    edXProjectUser3:~/environment $ git config -l
    core.editor=/usr/bin/nano
    user.name=Brian Bruce
    user.email=public1@brianbruce.org

Create the Git repo in Github, but do not add any files like README.MD, or .gitignore, etc (otherwise, you will get errors)

Copy the URL for the repo.

In your Cloud9 instance, run these commands:

From: https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/

    # Initialize the local directory as a Git repository.
    git init 
    
    # # Adds the files in the local repository and stages them for commit. 
    git add . 
    
    # Commit the files that you've staged in your local repository.
    git commit -m "first commit" 
    
    # Sets the new remote
    git remote add origin https://github.com/brbruce/AWS_EdXWeek5Cloud9.git
    
    # Pushes the changes in your local repository up to the remote repository you specified as the origin
    git push -u origin master (paste username and password)

## Availablilty

Types of failures: Hardware, Planned (maintenance, upgrades), Unplanned downtime (bugs, human error, external issues like network, weather)

Failover a server to a different one

Or, loadbalance between two servers realtime

* S3 service - Replicated automatically.

* RDS - Automatic nightly backups.  Can also run multi-AZ or multi AWS Regions.

__Elastic Load Balancer__ (Application load balancer) - Fully managed service.

Configuration:  
* VPC  
* Security Groups  
* Ports to listen on  
* AZs to route to  
* EC2 servers to route to
* Healthchecks - Only send traffic to health servers  
* DNS - Using Amazon Route 53 DNS service

Clients must use DNS name, not IP.

Go to EC2 Console and select Options to configure an LB.

LB types:
* Application LB (* Choose this one)  
* TCP LB  
* Classic LB  

## Exersize 8

1. Start existing RDS Instance - __edx-photos-db__  
2. Create new security group in VPC __edx-build-aws-vpc__  
* Security Groups Name: __web-server-sg__  (sg-044225f0a11a1a7a9)  
* Go to EC2 console page, select Security Groups left nav menu.

3. Modify RDS security group to allow web server connection.  
* Select security group __rds-launch-wizard (sg-0965722cdbe774e58)__  
* Add inbound rule and select source = the new security group: __sg-044225f0a11a1a7a9 web-server-sg__  

4. Create new S3 bucket for deployment artifacts (?) - S3 console, Create new bucket: __ex8bucketbrbruce__  

5. Update app.ini file in rds exercise:  
* DATABASE_HOST=edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com  
* DATABASE_USER=web_user  
* DATABASE_PASSWORD=1W4&IK%oDoKQpKGQ   
* DATABASE_DB_NAME=Photos  
* FLASK_SECRET=asdfwe2342s  
* PHOTOS_BUCKET=bbruceedxbucket  
6. Update userdata.txt with new bucket name.
7. Zip up deploy and flaskApp dirs.  Need this to deplot to 2 new EC2 instances.
8. Copy zip to new S3 bucket:
    aws s3 cp ~/deploy-app.zip s3://ex8bucketbrbruce/

9. IAM Console - Roles - create role - type = EC2  
10. Use case = EC2 - Allows EC2 instances to call AWS services on your behalf.
11. Add 2 policies.  
* Search for "s3" and select AmazonS3FullAccess  
* Search for "Rekognition" and select AmazonRekognitionReadOnlyAccess.  
12. New role name = __ec2-webserver-role__

Trusted entities: AWS service: ec2.amazonaws.com  
Policies:  
[AWS Managed Policy] AmazonS3FullAccess  
[AWS Managed Policy] AmazonRekognitionReadOnlyAccess   

13. Provision Amazon EC2 instances and deploy the application via user data (in userdata.txt).  Create new EC2 instance just like before.
* Amazon Linux AMI  
* t2.micro  
* network=edx-build-aws-vpc, subnet=edx-subnet-public-a, IAM Role=ec2-webserver-role  
* Advanced Details: (Copy and paste contents of userdata.txt as Text)  
* Add Tag: Name=WebServer1  
* Configure security group - Select an existing security group option: web-server-sg  
* Launch



