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
![image.png](attachment:df57e481-e291-473c-bc33-2f548cf9e24e:image.png)
