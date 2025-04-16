# Implementing Auto Scaling to Manage Two Different Target Groups

## 📌 Overview

This project demonstrates how to implement **Auto Scaling** in AWS for **two different target groups**, each managing **at least two EC2 instances**, and routing traffic through an **Application Load Balancer (ALB)**. A sample web application is deployed to all instances to demonstrate how scaling works in real time.

---

## 🧱 Architecture Summary

- **Application Load Balancer (ALB)** to distribute traffic.
- **Two Auto Scaling Groups (ASGs)**:
  - Each with a unique launch template.
  - Each linked to a different target group.
- **Two Target Groups** (Group A and Group B), each pointing to EC2 instances.
- Each group has a **minimum of 2 EC2 instances** running a web app.

---

## 🚀 Deployment via AWS Console

> ✅ Pre-requisites: VPC, public subnets, a security group allowing HTTP (port 80), and a working key pair (for optional EC2 SSH access).

### Step 1: Create Target Groups

1. Go to **EC2 Dashboard → Target Groups → Create target group**.
2. Select **Instances** as target type.
3. Create:
   - `target-group-a`
   - `target-group-b`
4. For each, choose:
   - Protocol: `HTTP`
   - Port: `80`
   - Health check path: `/`
5. Leave default settings and create both.

### Step 2: Create Launch Templates

1. Go to **EC2 Dashboard → Launch Templates → Create launch template**.
2. Fill in:
   - Name: `lt-group-a` / `lt-group-b`
   - AMI: Select an Amazon Linux or your own custom AMI with the app.
   - Instance type: e.g., `t2.micro`
   - Key pair: Select your key (optional for SSH)
   - Security group: Allow port 80 (HTTP)
   - User data: Add script to install and run your app (e.g., basic HTML or Node.js)
3. Repeat for both templates (Group A and Group B).

### Step 3: Create Auto Scaling Groups

1. Go to **EC2 Dashboard → Auto Scaling Groups → Create Auto Scaling group**.
2. Set:
   - Name: `asg-group-a` / `asg-group-b`
   - Launch template: Select the corresponding one.
3. Network:
   - Select appropriate VPC and public subnets.
4. Attach to target group:
   - Select `target-group-a` for ASG A
   - Select `target-group-b` for ASG B
5. Set desired capacity: **Minimum: 2, Desired: 2, Maximum: 4**
6. Skip scaling policies for now (optional).
7. Create both ASGs.

### Step 4: Create an Application Load Balancer

1. Go to **EC2 Dashboard → Load Balancers → Create Load Balancer** → **Application Load Balancer**.
2. Name: `web-alb`
3. Scheme: **Internet-facing**
4. Listeners: Protocol `HTTP`, Port `80`
5. Availability Zones: Select public subnets
6. Security group: Allow HTTP (port 80)
7. Target Groups:
   - Choose existing target groups: `target-group-a` and `target-group-b`
8. Listener Rules:
   - Forward `/group-a` traffic to `target-group-a`
   - Forward `/group-b` traffic to `target-group-b`

### Step 5: Testing

Once everything is set up:

- Access:
http://<your-load-balancer-dns>/group-a http://<your-load-balancer-dns>/group-b

- You should see the web app running from each target group.

---

## 🛠️ Technologies Used

- Amazon EC2
- Auto Scaling Groups
- Target Groups
- Application Load Balancer (ALB)
- EC2 User Data for deployment

---

## 📬 Contact

For questions or ideas, feel free to reach out via GitHub or email. Happy to collaborate!

**Happy Scaling 🚀**
