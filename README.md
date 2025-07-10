🔧 STEP 1: Create 2 EC2 Servers (Chhoti Machines)
AWS Console pe jao → Search: EC2

Click Launch Instance

Instance Name: Server 1

AMI: Amazon Linux 2

Instance type: t2.micro (Free tier)

Key Pair: Choose existing or create new

Network settings:

Create new Security Group

Allow port 22 (SSH) and port 80 (HTTP)

Scroll to Advanced → User Data → Paste this:

bash
Copy
Edit
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
echo "<h1>This is Server 1</h1>" > /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
Launch

Repeat same for Server 2, just change this line:

bash
Copy
Edit
echo "<h1>This is Server 2</h1>" > /var/www/html/index.html
🟢 Dono servers ka status running hona chahiye
🧪 Browser mein daalke test karo:
http://<EC2-IP> → dikhega Server 1 ya Server 2

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

🎉 Done → Tumhara “server group” ready ho gaya

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

🎉 Congrats bhai! Tu Load Balancer practical kar gaya!
