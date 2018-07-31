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

To add and commit changes from Cloud9:

    cd ~/repository
    git status
    git add .
    git status
    git commit -m "some message"
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
* DATABASE_PASSWORD=1W4&IK%oDoKQpKGQ   <-- Changed to ShowMeTheMoney
* DATABASE_DB_NAME=Photos  
* FLASK_SECRET=asdfwe2342s  
* PHOTOS_BUCKET=bbruceedxbucket  
6. Update userdata.txt with new bucket name.
7. Zip up deploy and flaskApp dirs.  Need this to deploy to 2 new EC2 instances.
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
* Launch - Choose existing key pair: AWS_E3_KeyPair2

IPv4 Public IP for WebServer1 = 34.221.235.118

14. Provision second Amazon EC2 instances and deploy the application via user data (in userdata.txt).  Create new EC2 instance just like above.  Only differences:
* subnet=edx-subnet-public-b  
* Add Tag: Name=WebServer2  

IPv4 Public IP for WebServer2 = 34.219.254.8

15. Test webservers by entering IP address in browser.  

NOTE: Got error 500 on both.  
 
### Debugging steps:
1. EC2 instance Actions > Instance Settings > Get System Log  
* Looks OK.  Last line is SSH login prompt.  
2. SSH to each webserver instance and check local logs.  Make sure to use ec2-user@<34.221.235.118> and use the latest keys AWS_E3_KeyPair2.ppk  

Check log:

    cat /var/log/cloud-init-output.log

Looks OK.

    sudo cat /var/log/uwsgi.log
    
    [2018-07-14 06:03:33,131] ERROR in app: Exception on / [GET]
    Traceback (most recent call last):
        ...
        File "./database.py", line 17, in list_photos
        conn = get_database_connection()
        ...
        File "/usr/local/lib64/python3.6/site-packages/mysql/connector/connection.py", line 210, in _auth_switch_request
        raise errors.get_exception(packet)
        mysql.connector.errors.ProgrammingError: 1045 (28000): Access denied for user 'web_user'@'10.1.2.8' (using password: YES)

Password problem or network problem?

Tested by installing mysql client on the webserver instance, and connecting.  Worked OK.

    sudo yum install mysql

    mysql -h edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com -u web_user -p

OK.  So connectivity and password are not the issue.

On each instance, added password debugging in database.py get_database_connection():     

    print("********Password: "+config.DATABASE_PASSWORD)
 
Restart the uwsgi process (as root):

    sudo restart uwsgi

Now reload the webserver page, and check the uwsgi logs:

    sudo cat /var/log/uwsgi.log
    ...
    ********Password: 1W4&IK/photos/Deploy/app.iniDoKQpKGQ

The password is actually "1W4&IK%oDoKQpKGQ" but contains "&o" which seems to be getting expanded to "/photos/Deploy/app.ini".

Try adding a "\" to escape it, and restart uwsgi.  No good:

    ********Password: 1W4&IK\/photos/Deploy/app.iniDoKQpKGQ

Need to change the password on the RDS server.

RDS Console > Instance > Actions > Modify > change Master password

New RDS password: "ShowMeTheMoney"

Wait till RDS server updated master password.  

Wait, need to update the password for web_user.  Connect to mysql using master user (with new password)

    mysql -h edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com -u master -p
    SET PASSWORD FOR 'web_user' = PASSWORD('ShowMeTheMoney');
    Query OK, 0 rows affected (0.00 sec)

Test the new password using mysql with web_user.  OK.

Update the app.ini file with the new password on both servers and restart uwsgi and retest the web pages. OK!!!

(Update the local Cloud9 instance ENV settings as well.)

16. Create and configure the Application Load Balancer.

* EC2 Console - Load Balancing - Load Balancers > Create > Application Load Balancer (HTTP)   
* Name: __photos-alb__  
* VPC: edx_build_aws_vpc  
* Select both Availablity Zones, and select the public subnet for each.  
* Select existing security group > Unselect Default, select web-server-sg  
* Configure Routing > New target group > Name = __webserver-target__  
* Register Targets > Select WebServer1 and WebServer2.  Click Register.
* Review and create.  Wait till status becomes Active.

Check Target Groups on left side navbar.  In Target tab in subpane, status of each registered target should be Health.

Go to Load Balancers, and select photos-alb and copy DNS name:

DNS Name: __photos-alb-299294069.us-west-2.elb.amazonaws.com__  

Check in browser.  App should appear.

17. Take one web server down and test for the availability of the application

EC2 console > Instances > select WebServer2 > Copy IP address.

SSH to WebServer2 in putty via IP address.  Delete the /photos/ app and restart uwsgi:

    sudo rm -rf /photos/
    sudo restart uwsgi 

If you connect in browser, you will see an nginx error! page.

Check Target Groups console page.  WebServer2 status should be unhealthy.

Test app in browser using LB DNS name.  Should still work.  OK!!

18. Cleanup

* Terminate both EC2 instances.  
* Delete the LB  
* Stop RDB  

19. Update github with latest changes to the Cloud9 app.


## Optional Challenge: Create AWS Elastic Beanstalk to deploy the flask app to.

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-flask.html

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/GettingStarted.html

1. Create "application source bundle".  I guess it's the same zip file as in the exercise?  Create new deploy-app.zip with corrected password.  Upload to S3 bucket for deployment.

    rm ~/deploy-app.zip
    cd ~/environment/exercise-rds
    zip -r ~/deploy-app.zip Deploy/ FlaskApp/
    aws s3 cp ~/deploy-app.zip s3://ex8bucketbrbruce/


NOTE: I tried to create an Elastic Beanstalk application but got an error that user edXProjectUser3 is not authorized to perform elasticbeanstalk:CreateApplication.

I checked, and the user's policy "edXProjectPolicy" does not have permission.  Logged in as root user, and edited the policy and clicked "Add additional permissions" link at the bottom right of the permissions list, then selected Elastic BeanStalk and added all actions, and all resources (got a warning that it needed some resource.  Gave it all, same as all the other permissions.)  Saved.

Log out, and log back in as edXProjectUser3. Click link to Sign in as different account, then enter anything as the account id or alias.  On the next screen, use lastpass to enter details for edXProjectUser3 (Account id is 570589970431)

NOTE 2: When I tried to start the Elastic Beanstalk instance, I got an error:

Creating Auto Scaling launch configuration failed Reason: API: autoscaling:CreateLaunchConfiguration User: arn:aws:iam::570589970431:user/edXProjectUser3 is not authorized to perform: autoscaling:CreateLaunchConfiguration

I needed to also add "Auto Scale" permission (all action, all resources).

2. Go to EB console, and click the "Create New Application" link in the top right corner. (rest of page is not clickable)  
* Application Name: __bbruceEB1__  
* Worked this time. On next page, select Create Environment, choose Web server env.  
    - Env name: __Bbruceeb1-env__
    - Platform: Python
    - Application code: Upload your code.  Select Public S3 URL: 
        - https://s3-us-west-2.amazonaws.com/ex8bucketbrbruce/deploy-app.zip
        - NOTE: At first, I tried s3://ex8bucketbrbruce/deploy-app.zip but it would not accept it.  I had to go to the S3 console, and click on the ex8bucketbrbruce link and then the deploy-app.zip link to get the public URL for the file, which is https://...

3. Due to the Auto Scale permission error, the instance was created but not started.  Go back to the EB console, select the instance and environment, and in the Actions menu, select "Rebuild Environment".  That started the instance.  The Health status is OK and green, but in the event log, there is an error:

Your WSGIPath refers to a file that does not exist.

URL: http://bbruceeb1-env.35fny3di4u.us-west-2.elasticbeanstalk.com/

(404 error)

The exercise hint says the application is just the flask app, and not the deploy directory.  Also, it says I need to set up environment variables.

How to SSH to the instance?

EC2 console > Security Groups > select Bbruceeb1-env > Incoming > Add SSH with source 0.0.0.0/0.

Click EC2 Instances for Bbruceeb1-env and get public IP: 52.36.184.13

Go to EB console, select the instance and env, select Configurations > Security > Modify > EC2 Key pair - Select "AWS_E3_KeyPair2" and Apply.

After instance restarts, SSH using putty to the EB instance.

App location: /opt/python/current/app/FlaskApp <-- Might not need the FlaskApp directory itself.

App log location(?): /var/log/httpd/access_log and error_log

I manually moved the files from app/FlaskApp up to app/.  Then I tried again in the browser.  Got a different error in the browser - 500 error.  In the httpd error_log:

[Sat Jul 14 22:03:37.528007 2018] [:error] [pid 2666] [remote 68.196.31.5:28160] ModuleNotFoundError: No module named 'requests'

If I run "pip freeze", it lists requests:

    pip freeze
    requests==1.2.3

That version is OK, as the requirements.txt file specifies "requests<2.19".

I tried restarting the app servers (EB console > Instance > Action > Restart Application Server(s)) but that did not help either.

Tried rebuilding the environment.  Disconnected SSH and restarted the instance.

Next thing to try is to build the application source bundle (deploy-app.zip) with only the application files inside FlaskApp/ and reupload to S3 and rebuild the environment.

    cd /home/ec2-user/environment/exercise-rds/FlaskApp
    rm -rf ~/deploy-app.zip
    zip -r ~/deploy-app.zip .
    less ~/deploy-app.zip (check the directories and files)
    aws s3 cp ~/deploy-app.zip s3://ex8bucketbrbruce/

That worked.  Did not need to change the version or update the EB instance or env.  Just replaced the old zip with new zip in S3, and rebuilt the EB environment.  In the EB event log, there was no error message this time about the WSGIPath.  The /opt/python/current/app directory had the application.py and other files, including the requirements.txt file, and the rebuild must have redeployed the app and executed the pip commands automatically.

4. The app still gets a 500 server error, but the httpd error_log shows it's because of the missing env settings.  I need to set them in the EB instance config page.

EB Console > (select instance) > Configuration > Software - Scroll down to Environment Properties:

* DATABASE_HOST=edx-photos-db.ccd69a3vcffc.us-west-2.rds.amazonaws.com  
* DATABASE_USER=web_user  
* DATABASE_PASSWORD=ShowMeTheMoney
* DATABASE_DB_NAME=Photos  
* FLASK_SECRET=asdfwe2342s  
* PHOTOS_BUCKET=bbruceedxbucket  

5. The EB instance is not able to communicate with the RDS database.  Need to update the security group.

    [Sat Jul 14 23:50:28.361525 2018] [:error] [pid 4911] [remote 68.196.31.5:168] mysql.connector.errors.InterfaceError: 2003: Can't connect to MySQL server on 'edx-photos-db.ccd69a3v

RDS is configured to use security group "rds-launch-wizard", which is configured in the Inbound rule Source to accept connections to port 3306 from Cloud9 security group, and from web-server-sg security group.  The EC2 instances and the ELB were configured to use this security group.  

I think we need to get the EB security group, and add it as a source sg in the "rds-launch-wizard" security group for port 3306.

EC2 Console > Instances - select BBruceeb1-env.  Looks in Description for security group:  __awseb-e-cz35wnbsmc-stack-AWSEBSecurityGroup-JLN7HCWR39MD__  
Group id: __sg-016d7450f169ba56c__  

EC2 Console > Security groups - select rds-launch-wizard.  Select Inbound.  Edit.  Add MYSQL/Aurora, and select the EB security group id.  

Uh oh, it does not show up in the dropdown list if you type "sg-" in source.  This is because the EB instance is not in the same VPC as the app.

There is a Network configuration card, but it says the env is not part of a VPC.  I think that is a major problem.  It should be part of the same VPC as the RDS.  Otherwise, the security groups can't be shared.

Start over from scratch.  I terminated the Bbruceeb1-env instance.  Also deleted the Bbruceeb1 application.

EB Console > Get Started (works now.)  Shows a page with "Create a web app".

App Name: bbruceeb2  
Platform: Python  
App Code: Upload  
App Code S3 URL: https://s3-us-west-2.amazonaws.com/ex8bucketbrbruce/deploy-app.zip

Important!!!  Click "Configure More Options" instead of creating the instance right away, so we can set up the VPC and security groups.

Configure Bbruceeb2-env: 
Configuration Presets: Low Cost (Free Tier Eligible)  
Software: Env Properties: (Enter same env settings as above)
Capacity: Single Instance (default is OK)
Instances: (Need to set Security groups after setting VPC.  Come back later)
Security: EC2 Key pair: AWS_E3_KeyPair2
Network: VPC: edx-build-aws-vpc
Network: Instance subnets: Check "Assign Public IP address" and select the public-a and public-b subnets.  (Because there is no load balancer with a single instance EB).

Go back to Instances, and select "web-server-sg" as the EC2 security group.  This group has access to RDS already.

Create App

This time, the app started and I could see the upload form and saved images.  So the EB now has access to RDS.

BUT, the images are broken.  There is an access error in the S3 image links.  EB needs to have access to S3 service.  Need to use Roles for API access.  (Security groups are for network access)

There are roles for S3 access already created.  "ec2-webserver-role" has permission policies for AmazonS3FullAccess and AmazonRekognitionReadOnlyAccess.

EB has a role already set up:  __aws-elasticbeanstalk-ec2-role__ (Listed under the Security panel in Configuration).  Add the same permissions there.

IAM Console > Roles > aws-elasticbeanstalk-ec2-role > Attach Permissions:  
* AmazonS3FullAccess  
* AmazonRekognitionReadOnlyAccess  

NOTE: I tried adding them to aws-elasticbeanstalk-service-role first, but that did not work.

That worked.  I can see pictures now.

But, when I upload, there was a server error.  httpd error_log shows:

    [Sun Jul 15 01:48:17.460386 2018] [:error] [pid 2855] [remote 68.196.31.5:172] botocore.exceptions.NoRegionError: You must specify a region.

Need to set env property for AWS_DEFAULT_REGION=us-west-2.  This setting was in the /Deploy/app.ini file, but that is not used for EB.  I added it to the EB instance configuration in the Software panel.

FINALLY WORKS!!!!





### Side Note:  Installing EB CLI

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html?icmpid=docs_elasticbeanstalk_console

    pip install awsebcli --upgrade --user

To update the install, just run the same command again.

$PATH already has ~/.local/bin in it for the Cloud9 server.

Some commands:

    eb ....

vs ???

    aws elasticbeanstalk ...


    
---------------------

When you create a Elastic Beanstalk env, it automatically creates the following:

    EC2 instance – An Amazon Elastic Compute Cloud (Amazon EC2) virtual machine configured to run web apps on the platform that you choose.

    Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof. Most platforms use either Apache or nginx as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs.

    Instance security group – An Amazon EC2 security group configured to allow ingress on port 80. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app. By default, traffic isn't allowed on other ports.

    Load balancer – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application. A load balancer also eliminates the need to expose your instances directly to the internet.

    Load balancer security group – An Amazon EC2 security group configured to allow ingress on port 80. This resource lets HTTP traffic from the internet reach the load balancer. By default, traffic isn't allowed on other ports.

    Auto Scaling group – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable.

    Amazon S3 bucket – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk.

    Amazon CloudWatch alarms – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low. When an alarm is triggered, your Auto Scaling group scales up or down in response.

    AWS CloudFormation stack – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes. The resources are defined in a template that you can view in the AWS CloudFormation console.

    Domain name – A domain name that routes to your web app in the form subdomain.region.elasticbeanstalk.com.

    All of these resources are managed by Elastic Beanstalk. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains.

---------------------------

## Week 4

### Amazon Cognito to authenticate and authorize.

User pool:
* Attributes  
* Policies  

App Client - Can have different read/write permissions

### Login process:

1) Login - When the app gets the login request, it creates a "csrf_state" random hex bytes.  Then it creates a redirect URL to the Cognito server.  Sends to the browser.

The URL contains the app's Cognito client id, the csrf_state, and a callback URL to the app.  

2) The browser redirects to the Cognito server and logs in.  The Cognito server will redirect back to the app's callback URL, and passes back a code which indicates the user was authenticated, and the csrf_state.

3) The app gets the callback request.  It will directly call the Cognito server to check the code is valid.  We pass the app's Cognito client id and the code, and the original redirect URL.  The Cognito server will return some tokens. ID and Access tokens.

4) The app processes the returned tokens, and verifies them using some keys from the Cognito user pool.  It also checks the csrf_state with the original one in the session.  From the ID token, you get the cognito user name, nickname, etc.

### TLS to secure connection

HTTPS connection to prevent eavesdropping and MITM attacks.  Need server certificate.

Amazon certificate manager - For certs to be used on AWS services like ELB.  Need Domain.

Browser to ELB.

End-to-End encryption - Might need more security.
* In transit
    - Between ELB and EC2.  HTTPS and a cert for EC2.
    - API calls. HTTPS endpoints for S3, Rekognition and Cognito.
* At rest.  (In a DB)
    - Cognito - handles encryption
    - RDS - enable encryption flag.
    - S2 - enable encryption flag.  Bucket or per object level.
    - Rekognition - Does not store data.
    - EC2 - Uses EBS
    - EBS - (Elastic block store) enable encryption flag.

### AWS Certificate manager to set up HTTPS on ELB

ELB - When creating the LB, need to add HTTPS listener.  Will be prompted for a certificate.  Hostname must match the certificate.  Can set up a CNAME for the server in DNS to match the cert.

Security group must include both port 80 and 443.

Can integrate ELB with Amazon Web Application Firewall to prevent attacks.  Sql injection, Cross site scripting, etc.

### SSH Tunnel from localhost to C9 server (For testing without ELB certs)

Setting up local PC port forwarding (using Putty, no Cygwin installed):

http://realprogrammers.com/how_to/set_up_an_ssh_tunnel_with_putty.html

    Create a session in PuTTY and then select the Tunnels tab in the SSH section. 

    In the Source port text box enter 3306. This is the port PuTTY will listen on on your local machine. It can be any standard Windows-permitted port. 

    In the Destination field immediately below Source port enter 127.0.0.1:3306. This means, from the server, forward the connection to IP 127.0.0.1 port 3306. 

    Click the Add button.  Return to the Session tab and click Save

Using ssh command in bash shell:

    ssh -i certname ec2-user@(server IP) -L localhost(on pc):5000:localhost(on server):5000


## Exercise 9 - Add sign-in using Cognito

Login with edXProjectUser3.  
Start RDS server.  (Was already running.  Started automatically)  
Set up Amazon Cognito user pool.  
* Log in with email  
* Required attr: Nickname.  
* Next  
* Next (Security Policy)  
* Next (MFA)  
* Verification Type = Link  
* Skip to App Clients.  Create new app client named "WebsiteClient"  
* Create Pool.  

Pool Id: __us-west-2_f7H1OM1ys__

Pool ARN: __arn:aws:cognito-idp:us-west-2:570589970431:userpool/us-west-2_f7H1OM1ys__

* App Client Settings -  
* Domain Name - https://brbruce-ex9.auth.us-west-2.amazoncognito.com  
* App Client:
    - ID: 7l7q0a6u04e5ffjs1sqm6fjeoh  
    - Secret: 19cs5t5ba5di21qs8usffv16qnincmf9rdciptf1c9iqq4rvvli4  

Cloud9: Download updated code:

    cd ~/environment 
    wget https://us-west-2-tcdev.s3.amazonaws.com/courses/AWS-100-ADG/v1.0.0/exercises/ex-cognito.zip
    unzip ex-cognito.zip

The login, logout and callback route functions require config params, which are loaded from the server env.  Need to set that up.

Start puttygen and load private key "AWS_E3_KeyPair2.ppk".  Copy private key string and copy and paste in .ssh/authorized_keys file.

Check public IP of cloud9 instance: (Share > Application = 35.160.226.19.  Or check running EC2 instance)

Start Putty and set up SSH tunnel. (Follow details above)  

* ec2-user@35.160.226.19    
* Connection > SSH > Auth > Private Key > ....  
* Connection > SSH > Tunnel > Source port 5000, server port localhost:5000.  Add.  
* Save connection.  Open.  Test in browser.  Worked.    
http://localhost:5000 (not https)

## Exercise 9 Part 2 - Enable "Log in with Amazon"

https://login.amazon.com/website

NOTE: Only set up the application.  Do not follow the rest of the instructions on the site above.

### Amazon Developer Registration

Using my login brian@brianbruce.org

Display Name: Brian Bruce

Merchant picker: Login with Amazon

Login with Amazon App Developer Home Page:  
https://sellercentral.amazon.com/home?ref_=mp_home_logo_xx&cor=mmp_NA

Register New Application:  
* Name: EdXEx9  
* Privacy URL: https://brbruce-ex9.auth.us-west-2.amazoncognito.com/privacy.html  (Should I use http://35.160.226.19:5000 instead?)  

Web settings:  
* Client Id: amzn1.application-oa2-client.115c03197cab43978421b67260434289   
* Client Secret: (Secret)  
* Allowed return URLs: https://brbruce-ex9.auth.us-west-2.amazoncognito.com/login   
* Also added return URL:  https://brbruce-ex9.auth.us-west-2.amazoncognito.com/oauth2/idpresponse (Due to error message. See below)


### Set up Cognito to support LoginWithAmazon

In Cognito User Pools, you will need to add Log in with Amazon as an identity provider. For more information, see Configuring Federation with a Social Identity Provider.

https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-identity-provider.html

Enable Log in with Amazon as an identity provider with your Amazon Cognito app client
The application wants a nickname. This will need to be mapped from a Log in With Amazon attribute to a User Pool attribute.

Go to AWS Cognito Console.  (Need to log in as root user.  EdX user does not have role for Cognito.)

Manage User Pools:  photos-pool

Select Federation in side menu.  Enable Identity Providers.  Select Log in with Amazon:

* Amazon App Id:  (Client Id from app above)  amzn1.application-oa2-client.115c03197cab43978421b67260434289  
* App Secret: (from secret above)  
* Authorize_scope: profile (guessing based on cognito user pool setup required attr = nickname and login with email above)   
    - See https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-identity-provider.html  
    - https://developer.amazon.com/docs/login-with-amazon/customer-profile.html  
    - Profile scope includes user_id, name and email.

Click Enable login with Amazon.

Click Configure Attribute Mapping.  Click Amazon.  
* Amazon Attribute: user_id = User pool attribute: Username (Cannot change this!!!)   
* Amazon Attribute: email = User pool attribute: email
* Amazon Attribute: name = User pool attribute: nickname   
   

Save changes.

You will also need to choose which identity providers to enable for each app on the Apps settings tab under App integration.

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-idp-settings.html

On the Cognito console, select App Client Settings.

App client WebsiteClient   
ID 7l7q0a6u04e5ffjs1sqm6fjeoh   

Change checkbox from Cognito User Pool to LoginWithAmazon.  Save changes.

Tested login.  Got some errors because of the scope.  Set to "profile" as above.

Tested again.  Got error that redirect URL was not whitelisted:

    Error Summary
    400 Bad Request
    The redirect URI you provided has not been whitelisted for your application. Please add your redirect URI in the 'Allowed Return URLs' section under 'Web Settings' on the Amazon Seller Central App Console for Login with Amazon.
    Request Details
    client_id=amzn1.application-oa2-client.115c03197cab43978421b67260434289
    redirect_uri=https%3A%2F%2Fbrbruce-ex9.auth.us-west-2.amazoncognito.com%2Foauth2%2Fidpresponse
    scope=profile
    response_type=code 

Added to allowed return URL: (In LoginWithAmazon console app page) https://brbruce-ex9.auth.us-west-2.amazoncognito.com/oauth2/idpresponse 

Tested.  Got Amazon login page (brian@brianbruce.org).  Got prompted for access to Profile.  Allowed.

The test app appeared, and I was logged in with "Brian Bruce"!

Also worked when I enabled BOTH LoginWithAmazon and Cognito user pool.  Login page shows both Amazon id and email login.

## Exercise 9 Part 3 - Optional Advanced Challenge 2

A second advanced challenge: the code is currently signing out users after the Amazon Cognito access_token expires. 

Instead of signing users out when the access_token expires, you can exchange the refresh_token for id_token and access_token. For more information, see TOKEN Endpoint. Replace the return None code above with code to exchange the refresh_token. If successful, the user session can be repopulated and logged in with flask_login.login_user. 

https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html









