# AWS-DEPLOY-REPO
This REPO uses GitHub Actions with AWS CodeDeploy to update an AWS EC2 instance. The file structure is heavily based on [this](https://aws.amazon.com/blogs/devops/integrating-with-github-actions-ci-cd-pipeline-to-deploy-a-web-app-to-amazon-ec2/) tutorial

The infrastructure follows this workflow (INSERT THE IMAGE I SENT OVER HERE)

## Function
Every time there is a commit to the ```main``` branch, the ```CODEPLOY_FOLDER```'s contents will be updated in an associated EC2 instance. 

## Setup 
ONLY FOLLOW THESE INSTRUCTIONS IF THE INFRASTRUCTURE IS NOT WORKING

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

### EC2 Instance Setup
You must make sure the EC2 instance is created using the ```AL2023-lambda-ec2-image``` Image. From the AWS console, select **EC2 &#8594; Images (dropdown) &#8594; AMI**. Select the appropriate image. Then select **Launch Instance From AMI**. You may change the instance to whatever specifications you like.

Complete the steps in [this](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-configure.html) tutorial to configure it with CodeDeploy. 




## Usage

HOW TO USE THE INFRASTRUCTURE

Configure git on your device and input the following commands consecutively into a terminal in your preferred work environment.

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

## Contributing
Organization members only
