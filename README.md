# Secure VPC Setup with EC2 Instances

A hands-on AWS networking project demonstrating how to design and configure a **secure and scalable VPC infrastructure** using public and private subnets, EC2 instances, IAM, Security Groups, Network ACLs, Internet Gateway, and NAT Gateway.

## Architecture

The project implements a secure AWS network architecture with:

* Custom VPC and CIDR range
* Public and private subnets
* Internet Gateway for public subnet connectivity
* NAT Gateway for private subnet outbound internet access
* Route tables for traffic management
* Security Groups for instance-level access control
* Network ACLs for subnet-level traffic filtering
* EC2 instances in public and private subnets
* IAM roles and policies for AWS resource access
* SSH key-pair authentication

## AWS Services Used

| Service              | Purpose                                                       |
| -------------------- | ------------------------------------------------------------- |
| **Amazon VPC**       | Creates the isolated network environment                      |
| **Amazon EC2**       | Hosts instances in public and private subnets                 |
| **IAM**              | Manages roles and permissions                                 |
| **Internet Gateway** | Provides internet access to public subnet resources           |
| **NAT Gateway**      | Provides outbound internet access to private subnet resources |
| **Security Groups**  | Controls instance-level inbound and outbound traffic          |
| **Network ACLs**     | Controls subnet-level network traffic                         |
| **Route Tables**     | Defines network traffic routing                               |
| **SSH Key Pair**     | Provides secure EC2 authentication                            |

## Architecture Flow

```text
                         Internet
                            |
                    Internet Gateway
                            |
                    +----------------+
                    |   Public Subnet |
                    |                |
                    |  EC2 Instance  |
                    +-------+--------+
                            |
                       NAT Gateway
                            |
                    +-------+--------+
                    |  Private Subnet|
                    |                |
                    |  EC2 Instance  |
                    +----------------+
```

## Project Implementation

### 1. Create the VPC

Created a custom VPC with a dedicated CIDR block to provide an isolated networking environment.

Example:

```text
VPC CIDR: 10.0.0.0/16
```

### 2. Configure Subnets

Created separate public and private subnets.

```text
Public Subnet:  10.0.1.0/24
Private Subnet: 10.0.2.0/24
```

The public subnet is configured to access the internet through the Internet Gateway, while the private subnet uses the NAT Gateway for outbound internet connectivity.

### 3. Configure Internet Gateway

Created and attached an Internet Gateway to the VPC.

The public subnet route table contains:

```text
0.0.0.0/0 → Internet Gateway
```

This allows resources in the public subnet to communicate with the internet.

### 4. Configure NAT Gateway

Created a NAT Gateway in the public subnet.

The private subnet route table contains:

```text
0.0.0.0/0 → NAT Gateway
```

This allows private EC2 instances to access the internet for outbound connections without exposing them directly to inbound internet traffic.

### 5. Configure Route Tables

Configured separate route tables for public and private subnets.

**Public Route Table**

```text
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
```

**Private Route Table**

```text
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         NAT Gateway
```

### 6. Configure Security Groups

Security Groups were configured to allow only the required traffic.

Example:

```text
SSH   → Port 22
HTTP  → Port 80
HTTPS → Port 443
```

SSH access was restricted to the required source IP rather than allowing unrestricted access.

### 7. Configure Network ACLs

Network ACLs were configured to provide an additional layer of subnet-level security by controlling inbound and outbound traffic.

The configuration was designed to allow only the required protocols and ports.

### 8. Launch EC2 Instances

EC2 instances were launched in both public and private subnets.

**Public EC2**

* Located in the public subnet
* Associated with a public route table
* Accessible through controlled SSH access

**Private EC2**

* Located in the private subnet
* No direct inbound internet access
* Uses NAT Gateway for outbound connectivity

### 9. Configure IAM Roles

IAM roles were created and attached to EC2 instances where required.

Permissions were configured following the **principle of least privilege**, providing instances only the AWS permissions necessary for their tasks.

### 10. Configure SSH Access

Generated an EC2 key pair and securely stored the private key.

SSH access was tested using:

```bash
ssh -i key.pem ec2-user@<PUBLIC-IP>
```

The private key was not committed to the GitHub repository.

## Testing & Validation

The following tests were performed:

### Public EC2 Internet Connectivity

Verified that the public EC2 instance could access the internet through the Internet Gateway.

### Private EC2 Internet Connectivity

Verified that the private EC2 instance could access external resources through the NAT Gateway.

### Inter-Subnet Communication

Tested connectivity between EC2 instances located in different subnets.

### Security Group Validation

Verified that only configured ports and protocols were accessible.

### Network ACL Validation

Tested inbound and outbound traffic according to the configured NACL rules.

### SSH Authentication

Verified EC2 access using the configured SSH key pair.

## Security Considerations

* Used separate public and private subnets.
* Restricted SSH access to trusted source IPs.
* Applied Security Groups and Network ACLs as multiple security layers.
* Used IAM roles instead of storing AWS credentials on EC2 instances.
* Followed the principle of least privilege.
* Kept the SSH private key outside the Git repository.
* Avoided exposing private EC2 instances directly to the internet.

## Key Learnings

Through this project, I gained practical experience with:

* AWS VPC architecture
* Public vs. private subnet design
* AWS routing and route tables
* Internet Gateway and NAT Gateway
* EC2 networking
* Security Groups and Network ACLs
* IAM roles and policies
* SSH-based EC2 access
* Network troubleshooting and connectivity validation
* AWS security best practices

## Technologies Used

**AWS:** VPC, EC2, IAM, Security Groups, Network ACLs, Internet Gateway, NAT Gateway, Route Tables

**Networking:** TCP/IP, Subnets, Routing, SSH

## Project Outcome

Successfully designed and validated a **secure AWS VPC architecture with isolated public and private workloads**, controlled network access, IAM-based permissions, and secure outbound connectivity from private resources.

## Author

**Rick Ritwik**

B.Tech Computer Science & Engineering | KIIT University
