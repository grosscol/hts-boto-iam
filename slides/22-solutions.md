---
author: Colin Gross
title: AWS IAM Woes
---

# Problem Summary

- HTSlib service writes and re-reads credentials file.
- Boto3 reads, but never re-reads, credentials file.
- When credentials expire, HTSLib re-reads new credentials file.
- Boto3 continues with expired credentials.


## A Solution
Avoid putting the credentials in a file that Boto3 will use.

- Boto3 will use instance credentials and reaquire them.  
- HTSlib can continue reading and re-reading from the file.

## Non-shared Credentials Location

- HTSlib will use a config from a third party tool, s3cmd.
- s3cfg config specified by `HTS_S3_S3CFG`

