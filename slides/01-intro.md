---
author: Colin Gross
title: AWS IAM Woes
date: 2025-10-02
---

# AWS IAM Woes
A small issue with IAM when using boto and pysam(htslib)

- AWS IAM
- Python Boto3
- HTSlib


## HTSLib & Boto 
- HTSLib is the underlying C library for working with VCFs and Tabix indexed files.
  - Pysam includes HTSlib as a dependency.
- Boto3 is the Amazon provided python library for using AWS APIs.

## Relevant Stack

![BRAVO Dependencies](assets/bravo-data-deps.svg){ width=250px }

