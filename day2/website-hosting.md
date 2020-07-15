# Static Website Hosting

![have this you must](https://i1.wp.com/www.onefootinthecave.net/wp-content/uploads/2015/08/must-have.jpg)

## Overview

We'll use the following services:

* __S3__ for static web hosting.
* __Route 53__ for DNS Records and domain management.
* __Cloudfront__ for CDN.
* __Certificate Manager__ for providing TLS certificates (HTTPS).

## Setting up S3, web hosting

![s3](https://media.makeameme.org/created/aws-s3.jpg)

* Create bucket
	* Configure options -> Uncheck block off public access
* Properties -> Static web hosting -> index.html (Access denied)
* Permissions -> Bucket Policy -> Allow all reads

```
{
    "Id": "Policy1594683466910",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1594683465263",
            "Action": [
                "s3:GetObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::super-website/*",
            "Principal": "*"
        }
    ]
}
```

* 404
* `npm run build` -> upload build folder

### Continuous Deployment?

```
s3 ls
s3 ls s3://super-website
s3 cp|mv|rm|sync <local> <bucket>
s3 cp ~/Downloads/index.html s3://super-website/
```

Now if you navigate to your provided endpoint url, e.g. http://super-website.s3-website-us-east-1.amazonaws.com, you'll find your website hosted. Notice that even though s3 bucket files are accessed through https, the bucket web hosting is http only.

If we want to access our website through https, we need to set up our CDN that will work as a 'proxy' (well not really).

### Quick word about S3

* Versioning
    * Rollback
* Security concerns
    * User based (IAM)
    * Resource based (Bucket policies, Object Access Control List)
    * Encryption in flight
        * Server side (SSE-S3 & SSE-KMS)
        * SSE-C (Client side)
    * Logging and Audit (requires another bucket)
        * Amazon Athena
    * Pre-signed URLS (expiration time, default 3600 seconds) for upload/download.
    * MFA for deleting
    * CORS (important if you have for example assets in other bucket, or to limit requests)
* Performance
    * Cloudfront to distribute and cache around the world
    * Encryption limits
    * Multipart upload
* Storage classes
    * Based on access
    * Transition between classes -> Lifecycle rules (can schedule delete)
* Event Notifications -> Lambda

![s3](https://memegenerator.net/img/instances/71730919.jpg)

## Setting up Route 53, DNS

![dns](https://memegenerator.net/img/instances/72589866.jpg)

### Domain name

* Route 53 -> Domains -> Registered domains

### DNS Records

* Route 53 -> Hosted Zones

A hosted zone is a __container for records__, and records contain information about how you want to route traffic for a specific domain and it subdomains.

* Public hosted zones
* Privated hosted zones (Amazon VPC)

We can create an ALIAS record to route our domain to another domain, in this case the bucket endpoint. We'll get back to this later.

### Quick chat on DNS

For more info, check my other repo: [Ignacio Herrera, TCP/IP protocol suite](https://github.com/herrera-ignacio/tcp-ip)

## Certificate Manager: set up HTTPS

![https](https://media.makeameme.org/created/https-https.jpg)

For HTTPS to work, we need a TLS certificate.

* Certificate Manager (North Virginia) -> Request a certificate -> public -> add domain details (with and without www) -> DNS Validation

### Quick chat on how TLS (HTTPS) work

* Asymmetric encryption (public/private key)
* MAC (!= MAC address)
* Certificate of authority

For more information, check my other repo: [Ignacio Herrera, Cybersecurity Guidelines](https://github.com/herrera-ignacio/cybersecurity_guidelines)

## Cloudfront: distribute globally

![cloudfront](https://miro.medium.com/max/1302/0*ljKpV3Y_BvTXo4Kf.jpg)

* CloudFront -> Create Distribution-> Web
* Origin Domain Name -> S3 bucket endpoint
* Custom SSL Certificate -> select accordingly

It can take about 30 minutes to spin up.

### Quick chat on capabilities

* Origins -> AWS Resources, even ALB.
* Behaviors
* Error Pages/Responses
* Restrictions
* Invalidations
* Tags 
* Signed URL/Cookies (!= S3 Pre-Signed URL)

## Finish DNS Set up

* Route 53 -> Hosted zone -> Create Record Set -> point Alias records to CDN, which in turn, will point to your S3 bucket.
* Navigate to https://domain

### Quick chat on Route53

* Health Checks (HTTP/HTTPS/TCP)
    * Availability
    * Performance
* Monitoring, alarms
* DNS Failover (respond DNS queries using only health resources)
* Traffic policies -> Policy records (vs Routing Policy)
    * Failover
    * Weighted records
    * Geolocation
    * Multiple criteria (latency + weighted + health, etc)

## Code purging

On each code push, we want caches to be purged on your CDN.

![dns](https://platypusplatypus.com/wp-content/uploads/what-is-dns.png)

* [Invalidate from edge caches](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html).
* Use file versioning.

## Quick words on CI/CD

![cicd](https://miro.medium.com/max/2484/0*tpKI_meIRGMbBllI.jpg)

Doing all these manually every time we want to make a deployment seems really inefficient (a.k.a, a pain in the butt).

How do you think we could progresivelly automate this?

### Some improvements

* Installing aws-shell / aws-sdk (depends on service)
* Upload to s3 by running locally a script before pushing to repo (e.g. `package.json`)
* Make script to upload to s3 and invalidate CloudFront cache
* Create a Jenkins server that has aws-shell installed and can run script on every push to repo.
* Gitlab CI/CD & Gitlab Runners
	* [Mine'd example, boto3 Python SDK and docker](https://git.amalgama.co/mined/mined-server/-/blob/master/.gitlab-ci.yml)

![devops](https://www.metaltoad.com/sites/default/files/inline-images/22605665.jpg)
