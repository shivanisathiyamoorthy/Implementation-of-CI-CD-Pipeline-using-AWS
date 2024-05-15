# Implementation-of-CI-CD-Pipeline-using-AWS
## Pre requestics
 - Aws console setup
 - Linux cognition
 - Download Putty on System
## AWS services utilized
 - EC2 Server
 - S3 bucket
 - IAM roles and user
 - Code Deploy
 - Code Pipeline
## Implementation
### Step 1 : Creating IAM Role and Users
 - Create two IAM roles. For the first role, grant EC2 access and S3 permissions. For the second role, provide permissions for CodeDeploy .
 - Now, create an IAM user, attach policies, and grant three permissions:
      - Full EC2 access
      - Full access to S3
      - Full access to CodeDeploy
 - After creating a user, proceed to the security credentials section, grant CLI access permissions, generate an access key, and download it.
 ### Step 2 : Installation of Code Deploy Agent 
  - To install the CodeDeploy agent, first launch an  EC2 instance.
  - EC2 instance launching
    - Provide a addional tag name for the EC2 instance, rather than a generic name.
    - Select an Linux machine.
    -  include http protocol in the security group.
    - In advance details , attach the IAM role.
  - Now, log in to the linux server using PuTTY and convert into Root user.
  - Provide the following seven commands for installing the CodeDeploy agent.
```
yum update
yum install ruby –y
yum install wget –y
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
chmod +x install
service codedeploy-agent status
```


