# AWS Developing

## CLI

Unified tool to manage multiple AWS services from the command line and automate them through scripts.

Install!
```
sudo pip install aws-shell
```

Configuration
```
configure
AWS Access Key ID [None]: your-access-key-id
AWS Secret Access Key [None]: your-secret-access-key
Default region name [None]: region-to-use (e.g us-west-2, us-west-1, etc).
Default output format [None]:
```

Supports profile
```
.profile
aws-shell --profile prod
```

What we'll use:
```
s3 ls
```

## SDK

* [Node SDK](https://github.com/aws/aws-sdk-js)

```
npm install aws-sdk
```

```js
// ES6 modules
import AWS from 'aws-sdk';
import S3 from 'aws-sdk/clients/s3';

// CommonJS
const AWS = require('aws-sdk')
const S3 = require('aws-sdk/clients/s3');
```