# aws1
Deploying a custom vpc for a multi-tier web application to be hosted inside AWS. it's the project for the course: [AWS Introduction For The Absolute Beginners](https://www.dolfined.com/courses/arabic-aws-introduction-for-absolute-beginners) at Dolfined.com 


## Roadmap
![](https://github.com/Moka1302/aws1/blob/main/roadmap.png)


## Resources to be built:
- Custom VPC in AWS 
- 4 subnets, 2 public and 2 private
- Internet Gateway to be created and attached to the VPC
- 2 EBS-backed EC2 instances (Instance A and Instance B)
- 2 security groups WebSG and ALBSG
- A target group WebTG
- An applicaIon load balancer WebALB
- Launch template
- Auto-scaling group


## Steps:
1. I created VPC with 2 public and 2 private subnets. with the corresponding Internet Gateway and Route Tables settings.
2. The private subnets do not have routes to the igw, and the public subnets must have routes to the igw.
3. I launched 2 EC2 instances in the private subnets with no public IP, and attached a user data script to download httpd and execute:
`echo " This is server *1* in AWS Region US-EAST-1 in AZ US-EAST-1A " > /var/www/html/index.html`
`echo " This is server *2* in AWS Region US-EAST-1 in AZ US-EAST-1B " > /var/www/html/index.html`
I was able to connect to the ec2 using EC2 Instance Connect Endpoint. I just followed the aws docs [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-with-ec2-instance-connect-endpoint.html) to do it, it's very easy.
4. I created a security group to allow traffic from the load balancer security group only and allow all outbound traffic.

