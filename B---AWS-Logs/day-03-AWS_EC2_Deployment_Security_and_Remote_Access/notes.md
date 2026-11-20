# 1. AWS Management Console (Dashboard)

The **AWS Management Console** is the web-based interface used to access and manage AWS services and resources.

## Important points

- It is the central place to access AWS services.
- Use the service search bar to find services such as EC2.
- The EC2 dashboard provides access to EC2 resources and management options.
- The selected AWS Region is shown in the console navigation bar.
- Resources such as EC2 instances are associated with a specific Region. When you switch Regions, you see resources belonging to the newly selected Region.

## Basic navigation

1. Sign in to the AWS Management Console.
2. Use the search bar to search for **EC2**.
3. Select **EC2** to open the EC2 dashboard.
4. From the EC2 dashboard, access options such as Instances and Security Groups.
5. Check the selected Region before creating or searching for resources.

---

# 2. Region vs Availability Zone (AZ)

AWS resources are deployed in Regions and Availability Zones to support availability, fault tolerance, and low latency.

## Region

- A geographical area where AWS has infrastructure, such as **Asia Pacific (Mumbai)** or **US East (N. Virginia)**.
- Each Region is designed to be isolated from other Regions.
- Choose a Region based on user proximity, compliance requirements, and cost considerations.
- AWS resources are Region-specific unless a service provides a separate cross-Region feature.

## Availability Zone (AZ)

- An isolated location within an AWS Region.
- Each Region contains multiple Availability Zones.
- Availability Zones in a Region are connected through low-latency links.
- An AZ identifier has the Region code followed by a letter, such as `ap-south-1a`.

## Why Regions and AZs matter

- **High availability:** Deploying resources across multiple AZs can help an application continue operating if one AZ has a failure.
- **Low latency:** Selecting a Region closer to users can reduce network delays.
- **Fault tolerance:** Separating resources across AZs reduces dependence on a single location.
- **Cost considerations:** Service prices can vary between Regions.

---

# 3. Introduction to EC2

**Amazon EC2 (Elastic Compute Cloud)** is an AWS service that provides on-demand, scalable computing capacity in the cloud.

In simple words, EC2 allows you to create and use virtual servers without purchasing physical server hardware.

## Features

- **Scalability:** Choose or change instance capacity according to workload requirements.
- **Customizable instances:** Select instance types based on CPU, memory, storage, and networking requirements.
- **Secure access:** Use key pairs and Security Groups to control access.
- **Flexible purchasing options:**
  - **On-Demand:** Pay for compute capacity as you use it, without a long-term commitment.
  - **Reserved Instances:** Make a usage commitment in exchange for discounted pricing.
  - **Spot Instances:** Use spare AWS capacity at discounted prices; Spot Instances can be interrupted by AWS.

> **Note:** Pricing, discounts, and Free Tier eligibility depend on the current AWS offer, Region, instance type, and account eligibility. Check the pricing shown in your account before launching resources.


## Use Cases of AWS EC2

### 1. Website and Web Application Hosting
Host websites, frontend applications, and backend applications using Nginx, Apache, Node.js, Python, etc.

**Example:** Hosting your React + Node.js application.

### 2. Application and API Hosting
Run backend services, REST APIs, and application servers.

**Example:** Hosting your MERN application's Express.js backend.

### 3. CI/CD and DevOps Infrastructure
Run build servers, deployment agents, automation scripts, and CI/CD tools.

**Example:** Installing Jenkins on an EC2 instance to automate deployments.

### 4. Database Hosting
Run database software such as MySQL, PostgreSQL, or MongoDB on EC2 when you need to manage the database infrastructure yourself.

**Example:** Running MongoDB on an Ubuntu EC2 instance.

### 5. Machine Learning and Compute-Intensive Workloads
Run model training, scientific computing, and GPU-intensive applications using suitable EC2 instance types.

**Example:** Training a deep learning model on a GPU-enabled EC2 instance.

### 6. Docker and Container Hosting
Install Docker and run multiple containerized applications on EC2. You can also use EC2 instances as the compute infrastructure for ECS or EKS.

### 7. Windows Virtual Machines
Run Windows Server applications, remote desktop environments, and Windows-based workloads.

## Important EC2 terms

| Term | Meaning |
|---|---|
| EC2 Instance | A virtual server running in AWS. |
| AMI | Amazon Machine Image; a template used to launch an EC2 instance. |
| Instance Type | Defines the instance's hardware capacity, including CPU and memory. |
| Key Pair | Public/private key pair used for secure access. |
| Security Group | Virtual firewall that controls instance traffic. |
| Public IP | IP address used to reach an instance over the internet when network configuration permits. |
| Elastic IP | Static public IPv4 address that can be associated with an AWS resource. |

---

# 4. Creating an Ubuntu EC2 Instance

## Steps to launch an EC2 instance

1. Sign in to the AWS Management Console.
2. Search for **EC2** and open the EC2 service.
3. Click **Launch instance**.
4. Enter a name for the instance.
5. Under **Application and OS Images (AMI)**, select an Ubuntu Server image.
6. Choose an **Instance Type** based on the required CPU, memory, and workload.
7. Under **Key pair (login)**, select an existing key pair or create a new one.
8. Configure the network settings:
   - Select the VPC and subnet.
   - Enable auto-assign public IP if the instance needs direct public IPv4 connectivity.
   - Select or create a Security Group.
9. Configure storage as required.
10. Add tags if needed, for example:
    - Key: `Name`
    - Value: `MyFirstInstance`
11. Review the configuration and click **Launch instance**.
12. If you created a new key pair, download and securely store the private key file (`.pem`).

> **Important:** Instance types and Free Tier eligibility can change. Do not assume that a particular instance type is free; check the current eligibility and estimated charges in the AWS console.

---

# 5. Security Groups in AWS

## What is a Security Group?

A **Security Group** acts as a virtual firewall for AWS resources such as EC2 instances.

It controls the incoming and outgoing network traffic allowed for the associated resources.

## Types of rules

### 1. Inbound Rules

Inbound rules define which incoming traffic is allowed to reach the instance.

Example:
- Allow SSH traffic on port `22`.
- Allow RDP traffic on port `3389`.
- Allow HTTP traffic on port `80` when hosting a public website.

### 2. Outbound Rules

Outbound rules define which outgoing traffic is allowed to leave the instance.

- A newly created Security Group has no inbound rules by default.
- A newly created Security Group has a default outbound rule that allows all outbound traffic.
- You can modify outbound rules to restrict outgoing traffic.

## Important characteristics

- Security Groups are associated with EC2 instances or other supported resources.
- You can associate multiple Security Groups with an instance.
- A Security Group can be associated with multiple instances.
- Rules can be modified after the Security Group is created.
- Security Groups are **stateful**. Response traffic for an allowed connection is automatically allowed, regardless of the opposite-direction rule.
- Security Groups support allow rules; they do not have deny rules. Traffic not allowed by the rules is not permitted.

## Common ports

| Service | Protocol | Port | Purpose |
|---|---|---:|---|
| SSH | TCP | 22 | Secure remote access to Linux instances. |
| RDP | TCP | 3389 | Remote access to Windows instances. |
| HTTP | TCP | 80 | Accessing websites over HTTP. |
| HTTPS | TCP | 443 | Accessing websites securely over HTTPS. |

## Steps to configure a Security Group

1. Open the **EC2 Dashboard**.
2. In the navigation pane, select **Security Groups**.
3. Click **Create security group**.
4. Enter a descriptive name and description.
5. Select the VPC in which your instance is running.
6. Add the required inbound rules.
7. Review the outbound rules. Keep the default outbound rule or configure it according to your requirements.
8. Click **Create security group**.
9. Associate the Security Group with your EC2 instance during launch or after launch.

## Example: Security Group for remote access

| Type | Protocol | Port | Source | Purpose |
|---|---|---:|---|---|
| SSH | TCP | 22 | My IP | Connect to a Linux instance. |
| RDP | TCP | 3389 | My IP | Connect to a Windows instance. |

## Security Group best practices

- Restrict SSH and RDP access to specific trusted IP addresses.
- Avoid allowing SSH (`22`) or RDP (`3389`) from `0.0.0.0/0` or `::/0`.
- Use separate Security Groups for different applications or roles when their access requirements differ.
- Open only the ports required by your application.
- Review Security Group rules regularly.

---

# 6. SSH (Secure Shell)

## What is SSH?

**SSH (Secure Shell)** is a cryptographic network protocol used to securely access and manage remote computers over an encrypted connection.

It allows administrators and developers to:
- Log in to remote servers.
- Execute commands remotely.
- Manage server resources.
- Transfer files securely.

## Key features of SSH

- **Secure communication:** Encrypts communication between the client and server.
- **Authentication:** Supports methods such as password-based and key-based authentication.
- **Port forwarding:** Provides secure tunnelling for other network connections.
- **File transfer:** Can be used with tools such as `scp` and `rsync`.
- **Remote command execution:** Allows commands to be executed on remote systems.

## SSH in AWS EC2

SSH is commonly used to access and manage Linux-based EC2 instances.

Important details:

- Default SSH port: `22` (TCP).
- Authentication: Commonly uses the private key corresponding to the key pair selected when launching the instance.
- Security Group: Must allow inbound SSH traffic from the connecting computer's public IP address or permitted network range.
- Ubuntu default username: `ubuntu`.

## Understanding an EC2 key pair

An EC2 key pair contains two keys:

- **Public key:** AWS places the public key on the instance.
- **Private key:** You download and keep the private key file on your computer.

The private key is used to prove that you are authorized to connect to the instance.

> **Important:** Keep the `.pem` private key file safe. Do not share it or upload it to a public repository. Anyone who obtains the private key may be able to access instances that trust the corresponding public key.

## Connecting to an Ubuntu EC2 instance

### Step 1: Check the prerequisites

- The EC2 instance should be running and pass its status checks.
- The instance should have a reachable public IP or public DNS name if connecting over the internet.
- Its Security Group must allow inbound TCP port `22` from your IP address.
- You must have the correct private key file.
- You must know the instance's default username.

### Step 2: Connect using SSH

General command:

```bash
ssh -i /path/to/key.pem ubuntu@<Public-IP-Address>
```

Example in Windows PowerShell:

```powershell
ssh -i ".\cloudblitz.pem" ubuntu@<EC2-PUBLIC-IP>
```

### Explanation of the command

| Part | Meaning |
|---|---|
| `ssh` | Starts an SSH connection. |
| `-i` | Specifies the identity/private key file. |
| `cloudblitz.pem` | Private key file downloaded during instance creation. |
| `ubuntu` | Default username for an Ubuntu EC2 AMI. |
| `<EC2-PUBLIC-IP>` | Public IPv4 address of the EC2 instance. |

### Step 3: Set private key permissions

On Linux or macOS, use:

```bash
chmod 400 /path/to/key.pem
```

This restricts access to the private key file.

### Step 4: Verify the connection

After a successful connection, you will get a terminal session on the remote Ubuntu instance and can execute Linux commands.

## SSH troubleshooting

If SSH connection fails:

1. Verify that the instance is running and has passed its status checks.
2. Check that the public IP or public DNS name is correct.
3. Confirm that inbound TCP port `22` is allowed in the attached Security Group.
4. Confirm that the source IP in the rule matches your current public IP.
5. Verify that you are using the correct private key.
6. Verify that the username is `ubuntu` for an Ubuntu AMI.
7. Check that the instance is reachable through its network configuration.

---

# 7. Creating a Windows EC2 Instance

AWS EC2 also supports Windows Server instances.

## Steps to launch a Windows instance

1. Sign in to the AWS Management Console.
2. Search for **EC2** and open the service.
3. Click **Launch instance**.
4. Enter a name for the instance.
5. Under AMI, select a suitable Windows Server image.
6. Select an instance type based on your requirements.
7. Configure network settings and select or create a Security Group.
8. Add an inbound rule for RDP (TCP port `3389`) from your IP address.
9. Select or create a key pair.
10. Configure storage and launch the instance.
11. Wait for the instance to become ready.

## Retrieve the Windows Administrator password

1. Open the EC2 Dashboard.
2. Select **Instances**.
3. Select your Windows instance.
4. Choose **Connect**.
5. Open the **RDP client** tab.
6. Choose **Get password** when the password is available.
7. Upload the private key (`.pem`) file used when launching the instance.
8. Choose **Decrypt password**.
9. Copy the Administrator password.

> **Note:** Password generation can take several minutes after launch. The private key is used to decrypt the initial Administrator password; it is not the RDP login password itself.

---

# 8. Remote Access: Linux vs Windows

Remote access allows you to connect to and manage a computer from another machine over a network.

## Linux Instance Remote Access

Linux EC2 instances are commonly accessed using SSH.

### Prerequisites

- SSH must be allowed in the Security Group.
- The correct `.pem` private key must be available.
- The instance must be reachable over the network.

### SSH command

```bash
ssh -i /path/to/key.pem ubuntu@<Public-IP>
```

### Troubleshooting

- Verify that the instance is running.
- Verify the public IP address.
- Check that the Security Group allows inbound SSH traffic from your IP address.
- Check that the correct private key and username are being used.

## Windows Instance Remote Access

Windows EC2 instances are commonly accessed using Remote Desktop Protocol (RDP).

### Prerequisites

- RDP must be allowed in the Security Group.
- The instance must be reachable over the network.
- The Administrator username and decrypted password must be available.

### Steps

1. Open Remote Desktop Connection on Windows by running:

```text
mstsc
```

2. Enter the public IP address of the Windows EC2 instance.
3. Enter the Administrator username.
4. Enter the decrypted Administrator password.
5. Connect to the instance.

### Troubleshooting

- Check that inbound TCP port `3389` is allowed.
- Verify the instance's public IP address and network connectivity.
- Verify the Administrator username and password.

## SSH vs RDP

| SSH | RDP |
|---|---|
| Commonly used for Linux instances. | Commonly used for Windows instances. |
| Uses TCP port `22` by default. | Uses TCP port `3389` by default. |
| Connects through an SSH client or terminal. | Connects through a Remote Desktop client. |
| Commonly uses a private key for EC2 authentication. | Commonly uses the Administrator password, which can be decrypted using the EC2 private key. |

---

# 9. Key Takeaways

- **AWS Management Console:** Web interface for accessing and managing AWS services.
- **Region:** Geographical area containing AWS infrastructure.
- **Availability Zone:** Isolated location within a Region.
- **EC2:** AWS service for launching virtual servers.
- **AMI:** Template used to launch an EC2 instance.
- **Instance Type:** Defines the computing capacity of an instance.
- **Security Group:** Virtual firewall that controls inbound and outbound traffic.
- **Inbound Rules:** Control traffic allowed to reach an instance.
- **Outbound Rules:** Control traffic allowed to leave an instance.
- **SSH:** Secure remote access protocol commonly used for Linux instances.
- **RDP:** Remote access protocol commonly used for Windows instances.
- **Key Pair:** Public/private key pair used for EC2 authentication and Windows password decryption.
- **Port 22:** Default SSH port.
- **Port 3389:** Default RDP port.
- Always restrict remote administration access to trusted IP addresses.
- Keep your private key file secure.

--- 

Dated : 19 November 2026