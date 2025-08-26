# Developing a VPC with Public and Private Subnets in Production

## Overview
This project demonstrates how to set up a Virtual Private Cloud (VPC) in AWS for a production-ready environment.  
The design includes:
- Public and private subnets across two Availability Zones for resiliency.
- An Application Load Balancer (ALB) in the public subnet.
- Auto Scaling group for servers in the private subnet.
- NAT Gateways for internet access from private subnets.
This setup improves availability, scalability, and security.

## Architecture

**Public subnets include:**
- A NAT Gateway
- A Load Balancer node

**Private subnets include:**
- Application servers launched via an Auto Scaling group
- Servers receive traffic through the Load Balancer
- Outbound internet access through the NAT Gateway

**Diagram:**  
<img width="448" height="349" alt="image" src="https://github.com/user-attachments/assets/bb2fa980-b534-499e-84c6-126fcfe57d8e" />

## Steps to Deploy

1. **Create the VPC**
   - Define the VPC CIDR block (e.g., `10.0.0.0/16`).
   - Create subnets (2 public, 2 private across AZs).

2. **Create the Internet Gateway**
   - Attach it to the VPC.
   - Route public subnets to the Internet Gateway.

3. **Deploy NAT Gateways**
   - One NAT Gateway per Availability Zone in the public subnet.
   - Update private subnet route tables to use the NAT Gateway.

4. **Create Application Load Balancer**
   - Deploy the ALB in the public subnets.
   - Configure listeners and target groups.

5. **Configure Auto Scaling Group**
   - Launch EC2 instances in private subnets.
   - Attach them to the ALB target group.
   - Set scaling policies for high availability.

6. **Security Groups & NACLs**
   - **Load Balancer SG:** allow inbound HTTP/HTTPS from the internet.
   - **Server SG:** allow inbound only from the Load Balancer SG.
   - **Outbound:** servers access internet via NAT Gateway.

## Connectivity
- **Public access:** Internet → ALB → Private servers.
- **Private server internet access:** Private subnet → NAT Gateway → Internet Gateway.

## Screenshots
- **VPC creation:**
<img width="706" height="257" alt="image" src="https://github.com/user-attachments/assets/52d464f7-ea97-46b1-9e32-a38d87bfd1a6" />

- **Auto Scaling group:**
<img width="706" height="117" alt="image" src="https://github.com/user-attachments/assets/aa541579-7fe1-413b-8adb-03d0e90326d6" />

- **EC2 Instances:**
<img width="708" height="80" alt="image" src="https://github.com/user-attachments/assets/e9daac41-80b2-42c5-ae08-2245d7a69931" />
<img width="313" height="130" alt="image" src="https://github.com/user-attachments/assets/7334f5f8-f0fb-4540-95d2-b0d38934e182" />

### Bastion Host Setup
- **Instance summary of Bastion Host:**
<img width="706" height="328" alt="image" src="https://github.com/user-attachments/assets/a7ab0509-ed3e-467a-86c5-cffda113544c" />

- **Checking pem key in directory:**
<img width="590" height="229" alt="image" src="https://github.com/user-attachments/assets/cf868938-8d97-4f35-8c79-3485db9240c5" />

- **Setting permissions for pem key:**
<img width="563" height="64" alt="image" src="https://github.com/user-attachments/assets/8eb7446e-973a-4568-93fb-2373e5a373cd" />

- **Copying pem key using scp:**
<img width="707" height="68" alt="image" src="https://github.com/user-attachments/assets/495357eb-d71a-4ac2-bd76-84a4992c3eb0" />

- **SSH into Bastion Host:**
<img width="698" height="45" alt="image" src="https://github.com/user-attachments/assets/04f98ee8-e545-4e27-a6e0-48c575bdb2c1" />

### Private EC2 Instance
- **PEM key present in ec2 (bastion host) instance:**
<img width="366" height="60" alt="image" src="https://github.com/user-attachments/assets/869dc5c9-3689-47b9-8895-d8ca370f34e1" />

- **SSH into private EC2 instance:**
<img width="690" height="18" alt="image" src="https://github.com/user-attachments/assets/f2e52129-5def-40f5-ab22-694aeda74075" />

- **IP verification against AWS console:**
<img width="128" height="38" alt="image" src="https://github.com/user-attachments/assets/37f720ac-9b7f-442e-9347-9bd2b3790219" />

### Application Setup
- **index.html file creation:**
<img width="713" height="319" alt="image" src="https://github.com/user-attachments/assets/8fa9f412-7bd6-4b3d-8190-75ba26d6dec2" />

- **Running Python3 server on port 8000:**
<img width="715" height="79" alt="image" src="https://github.com/user-attachments/assets/b04b4dfb-d026-4f74-89b2-008910670e71" />


### Load Balancer & Target Group
- **Creating Target Group:**
<img width="705" height="87" alt="image" src="https://github.com/user-attachments/assets/1673bb05-7d45-4322-bcff-6c2a9a0278f2" />
 
- **Creating Load Balancer:**
<img width="708" height="96" alt="image" src="https://github.com/user-attachments/assets/803af8bc-887d-4d52-a356-41ed76bce2ab" />

- **Listener port 80 not reachable:**
<img width="708" height="110" alt="image" src="https://github.com/user-attachments/assets/e8597c5f-cd33-4945-b728-de1a049d3038" />

- **Inbound rule added for port 80:**
<img width="711" height="17" alt="image" src="https://github.com/user-attachments/assets/9c742b9d-a491-4e24-9aa4-afa21e53e064" />

- **Load Balancer listener fixed:**
<img width="704" height="109" alt="image" src="https://github.com/user-attachments/assets/af7eb9c2-6856-4382-9345-9a69b889c55e" />


### Testing the Application
- **Accessing app via Load Balancer DNS:**
<img width="709" height="301" alt="image" src="https://github.com/user-attachments/assets/54d3ecc6-1312-4c48-adb4-9b51f4a7c08c" />


