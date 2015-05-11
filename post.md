
## Introduction

Each Amazon EC2 region is designed to be completely isolated from the other Amazon EC2 regions. This achieves the greatest possible fault tolerance and stability.

Amazon EC2 provides multiple regions so that you can launch Amazon EC2 instances in locations that meet your requirements. For example, you might want to launch instances in Europe to be closer to your European customers or to meet legal requirements. The following table lists the regions that provide support for Amazon EC2.

Amazon EC2 is hosted in multiple locations world-wide. These locations are composed of regions and Availability Zones. Each region is a separate geographic area. Each region has multiple, isolated locations known as Availability Zones. Amazon EC2 provides you the ability to place resources, such as instances, and data in multiple locations. Resources aren't replicated across regions unless you do so specifically.

Amazon operates state-of-the-art, highly-available data centers. Although rare, failures can occur that affect the availability of instances that are in the same location. If you host all your instances in a single location that is affected by such a failure, none of your instances would be available.All communications between regions is across the public Internet.

Source : http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html

In this Article , I'll demonstrate the feature of AWS Route 53 which can be used for Cross Region Failover of application deployed in multiple regions.
For Example : One Application is deployed in N. Virginia and same in N. California
so when N. Virginia goes down we'll be having access to our application which is in N. California.

### Step 1
Create two ec2 instances in two different regions . In our case I am provisioning one instance in N.Virginia and other one in N. California Region.

### Instance in N.Virginia
![Image 2](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/2.JPG)

### Instance in N. California
![Image 1](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/1.JPG)

### Step 2
Install httpd server on each server and create a index.html file.


```
# yum install httpd
# cd /var/www/html/
# vim index.html
```
Add Contents in index.html file in order to distuinguish in between the regions.

index.html in N.Virginia
```
# <h1>This Webpage is from N.Virginia Region
```
index.html in N.California
```
# <h1>This Webpage is from N.California
```
### Step 3
Start the httpd Server
```
# service httpd start
```
### Step 4 
Goto Route 53 and create health check at port 80 for both the instances as by default httpd server works on port 80. Here we are monitoring the health (service running) on the instances.

![Image 3](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/4.JPG)

wait till the status becomes healthy.

### Step 5
Now in Route53,  create a recordset in a hosted zone.
  In this article I have a domain registered with name ishantkumar.in in Route53. I am going to create  one recordset with name test.ishantkumar.in.
Select the recordset and click on Go to Record Sets.
![Image 4](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/3.JPG)

### Step 6
Enter the Recordset name. I created test.ishantkumar.in.

### Step 7
Enter the public IPs of both the instances (one in a line only).
Set the routing policy as failover.

![Image 5](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/5.JPG)

### Step 8
Associate the health check type of one the instance which we created in step 5.
In this Article, I am giving NVirginiaHealthCheck as primary.

![Image 6](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/6.JPG)

### Step 9
Test the website. (test.ishantkumar.in) . It will show you the webpage associated with Primary Health Check . In our case web page hosted in N.Virginia Region.

![Image 7](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/7.JPG)

### Step 10

Delete the N.Virginia's instance  and check your website.

![Image 8](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/8.JPG)

Still we are able to see the website but this time we are getting web page hosted in N.California Region. It shows even one region goes down, website is still up and running in another region.


![Image 9](https://s3-us-west-2.amazonaws.com/ishant/demoRegionFailover/9.JPG)

I believe you found this article useful. I will continue to write more posts on AWS and DevOps. If you want any help or article need to be written just let me know.
<hr>
<hr>
###Thanks
Ishant Kumar

