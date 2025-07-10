🔧 STEP 1: Create 2 EC2 Servers
AWS Console pe jao → Search: EC2

Click Launch Instance

Create 2 Instances --> Http port enabled 




SAME AS FOR EC2 INSTANCE TOO 

🎯 STEP 2: Create Target Group (Group of Servers)
AWS Console → EC2 → Target Groups

Click Create Target Group

Settings:

Target type: Instances

Name: ritik-target-group

Protocol: HTTP

Port: 80

VPC: Same as EC2 VPC

Health check:

Protocol: HTTP

Path: /

Next → Register Targets:

Select Server 1 and Server 2

Click Include as pending

Click Create Target Group

🎉 Done 

🎯 STEP 3: Create Load Balancer (ALB)
EC2 → Load Balancers → Create Load Balancer

Select Application Load Balancer

Settings:

Name: ritik-alb

Scheme: Internet-facing

IP: IPv4

Select at least 2 public subnets from different AZs

Security Group:

Create new SG: Allow port 80 (HTTP)

Listener:

Listener HTTP:80 → Forward to → Select your ritik-target-group

Review & Create

⏳ Wait for ALB to become Active

🧪 STEP 4: Test Your Load Balancer
Go to Load Balancers → ritik-alb → Description tab

Copy the DNS name
Example: ritik-alb-123456.ap-south-1.elb.amazonaws.com

Paste in browser

👀 Output:

Refresh again and again → kabhi This is Server 1, kabhi This is Server 2

🎉 Congrats
