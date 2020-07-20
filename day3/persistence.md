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

Let's set up a database, we'll go with MySQL for the example.

We'll do some fast-forwarding with coding again...

[Check code!]()

Following up the example, we'll use RDS to store the urls of the files we uploaded so far.

