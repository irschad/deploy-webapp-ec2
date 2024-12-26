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
- Launch an **Amazon Linux 2** instance with **t2.micro** instance type.  Indicate Name as 'my-instance'
- Set the **Auto-assign Public IP** to **Enable**.
- Create a security group called "security-group-docker-server" allowing SSH (port 22) access from **My IP**.
- Create key-pair and name it 'docker-server'. Select private key file format as 'pem' for Linux. 
- Add tag: enter Key as 'Type' and Value as "web-server-with-docker"

### **2. Connect to EC2 Instance via SSH**
Move the 'docker-server.pem' key file into the ssh folder ~/.ssh. 
Restrict the file permissions: 
```bash
chmod 400 ~/.ssh/docker-server.pem
```
Connect to the EC2 instance using the private key:
```bash
ssh -i ~/.ssh/docker-server.pem ec2-user@http://98.80.231.175/
```

### **3. Install Docker on EC2 instance**
- Update package manager
  ```bash
  sudo yum update
   ```
- Run docker install command.
  ```bash
  sudo yum install docker -y
  ```
- Start docker daemon
  ```bash
  sudo service docker start
  ```
- Check if docker is running
  ```bash
  ps aux | grep docker
  root        2358  0.1 10.5 2028232 102672 ?      Ssl  12:02   0:24 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=32768:65536
  root        5297  0.0  0.3 1531804 3488 ?        Sl   12:31   0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3000 -container-ip 172.17.0.2 -container-port 3080
  root        5302  0.0  0.4 1683364 3904 ?        Sl   12:31   0:00 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 3000 -container-ip 172.17.0.2 -container-port 3080
  ec2-user   18188  0.0  0.2 222312  2040 pts/0    S+   16:26   0:00 grep --color=auto docker
  ```
- Add current user to docker group 
  ```bash
  sudo usermod -aG docker $USER
  ```
- Disconnect and reconnect via SSH to apply the changes.
  Run 'groups' command to see docker group is added
  ```bash
  groups
  ec2-user adm wheel systemd-journal docker
  ```

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
  - Open the Inbound Rules tab and click 'Edit inbound rules'.
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






