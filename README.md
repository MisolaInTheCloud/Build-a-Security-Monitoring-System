# Build-a-Security-Monitoring-System
<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-monitoring)

**Author:** misolakhemmy@gmail.com  
**Email:** misolakhemmy@gmail.com

---

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

 In this project, I will demonstrate how to store secrets securely,  track who's accessing them, and get notified instantly when something suspicious happens. I'm doing this project to learn how to protect Data and build a powerful Monitor using AWS.

### Tools and concepts

Services I used were CloudTrail, which allowed me to monitor API activity across my AWS account. 

S3 for storing CloudTrail logs securely.
CloudWatch for setting alarms and managing log groups.
Secrets Manager for securely handling sensitive information, and SNS for creating topics and managing subscriptions for notifications.

Key concepts I learnt include the importance of CloudTrail trails for tracking user actions and maintaining an audit trail, the role of the S3 bucket in securely storing logs for compliance and analysis.

The functionality of CloudWatch alarms in monitoring resource performance and triggering alerts, the significance of Secrets Manager in securely managing sensitive data, and the utility of SNS topics and subscriptions for enabling real-time notifications to stakeholders about critical events.

 These services and concepts collectively enhanced my ability to manage and secure my AWS environment effectively.

### Project reflection

This project took me approximately 3 hours. 

The most challenging part was configuring the CloudTrail and ensuring that all necessary services, like SNS and CloudWatch, were integrated correctly. 

It was most rewarding to see my monitoring system in action, receiving real-time alerts, knowing that I had implemented a robust security and monitoring solution for my Secrets Manager. 

This not only boosted my confidence in managing cloud resources but also provided peace of mind regarding the security of the project

---

## Create a Secret

Secrets Manager is a secure service that helps you manage, retrieve, and rotate secrets such as API keys, passwords, and database credentials. 

You could use Secrets Manager to store sensitive information safely, ensuring that it is encrypted both at rest and in transit. This service allows you to control access through fine-grained permissions, enabling only authorised users or applications to retrieve the secrets.


To set up for my project, I created a secret called TopSecretInfo that contains Secret type, Encryption, aws/secretsmanager, Secret name and Description. 

It also contains a Ready-to-use code for programmers in multiple programming languages (like Python, Java, JavaScript, .NET). 

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

CloudTrail is a monitoring service. I set up a trail to keep track of all my activities, what activity to record, and the location (which will be my S3 Bucket) to save those logs. 

In this project, I will be using it to keep an eye on who's accessing my secret manager.

CloudTrail events include different types like Management events, Data events, Insights events, and Network activity events, which track network-related activities, like changes to your VPC configuration or traffic to a subnet. of events, each showing you a differ

### Read vs Write Activity

Read API activity involves when someone views but doesn't change anything, while Write API activity occurs when changes happen - creating, deleting, modifying resources, or even retrieving the value of a secret (which is what we want to monitor).

 For this Project, we need to monitor the Write API Activity. 

---

## Verifying CloudTrail

I retrieved the secret in two ways: First, by retrieving my secret value in my Secrets Manager. Second, using AWS CLI, which showed me my secrets in JSON Format when I ran the code "ws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region us-east-1".

To analyze my CloudTrail events, I visited my CloudTrail dashboard to check the historical events. I found out all of my account's management events from my activity through AWS CLI & through Secrets Manager. 

This tells me that CloudTrail captured all my secret access events, without having to dig through the raw log files in S3.

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

CloudWatch Logs is a service that helps you bring together your logs from different AWS services, including CloudTrail, for visibility, troubleshooting, and analysis. 

It is important for monitoring because once logs are in CloudWatch, alerts can be created based on specific patterns (such as someone accessing your secret), visualise trends, or trigger automated responses. 

CloudTrail's Event History is useful for tracking and auditing API calls made within your AWS account, providing a comprehensive log of actions taken by users, roles, or services. It captures details such as who made the request, what actions were performed, and when they occurred, making it ideal for compliance and security audits.

On the other hand, CloudWatch Logs are better for real-time monitoring and operational insights. They allow you to collect and store logs from various AWS services and applications, enabling you to analyze log data for performance metrics, set alarms for specific events, and troubleshoot issues. 

A CloudWatch metric is a quantitative measure used to monitor the performance and health of AWS resources and applications. When setting up a metric, the metric value represents the specific data point that you want to track, such as CPU utilization, memory usage, or request count. 

The default value is used when there is no data available for the specified metric during a particular time period. It provides a baseline to ensure that your monitoring remains consistent and helps avoid gaps in data visualization.

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

A CloudWatch alarm is a monitoring tool that enables you to set specific conditions to track the performance of your AWS resources.

 I set my CloudWatch alarm threshold to static, so the alarm will trigger when it is accessed more than or equal to 1 in 5 minutes. This setup allows me to monitor for any unexpected access or usage patterns, ensuring that I can respond quickly to potential issues or security concerns. 

I created an SNS topic along the way. An SNS topic is a communication channel used by SNS to facilitate the delivery of messages to multiple subscribers simultaneously. It acts as a logical access point that allows you to group and manage messages sent to various endpoints, such as email addresses, SMS numbers, or other AWS services.

My SNS topic is set up to send notifications to my email whenever specific events occur, especially the CloudWatch alarms triggers I set up.

By subscribing different endpoints to my SNS topic, I can ensure that relevant stakeholders are immediately informed about important updates, enhancing communication and response times across my applications and infrastructure.

AWS requires email confirmation because it ensures that the owner of the email address has explicitly agreed to receive messages from the SNS topic. 

This helps prevent unauthorized or unintended subscriptions, protecting users from spam and ensuring that only interested parties receive notifications. 

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

To test my monitoring system, I accessed my Top Secret info in my Secrets Manager. The results were exposed, but I didn't get notified via my email as per my Cloud Watch Alarm set up. 

I will be troubleshooting what could be the issue. 

When troubleshooting the notification issues, I checked all my setups, my CloudTrail, my Log Delivery, my Metric Filter, my Alarm Configuration and SNS Topics configuration. 

I discovered that the problem was from my Alarm Configuration, and adjustments were made. 

 I initially didn't receive an email before because of some of my setups. The key solution was updating my Cloudwatch Statistics set up from 'Average' to 'Sum, I also updated the timeframe from '5 minutes' to '1 minute' so that I can trigger the alarm even faster. 

In addition, I tested my new setup by pushing SNS Topics directly to my email, which I received. 


---

## Success!

To validate that my monitoring system works, I checked the configuration of my CloudWatch Alarms and CloudTrail SNS notifications to ensure they were set up correctly. I reviewed the thresholds and notification settings to confirm that they aligned with my monitoring objectives.

After verifying the configurations, I triggered the event by viewing my secrets in my Secret Manager, which would log entries in CloudTrail. I received immediate notifications via email for both the CloudWatch Alarm and CloudTrail events, confirming that the system was functioning as intended. 

![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

In a project extension, I updated my CloudTrail configurations because I needed to enable the SNS notification delivery setting. 

This change was essential for ensuring that I received real-time alerts about significant API activities and changes within my AWS environment. 

By enabling SNS notifications, I could promptly respond to any unauthorized access or unusual behaviour, enhancing the overall security posture of my project.



After enabling CloudTrail SNS notifications, my inbox started receiving real-time alerts after I accessed my Secret in my Secret Manager.  

Each email contained detailed information about the event, including the time, action taken, and the identity of the user or service that initiated the action.

In terms of the usefulness of these emails, I thought they were invaluable for maintaining security and compliance. The immediate notifications allowed me to quickly respond to any unauthorized or unexpected activities, enhancing my ability to monitor and audit actions within my AWS environment. 

Overall, these alerts provided a proactive approach to managing my resources and ensuring that my account remained secure.



![Image](http://learn.nextwork.org/curious_olive_fierce_frog/uploads/aws-security-monitoring_d7e8f9g0)

---

---
