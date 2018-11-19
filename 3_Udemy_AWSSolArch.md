Udemy AWS Solutions Architech Associate Certification Course
=============================================================

https://www.udemy.com/aws-certified-solutions-architect-associate/learn/v4/content

Section 1 - 10/8/2018
=====================

1 - Intro to AWS certs and exams:
---------------------

(Easy to hard)
1. Cert Cloud Practitioner
2. Dev Assoc
3. Sol Arch Assoc <==========
4. Sysops Admin assoc
5. ... Specialty
6. Devops pro
7. Sol Arch pro

acloud.guru - training for pro courses not on udemy.  Need membership.

Discussion forum for the course:

https://acloud.guru/forums/aws-certified-solutions-architect-associate/recent?p=1

Old vs new exam:
https://www.youtube.com/watch?v=8f9YtBH23e8

New exam (Beta)
https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS_Certified_Solutions_Architect_Associate_Exam_Guide_BETA.PDF

New Exam Guide:
https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS_Certified_Solutions_Architect_Associate_Feb_2018_%20Exam_Guide_v1.5.2.pdf

Well-architected framework whitepaper: 
https://aws.amazon.com/architecture/well-architected/

https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

Operational Excellence:
https://d1.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf

Security:
https://d1.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf

Reliability:
https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf

Performance Efficiency:
https://d1.awsstatic.com/whitepapers/architecture/AWS-Performance-Efficiency-Pillar.pdf

Cost Optimization:
https://d1.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf

AWS Best Cloud Practices: 
https://d0.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf


3 - The Exam Blueprint
---------------------

New Exam Details:

Pass need 70%
$75
150 mins
80 questions
Valid for next 2 years



Section 2 - 10000 ft overview - 11/2/2018-11/12/2018
=====================

Global Infra
--------------------

* **Regions** - Geographical area - 16

* **Availablity zones** - data center - 44

* **Edge location** - endpoints - cache contents (CloudFront - CDN network) - 96

Services
----------------

#### Compute
* EC2 - VMs and physical
* EC2 Container - manage Docker containers
* Elastic beanstalk - Upload code.  Auto provisions EC2 instances, etc.
* Lambda - Upload code.  Control when it executes via events, etc.
* Lightsail - VPS service, watered down EC2, provision server and fixed ip and rds/ssh access.
* Batch - batch computing.  Not covered in exam.

#### Storage
* S3 - Simple storage service - Object based storage.  Buckets in the cloud.  Also flat files (quiz)
* EFS - Elastic file system - network attached storage.  Mount to mult VMs
* Glacier - archive data longterm storage
* Snowball - import large amount of data, via disk

#### Databases
* RDS - Relational
* DynamoDB - Non relational
* Elasticache - cache data from db to webservers
* Red Shift - complex queries - Data warehouse

#### Migration services - Migrating apps to AWS
* Migration Hub - Tracking service as you migrate apps to AWS
* App discovery service - detects dependencies of your apps
* DB migration service - Migrate from other dbs like oracle
* Server migration service - Migrate servers
* Snowball - migrate data

#### Networking & Content delivery
* VPC !!! - Virtual private cloud - Important!!!!!
* CloudFront - CDN
* Route 53 - DNS service
* API Gateway - Way to create own APIs for your other services
* Direct Connect - Dedicated line from corp office to AMZ VPC

#### _Dev Tools <=== Not on any exams_
* CodeStar - Project manage code
* CodeCommit - Source code control - git repos
* CodeBuild - Compile and test and package
* CodeDeploy - 
* CodePipeline - Continous Delivery
* X-Ray - Debug and analyze, request tracing, perf bottlenecks
* Cloud9 - IDE

#### Management tools
* CloudWatch - Monitoring system
* CloudFormation - Sol Architect!!!!  Scripting Infrastructure, code and template to deploy infra.
* CloudTrail - AWS mgt console audit, logs changes to your aws env.
* Config - Monitor configuration, Real time snapshots, 
* OpsWorks - Like Elastic Beanstalk, uses Chef and Puppet to automate envs, and configuration.
* Service Catalog - Catalog of services
* Systems Manager - Interface for managing AWS resources like EC2.  Patch maintenance across 1000s of instances, etc.
* Trusted Advisor - vs Inspector - Advice and suggestions around security, improve usage for AWS resources, save you money, etc.
* Managed Services - If you don't want to manage AWS yourself, Amazon will do it for you.

#### _Media Services <=== Not on any exams_
* Elastic Transcoder - Takes video and resizes it for diff clients
* MediaConvert - File based video transcoding
* MediaLive - Broadcast video streams
* MediaPackage - Protect media over the internet
* MediaStore - Storage service optimized for media
* MediaTailor - Target advertising into video streams

#### _Machine Learning <=== Not on any exams_
* SageMaker - Easy for devs to use deep learning (Neural networks)
* Comprehend - Does sentiment analysis, are people saying good or bad things about your products
* DeepLens - Camera with AI, doing this on the camera.  Front door camera, evaluate vistors
* Lex - Alexa service, AI speech
* Machine Learning - diff from deep learning (SageMaker).  Deep learning is Neural nets, which is more intelligent than ML.  Load dataset into the cloud, predict outcome.  (used for recommended products in Amazon store)
* Polly - Text to speech (mp3 files).  Good speech.  Sounds human and choose languages.
* Rekognition - Video and image recognition, keywords.
* Translate - Language translation
* Transcribe - Auto Speech to text

#### Analytics
* Athena - SQL queries on objects in S3 buckets
* EMR !!! - Elastic Map Reduce - Big Data.  Chops data up for analysis on lots of servers
* CloudSearch
* ElasticSearch Service
* Kinesis !!! - Ingest lots of Big Data into AWS.  Social media feeds and tweets.  Search for hashtags.
* Kinesis Video Streams - Same for video
* QuickSight - business intelligence tool, very cheap
* Data Pipeline !!! - Moving data between services
* Glue - ETL - Extract Transform Load

#### Security Identity and Compliance
* IAM !!! - Identity Access Mgt - 
* Cognito - Authentication for mobile devices, grant temporary access to AWS resources
* GuardDuty - Monitor for malicious activity on AWS account
* Inspector - Agent to run tests, generate report for severity lists.
* Macie - Scan S3 buckets for Personal information and alert
* Certificate Manager - Manage ssl certs
* CloudHSM - Hardware security module, store keys
* Directory Services - Integrate microsoft dir services with aws
* WAF - Web App Firewall - Layer 7 Firewall. Stops XSS cross site scripting, sql injection, application level.
* Shield - DDOS mitigation.  Advanced shield
* Artifact - Audit and compliance portal, on demand access to AWS compliance reports, manage select agreements (???)

#### _Mobile Services <=== Not on any exams_
* Mobile Hub - Mgt console for mobile apps.
* Pinpoint - Targets push notifications
* AppSync - Updates data in web and mobile apps realtime
* Device Farm - Way to test apps on real client hardware, android, Iphone
* Mobile Analytics

#### _AR/VR <=== Not on any exams_
* Sumerian

#### App Integration
* Step Functions - Way to manage lambda functions
* Amazon MQ
* SNS !!! - Notification svs
* SQS !!! - Queue svs, queue and poll
* SWF !!! - Workflow service

#### _Customer Engagement <=== Not on any exams_
* Connect
* Simple Email Service - Scalable, cost effective

#### _Business Productivity <=== Not on any exams_
* Alexa for business - 
* Chime - Video conferencing
* Work Docs !!! - Dropbox file sharing
* WorkMail - 

#### Desktop and App Streaming
* Workspaces - Run desktop in the cloud
* AppStream - Like Citrix, run apps in the cloud

#### _IOT  <=== Not on any exams_
* IOT - Internet devices, Sensors
* IOT Device Management
* FreeRTOS - OS for IOT
* Greengrass - Secure way to run stuff on devices

#### _Game Dev  <=== Not on any exams_
* GameLift - 

### What is on the Solutions Architect Assoc test?

* [Global Infra](#Global-Infra)
* [Compute](#Compute)
* [Storage]()
* [Databases]()
* [Migration services](#Migration-services-\--Migrating-apps-to-AWS)
* [Networking & Content delivery](#Networking--Content-delivery)
* [Management tools](#Management-tools)
* [Analytics](#Analytics)
* [Security Identity and Compliance](#Security-Identity-and-Compliance)
* [App Integration](#App-Integration)
* [Desktop and App Streaming](#Desktop-and-App-Streaming)

### Dev Assoc Test?

* [Global Infra](#Global-Infra)
* [Compute](#Compute)
* [Storage]()
* [Databases]()
* [Networking & Content delivery](#Networking--Content-delivery)
* [Management tools](#Management-tools)
* [Analytics](#Analytics)
* [Security Identity and Compliance](#Security-Identity-and-Compliance)
* [App Integration](#App-Integration)

### SysOps Admin Assoc Test?

* [Global Infra](#Global-Infra)
* [Compute](#Compute)
* [Storage]()
* [Databases]()
* [Networking & Content delivery](#Networking--Content-delivery)
* [Management tools](#Management-tools)
* [Security Identity and Compliance](#Security-Identity-and-Compliance)
* [App Integration](#App-Integration)

### 11. Sign up to AWS Free Tier

http://aws.amazon.com

AWS Root user login: junk1*****

Basic Plan




Section 3 - Identity Access Management - 11/12/2018 - 11/13/2018
=====================

12 - IAM 101
-------------------

### Features:

* Central control of AWS account
* Shared access to AWS account
* Granular permissions
* Identity federation (across multiple directories)
* Multifactor auth
* Temporary acesss for users and devices and services
* Set up own pwd rotation policy
* Integrates with many AWS services
* Supports PCI DSS compliance (Payment Card Industry Data Security Standard) - Credit cards

### Terms:

**Users**

**Groups** - Users under one set of permissions

**Roles** - Assign roles to AWS resources

**Policies** - Documents that defines one or more permissions


### IAM Lab 1

IAM service is _global_ service, not regional.

Go to IAM console.  Customize login link with alias to make it easier.

https://bb*********.signin.aws.amazon.com/console 

Protect root account.  Don't use it frequently.  Set up non-root users.

Also set up Multi-factor auth on the root account.

Create new users ryan and john with both Programmatic and Console access.

Create new group for the users.  system-admins

We want them to be able to administrate the aws env. Policy list - search for 'admin'

Policy: AdministratorAccess - Full access, like root account.  Assign this for the ex.

Create users and groups.

Each user has Access Key ID, and Secret Access Key.  These are used for programmatic access, not login.  Users can use user id and password for login.

Download the credentials.  (UdemyAWSSolArchTrainingUserKeys)

Create group HR with AmazonS3ReadOnlyAccess.  Move John from system-admins group to HR group.  

Add permissions to John directly. AmazonGlacierFullAccess

Apply an IAM password policy.

Roles:  Roles are used to grant permissions to trusted entities like:

* IAM user in diff account
* Application code running on EC2
* AWS service acting on resources in your account
* Corp users using identity federation

Create a role to allow EC2 instances to write to files to S3. Select Role type = EC2

Select permissions policy = AmazonS3FullAccess. Create role name = S3-Admin-Access

### IAM Lab 2 - Billing alarm

My Account > My Billing Dashboard > Preferences > enable Receive Billing Alerts

Then click on Manage Billing Alerts link in the text of the Receive Billing Alerts checkbox.  Goes to Cloudwatch > Alarms.  Select Billing > Create Alarm.  Set Total EstimatedCharges > $5.  Save.

### IAM Recap

IAM is universal, global.  Not regional.

* Users
* Groups
* Roles
* Policy Documents (json format)

Root account has complete admin access.

New users have no permissions by default.

New users have access key id and secret access keys for API and CLI access.  Not same as password.  Must save them in secure location.

Power users have access to all AWS services except management of groups and users within IAM. (From quiz.)

Always set up multifactor authentication on root account.  (Also available for normal users)

Also set up password rotation.




Section 4 - AWS Object Storage and CDN - S3, Glacier and CloudFront - 11/13/2018 - 
===============================

16 - S3 101
-------------------

S3 = SSS = Simple Storage Service - Place to store your files, object based, pictures, video, document.

Data is spread across multiple devices and facilities

Size from 0 to 5 TB.  Pay by the gig.

Uplaoded and stored in Buckets (folder in the cloud).  Each bucket has global unique namespace.  Part of a URL.

URL pattern:  `http://s3-<region>.amazonaws.com/<bucketname>`

Successful upload to S3 results in HTTP 200 status code

### Data Consistency Model for S3

Read after write for new PUTS.  Can read immediately creating.

Eventual Consistency for updates (overwrite PUTS) and DELETES.  Can take some time to propagate.

Simple key-value store.  Object based:
* Key
* value
* Version ID
* Metadata
* Subresources:
  * Access Control Lists
  * Torrent

Built for 99.99% availability.  Amazon guarantees 99.9% availability.

11 x 9s for durability of data.  Won't lose data.

Tiered Storage classes avail.

Lifecycle management - Age of file, archive, etc.

Versioning - multiple versions of a file

Encryption

ACLs for file level and Bucket policies

#### S3 Tiers/classes

* S3 Standard - 2-9s avail, 11-9s durability.  Multiple devices and facilities (AZs).  Designed to withstand loss of 2 facilities.

* S3 IA - Cheaper.  Infrequent access, but still rapid access.  Lower cost than standard but there is a retrieval fee.

* S3 One Zone IA - Even cheaper.  If you don't need multiple AZ resilience.

* Glacier - Cheapest, archival only.  Expedited,  Standard, Bulk - different fees for retrieval times.  Standard is 3.5 hour retrieval time.

#### S3 charges

Charged for:
* storage
* Requests
* Storage management pricing (tags and metadata)
* Data transfer pricing - Cross region replication
* Transfer acceleration - Use of caching.  Edge locations, CloudFront

**Read S3 FAQ !!!!**

17 - Lab - Create S3 bucket
-----------------------------

S3 console - Global level

Create bucket, add tag

Upload image, make it public, add tag

Minuimum size is 0 bytes.

Manage entire bucket - Can control permissions in 2 ways:

* ACL Control list - Owner, User, Public

* Bucket Policy - Lets you edit a policy doc for the bucket.
  * Can use policy generator.
    * Policy Type = S3 Bucket Policy
    * Principal, AWS Service, Actions or ARN

** By default, all buckets and objects in them are PRIVATE. **

Encryption:

* Client side
* Server side Encryption (SSE)
  * Amazon S3 Managed Keys (SSE-S3)
  * KMS (SSE-KMS)
  * Customer provided keys (SSE-C)

18 - Lab - Version control
------------------------------

S3 Console - Select bucket created above.  Select Properties > Versioning > Enable Versioning

NOTE: Once you enable versioning, you cannot turn it off!  You can suspend it only.

Versioning can cost more due to higher storage.

Upload a file.  Then edit it and upload again.  You can view both versions.

To restore the old version, you can either reload the old version again, or delete the latest version by clicking the garbage can icon in the list.

You can also delete the entire object.  But you can still view it by setting Versions from Hide to Show.  You can see there is a Delete Marker above the object.  You can restore the object by selecting the Delete Marker and deleting it via the Actions.  Then you can see all the previous versions which were there before.  (If you delete a specific version, it's gone for good though.)

#### MFA Delete - Multifactor 

Optional additional protection

19 - Lab - Cross Region Replication for S3
--------------------------------------------

S3 console > Buckets

Create new bucket, with different region from first bucket.

Turn on cross region replication - Select a bucket > Management > Replication

(NOTE: It should fail because cross-region replication requires Versioning to be turned on for both buckets.)

Create Replication rule.  All contents of the bucket (could do Prefix, which is folders). 

When I select the new bucket to create a rule, I see a message that versioning must be turned on.  When I select the existing bucket, which does have versioning, the message does not show up.  (Probably will later on.)

Next.  Select destination bucket.  Select the new bucket.  Now I see the warning about versioning again.

Can also change the storage class for the destination.  Ex. From Standard to Standard IA (Infrequently accessed) if you are only replicating as a backup.

Next.  Select a Role.  You can tell it to create a new role.  Enter a rule name, and mark it enabled.

Next.  It got created.

However, only new and updated objects will get replicated.  Existing objects will not get replicated.

#### Bucket Replication via command line

To replicate existing object, easiest way is via commandline.

Download AWS commandline tool and run it from commandline window:

aws configure - Need to provide a user's API key and secret.

Create a new group - cliAdmins1 - and a new user udemyCli1.  Get the API key and secret and use it in the aws configure commandline.  Use "us-east-1" as the default region.  None as default output format.

**aws s3 ls** - List the buckets in S3.

```
C:\Users\Brian>aws s3 ls
2018-11-13 16:19:21 bbruce2
2018-11-13 15:11:28 brbruce

C:\Users\Brian>aws s3 cp --recursive s3://brbruce s3://bbruce2
copy: s3://brbruce/test1.txt to s3://bbruce2/test1.txt
copy: s3://brbruce/barknado_pooch_cafe_small.jpg to s3://bbruce2/barknado_pooch_cafe_small.jpg
```

NOTE: In the video, they demonstrated that deleting a file is replicated.  However, this did not work for me, and I found info that in V2 of replication config, deletes are not replicated:

https://docs.aws.amazon.com/AmazonS3/latest/dev/crr-add-config.html#crr-backward-compat-considerations

> When you delete an object from your source bucket without specifying an object version ID, Amazon S3 adds a delete marker. If you use V1 of the replication configuration XML, Amazon S3 replicates delete markers that resulted from user actions. In other words, if the user deleted the object, and not if Amazon S3 deleted it because the object expired as part of lifecycle action. In V2, Amazon S3 doesn't replicate delete markers and therefore you must set the DeleteMarkerReplication element to Disabled. 

#### What gets replicated?

* New objects (not existing objects before replciation is turned on)
* Must be readable to owner of the source bucket
* ACLs and metadata and tags are replicated
* Objects encrypted using Amaz S3 managed keys (SSE-S3).  And AWS KMS managed keys (SSE-KMS) if explicitly enabled.

Not replicated:

* Objects which existed before replication was enabled.
* Deletes of the latest version (delete marker) - as of V2 replication config
* Deletes of specific versions
* objects which owner of source bucket cannot read
* AWS KMS managed key encrypt unless enabled.
* bucket level sub-resources such as lifecycle configurations
* actions performed by lifecycle configs
* source objects which are already replicas of other sources.
* In V1, the delete marker was replicated, but if you deleted the delete marker, that was not replicated, so the destination still has the delete marker. 

#### Tips

* Versioning must be enabled on both buckets
* Regions must be different
* Existing files are not replicated, only new and updated files.
* Cannot replicated to multiple buckets or daisy-chain
* Delete markers are NOT replicated (as of V2)
* Individual version delete are not replicated

19B - Cross-Region Replication Monitor
---------------------------------------

https://docs.aws.amazon.com/solutions/latest/crr-monitor/welcome.html

https://docs.aws.amazon.com/solutions/latest/crr-monitor/template.html

CloudFormation template to deploy set of services which will monitor S3 CRR. Uses CloudTrail, CloudWatch, SNS, SQS, Lambda, DynamoDB and Kinesis.

See crr-monitor.template

20 - Lab - Lifecycle management, S3-IA, Glacier
---------------------------------------------------

Lifecycle - Move data from S3 to S3-IA and later to Glacier, and eventually delete the object.  Lifecycle, lives in S3 buckets.

Create new bucket "bblifecyclebucket" with versioning turned on, all other defaults.

View bucket detail, click on Management > Lifecycle.  Can use it to:

1. Manage object's lifecycle
2. Automate transition to tiered storage
3. Expire objects

Click Create Lifecycle, enter name, next.

Configure Transitions.  Check Current version = After object creation.

Transition to Standard IA after 30 days.\
Transition to Amazon Glacier after 60 days.

Check Previous versions = After object becomes a previous version (has been replaced).

Same transitions.

Expire rules - after 425 days (both current and previous versions).

NOTE: Cannot enable clean up expired object delete markers if you enable Expiration. 

#### Tips

* Used to minimize storage costs.
* Can be used with or without versioning
* Can be applied to current version and previous versions
* Can transition from S3 to S3-IA after minimum of 30 days after creation date
* Can transition from S3-IA to Glacier after minimum of 30 days after S3-IA transition
* Can permanently delete objects

21 - CloudFront CDN
--------------------------

CDN = Content delivery Network.  Geographic caching.

Edge Location = Where content is cached.  Different from Region or AZ.

Origin = Source of files.  S3 bucket, EC3 instance, Elastic LB or Route 53

    NOTE: Can have multiple origins within the same distribution.

Distribution (noun) = CDN, which consists of multiple Edge Locations

Will use a "distribution" URL, which goes to an edge location.

Request goes from user to Edge location first, checks if the content is already cached.  If not, then goes to the origin.  When cached, will remain in the edge location for the TTL duration.  

Slow for first user, but fast for other users.

Web Distribution - Web content

RTMP - Media Streaming

Can also write (Put) objects in Edge locations, not just read-only.  Will be replicated back to the origin server.

Objects have TTL - How long it will be cached before expiring.

Can clear cached objects if you need to do an update, but will be charged.

21B - Lab - CloudFront distribution
-----------------------------------------

Go to CloudFront console.  Create Distribution.  Web or RTMP.

Select Web Dist.  Select an origin bucket for the Origin domain name.  Will fill in Origin ID automatically, but you can edit it.

Select Restrict Bucket Access = Yes - Can force all access to go through CloudFront, and not directly to the origin.

Path patterns - Reg Exp.  Select among multiple origins.

Protocol policies - Redirect HTTP to HTTPS

Headers - Include GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE

TTL Min = 0\
Max = 31,536,000 (365 days)\
Default = 86,400 (24 hours)

Restrict Viewer Access - Signed URLs or Signed Cookies - Secure videos to authorized users, etc.

Price Class - Use all edge locations for best performance (vs cost)

WAF Web ACL - Web application firewall

Alternate domain names - The default URL is very messy, random letters and numbers.  Can specify something human readable like "cdn.acloud.guru" (CNAME)

SSL cert - If you use an alternate domain name, you would need to use a custom ssl cert to match it.

Create.  Wait.  Status is "In Progress".  Takes a while.

























