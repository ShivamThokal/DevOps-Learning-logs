# AWS EC2 – Advanced Settings and Purchasing Options

## 1. Advanced Details Tab (While Creating an Instance)

The **Advanced Details** section allows us to configure additional settings that control the behavior and initialization of an EC2 instance.

### 1.1 Shutdown Behavior

Defines what happens to an EC2 instance when an operating system shutdown command is performed.

There are two options:

| Option | Description |
|---|---|
| Stop | The instance stops running. Its EBS volumes and data are retained. |
| Terminate | The instance is permanently terminated, and its associated resources may be deleted depending on their settings. |

**Example:**
- If shutdown behavior is set to Stop, executing `sudo shutdown now` stops the instance.
- If shutdown behavior is set to Terminate, executing the same command terminates the instance.

**Note:** Shutdown behavior applies to an OS-initiated shutdown. It is different from manually selecting Stop or Terminate in the AWS Console.

### 1.2 Hibernate Behavior

Hibernation allows us to stop an instance while preserving its in-memory (RAM) state.

- The contents of RAM are saved to the root EBS volume.
- When the instance is started again, it restores the previous RAM state.
- Applications and processes can resume from where they were left off.

**Example:** If an application is running and we hibernate the instance, its memory state is saved. After starting the instance again, it can resume from the previous state.

**Important points:**
- Hibernation must be enabled when launching the instance.
- Only supported instance types and operating systems can use hibernation.
- The root EBS volume must be encrypted and large enough to store the RAM contents.
- Hibernation is different from a normal Stop operation.

### 1.3 Termination Protection

Prevents an EC2 instance from being accidentally terminated.

- When enabled, the instance cannot be terminated through normal termination requests.
- It helps protect important instances from accidental deletion.
- It can be disabled when termination is required.

**Example:** Enable termination protection for a production server to prevent accidental termination.

### 1.4 Stop Protection

Prevents an EC2 instance from being accidentally stopped.

- When enabled, the instance cannot be stopped through normal stop requests.
- It is useful for instances that must remain running.
- It can be disabled when stopping the instance is required.

**Example:** Enable stop protection for a critical application server.

**Quick Comparison**

| Feature | Purpose |
|---|---|
| Shutdown Behavior | Defines whether OS shutdown stops or terminates the instance |
| Hibernate | Saves RAM contents before stopping the instance |
| Termination Protection | Prevents accidental termination |
| Stop Protection | Prevents accidental stopping |

---

## 2. User Data Script – Install and Run Apache

**User Data** allows us to execute scripts automatically when an EC2 instance is launched.

For example, we can install Apache automatically instead of manually connecting to the instance and installing it.

### Apache Installation Script (Ubuntu)

```bash
#!/bin/bash

# Update package information
apt update -y

# Install Apache web server
apt install apache2 -y

# Start Apache service
systemctl start apache2

# Enable Apache to start automatically on boot
systemctl enable apache2
```
> A similar script can be written for nginx too.

### Explanation

| Command | Purpose |
|---|---|
| `#!/bin/bash` | Specifies Bash as the script interpreter |
| `apt update -y` | Updates package information |
| `apt install apache2 -y` | Installs Apache |
| `systemctl start apache2` | Starts the Apache service |
| `systemctl enable apache2` | Enables Apache to start automatically during boot |

### Steps to Execute User Data

1. Open the AWS EC2 Dashboard.
2. Click Launch Instance.
3. Configure the instance details.
4. Expand **Advanced Details**.
5. Find the **User Data** section.
6. Paste the Apache installation script.
7. Launch the instance.
8. Wait for the installation to complete.
9. Copy the instance's Public IPv4 address.
10. Open the address in a browser.

Example:

```text
http://<EC2-Public-IP>
```

You should see the Apache2 Ubuntu Default Page.

**Important Notes:**
- By default, EC2 User Data scripts execute during the first boot.
- User Data scripts generally run with root privileges.
- Make sure the Security Group allows inbound HTTP traffic on port `80`.
- This script is for Ubuntu/Debian-based systems. Amazon Linux uses `dnf` or `yum` and the `httpd` package instead.

---

## 3. EC2 Instance Types and Purchasing Options

AWS provides different purchasing options depending on cost, workload requirements, flexibility and infrastructure needs.

### 3.1 On-Demand Instances

- Pay for compute capacity based on actual usage.
- No long-term commitment is required.
- Suitable for unpredictable or short-term workloads.
- You can launch, stop and terminate instances as needed.

**Example:** Running a development or testing server for a few hours.

### 3.2 Reserved Instances (RI)

- Provide discounted pricing in exchange for a 1-year or 3-year commitment.
- Discounts can reach up to 72%, depending on the offering and comparison.
- Suitable for workloads with predictable, long-term usage.
- Standard and Convertible Reserved Instances are available.

**Example:** A production application server that runs continuously throughout the year.

**Important:** Reserved Instances are primarily a billing discount commitment. They do not automatically reserve physical capacity unless you use a Zonal Reserved Instance.

### 3.3 Savings Plans

- Provide discounted pricing in exchange for a commitment to a consistent amount of usage (measured in dollars per hour).
- Commitment is generally for 1 or 3 years.
- Offer more flexibility than Reserved Instances.
- Discounts can reach up to 72% for certain EC2 Instance Savings Plans.

Types:
- Compute Savings Plans – More flexible across instance families, regions and supported compute services.
- EC2 Instance Savings Plans – Provide discounts for a specific EC2 instance family in a selected region.

**Example:** A company commits to spending a certain amount per hour on AWS compute usage to receive discounted rates.

### 3.4 Spot Instances

- Use unused EC2 capacity available in AWS.
- Can offer discounts of up to 90% compared with On-Demand prices.
- AWS can interrupt Spot Instances when it needs the capacity back.
- Suitable for flexible and fault-tolerant workloads.

**Examples:**
- Batch processing
- Data analysis
- Background jobs
- Large-scale testing

**Important:** Do not use Spot Instances for workloads that cannot tolerate interruptions unless you have designed the application to handle them.

### 3.5 Dedicated Hosts

- A physical server dedicated entirely to a single AWS customer.
- Provides visibility into physical sockets and cores.
- Useful for software licensing requirements and compliance.
- You have more control over instance placement on the physical server.

**Example:** An organization running software that requires licenses based on physical CPU sockets or cores.

**Note:** A Dedicated Host is a physical server, not necessarily an entire server rack.

### 3.6 Dedicated Instances

- EC2 instances running on hardware dedicated to a single AWS customer.
- Other AWS customers cannot share that hardware.
- You do not get the same level of physical server visibility and placement control as Dedicated Hosts.

**Example:** An organization requiring dedicated hardware for compliance or isolation purposes.

**Difference between Dedicated Hosts and Dedicated Instances:**

| Dedicated Hosts | Dedicated Instances |
|---|---|
| Entire physical server dedicated to one customer | Instances run on hardware dedicated to one customer |
| Provides visibility into physical sockets and cores | Does not provide the same physical server visibility |
| Supports certain BYOL licensing requirements | More limited physical licensing control |
| More control over instance placement | Less placement control |

### 3.7 Capacity Reservations

- Allows you to reserve EC2 compute capacity in a specific Availability Zone (AZ).
- Ensures that the required capacity is available when you need to launch an instance.
- Can be created for immediate or future use, depending on the reservation type.
- Charged at the applicable On-Demand rate, whether the reserved capacity is used or not.

**Example:** A company expects high traffic during a festival sale and wants to ensure that EC2 capacity is available in a particular Availability Zone.

**Important:**
- Capacity Reservations are AZ-specific.
- They do not automatically provide discounted pricing.
- They can be combined with eligible billing discounts, such as Savings Plans or Reserved Instances.

---

## 4. Quick Comparison – EC2 Purchasing Options

| Purchasing Option | Commitment | Main Benefit | Best Use Case |
|---|---|---|---|
| On-Demand | No | Flexibility | Short-term workloads |
| Reserved Instances | 1 or 3 years | Discounted pricing | Predictable workloads |
| Savings Plans | 1 or 3 years | Flexible discounted pricing | Consistent compute usage |
| Spot Instances | No long-term commitment | Very low cost | Interruptible workloads |
| Dedicated Hosts | Depends on billing option | Physical server control | Licensing requirements |
| Dedicated Instances | Depends on billing option | Hardware isolation | Compliance requirements |
| Capacity Reservations | Reservation-based | Guaranteed AZ capacity availability | Critical workloads |

---

## 5. Important Points to Remember

1. **On-Demand:** Pay as you use, without a long-term commitment.
2. **Reserved Instances:** Commit for 1 or 3 years to receive discounted pricing.
3. **Savings Plans:** Commit to a certain amount of compute usage per hour.
4. **Spot Instances:** Use unused AWS capacity at a significant discount, but interruptions are possible.
5. **Dedicated Hosts:** Get an entire physical server dedicated to your use.
6. **Dedicated Instances:** Run instances on hardware dedicated to your AWS account.
7. **Capacity Reservations:** Reserve compute capacity in a particular Availability Zone.
8. **Hibernate:** Saves RAM contents to the root EBS volume before stopping.
9. **Termination Protection:** Protects an instance from accidental termination.
10. **Stop Protection:** Protects an instance from accidental stopping.

---

**Dated:** 24 December 2026