## 1. High Availability using Multiple Availability Zones

**Goal:** Keep a website/application available even if an EC2 instance or an Availability Zone (AZ) has a problem.

An **Availability Zone (AZ)** is an isolated location within an AWS Region. A Region contains multiple AZs.

### Basic architecture

```text
                    Users
                      |
                      v
              Load Balancer
                /       \
               v         v
          AZ - A        AZ - B
         EC2 #1         EC2 #2
               \         /
                \       /
                 Database
```

### How it works

1. Deploy application instances in at least two Availability Zones.
2. Place a load balancer in front of the instances.
3. Configure health checks so unhealthy instances stop receiving traffic.
4. Use an Auto Scaling Group to replace unhealthy instances and adjust capacity when demand changes.
5. Configure the database layer for high availability too, if the application requires it.

**Important:** Simply creating two instances does not automatically provide high availability. They should be in different AZs, traffic should be routed to healthy instances, and application data must be stored or replicated safely.

### Key terms

| Term | Meaning |
|---|---|
| Region | A separate AWS geographic area |
| Availability Zone (AZ) | An isolated location within a Region |
| Load Balancer | Distributes incoming traffic across healthy targets |
| Auto Scaling Group | Maintains and adjusts the number of EC2 instances |
| Health Check | Checks whether a target is healthy and able to receive traffic |

---

## 2. What is an AMI?

**AMI (Amazon Machine Image)** is a template used to launch EC2 instances.

An AMI can include:
- Operating system
- Installed software and applications
- Configuration
- One or more EBS volume snapshots / block-device mappings

**Example:** Suppose an EC2 instance has Ubuntu, Nginx and your website configured. You can create an AMI from it and use that AMI to launch more instances with the same prepared server setup.

### Create an AMI from an existing EC2 instance

**Step 1:** Open the AWS Management Console and go to **EC2 → Instances**.

**Step 2:** Select the instance from which you want to create an image.

**Step 3:** Choose:

```text
Actions
  → Image and templates
      → Create image
```

**Step 4:** Enter:
- Image name
- Description (optional)
- Reboot preference

**Step 5:** Choose **Create image**.

**Step 6:** Go to **EC2 → AMIs** and check the AMI status.

The status initially appears as `pending` and changes to `available` when it is ready. The time varies depending on the size and number of volumes; it is not always 5–6 minutes.

### What happens after creating an AMI?

- AWS creates the AMI and the required EBS snapshots.
- You can launch new EC2 instances using that AMI.
- You can use it to recreate a server if the original instance is deleted or fails.
- The original instance and AMI are separate resources.

**Important points:**
- Deleting the original EC2 instance does not automatically delete the AMI.
- Do not delete the AMI's required snapshots if you still need to use the AMI.
- An AMI is Region-specific. Copy it to another Region if you need to launch instances there.
- An AMI is not a complete backup of every aspect of an EC2 instance. Review attached volumes and application data, and maintain separate backups where required.
- For consistent EBS snapshots, AWS normally reboots the instance during image creation by default. Review the reboot option and application impact before creating a production image.

### Launch a new instance from an AMI

1. Go to **EC2 → AMIs**.
2. Select the AMI.
3. Choose **Launch instance from AMI** (or the available launch action).
4. Select instance type, key pair, network and security group settings.
5. Review the configuration and launch.

---

## 3. What is an EC2 Launch Template?

A **Launch Template** stores the configuration parameters used when launching EC2 instances.

It avoids entering the same settings every time you launch an instance.

A launch template can include:
- AMI ID
- Instance type
- Key pair
- Network and subnet settings
- Security groups
- Storage configuration
- IAM instance profile
- User data (startup commands)
- Tags and other launch settings

Not every field is mandatory; unspecified values can be supplied when launching the instance or use applicable defaults.

### Create a Launch Template

**Step 1:** Open **AWS Console → EC2**.

**Step 2:** In the navigation pane, choose **Launch Templates**.

**Step 3:** Choose **Create launch template**.

**Step 4:** Enter:
- Launch template name
- Description (optional)

**Step 5:** Configure the launch settings:
1. Select an AMI.
2. Select an instance type.
3. Select an existing key pair or choose not to include one.
4. Configure network settings and security groups.
5. Configure storage.
6. Add tags and optional advanced settings, such as IAM role or user data.

**Step 6:** Review the settings and choose **Create launch template**.

### Create a Launch Template from an existing instance

You can also create a template using an existing EC2 instance:

1. Go to **EC2 → Instances**.
2. Select the instance.
3. Choose **Actions → Image and templates → Create template from instance** (wording may vary in the console).
4. Enter a template name.
5. Review the copied configuration.
6. Adjust settings if required.
7. Create the launch template.

Review the generated settings rather than assuming every instance property is copied.

### Launch an instance using a Launch Template

1. Go to **EC2 → Launch Templates**.
2. Select your template.
3. Choose **Actions → Launch instance from template**.
4. Select the template version.
5. Review or adjust permitted launch settings.
6. Launch the instance.

### Launch Template versions

A launch template can have multiple numbered versions. For example:
- Version 1: Ubuntu AMI and `t3.micro`
- Version 2: Updated AMI and a different security group
- Version 3: Updated user data

You can choose which version to use and set a default version.

---

## 4. Difference between AMI and Launch Template

| Feature | AMI | Launch Template |
|---|---|---|
| Full form | Amazon Machine Image | EC2 Launch Template |
| Main purpose | Provides the image used to create an instance | Stores instance launch configuration |
| Contains | OS, installed software, image/volume information | Settings such as AMI ID, instance type, key pair and networking |
| Used for | Recreating a prepared server image | Launching instances consistently |
| Versions | AMI IDs are distinct images | Supports numbered versions |
| Example | Ubuntu + Nginx + configured website | Launch that AMI as `t3.micro` with a chosen security group |

**Easy way to remember:**

- **AMI = What software/image should the instance start with?**
- **Launch Template = How should the instance be launched?**

A Launch Template can refer to an AMI. They are complementary, not alternatives.

---

## 5. Types of EC2 Instances

EC2 instance families are designed for different workloads. The main categories include:

| Category | Main strength | Common use cases |
|---|---|---|
| General Purpose | Balanced CPU, memory and networking | Web servers, development, small databases |
| Compute Optimized | High CPU performance | Batch processing, scientific modelling, compute-heavy applications |
| Memory Optimized | Large memory capacity | In-memory databases, real-time analytics, large caches |
| Storage Optimized | High local storage performance | High-throughput databases, data warehouses, distributed file systems |
| Accelerated Computing | Hardware accelerators such as GPUs | Machine learning, graphics, video processing |

### 1. General Purpose

Provides a balanced combination of compute, memory and networking.

Examples: `t3`, `t4g`, `m7i`, `m7g`

Use cases:
- Websites
- Application servers
- Development and testing environments

### 2. Compute Optimized

Designed for workloads that need high CPU performance.

Examples: `c7i`, `c7g`, `c8g`

Use cases:
- Batch processing
- High-performance web servers
- CPU-intensive applications

### 3. Memory Optimized

Designed for workloads that process large datasets in memory.

Examples: `r7i`, `r7g`, `x2`

Use cases:
- In-memory databases
- Real-time data analytics
- Large caching workloads

### 4. Storage Optimized

Designed for workloads that need high, fast local storage I/O.

Examples: `i4i`, `im4gn`, `is4gen`, `d3`

Use cases:
- Distributed databases
- Data processing
- High-throughput local storage workloads

### 5. Accelerated Computing

Uses hardware accelerators such as GPUs or specialized chips.

Examples: `p` and `g` instance families.

Use cases:
- Machine learning and AI
- Graphics rendering
- Video processing

**Note:** Exact families and availability depend on the AWS Region. Check current specifications, pricing and workload compatibility before choosing an instance.

---

## 6. Quick Revision

| Question | Answer |
|---|---|
| What is an AMI? | An image used to launch EC2 instances |
| Why create an AMI? | To recreate or launch instances with a prepared software setup |
| What is a Launch Template? | A reusable set of EC2 launch parameters |
| Can a Launch Template use an AMI? | Yes |
| What is an AZ? | An isolated location within an AWS Region |
| How can an application stay available if one AZ fails? | Run instances in multiple AZs and route traffic to healthy targets |
| What maintains the desired number of EC2 instances? | Auto Scaling Group |
| Which instance family is suitable for CPU-intensive tasks? | Compute Optimized |
| Which category is suitable for large in-memory workloads? | Memory Optimized |


---

**Dated —** 23 November 2026
