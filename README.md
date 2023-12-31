
# Deploy-HTML-based-static-web-application-on-AWS-EC2# Application Setup

This project was adapted from: https://devopsrealtime.com/deploy-html-based-static-web-application-on-aws-ec2/

![image](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/36532dcc-7a18-44fb-b96c-f4af40d17f4c)

# Pre-Deployment

# Create the Golden AMI with application dependencies

# Step 1: Launch EC2 instance for Golden AMI

1. In the AWS Console Home, search for and select "EC2" resource.

2. In the EC2 Console, select "Launch Instances".

3. In the "Launch Instance" page, enter the name of the webserver: HTML Server.

   Instance type: t2.micro
   
   Key pair: CICD

   Select Edit in Network Settings
   
   select create security group

   Security group name: HTML SG

   Rules: SSH, port 22 and HTTP, port 80

   SSH source: My IP, Custm TCP source: Everywhere IPv4

   NB: The source, "My IP" is selected for added security.
   
5. Click "Launch Instance"

6. Click "View Instances"

![Screenshot 2023-10-05 191914](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/f24e0c08-1108-44b3-acd7-15805b217fe8)

7. Refresh until status check for the instance shows "2/2 checks passed".

8. In this project, we'll connect to the instance using SSH.

![Screenshot 2023-10-05 191940](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/736ce934-590b-4345-8789-9d24dd01fd4d)

# Step 2: Install Application dependecies

1. Install AWSCLI using the installation instructions from the AWS Documentation: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

![Screenshot 2023-10-05 192256](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/334183c2-dae7-48ee-a986-6f0dbd60dd6f)

2. Install and start Apache Web Server using the following commands:

   sudo apt update

   sudo apt install apache2

   sudo systemctl start apache2

   sudo systemctl status apache2

   ![Screenshot 2023-10-05 192720](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/881fffe0-dea4-448a-b22d-63425ceb8dcf)

# Step 3: Configure Apache to start automatically after the instance reboot 

1. Enable Apache to start on reboot: sudo systemctl enable apache2

![Screenshot 2023-10-05 192525](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/4dc65a59-a86f-4146-88bf-5f1210b968e8)

# Step 4: Verify Installation of Git

Check if git is already installed: git --version

![Screenshot 2023-10-05 192602](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/9e28c2ec-fa16-4099-879b-d4c9636dc990)

# Step 5: Create Golden AMI

1. Stop and select the HTML server, Click on the actions tab, select "Images and template" from the drop down menu, select "create image" from the drop down menu.

![Screenshot 2023-10-06 093120](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/323a2947-ff84-492a-b38f-5ee6625ab059)


2. In the Create image page, enter the name of the image and enable reboot and click "Create image".

3. In the EC2 Console, select AMIs under Images

    ![Screenshot 2023-10-05 192955](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/37049454-fe28-46be-a5e0-29d3c19df50e)

5. Refresh until the AMI is available
 
   ![Screenshot 2023-10-06 093909](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/1470877a-349b-4d29-9da1-3ce8bd546b46)

# Deployment

# Infrastructure Setup

# Step 1: Create Security Group allowing port 22 from custom IP source and port 80 from public
 
  The HTML SG created earlier is used
 
  ![Screenshot 2023-10-06 094302](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/8b473761-d71e-4afd-acb7-689ca835ec71)

# Step 2: Create Key-Pair and download the private key
 
  The CICD key pair is used

# Step 3: Create t2.micro type EC2 instance using Golden AMI

1. In the AWS Console Home, search for and select "EC2" resource.

2. In the EC2 Console, select "Launch Instances".

3. In the "Launch Instance" page, enter the name of the webserver: Static Server.

   Application and OS Images (Amazon Machine Image): Select My AMIs and select the HTML AMI.
 
  ![Screenshot 2023-10-06 100036](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/8488dbde-7fcb-4c44-b65c-4081d4f022ed)

   Instance type: t2.micro
   
   Key pair: CICD

   In the Network settings section, select the existing HTML SG security group.

4.  Click "Launch Instance"

5.   Click "View Instances"

![Screenshot 2023-10-06 095831](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/06b17891-0438-4bd0-9016-879d7fb47fdc)

# Step 4: Create Elastic IP and associate the IP to EC2 instance

1.  In the EC2 Console, select "Elastic IPs".

2.  Click "Allocate Elastic IP"

![Screenshot 2023-10-06 100813](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/b5d18f33-b5d5-480c-a36e-72508e7f85a5)

3.  Under Allocate Elastic IP address, leave the defaults and click Allocate.

![Screenshot 2023-10-06 101437](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/d9134897-2679-481f-9591-e37e53553ffe)

4.  Click on the actions tab, select "Associate Elastic IP Address" from the drop down menu, select "create image" from the drop down menu.

![Screenshot 2023-10-06 101542](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/9261e9a0-1f93-49eb-b498-b51e099a712d)

5.  In the "Associate Elastic IP address" page, under Resource type, select Instance, enter the id of the instance and click Associate.

![Screenshot 2023-10-06 110330](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/87c2b7f7-662e-465b-8c48-4382a01a8185)

# Step 5: Create GP2 type EBS volume named /dev/xvdb of size 5GB in same AZ where EC2 was created

1.  In the EC2 Console, select "Volumes" under Elastic Block Store and click "Create volume".

![image](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/8aee00e7-4f8a-4b36-8368-c78178b1c57b)

2. Choose the AZ of the EC2 instance, select Encryption and click Create volume.

3. Select the created volume and enter the name of the volume.

![Screenshot 2023-10-06 111109](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/158c6f9c-39f0-416c-8298-277c5841fa12)

# Step 6: Attach EBS volume to EC2 instance

1. Click on the actions tab, select "Attach volume".

2. Select the ec2 instance from the instance text box.

3. Choose "Attach".

# Step 7: Create Route53 hosted zone with your domain name and configure A record pointing to the EC2 EIP

1. In AWS Management Console, navigate to Route 53.

2. In the Route 53 console, click on "Register a Domain".

![Screenshot 2023-10-06 013315](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/b9a1f52f-1b9a-4521-9952-a3fb0947bb99)

3. Under Register domains, enter a name and check its availability.

4. Select the domain name you want to register and click "Proceed to register".

![Screenshot 2023-10-06 014252](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/65f4272b-3e42-4ed8-999f-ff40a16a693d)

5.  Fill in your contact infomation, click "Next" and submit.

6.  Refresh till the registration is successful.

![Screenshot 2023-10-06 020834](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/32998a2e-57e5-4474-b288-21e36d21b03c)

8. Click Hosted zone and select your domain name.

![Screenshot 2023-10-06 021016](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/4d7eb3a3-7691-451b-afb1-7b85ce2025d7)

9. Click Create record. In the Quick create record page, select Record type A and click "Create Records".

![Screenshot 2023-10-06 021543](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/59ce272d-0876-4ae4-b624-c6d90354cb91)

# Application Setup

# Step 1: Create File System on xvdb volume and mount it on /var/www/html directory

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html

1. Run the following command to view your available disk devices to help you determine the correct device name to use:  lsblk

2. Run the following command to get information about the EBS volume: sudo file -s /dev/xvdf

![Screenshot 2023-10-06 015319](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/ff301dfc-e2bd-469d-9796-e56ef62d5dd3)

3. Run the following command to create a file system on the volume: sudo mkfs -t xfs /dev/xvdf

4. Run the following command to mount the EBS Volume: sudo mount /dev/xvdf /var/www/html

5. Verify that the EBS volume is mounted properly using the following command: df -h

![Screenshot 2023-10-06 015648](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/f74015d2-5d7e-4891-8684-85a74b6a844d)

# Step 2: Use Git commands and clone the source code from Bit Bucket repository provided in the pre-requisites

1. Git clone the source code: git clone https://bitbucket.org/dptrealtime/html-web-app.git

![Screenshot 2023-10-06 020015](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/c661aac4-a8aa-4ad4-b502-32476b6213e1)

# Step 3: Deploy the source code into web server document root folder – /var/www/html

1. Change directory into the cloned directory

![Screenshot 2023-10-06 020213](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/64adeb75-e080-4f5e-b02f-f507e965a8d1)

2. Copy the required files and restart the service

sudo cp -r css/ images/ index.html js error.htm ok.htm header.html /var/www/html

sudo systemctl restart apache2

![Screenshot 2023-10-06 020228](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/505a5464-7a2e-4c82-a256-77bf2c0f492b)

4. Enter the public IP of the EC2 instance in the browser to verify deployment
 
  ![Screenshot 2023-10-06 022357](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/296a6ce9-4a6e-42a9-8484-bbe0e33d7d5b)

# Post-Deployment

# Configure Cloudwatch agent to monitor Memory utilization of EC2 instance

# Step 1: Create IAM Role

https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent-commandline.html

1.  In the AWS Management Console, navigate to the IAM console.

2.  Select Roles and then Create role.

![Screenshot 2023-10-06 113457](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/5833846c-f312-4933-88e5-fdf1cc25b72b)

3.  Select AWS service under Trusted entity type. For Use case, choose EC2 under Common use cases and click Next.

![Screenshot 2023-10-06 113600](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/be8ad601-96d5-4abb-9bfb-df6e84701549)

4.  In the list of policies, select the CloudWatchAgentServerPolicy and click Next.

5.  Enter a name for the role and click Create role.

# Step 2: Install CloudWatch Agent

https://www.cloudthat.com/resources/blog/steps-to-monitor-memory-utilization-of-ec2-instance-with-cloudwatch

https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html

1. First create access keys and run: aws configure

![Screenshot 2023-10-06 023029](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/7036b3e9-5279-4f2d-9bcc-285c475d3039)

2. Download the CloudWatch agent: wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

![Screenshot 2023-10-06 023346](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/a3a5d6b5-86eb-4742-827b-ed52a21a473c)

3. Change to the directory containing the package and run the following:

   sudo dpkg -i -E ./amazon-cloudwatch-agent.deb

   sudo apt-get install -f

![Screenshot 2023-10-06 023731](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/3c955836-faf2-4a43-9fe0-07b2ac48dc81)

5.  Start the CloudWatch Agent: sudo systemctl start amazon-cloudwatch-agent
   
# Step 3: Create the CloudWatch agent configuration file with the wizard

1.  Start the CloudWatch Agent Configuration Wizard: sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

 ![Screenshot 2023-10-06 024444](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/67ca25c2-6478-4d24-bfa1-a917a00e48d7)

2. Follow the prompts to create the CloudWatch agent configuration file with the wizard

![Screenshot 2023-10-06 024715](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/015fa7e9-474c-4de4-8cc7-1987543b8991)

3. Restart and enable Cloudwatch:

   sudo systemctl start amazon-cloudwatch-agent

   sudo systemctl enable amazon-cloudwatch-agent
   
# Create Cloudwatch Dashboard to monitor CPU & Memory metrics of the EC2 instance

1. In AWS Management Console, navigate to CloudWatch.

2. In the CloudWatch console, click on "Dashboards" in the left sidebar.

![Screenshot 2023-10-05 222212](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/aed04b9d-e2b2-4a37-946c-8b367f02631b)

3. Click on the "Create dashboard" button.

![Screenshot 2023-10-05 222443](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/71458dc8-1f2e-4cbf-8f6f-5372d6187026)

4. Enter Dashboard name and press "Enter".

![Screenshot 2023-10-05 222710](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/7a9654a7-2c25-4c48-ab02-e39d88fbf055)

5. In the "Add Widgets" page, select "Line", "Metrics" and click "Next".

![Screenshot 2023-10-05 222831](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/eb5d6407-78c2-43fd-a9e9-ebb0b2264262)

6. Enter "CPU" in the search bar, choose "Per-Instance Metrics", select "Static Server - Cpu Utilization" and click "Create Widget".

![Screenshot 2023-10-05 223554](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/bda6ca1e-ec25-4282-95d3-62f395d58695)

7. Select "static_html" and click "Create Dashboard".

![Screenshot 2023-10-05 223916](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/24124bf6-cbec-4951-bd0f-9800a4694d2c)

8. Click the plus sign to add another widget.

   ![Screenshot 2023-10-05 224430](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/ededceb7-24dd-4628-a123-54ee186ca8b9)

10. In the "Add Widgets" page, select "Line", "Metrics" and click "Next".

11. Click "CWAgent", "InstanceId" and select "Static Server" and click "Create Widget".

![Screenshot 2023-10-05 224945](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/37fa7d11-3f1b-4c87-a57a-5b6cad9fe88b)

12. The graphs for  CPU and Memory are displayed on the page. Click "Save".

![Screenshot 2023-10-05 225137](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/92d6aa49-f59c-4356-98b9-69980d36fa6c)

# Configure SNS topic and subscribe e-mail to the Topic

1. In AWS Management Console, navigate to SNS.

2. In the SNS console, click "Topic", "Create Topic".

![Screenshot 2023-10-05 225553](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/480eb398-bb94-46d6-b40e-9949b9cbc578)

3. Select "Standard", enter the name of the topic and click "Create Topic".

![Screenshot 2023-10-05 230815](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/0a41cd69-4843-4459-9fe2-0910c43074f4)

4. In the topic details page, click on "Create subscription".

![Screenshot 2023-10-05 231022](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/0b9709ab-a5d3-4055-a90c-bd28c791373b)

5. Under Protocol, select Email, under Endpoint, enter the email address the notification should be sent to and click "Create Subscription".

![Screenshot 2023-10-05 231412](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/68966043-bbbc-460a-8ed3-71d4162bbccc)

6.  Confirm the subscription by clicking a confirmation link sent to the email you entered.

![Screenshot 2023-10-05 231751](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/47d30f46-60e2-43ac-9a71-d050d834648f)

7.  Voila!!! You have successfully subscribed.

![Screenshot 2023-10-05 232153](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/3337be34-cc0e-4bd0-9a9c-10e3f6af687c)

![Screenshot 2023-10-05 232737](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/35f19cb3-9879-4b75-8dd4-91e8c6b491f5)

# Configure Cloudwatch alarm with 1 data point and 5 minutes interval rate to notify to SNS topic when average CPU utilization is greater than 80% threshold.

1. In AWS Management Console, navigate to CloudWatch.

2. In the CloudWatch console, click on "Alarms", then "In Alarm" in the left sidebar and click "Create Alarm".

![Screenshot 2023-10-05 233124](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/f669033c-9872-4b8a-b1d1-313b3ace425d)

3.Select "Select metric".

4. In the "Select Metric" page, enter "CPU" in the search bar, choose "Per-Instance Metrics", select "Static Server - Cpu Utilization" and click "Select metric".

![Screenshot 2023-10-05 222831](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/eb5d6407-78c2-43fd-a9e9-ebb0b2264262)

5. Under Metric, enter "5 minutes". Under Conditions, enter the following and click "Next".

   Threshold type: Static

   Whenever CPUUtilization is... : Greater than - 80

![Screenshot 2023-10-05 234316](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/ea7eb6e2-e695-43a1-9a4d-cc00d6be0afb)

7. Under Notification, select the SNS topic you created earlier and click "Next".

![Screenshot 2023-10-05 234640](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/12f834ee-2346-4bb5-852e-103a0a49a575)

8. Under Name and Description, enter a name for your alarm, click "Next" and "Create Alarm".

#  Configure Cloudwatch alarm with 1 data point and 5 minutes interval rate to notify to SNS topic when average Memory utilization is greater than 60% threshold. 

1. In AWS Management Console, navigate to CloudWatch.

2. In the CloudWatch console, click on "Alarms", then "In Alarm" in the left sidebar and click "Create Alarm".

![Screenshot 2023-10-05 233124](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/f669033c-9872-4b8a-b1d1-313b3ace425d)

3.Select "Select metric".

4. In the "Select Metric" page,  click "CWAgent", "InstanceId" and select "Static Server" and click "Select metric".

![Screenshot 2023-10-05 235451](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/8ff4637f-7304-49f5-a855-0fb3da92faf4)

5. Under Metric, enter "5 minutes". Under Conditions, enter the following and click "Next".

   Threshold type: Static

   Whenever mem_used_percent is... : Greater than - 60

![Screenshot 2023-10-05 235753](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/e0333f19-f227-49df-ae5f-d7b204df2a07)

7. Under Notification, select the SNS topic you created earlier and click "Next".

8. Under Name and Description, enter a name for your alarm, click "Next" and "Create Alarm".

9. In the CloudWatch console, click on "Alarms", then "All Alarms". Your alarms are displayed.

![Screenshot 2023-10-06 000243](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/0ee40763-5695-43dd-aa77-b7bfda74b374)

# Use AWS CLI commands to push webserver logs to S3 bucket.

1. Open the main Apache configuration file: sudo vi /etc/apache2/apache2.conf

2. Add the following lines to configure the access log: CustomLog /var/log/apache2/access.log combined

![Screenshot 2023-10-06 001221](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/1467f761-63a8-46ef-ab18-34b14dc51a50)

3. Add the following line to configure the error log: ErrorLog /var/log/apache2/error.log

![Screenshot 2023-10-06 001812](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/7bae8c32-7864-495d-8365-917738bf24d2)

4. Restart the Apache web server to apply the changes: sudo service apache2 restart

5. Run the following commands:

aws s3 cp /var/log/apache2/access.log s3://htmlstaticbucket/logs/logfile.log

aws s3 cp /var/log/apache2/error.log s3://htmlstaticbucket/logs/errorfile.log

![Screenshot 2023-10-06 004112](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/2833f3c5-fa72-47f0-bae8-c1cd806b60c8)

6. In AWS Management Console, navigate to S3.

7. In the S3 console, select Buckets > htmlstaticbucket > logs. The files would be displayed.

![Screenshot 2023-10-06 004240](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/4bb62d5f-7991-4633-b184-a4e39032cce8)

8. To automate log uploading, a cron job is used.

   Run this command: crontab -e

   Enter the following lines in the crontab file:

   0 * * * * aws s3 cp /var/log/apache2/access.log s3://htmlstaticbucket/logs/logfile-$(date +\%Y\%m\%d\%H).log

   0 * * * * aws s3 cp /var/log/apache2/error.log s3://htmlstaticbucket/logs/errorfile-$(date +\%Y\%m\%d\%H).log

    ![Screenshot 2023-10-06 005205](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/512332d7-8d46-4ec6-89ce-80784f80b974)

# Configure S3 life cycle rules to transit previous version objects to Glacier after 30 days and delete the objects after 90 days of object creation date

1. In the htmlstaticbucket page, click on "Management" and click "Create lifecycle rule".

![Screenshot 2023-10-06 010026](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/549d9753-5b89-4a6e-9937-b9c00a724988)

2. Enter a  name for your rule, select "Apply to all objects in the bucket".

3. Under Lifecycle rule actions, select "Move noncurrent versions of objects between storage classes" and "Permanently delete noncurrent versions of objects".

![Screenshot 2023-10-06 011512](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/6c545062-7df7-4dba-81c9-9ea73e321a80)

4. Under Transition current versions of objects between storage classes, select "Glacier Flexible Retrieval" and enter 30 days.

![Screenshot 2023-10-06 011631](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/44149089-399c-4117-aeb4-5a237de0d805)

5. Under Permanently delete noncurrent versions of objects, select 90 days.

![Screenshot 2023-10-06 011702](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/2758a78e-89b9-4afb-9fde-316904b15591)

6. Click "Create".

![Screenshot 2023-10-06 012003](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/25131eb2-650a-4e09-9010-48bb765549cd)


Evidence of cronjob automation

![Screenshot 2023-10-06 012156](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/56e1fd00-faf6-4ba7-97ed-b6e49356e8d4)

# Validation

Verify if you are able to access the web application from internet browser. 

![Screenshot 2023-10-06 022208](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/abf8834f-c6f4-446d-b2a5-0ced06b4fc86)


