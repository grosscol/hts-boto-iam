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
Avoid putting the credentials for HTSlib in a file that Boto3 will use.

- Boto3 will use instance credentials and reaquire when they expire.  
- HTSlib can continue reading and re-reading from a credentials file.

## Non-shared Credentials Location

- HTSlib will use a config from a third party tool, s3cmd.
- s3cfg config specified by `HTS_S3_S3CFG`

## Modify Environment of API Process

Template of BRAVO API systemd service file
```python
{% if is_aws_environment %}
Environment="HTS_S3_S3CFG={{inst_dir}}/s3cfg"
{% endif %}
```

## Modify Where HTSLib Service Writes

```sh
curl -H "$tok_hdr: $token" "$creds_url/$role" |\
	jq --raw-output "${JQ_SCRIPT}" >> "${HTS_S3_S3CFG}.new"

```

## Result

![Non-Shared Credentials](assets/nonshared_creds.svg)


