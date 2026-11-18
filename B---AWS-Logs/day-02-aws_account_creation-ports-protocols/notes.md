# Creating an AWS Account

## Steps

1.  Go to the [AWS website](https://aws.amazon.com/).
2.  Choose **Create an AWS Account** and start the signup process.
3.  Sign up using an email address or an available sign-in option, such as Google (if offered).
4.  Enter your name and the required account details.
5.  Complete email/phone verification and set a secure password.
6.  Finish the signup process and sign in to the AWS Management Console.

---

# Steps for Creating an EC2 Instance

1.  Sign in to the **AWS Management Console**.
2.  Search for **EC2** and open the EC2 service. The EC2 dashboard will appear.
3.  Click **Launch instance**.
4.  Enter a name for your instance.
5.  Choose an operating system image (AMI). For example, select **Ubuntu**.
6.  Choose an **instance type**. It defines the virtual machine's hardware capacity, such as vCPUs, memory (RAM), and network performance.
7.  Create a **key pair** and download the private key file to your computer.
8.  Review the remaining settings. Configure the security group carefully, then launch the instance.

## Understanding the Key Pair

An EC2 key pair is used to authenticate securely when connecting to an instance.

- **Public key:** AWS places the public key on the EC2 instance.
- **Private key:** You download and keep the private key file on your computer.
- When connecting through SSH, the private key proves that you are authorized to access the instance.

> **Important:** Keep the private key file safe. Do not share it, upload it to GitHub, or send it to anyone. If you lose the private key, you may not be able to use it to authenticate to the instance.

---

# Ports and Protocols

## 1. What is a Port?

A **port** is a logical communication endpoint used by a computer to direct network traffic to the correct application or service.

- An IP address identifies a device on a network.
- A port number identifies the application or service on that device.
- Port numbers range from **0 to 65535**.

**Example:** When you open a website, your browser usually connects to the web server using port `443` for HTTPS.

## 2. What is a Protocol?

A **protocol** is a set of rules that defines how devices communicate and exchange data over a network.

Examples include:

- **HTTP/HTTPS:** Used to access websites.
- **SSH:** Used to securely access and manage remote machines.
- **FTP/SFTP:** Used to transfer files.
- **DNS:** Used to translate domain names into IP addresses.
- **TCP/UDP:** Transport-layer protocols used to deliver data.

## 3. Difference Between a Port and a Protocol

| Port | Protocol |
|---|---|
| Identifies a communication endpoint or service on a device. | Defines the rules for communication. |
| Represented by a number, such as `22` or `443`. | Represented by a name, such as SSH, HTTP, TCP, or UDP. |
| Helps direct traffic to the correct application. | Defines how data is transmitted or interpreted. |
| Example: port `22` is commonly used by SSH. | Example: SSH is the protocol used for secure remote login. |

## 4. Common Ports, Protocols, and Their Use Cases

| Port | Protocol / Service | Transport | Use case |
|---:|---|---|---|
| 20, 21 | FTP | TCP | Traditional file transfer. Port 21 is used for control; port 20 is traditionally used for active-mode data transfer. |
| 22 | SSH | TCP | Secure remote login and server administration. Commonly used to connect to an Ubuntu EC2 instance. |
| 23 | Telnet | TCP | Remote terminal access; insecure because traffic is not encrypted. Avoid using it for production access. |
| 25 | SMTP | TCP | Sending email between mail servers. |
| 53 | DNS | UDP/TCP | Resolving domain names to IP addresses. UDP is common for queries; TCP is also used in cases such as zone transfers and large responses. |
| 67, 68 | DHCP | UDP | Automatically assigning IP configuration to devices. |
| 80 | HTTP | TCP | Accessing websites without TLS encryption. |
| 110 | POP3 | TCP | Retrieving email from a mail server. |
| 123 | NTP | UDP | Synchronizing system clocks over a network. |
| 143 | IMAP | TCP | Accessing and managing email stored on a mail server. |
| 161, 162 | SNMP | UDP | Monitoring and managing network devices. |
| 389 | LDAP | TCP/UDP | Accessing directory services. |
| 443 | HTTPS | TCP (commonly) | Secure website access using TLS. HTTP/3 uses QUIC over UDP port 443. |
| 445 | SMB | TCP | Sharing files and printers, mainly in Windows networks. |
| 3306 | MySQL | TCP | Connecting to a MySQL database server. |
| 3389 | RDP | TCP/UDP | Remote Desktop access to Windows machines. |
| 5432 | PostgreSQL | TCP | Connecting to a PostgreSQL database server. |
| 6379 | Redis | TCP | Connecting to a Redis server. |
| 27017 | MongoDB | TCP | Connecting to a MongoDB server. |

> **Note:** A port number is commonly associated with a service, but the number alone does not guarantee which application is running there. Administrators can configure services to use different ports.

## 5. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented. | Connectionless. |
| Provides reliable, ordered data delivery. | Does not guarantee delivery or ordering. |
| Retransmits lost data when needed. | Does not provide built-in retransmission. |
| Has more communication overhead. | Usually has lower overhead and latency. |
| Used by SSH, HTTP/1.1, HTTPS over TCP, and database connections. | Used by DNS queries, DHCP, NTP, voice/video streaming, and online gaming. |

## 6. Ports and Protocols in AWS EC2

When launching an EC2 instance, AWS uses a **Security Group** as a virtual firewall to control inbound and outbound traffic.

Common inbound rules for an Ubuntu EC2 instance:

| Type | Protocol | Port | Source | Purpose |
|---|---|---:|---|---|
| SSH | TCP | 22 | Your public IP address | Connect to the Ubuntu server using SSH. |
| HTTP | TCP | 80 | `0.0.0.0/0` (only if public access is intended) | Allow users to access a public website over HTTP. |
| HTTPS | TCP | 443 | `0.0.0.0/0` (only if public access is intended) | Allow users to access a public website securely. |

### Security Best Practices

- Allow SSH access only from your own public IP address whenever possible, rather than from `0.0.0.0/0`.
- Open only the ports your application actually needs.
- Do not expose database ports such as `3306`, `5432`, or `27017` to the entire internet.
- Prefer encrypted protocols such as SSH, HTTPS, and SFTP.
- Review Security Group rules regularly and remove unnecessary access.

## 7. Practical Example: Connecting to an Ubuntu EC2 Instance

```bash
ssh -i "cloudblitz.pem" ubuntu@<EC2-PUBLIC-IP>
```

- `ssh`: starts an SSH connection.
- `-i`: specifies the identity/private key file.
- `"cloudblitz.pem"`: the path to your downloaded private key.
- `ubuntu`: the default username commonly used for Ubuntu EC2 images.
- `<EC2-PUBLIC-IP>`: the public IPv4 address of your EC2 instance.
- SSH normally connects through **TCP port 22**.

On Windows, if the key is in the current directory, a PowerShell command may look like:

```powershell
ssh -i ".\cloudblitz.pem" ubuntu@<EC2-PUBLIC-IP>
```

---

# Quick Revision

- **IP address:** Identifies a device on a network.
- **Port:** Identifies a service/application endpoint on that device.
- **Protocol:** Defines the communication rules.
- **TCP:** Reliable, ordered transport.
- **UDP:** Lightweight transport without delivery guarantees.
- **SSH:** Secure remote access; commonly uses TCP port `22`.
- **HTTP:** Web traffic; commonly uses TCP port `80`.
- **HTTPS:** Encrypted web traffic; commonly uses port `443`.
- **DNS:** Resolves domain names; commonly uses port `53`.
- **AWS Security Group:** Controls inbound and outbound traffic for an
  EC2 instance.
