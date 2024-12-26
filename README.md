# Deploy Web Application on EC2 Instance (Manually)

## Project Overview

**Technologies Used**:  
- AWS  
- Docker  
- Linux  

### **Project Description**  
This project involves manually deploying a web application on an EC2 instance. The deployment process includes the following steps:  
1. Create and configure an EC2 Instance on AWS.  
2. Install Docker on the EC2 instance.  
3. Deploy a Docker image from a private Docker repository on the EC2 instance.  
4. Enable external access and run the web application.

---

## Steps to Deploy the Web Application  

### **1. Create and Configure an EC2 Instance on AWS**
Follow these steps to create and configure an EC2 instance:
- Launch an **Amazon Linux 2** instance with **t2.micro** instance type.  
- Set the **Auto-assign Public IP** to **Enable**.  
- Create a security group allowing SSH (port 22) access from **My IP**.  

### **2. Connect to EC2 Instance via SSH**
Connect to the EC2 instance using the private key:
```bash
ssh -i ~/.ssh/docker-server.pem ec2-user@http://98.80.231.175/
```

### **3. Install Docker on EC2 instance**
```bash
sudo yum update
sudo yum install docker
sudo service docker start
sudo usermod -aG docker $USER
```
Disconnect and reconnect via SSH to apply the changes.

### **4. Pull and run Docker image**
```bash
docker pull irschad/react-node-app:1.0
```
```bash
docker run -d -p 3000:3080 irschad/react-node-app:1.0
a5133606e540245192125d6653e0f34ca9dff94521406ac3d121e0bf90f6ca9c
```
```bash
docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                       NAMES
a5133606e540   irschad/react-node-app:1.0   "docker-entrypoint.s…"   5 seconds ago   Up 2 seconds   0.0.0.0:3000->3080/tcp, :::3000->3080/tcp   serene_aryabhata

```

### **5. Enable external access to the web application**
Update Security Group to Open Port 3000:
- Go to the EC2 Management Console:
  Navigate to your instance and select it.
- Edit Security Group Rules:
  - Go to the Security tab.
  - Click on the security group linked to your instance.
  - Open the Inbound Rules tab and click Edit inbound rules.
- Add Rule for Port 3000:
  - Add a rule with the following details:
    - Type: Custom TCP
    - Port Range: 3000
    - Source: Anywhere-IPv4
- Save the Rules:
  Click Save rules to apply the changes.

### **6. Access the web application**
1. Open in Browser:
    Access the application using the public IPv4 address of your EC2 instance:
    http://98.80.231.175:3000/
2. Access using DNS Name:
    Alternatively, use the instance's DNS name:
    http://ec2-98-80-231-175.compute-1.amazonaws.com:3000/

![Web App](https://github.com/user-attachments/assets/914b337e-4ef0-4434-8f47-315ce6fcaabe)






