# Implementation-of-CI-CD-Pipeline-using-AWS
## Process 
- Creating role 
- Iam user creation
- Code deploy Agent installation
- Development of code
- Pusihing code to s3
- Creating apllication in code deploy
- Selecting where / which apllication is to be deployed
-  Checking status of Pipeline
  ![implementation](https://github.com/shivanisathiyamoorthy/Implementation-of-CI-CD-Pipeline-using-AWS/assets/140683043/bcf7f1cc-977d-461a-8caf-9bfc13eb942f)

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
    - Provide a addional tag name as (appname , Sampleapp) for the EC2 instance, rather than a generic name.
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
### Step 3 : Code Development
 - Launch another  linux EC2 server for code development and name it as developer machine .
 - Login into the developer  machine as an IAM user using Access key which is downloaded previously .
 - Then make working directory and name it .
 - And create another working directory as sample app and insert the code inside "index.html " file using vi editor.
```vi index.html
```
- Now create  appspec.yml file and insert the following command .
```
version: 0.0
os: linux
files:
 - source: /index.html
   destination: /var/www/html/
hooks:
 BeforeInstall:
  - location: scripts/httpd_install.sh
    timeout: 300
    runas: root
  - location: scripts/httpd_start.sh
    timeout: 300
    runas: root
 ApplicationStop:
  - location: scripts/httpd_stop.sh
    timeout: 300
    runas: root
```
- Then make another directory as scripts and insert the following commands :
- For installing Apache server in developer machine and To insert the commande use vi editor .
- Create httpd_installing.sh file  and insert the command for installing Apache server.

```
#!/bin/bash
yum install -y httpd
```

- Now create httpd_start.sh file and insert the command:

```
#!/bin/bash
systemctl start httpd
systemctl enable httpd
```

- Finally,create httpd_stop.sh file and insert the command:
  
```
#!/bin/bash
systemctl stop httpd
systemctl disable httpd
```
- After inserting the commands, exit the directory.

### Step 4 : Pushing To S3 bucket :
- Go into s3 console and create s3 bucket with ACLS enabled .
- Now create apllication named as sampleapp in Code Deploy apllications.
- Then Push the code(sample app) from Linux terminal(application) to the S3 bucket using the following command :
```
 # aws deploy push --application-name sampleapp --s3-location s3://gir-sampleapp/sampleapp.zip
```
### Step 5 : Deployment Process
- Go to CodeDeploy, navigate to the application, and create a deployment group .
- Give a name and then select the service role which was created before in IAM with CodeDeploy permissions.
- Then select in-place deployment and disable Load balancer .
- Next, select Amazon EC2 instances in the environment configuration and provide the key and tag values(appname , Sampleapp)  of the previously created instance for the first EC2 server .
- After this , Create deployment and then select the deployment group .
- Choose the revision location, which is the S3 bucket, and create the deployment.
- Check for a successful status.
- To run the code, copy the public ip address of ec2 Server 1 and paste it into the browser URL.


### Step 6 : 
 


