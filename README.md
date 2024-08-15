# AWS-Project1
This project deploys a sample HTML application on EC2 instance using AWS CodeDeploy and AWS CodePipeline using GitHub as a source of code base. Also CodePipeline automatically identifies the changes committed to the GitHub repo and triggers the build.

**Step 1: Create IAM Roles**

Create two IAM Roles. One for the service EC2 and another for the service CodeDeploy.

Either during the role creation for EC2 or after the role creation we need to add permissions by searching CodeDeploy in the search bar and selecting the policy “AmazonEC2RoleforAWSCodeDeploy”.

**Step 2: Create EC2 Instance and Attach the Role “Role_EC2CodeDeploy” to the EC2 instance**

Chose an Amazon Linux 2023 AMI t2.micro EC2 instance.

Add the ports 22 and 80 as inbound rules.

Attach the role created for EC2 instance.

Paste the following script as User data:

_#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto
sudo yum install -y python-pip
sudo pip install awscli_

Launch the number of instances based on your need.

Instead of using the above script in User data we can try individual commands after launching and connecting to the EC2 instance.

**Step 3: Create an Application and Deployment Group under CodeDeploy**

Create an Application and Deployment Group under CodeDeploy.

Attach the role created for the service AWS CodeDeploy to the service “AmazonCodeDeploy” by selecting the name of the role from the “Service role” dropdown.

**Step 4: Create a Pipeline and Integrate GitHub with CodePipeline**

While creating the pipeline click on Connect to GitHub or paste the connection link if you already have the link.

Skip build stage

Select “AWS CodeDeploy” from the dropdown for “Deploy Provider”.

Review all the things and click on “Create pipeline”.

**Step 5: Access the Application**

Access the application on port 80 as already we have added the inbound rule 80 in the EC2 instance.

To do so copy the Public DNS of the EC2 instance and paste it in the browser.

Go to GitHub repo, make some minor changes in the file index.html or any other file by clicking on Edit/Pencil icon and commit the changes.

Now go back to Amazon CodePipeline and just refresh the page.

Then go back to the browser and just refresh the page.

The AmazonCodePipeline automatically identifies the code changes in the GitHub repo and triggers the build.
