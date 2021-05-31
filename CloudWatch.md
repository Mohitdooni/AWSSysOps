# CloudWatch ( A monitoring service for AWS Resources)
  It can be used for AWS resources as well as on premises ( You need to install SSM Agent and CloudWatch Agent) 
## Major services monitored by cloud watch
 ### Compute 
  <ul>
   <li> Autoscaling Groups</li>
   <li> ELB </li>
   <li> Route53 HealthChecks </li>
  </ul>	
 
 ### Storage and Content Delivery
  <ul>
	<li>EBS Volumes</li>
	<li>Storage Gateway</li>
	<li> CloudFront</li>
 </ul>	
 
 ### Database & Analytics
   #### DynamoDB
   #### RDS Instances
   #### Elastic Cache Nodes
   #### RedShift
 ### Other Services
   #### SNS
   #### SQS Queues
   #### OpsWorks
   #### CloudWatch Logs
   #### Billing
## EC2 and CloudWatch 
 ### Host Level Metrics Consist of 
   #### CPU
   #### Network
   #### Disk 
   #### Status Check
### Note :- If there is anything related to Ec2 Host Level we need to monitor then we need to use custom Metrics 
###         Eg RAM utilization is  custom metric 

### Note :- By default CloudWatch Metrics logs are available indefinitely but you can change the retention for each LogGroup at any time.
            You can also find the logs of EC2 instances after their termination as well.

## Metric Granularity ( how frequently montioring the service)
   
    It depends upon service to service. Some of the services are having 1 min whereas some of them are having 3 mins. for Custom Metric the minimum Granularity is 1 min.
	1 minute for detailed monitoring
	5 minute for standard monitoring

##	CloudWatch Alarm
  
     Alarm is used to monitor CloudWatch metric.You can set the threshold limit to trigger the alarm.You can also set the appropriate action when alarm is triggered. For example you can set up alarm for budget to billing cost such that if the Billing cost reached to 80% of total monthly cost then a notification is sent to Stake holders.
	 
	 
# Lab
## CaseStudey:- Create a Custom Metric for Monitoring EC2 instances Memory utilization and memory use.

#### Create an Ec2 Role ( Lets call it MyCloudWatchRole) with full Access to CloudWatch policy.
#### Create an Amazon Linux Ec2 instance and attach MyCloudWatchRole cd aws-scripts-mon/. Also attach a security group which has ssh and http open and also use below script in it bootstrap
     
	 #!/bin/bash
		yum update -y
		sudo yum install -y perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https perl-Digest-SHA.x86_64
		cd /home/ec2-user/
		curl https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.2.zip -O
		unzip CloudWatchMonitoringScripts-1.2.2.zip
		rm -rf CloudWatchMonitoringScripts-1.2.2.zip
#### Go To Cloudwatch Services and Browse the metrics for Ec2 instalnces ( You will find only PerInstances )

#### Connect to ec2 instance and run below commands one by one ( After running 3rd command you will notice that a custom metrics is created for Ec2 service)
         cd aws-scripts-mon/
         /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --verify --verbose
         /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --mem-used --mem-avail		 
		 #If you want to send the data with some frequency then create a cron job and add below entry in /etc/crontab file and after around 5 mins just check the cloudwatch metrics
		 */1 * * * * root /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --mem-used --mem-avail


		 
