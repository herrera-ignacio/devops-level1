# DevOps Level 1

This is the material and guideline used for my lecture on 'DevOps and AWS Fundamentals for Web Development' at [Amalagama](https://amalgama.co).

## Schedule

### [Day 1 (08/07/2020): Intro to Cloud & AWS](./day1)

* DevOps: what and why
* Cloud: what and why
* Developing with AWS
	* AWS Console
	* AWS CLI
	* AWS SDK
* Configuring your IAM Credentials
* Least Privilege Principle
* Core services for web development
* Regions & Availability Zones

### Day 2 (15/07/2020): Static website hosting

* Hosting static website, [Reference](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)
* S3 (http only), Route53 Alias Records, CloudFront (https and cdn)
* Uploading new files, fetching REST API, error pages
* Services in detail, logging, redirects (www), routing policies, geo restrictions, etc

### Day 3 (22/07/2020): Deploying services

* EC2, Security Groups, SSH into instances
* AMIs and User Data shell scripts
* Cloudwatch
* Storage types: EBS, Instance, S3, RDS, 
* Feel the lack of CI/CD, explain pros and cons.
* Autoscaling groups, EBS as storage (different types of storage)

### Day 4 (29/07/2020): Deploying services part 2

The plan is to continue with Day 3 topics, if enough time introduce some of the following:

* Elastic Beanstalk and ECS
* Why use containers (docker)?
