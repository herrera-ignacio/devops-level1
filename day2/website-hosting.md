# Static Website Hosting

## Overview

We'll use the following services:

* __S3__ for static web hosting.
* __Route 53__ for DNS Records and domain management.
* __Cloudfront__ for CDN.
* __Certificate Manager__ for providing TLS certificates (HTTPS).

## Setting up S3, web hosting

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

## Setting up Route 53, DNS

### Domain name

* Route 53 -> Domains -> Registered domains

### DNS Records

* Route 53 -> Hosted Zones

A hosted zone is a __container for records__, and records contain information about how you want to route traffic for a specific domain and it subdomains.

* Public hosted zones
* Privated hosted zones (Amazon VPC)

We can create an ALIAS record to route our domain to another domain, in this case the bucket endpoint. We'll get back to this later.

## Certificate Manager: set up HTTPS

For HTTPS to work, we need a TLS certificate.

* Certificate Manager (North Virginia) -> Request a certificate -> public -> add domain details (with and without www) -> DNS Validation

### Note on how HTTPS work

* Asymmetric encryption (public/private key)
* Certificate of authority

## Cloudfront: distribute globally

* CloudFront -> Create Distribution-> Web
* Origin Domain Name -> S3 bucket endpoint
* Custom SSL Certificate -> select accordingly

It can take about 30 minutes to spin up.

## Finish DNS Set up

* Route 53 -> Hosted zone -> Create Record Set -> point Alias records to CDN, which in turn, will point to your S3 bucket.
* Navigate to https://domain

## Code purging

On each code push, we want caches to be purged on your CDN.

* [Invalidate from edge caches](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html).
* Use file versioning.

## Quick words on CI/CD

Doing all these manually every time we want to make a deployment seems really inefficient (a.k.a, a pain in the butt).

How do you think we could progresivelly automate this?

### Some improvements

* Installing aws-shell / aws-sdk (depends on service)
* Upload to s3 by running locally a script before pushing to repo (e.g. `package.json`)
* Make script to upload to s3 and invalidate CloudFront cache
* Create a Jenkins server that has aws-shell installed and can run script on every push to repo.
* Gitlab CI/CD & Gitlab Runners
	* [Mine'd example, boto3 Python SDK and docker](https://git.amalgama.co/mined/mined-server/-/blob/master/.gitlab-ci.yml)
