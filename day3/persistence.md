# Persistence

![database](https://memegenerator.net/img/instances/36021260.jpg)

We briefly talked about Instance Storage, EBS, and EFS. For web apis you'll mostly find yourselves using S3 and RDS/ElastiCache/MongoAtlas.

For the sake of the example, we won't focus too much on best practices about credentials/security, but please refer to the following documentation if you want to know more:

[Good way to keep credentials](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

## S3

![not db](https://www.memecreator.org/static/images/memes/5039942.jpg)

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

![sqli](https://meme-generator.com/wp-content/uploads/mememe/2020/01/mememe_0aecf43e80c8d3b64ed40d3741762c87-1.jpg)

### Creation

Let's set up a database, we'll go with Postgres for the example.

Create Database -> Standard/Easy -> Additional configuration -> Create

#### Some important topics

![database](https://i.imgur.com/Nm2eU3C.png)

* Security Groups (First things to check... TIMEOUTS!)
* Storage type (General vs IOPS), burst options, burst limit, burst vs baseline performance
* Replicas
* AuroraDB

### Code

![contact sysadmin](https://scontent.faep9-2.fna.fbcdn.net/v/t1.0-9/54417682_2348708708719289_1272972102592364544_o.jpg?_nc_cat=111&_nc_sid=730e14&_nc_ohc=F5cKja99CJUAX8VxjOv&_nc_ht=scontent.faep9-2.fna&oh=ba13df7f63f84b6d32a7723c8aefc797&oe=5F3F1432)

We'll do some fast-forwarding with coding again...

[Check code!](https://github.com/herrera-ignacio/code-example/tree/database)

Following up the example, we'll use RDS to store the urls of the files we uploaded so far.

