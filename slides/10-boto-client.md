---
author: Colin Gross
title: AWS IAM Woes
---

# Public VCFs Feature
Provide trimmed down VCFs of BRAVO variants via pre-signed links to objects in S3 bucket.

- Prevent using links as data source for scripts.
- Enourage user to cache the data thamselves.
- Do not need to open access the bucket.
- Do not need a separate bucket.

## Client Credentials Timeout
Boto3 Client used to generate signed urls eventually (6 hours) fails with expired credentials.

```python
client = session.client(
  service_name='s3',
  config=Config(signature_version="s3v4",
  region_name=bucket_location))
```

## Expecting "Just Works"

Expect Boto3 library to grab credentials from the EC2 instance metadata.

> Applications [...] that run on the instance can then get automatic temporary security credentials from the instance metadata. You do not have to explicitly get the temporary security credentials.

## Where Boto3 Gets Credentials

1. Passing credentials as parameters in the boto.client() method
1. Passing credentials as parameters when creating a Session object
1. Environment variables
1. Shared credential file (~/.aws/credentials)
1. AWS config file (~/.aws/config)
1. Assume Role provider
1. Boto2 config file (/etc/boto.cfg and ~/.boto)
1. Instance metadata service on an Amazon EC2 instance that has an IAM role configured.
