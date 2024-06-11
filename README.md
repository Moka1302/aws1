# aws1
Deploying a custom VPC for a multi-tier web application to be hosted inside AWS. it's the project for the course: [AWS Introduction For The Absolute Beginners](https://www.dolfined.com/courses/arabic-aws-introduction-for-absolute-beginners) at![Dolfined.com](https://www.dolfined.com/)


## Overview
I created a custom VPC with 4 subnets, 2 public and 2 private. I deployed a load balancer that receives public requests from the Internet and distributes the workload between the instances. The instances exist in 2 private subnets while the load balancer exists in 2 public subnets. Finally, I created an auto-scaling group for high availability. 


## Roadmap
![](https://github.com/Moka1302/aws1/blob/main/roadmap.png)


## Resources to be built:
- Custom VPC in AWS 
- 4 subnets, 2 public and 2 private
- Internet Gateway and route tables
- 2 EBS-backed EC2 instances that will serve as the web and application tiers
- 2 security groups for the EC2 instances (named WebSG) and the load balancer (named ALBSG)
- A target group named WebTG
- An application load balancer named WebALB
- Launch template 
- Auto-scaling group 


## Steps:
1. I created VPC with 2 public and 2 private subnets. with the corresponding Internet Gateway and Route Tables settings.
2. The private subnets do not have routes to the IGW, and the public subnets must have routes to the IGW.
![](https://github.com/Moka1302/aws1/blob/main/vpc.png)


3. I created a security group for the web/app instances named WebSG, that allows inbound traffic only from the application load balancer security group as a source and allows all outbound traffic.
![](https://github.com/Moka1302/aws1/blob/main/WebSG.png)


I created another security group for the load balancer (ALBSG) that allows outbound HTTP to the web/app security group (WebSG) and allows inbound traffic from the internet on port HTTP.
![](https://github.com/Moka1302/aws1/blob/main/ALBSG.png)


4. I launched 2 EC2 instances in the private subnets with no public IP, and attached a user data script to download httpd and execute:
   
   `echo " This is server *1* in AWS Region US-EAST-1 in AZ US-EAST-1A " > /var/www/html/index.html` for server1
   
   `echo " This is server *2* in AWS Region US-EAST-1 in AZ US-EAST-1B " > /var/www/html/index.html` for server2
   
   I can connect the EC2 using the EC2 Instance Connect Endpoint and allow SSH inbound traffic. I just followed the AWS docs [here.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-with-ec2-instance-connect-endpoint.html) 


5. I created a target group and registered the EC2 instances.
![](https://github.com/Moka1302/aws1/blob/main/Target%20Group.png)


6. I created an Application Load Balancer assigned to the 2 public subnets in 2 different AZ
![](https://github.com/Moka1302/aws1/blob/main/Load%20Balancer.png)

Testing the Load Balancer from the browser: response from server1
![](https://github.com/Moka1302/aws1/blob/main/response%20from%20server1.png)

after refreshing the browser: the response from server2
![](https://github.com/Moka1302/aws1/blob/main/response%20from%20server2.png)


7. Created Launch template and auto-scaling group
   ![](https://github.com/Moka1302/aws1/blob/main/auto-scaling%20group.png)

