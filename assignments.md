To launch an instance

Open the Amazon EC2 console. 

From the console dashboard, choose Launch Instance.

The Choose an Amazon Machine Image (AMI) page displays a list of basic configurations, called Amazon Machine Images (AMIs), that serve as templates for your instance.

On the Choose an Instance Type page, you can select the hardware configuration of your instance.

Choose Review and Launch to let the wizard complete the other configuration settings for you.

On the Review Instance Launch page, under Security Groups, you'll see that the wizard created and selected a security group for you. You can use this security group, or alternatively you can select the security group that you created when getting set up using the following steps:

Choose Edit security groups.

On the Configure Security Group page, ensure that Select an existing security group is selected.

Select your security group from the list of existing security groups, and then choose Review and Launch.

On the Review Instance Launch page, choose Launch.

When prompted for a key pair, select Choose an existing key pair, then select the key pair that you created when getting set up.


To create an AMI from an instance using the console


In the navigation pane, choose Instances, select your instance, and then choose Actions, Image, Create Image.

Tip

If this option is disabled, your instance isn't an Amazon EBS-backed instance.

In the Create Image dialog box, specify the following information, and then choose Create Image.

Image name – A unique name for the image.

Image description – An optional description of the image, up to 255 characters.

No reboot – This option is not selected by default. Amazon EC2 shuts down the instance, takes snapshots of any attached volumes, creates and registers the AMI, and then reboots the instance. Select No reboot to avoid having your instance shut down.

Warning

If you select No reboot, we can't guarantee the file system integrity of the created image.

Instance Volumes – The fields in this section enable you to modify the root volume, and add additional Amazon EBS and instance store volumes. For information about each field, pause on the i icon next to each field to display field tooltips. Some important points are listed below.

If you select Delete on Termination, when you terminate the instance created from this AMI, the EBS volume is deleted. If you clear Delete on Termination, when you terminate the instance, the EBS volume is not deleted.


Delete on Termination determines if the EBS volume is deleted or not; it does not affect the instance or the AMI.

To view the status of your AMI while it is being created, in the navigation pane, choose AMIs. Initially, the status is pending but should change to available after a few minutes.

Launch an instance from your new AMI.


Now we can scale up our system as per our requirment. 


We can use Auto Scalling for same. 

Below are the steps for create a Launch configuration and Auto Scalling. 

From the dashboard, choose Launch Instance.

Choose an AMI, then choose an instance type on the next page, and then choose Next: Configure Instance Details.

In Number of instances, enter the number of instances that you want to launch, and then choose Launch into Auto Scaling Group. You do not need to enter any other configuration details on the page.

On the confirmation page, choose Create Launch Configuration.

You are switched to step 3 of the launch configuration wizard. The AMI and instance type are already selected based on the selection you made in the Amazon EC2 launch wizard. Enter a name for the launch configuration, configure any other settings as required, and then choose Next: Add Storage.

Configure any additional volumes, and then choose Next: Configure Security Group.

Create a new security group, or choose an existing group, and then choose Review.

Review the details of the launch configuration, and then choose Create launch configuration to choose a key pair and create the launch configuration.

On the Configure Auto Scaling group details page, the launch configuration you created is already selected for you, and the number of instances you specified in the Amazon EC2 launch wizard is populated for Group size. Enter a name for the group, specify a VPC and subnet (if required), and then choose Next: Configure scaling policies.

On the Configure scaling policies page, choose one of the following options, and then choose Review:

To manually adjust the size of the Auto Scaling group as needed, select Keep this group at its initial size. For more information, see Manual Scaling.

To automatically adjust the size of the Auto Scaling group based on criteria that you specify, select Use scaling policies to adjust the capacity of this group and follow the directions. For more information, see Configure Scaling Policies.

On the Review page, you can optionally add tags or notifications, and edit other configuration details. When you have finished, choose Create Auto Scaling group.



Resource Monitoring of these servers. Zox needs a dashboard where it can monitor CPU usage, RAM usage.  :-

We can monitore these servers by using cloudwatch. For all instances by default cloudwatch create metrics for CPU utilization. For RAM utilization we need to add some script on ec2 instance. Which will send RAM info to cloudwatch and then we can create ALARM for same.

For RAM utilization metrics we download below mention script.

mon-put-instance-data.pl. We can get link of this script from google. 

After download this script, We set a cron which run each 5 minutes on instance. This script send data to cludwatch. 

After that we can add metrics into our dashboard. 


First of all we create a dashboard in cludwatch. 

Below are the steps for same. 


To create a dashboard using the console

Open the CloudWatch console.

In the navigation pane, choose Dashboards, Create dashboard.

In the Create new dashboard dialog box, type a name for the dashboard and choose Create dashboard.

Do one of the following in the Add to this dashboard dialog box:

To add a graph to your dashboard, choose Line or Stacked area and choose Configure. In the Add metric graph dialog box, select the metrics to graph and choose Create widget. If a specific metric does not appear in the dialog box because it has not published data in more than 14 days, you can add it manually. 

To add a number displaying a metric to the dashboard, choose Number, Configure. In the Add metric graph dialog box, select the metrics to graph and choose Create widget.

To add a text block to your dashboard, choose Text, Configure. In the New text widget dialog box, for Markdown, add and format your text using Markdown. Choose Create widget.

Choose Save dashboard.


After dashboard creation we will create alarm for our metrics. 

Below are the steps for same. 

To create an alarm using the Amazon EC2 console

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

In the navigation pane, choose Instances.

Select the instance.

On the Monitoring tab, choose Create Alarm.

In the Create Alarm dialog box, do the following:

Choose create topic. For Send a notification to, type a name for the SNS topic. For With these recipients, type one or more email addresses to receive notification.

Specify the metric and the criteria for the policy. For example, you can leave the default settings for Whenever (Average of CPU Utilization). For Is, choose >= and type 80 percent. For at least, type 1 consecutive period of 5 Minutes.

Here we define alarm for RAM utilization as well. we will define >= 90% threshold value for same. For RAM utilization we will give type 1 consecutive period of 2 Minutes. So that it will send alert if RAM utilization is >= 90% for more than 2 minutes. 


Choose Create Alarm.




Below is the shell script which will print CPU, RAM configured, Linux  kernel and distribution information into a file. 


#!/bin/bash

echo "Below is the CPU info of instance" >> instance_info.txt

cat /proc/cpuinfo >> instance_info.txt

echo "Below is the RAM info of instance" >> instance_info.txt

free -m >> instance_info.txt

echo "Below is the Kernel verison" >> instance_info.txt

uname -r >> instance_info.txt

echo "Below is the distribution version" >> instance_info.txt

cat /etc/redhat-release >> instance_info.txt


