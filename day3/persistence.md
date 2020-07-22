# Persistence

We briefly talked about Instance Storage, EBS, and EFS. For web apis you'll mostly find yourselves using S3 and RDS/ElastiCache/MongoAtlas.

[Good way to keep credentials](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

## S3

First we need to set up a S3 bucket. We've already discussed how to do this on Day 2, so I'll just skip ahead that part.

We have our `amalga-assets` created.

Also, we need `aws-sdk` installed in our project.

[Check code!](https://github.com/herrera-ignacio/code-example/tree/s3-upload)

Of course, server won't know that we pushed some new code...

We need to pull this new code from our server:

```
cd code-example
git pull
git branch -a
git checkout s3-upload
sudo npm run dev
```

You may get timed out from application and want to kill node server process...

```
sudo lsof -i :80
sudo kill -9 <PID>
```

## RDS

### Creation

Let's set up a database, we'll go with Postgres for the example.

Create Database -> Standard/Easy -> Additional configuration -> Create

#### Some important topics

* Security Groups (First things to check... TIMEOUTS!)
* Storage type (General vs IOPS), burst options, burst limit, burst vs baseline performance
* Replicas
* AuroraDB

### Code

We'll do some fast-forwarding with coding again...

[Check code!](https://github.com/herrera-ignacio/code-example/tree/database)

Following up the example, we'll use RDS to store the urls of the files we uploaded so far.

## Problems so far

### 1. Deploying changest fast

* Need to manually install dependencies every time
* Set up environment & configurations files manually

#### Solution

* AMIs
* Docker
* CI/CD

### 2. Scalability & Reliability?

* What if we have an increasing throughput?
* What if we need to rapidly turn off idle resources? -> Problem 1
* What if server crashes? (Exceptions, or maybe just EC2 terminating)

#### Solution

* Process Managers (PM2)
* Clusters (Docker-Swarm, Kubernetes)
* AutoScaling Groups
* ECS (next class)

### 3. Security

![Am I a joke to you?]

* Public available databases with default credentials and ports
* S3 resources public available for all users with urls
* SSH ports opened

#### Solution (Out of scope)

* VPC & Security Groups
* No SSH, open for immediate use and then close, only for your IP
* Mask bucket with cloudfront, and restrict access (whitelist > blacklist).
* Don't allow user input in file names
	* Conflicts
	* XSS
