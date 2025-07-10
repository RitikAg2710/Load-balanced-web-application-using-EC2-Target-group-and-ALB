# Load-balanced-web-application-using-EC2-Target-group-and-ALB

🔧 STEP-BY-STEP SETUP
Step 1: Launch 2 EC2 Instances
Do this 2 baar:

AMI: Amazon Linux 2

Type: t2.micro (Free tier)

User Data:

bash
Copy
Edit
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
echo "<h1>Server 1</h1>" > /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
For 2nd EC2, just replace Server 1 → Server 2

Security Group: Allow port 80 (HTTP) inbound

Launch in same VPC (but preferably different AZs)

Step 2: Create a Target Group
Go to: EC2 Dashboard → Target Groups → Create

Name: my-target-group

Target type: Instances

Protocol: HTTP

Port: 80

VPC: Same as EC2s

Health Check:

Path: /

Protocol: HTTP

Click Next

Register Targets: Select both EC2s → Add to registered

Create

Step 3: Create Application Load Balancer
Go to: EC2 Dashboard → Load Balancers → Create

Type: Application Load Balancer

Name: my-alb

Scheme: Internet-facing

IP type: IPv4

Listeners: HTTP (Port 80)

Availability Zones: Select at least 2 AZs and subnets

Step 4: Configure Security Group for ALB
Create new SG → Allow port 80 (HTTP) from 0.0.0.0/0

Step 5: Configure Listener and Routing
Listener: HTTP:80

Default action: Forward to your target group my-target-group

Step 6: Review and Create
Click Create Load Balancer

Wait for it to become Active

Step 7: Test the Load Balancer
Go to Load Balancers → my-alb → Description

Copy DNS name (e.g., my-alb-123456.ap-south-1.elb.amazonaws.com)

Paste in browser multiple times → You’ll see Server 1, Server 2 alternately (round robin)
