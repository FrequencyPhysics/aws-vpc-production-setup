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

-**EC2 Instances**
<img width="708" height="80" alt="image" src="https://github.com/user-attachments/assets/e9daac41-80b2-42c5-ae08-2245d7a69931" />
<img width="313" height="130" alt="image" src="https://github.com/user-attachments/assets/7334f5f8-f0fb-4540-95d2-b0d38934e182" />


