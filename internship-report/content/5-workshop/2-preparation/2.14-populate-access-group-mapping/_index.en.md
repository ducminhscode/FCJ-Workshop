---
title : "Populate Access Group Mapping"
date :  "`r Sys.Date()`" 
weight : 14
pre: <b> 5.2.14 </b>
chapter : false
---

Access the [link](https://gitlab.com/ducminhscode/production-access-request-portal.git) on GitLab to clone the project, then run the following command in Terminal or Command Prompt/PowerShell:

```
cd production-access-request-portal/infrastructure/scripts
``` 

Run the following script:

```
python3 populate_access_group_mapping.py --environment {env} --region {region}
```

*Replace `{env}` with your development environment and `{region}` with the current region.*

### Prerequisites for running the script

For the command to run successfully and push data to AWS Secrets Manager, your machine needs:

- **Python:** Python 3 installed.
- **Boto3 library:** This is the library that allows Python to communicate with AWS. Install it with the command: `pip install boto3`.
- **AWS CLI & Credentials:** The machine must be configured with AWS access permissions. Run `aws configure` and enter:
  - **AWS Access Key ID** and **AWS Secret Access Key** (of an IAM account that has write permissions to Secrets Manager).
  - Default region name (Example: ap-southeast-1).