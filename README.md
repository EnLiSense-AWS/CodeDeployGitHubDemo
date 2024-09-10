# AWS-DEPLOY-REPO
This REPO uses GitHub Actions with AWS CodeDeploy to update an AWS EC2 instance. The file structure is heavily based on [this](https://aws.amazon.com/blogs/devops/integrating-with-github-actions-ci-cd-pipeline-to-deploy-a-web-app-to-amazon-ec2/) tutorial.

The infrastructure follows this workflow (INSERT THE IMAGE I SENT OVER HERE)

## Function
Every time there is a commit to the ```main``` branch, the ```CODEPLOY_FOLDER```'s contents will be updated in an associated EC2 instance. 

## Setup 
FOLLOW THESE INSTRUCTIONS IF INITIAL SETUP OR CHANGING EXISTING SETUP
### CodeDeploy Setup

Follow [these](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-create.html) instructions to create a deployment.

### Linking to GitHub

Follow [these](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-create-cli-github.html) instructions to connect the CodeDeploy Application created in the previous step to a GitHub repository.

The ```deploy.yml``` file in the ```workflows``` folder uses Github Secrets to link to AWS. 
```yaml
aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY }}
aws-secret-access-key: ${{ secrets.AWS_SECRET_KEY }}
token: ${{ secrets.CODEDEPLOY_TOKEN }} #Make sure CodeDeploy is linked to this repository
```
Update the above values using [this](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) tutorial to make sure it reflects an active user account's information.

Change the ```appspec.yml``` file to reflect the specifications of your EC2 instance using [this](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html) template.

### New EC2 Instance Setup
You must make sure the EC2 instance is created using the ```AL2023-lambda-ec2-image``` Image. From the AWS console, select **EC2 &#8594; Images (dropdown) &#8594; AMI**. Select the appropriate image. Then select **Launch Instance From AMI**. You may change the instance specifications however you like.

Next, follow from Step 3 of the tutorial below.

### Existing EC2 Instance Setup
It is recommended that you use the the ```AL2023-lambda-ec2-image``` Image.
For an existing Instance, follow the steps below:

1. In the **EC2** console, select the instance, select **Actions** &#8594; **Security** &#8594; **Modify IAM Role**. Attach the ```CodeDeployDemo-EC2-Instance-Profile``` IAM role. Click **Update IAM Role**
2. Install System Manager if not installed already. [Tutorial for AL2023](https://docs.aws.amazon.com/systems-manager/latest/userguide/agent-install-al2.html)
3. Install CodeDeploy using [this](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-linux.html) tutorial. Configure the instance with [this](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-configure.html)  tutorial
4. Connect to the instance. Follow [these](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) instructions.
5. Next, make sure the proper python packages are installed. Copy the items in the blurb below to your clipboard: 
```bash
attrs==20.3.0
aws-cfn-bootstrap==2.0
awscli==2.15.30
awscrt==0.19.19
Babel==2.9.1
catboost==1.2.5
cffi==1.14.5
chardet==4.0.0
chevron==0.13.1
cloud-init==22.2.2
colorama==0.4.4
configobj==5.0.6
contourpy==1.2.1
cryptography==36.0.1
cycler==0.12.1
dbus-python==1.2.18
distro==1.5.0
docutils==0.16
ec2-hibinit-agent==1.0.8
fonttools==4.53.1
gpg==1.15.1
graphviz==0.20.3
idna==2.10
importlib_resources==6.4.0
Jinja2==2.11.3
jmespath==0.10.0
jsonpatch==1.21
jsonpointer==2.0
jsonschema==3.2.0
kiwisolver==1.4.5
libcomps==0.1.20
lockfile==0.12.2
MarkupSafe==1.1.1
matplotlib==3.9.0
netifaces==0.10.6
numpy==1.23.0
oauthlib==3.0.2
packaging==24.1
pandas==2.2.2
pickle4==0.0.1
pillow==10.4.0
plotly==5.23.0
ply==3.11
prettytable==0.7.2
prompt-toolkit==3.0.24
pycparser==2.20
pyparsing==3.1.2
pyrsistent==0.17.3
pyserial==3.4
PySocks==1.7.1
python-daemon==2.3.0
python-dateutil==2.9.0.post0
pytz==2022.7.1
PyYAML==5.4.1
release-notification==1.2
requests==2.25.1
rpm==4.16.1.3
ruamel.yaml==0.16.6
ruamel.yaml.clib==0.1.2
scipy==1.13.1
selinux==3.4
sepolicy==3.4
setools==4.4.1
six==1.15.0
support-info==1.0
systemd-python==235
tenacity==9.0.0
tzdata==2024.1
urllib3==1.25.10
wcwidth==0.2.5
zipp==3.19.2
```
Now, in the instance, type/click the following commands/key binds consecutively:
```bash
sudo yum update -y && su ec2-user
```
```bash
curl -O https://bootstrap.pypa.io/get-pip.py
```
```bash
python3 get-pip.py --user
```
```bash
cd
```
```bash
vim requirements.txt
```
**Ctrl+Shift+V** 

**Shift+Z**

**Shift+Z**

```bash
pip install -r requirements.txt
```





## Usage

HOW TO USE THE SETUP

Install/configure ```git``` on your device and input the following commands consecutively into a terminal in your preferred work environment.

```bash
git clone https://github.com/EnLiSense-AWS/AWS-DEPLOY-REPO.git
```
```bash
cd AWS-DEPLOY-REPO
```
```bash
git remote add "origin" git@github.com:EnLiSense-AWS/AWS-DEPLOY-REPO.git
```
```bash
git remote set-url "origin" git@github.com:EnLiSense-AWS/AWS-DEPLOY-REPO.git
```
After this, make any changes to the repo locally, and proceed with the following commands.
```bash
git add .
```
```bash
git commit -m "put whatever message you want in here"
```
```bash
git push -u origin main
```

Now you have pushed the changes to the main branch of the ```AWS-DEPLOY-REPO``` repository and EC2 instance will be updated.

The above steps can be performed through GitHub Desktop by following [these](https://docs.github.com/en/desktop/making-changes-in-a-branch/committing-and-reviewing-changes-to-your-project-in-github-desktop) instructions.

The usage steps can be performed through GitHub Online by navigating into a file, pressing the pencil icon in the top right, and committing any changes. 

## Status Check

You may check the status of the deployment through the **Actions** tab in the GitHub repository. 

Click on the latest commit. 

Click on the only item under the **Jobs** sidebar. 

Click on the ```Run webfactory/create-aws-codedeploy-deployment@v0.2.2``` dropdown

The last line should read: ```🥳 Deployment successful```

ADD IMAGE HERE

## Caution and Warnings
If the pipeline is working properly, only add to the ```CODEPLOY_FOLDER/CRP_model_dir```directory in the GitHub. DO NOT MODIFY ANY OF THE **YAML** files for a simple usage case.

If you are changing the EC2 instance in any manner, make sure to reflect that change in the Instance ID in the ```lambda-ec2-function``` Lambda function's **instanceID** variable. Make sure the CodeDeploy application is also properly linked to the correct instance by performing a dummy deploy

This tutorial and its links are configured to work only with a Linux server (CentOS distro) in the ```us-east-1``` server.



## Contributing
Organization members only.
