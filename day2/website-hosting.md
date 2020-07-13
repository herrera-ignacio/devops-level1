# Static Website Hosting

## Overview

We'll use the following services:

* __S3__ for static web hosting.
* __Route 53__ for DNS Records and domain management.
* __Cloudfront__ for CDN.
* __Certificate Manager__ for providing TLS certificates (HTTPS).

## Setting up S3

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