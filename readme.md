# Learning AWS - "Fortune-A-Day"
A repository for all code related to the implementation of the "fortune-a-day" website exercise.

This readme also tracks personal progress through the objectives.

Sourced originally from [this reddit thread](https://www.reddit.com/r/sysadmin/comments/8inzn5/so_you_want_to_learn_aws_aka_how_do_i_learn_to_be/).

## Objectives

### Account Basics
&#9745; Create an IAM user for your personal use.

&#9745; Set up MFA for your root user, turn off all root user API keys.

&#9745; Set up Billing Alerts for anything over a few dollars.

&#9745; Configure the AWS CLI for your user using API credentials.
    
&#9745; <b>Checkpoint:</b> You can use the AWS CLI to interrogate information about your AWS account.

### Web Hosting Basics

&#9745; Deploy a EC2 VM and host a simple static "Fortune-of-the-Day Coming Soon" web page.

&#9745; Take a snapshot of your VM, delete the VM, and deploy a new one from the snapshot. Basically disk backup + disk restore.

&#9745; <b>Checkpoint:</b> You can view a simple HTML page served from your EC2 instance.

### Auto Scaling

&#9744; Create an AMI from that VM and put it in an autoscaling group so one VM always exists.

&#9744; Put a Elastic Load Balancer infront of that VM and load balance between two Availability Zones (one EC2 in each AZ).

&#9744; <b>Checkpoint:</b> You can view a simple HTML page served from both of your EC2 instances. You can turn one off and your website is still accessible.-

### External Data

&#9744; Create a DynamoDB table and experiment with loading and retrieving data manually, then do the same via a script on your local machine.

&#9744; Refactor your static page into your Fortune-of-the-Day website (Node, PHP, Python, whatever) which reads/updates a list of fortunes in the AWS DynamoDB table. (Hint: EC2 Instance Role)

&#9744; <b>Checkpoint:</b> Your HA/AutoScaled website can now load/save data to a database between users and sessions

### Web Hosting Platform-as-a-Service

&#9744; Retire that simple website and re-deploy it on Elastic Beanstalk.

&#9744; Create a S3 Static Website Bucket, upload some sample static pages/files/images. Add those assets to your Elastic Beanstalk website.

&#9744; Register a domain (or re-use and existing one). Set Route53 as the Nameservers and use Route53 for DNS. Make www.yourdomain.com go to your Elastic Beanstalk. Make static.yourdomain.com serve data from the S3 bucket.

&#9744; Enable SSL for your Static S3 Website. This isn't exactly trivial. (Hint: CloudFront + ACM)

&#9744; Enable SSL for your Elastic Beanstalk Website.

&#9744; <b>Checkpoint:</b> Your HA/AutoScaled website now serves all data over HTTPS. The same as before, except you don't have to manage the servers, web server software, website deployment, or the load balancer.

### Microservices

&#9744; Refactor your EB website into ONLY providing an API. It should only have a POST/GET to update/retrieve that specific data from DynamoDB. Bonus: Make it a simple REST API. Get rid of www.yourdomain.com and serve this EB as api.yourdomain.com

&#9744; Move most of the UI piece of your EB website into your Static S3 Website and use Javascript/whatever to retrieve the data from your api.yourdomain.com URL on page load. Send data to the EB URL to have it update the DynamoDB. Get rid of static.yourdomain.com and change your S3 bucket to serve from www.yourdomain.com.

&#9744; <b>Checkpoint:</b> Your EB deployment is now only a structured way to retrieve data from your database. All of your UI and application logic is served from the S3 Bucket (via CloudFront). You can support many more users since you're no longer using expensive servers to serve your website's static data.

### Serverless

&#9744; Write a AWS Lambda function to email you a list of all of the Fortunes in the DynamoDB table every night. Implement Least Privilege security for the Lambda Role. (Hint: Lambda using Python 3, Boto3, Amazon SES, scheduled with CloudWatch)

&#9744; Refactor the above app into a Serverless app. This is where it get's a little more abstract and you'll have to do a lot of research, experimentation on your own.

&#9744; The architecture: Static S3 Website Front-End calls API Gateway which executes a Lambda Function which reads/updates data in the DyanmoDB table.

&#9744; Use your SSL enabled bucket as the primary domain landing page with static content.

&#9744; Create an AWS API Gateway, use it to forward HTTP requests to an AWS Lambda function that queries the same data from DynamoDB as your EB Microservice.

&#9744; Your S3 static content should make Javascript calls to the API Gateway and then update the page with the retrieved data.
        Once you have the "Get Fortune" API Gateway + Lambda working, do the "New Fortune" API.

&#9744; <b>Checkpoint:</b> Your API Gateway and S3 Bucket are fronted by CloudFront with SSL. You have no EC2 instances deployed. All work is done by AWS services and billed as consumed.

### Cost Analysis

&#9744; Explore the AWS pricing models and see how pricing is structured for the services you've used.

&#9744; Answer the following for each of the main architectures you built:
- Roughly how much would this have costed for a month?
- How would I scale this architecture and how would my costs change?

### Automation
&#9744; Automate the deployment of the architectures above. Use whatever tool you want. The popular ones are AWS CloudFormation or Teraform. Store your code in AWS CodeCommit or on GitHub.

### Continuous Delivery

&#9744; Develop a CI/CD pipeline to automatically update a dev deployment of your infrastructure when new code is published, and then build a workflow to update the production version if approved.

### Miscellaneous / Bonus
#### IAM
&#9744; Learn how to create complex IAM Policies. You would have had to do basic roles+policies for the EC2 Instance Role and Lambda Execution Role, but there are many advanced features.

#### Networking
&#9744; Create a new VPC from scratch with multiple subnets.

&#9744; Once that is working create another VPC and peer them together. Get a VM in each subnet to talk to eachother using only their private IP addresses.

#### KMS
&#9744; Go back and redo the early EC2 instance goals but enable encryption on the disk volumes. Learn how to encrypt an AMI.