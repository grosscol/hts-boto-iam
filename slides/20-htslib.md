---
author: Colin Gross
title: AWS IAM Woes
---

# HTSlib S3 Plugin

> The S3 plugin allows htslib file functions to communicate with servers that use the AWS S3 protocol.

## Credentials Service
S3 [Plugin docs](https://www.htslib.org/doc/htslib-s3-plugin.html)

```sh
#!/bin/sh
instance='http://169.254.169.254'
tok_url="$instance/latest/api/token"
ttl_hdr='X-aws-ec2-metadata-token-ttl-seconds: 10'
creds_url="$instance/latest/meta-data/iam/security-credentials"
key1='aws_access_key_id = \(.AccessKeyId)\n'
key2='aws_secret_access_key = \(.SecretAccessKey)\n'
key3='aws_session_token = \(.Token)\n'
key4='expiry_time = \(.Expiration)\n'
while true; do
    token=`curl -X PUT -H "$ttl_hdr" "$tok_url"`
    tok_hdr="X-aws-ec2-metadata-token: $token"
    role=`curl -H "$tok_hdr" "$creds_url/"`
    expires='now'
```

## Service grabs EC2 Credentials

```
instance='http://169.254.169.254'
creds_url="$instance/latest/meta-data/iam/security-credentials"
```
And writes them to a file which gets re-read when credentials are expired.


## Credentials File

- Location controlled by environment variable `AWS_SHARED_CREDENTIALS_FILE`.
- Boto3 also reads this environment variable.

![Shared Credentials](assets/shared_creds.svg)


