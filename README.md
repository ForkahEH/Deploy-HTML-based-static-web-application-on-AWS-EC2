# Deploy-HTML-based-static-web-application-on-AWS-EC2

# Create HTML server


# Configure Cloudwatch agent to monitor Memory utilization of EC2 instance
1. Create IAM Role
2. Install CloudWatch Agent
3. Create the CloudWatch agent configuration file with the wizard

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

6. Under Notification, select the SNS topic you created earlier and click "Next".
![Screenshot 2023-10-05 234640](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/12f834ee-2346-4bb5-852e-103a0a49a575)

7. Under Name and Description, enter a name for your alarm, click "Next" and "Create Alarm".

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

6. Under Notification, select the SNS topic you created earlier and click "Next".

7. Under Name and Description, enter a name for your alarm, click "Next" and "Create Alarm".

8. In the CloudWatch console, click on "Alarms", then "All Alarms". Your alarms are displayed.
![Screenshot 2023-10-06 000243](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/0ee40763-5695-43dd-aa77-b7bfda74b374)

# Use AWS CLI commands to push webserver logs to S3 bucket.

1. Open the main Apache configuration file: sudo vi /etc/apache2/apache2.conf

2. Add the following lines to configure the access log: CustomLog /var/log/apache2/access.log combined
![Screenshot 2023-10-06 001221](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/1467f761-63a8-46ef-ab18-34b14dc51a50)

3. Add the following line to configure the error log: ErrorLog /var/log/apache2/error.log
![Screenshot 2023-10-06 001812](https://github.com/ForkahEH/Deploy-HTML-based-static-web-application-on-AWS-EC2/assets/127892742/7bae8c32-7864-495d-8365-917738bf24d2)

4. Restart the Apache web server to apply the changes: sudo service apache2 restart