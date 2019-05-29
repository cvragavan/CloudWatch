# How to Monitor Memory and Disk Metrics for Amazon EC2 Linux Instance

## Introduction
AWS CloudWatch provides most of the monitoring Metrics by default. But it doesn’t have any metrics for memory utilization details and Disk space uses. So if you want to monitor the memory on your system or monitor free disk space using CloudWatch. Then first you need to add these metrics to your account using custom scripts.

This article will help you to monitor EC2 Linux instance memory and disk metrics with AWS CloudWatch. Remember this will not work on any Linux machine outside the EC2 network.

## Prerequsiteis
For this tutorial, you will use the Perl scripts provided by AWS team, These scripts have some dependencies. You can use the following commands to install these dependencies as per your operating systems.

###Redhat Based Systems:

sudo yum install perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https perl-Digest-SHA
sudo yum install zip unzip

###Debian Based Systems:

sudo apt-get update
sudo apt-get install unzip libwww-perl libdatetime-perl

###SUSE Linux Enterprise Server:

sudo zypper install perl-Switch perl-DateTime
sudo zypper install –y "perl(LWP::Protocol::https)"

##Download and Configure Script

wget  http://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip
unzip CloudWatchMonitoringScripts-1.2.1.zip

Now create credentails file with coping template file.

cd /aws-scripts-mon
cp awscreds.template awscreds.conf

Now, You need to add AWSAccessKeyId and AWSSecretKey of your AWS account. This will verify account ownership for the script. If you don’t have, you can create keys in your account under Users >> Security credentials section.

Otherwise you can attached IAM role into your instances.

##Test and Schedule Scripts
At this point, your setup is complete. Let’s use the following command to verify the connectivity between script and your AWS account.

./mon-put-instance-data.pl --mem-util --verify --verbose
 (OR)
./mon-put-instance-data.pl --mem-used-incl-cache-buff --mem-util --mem-used --mem-avail

The output will be something like below on successful verification.

####Verification completed successfully. No actual metrics sent to CloudWatch.
 
##View Metrics in CloudWatch
You should wait for some time after adding crontab. So it can collect some data to view in metrics graph. After some time

>> Login AWS Dashboard
>> Go to CloudWatch Service
>> Click on Browse Metrics button
>> Select Linux System under Custom Namespaces.
