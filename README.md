# eLearning Platform - AWS Architecture Overview

## 📌 Overview
This **eLearning platform** is designed with a **highly available and scalable** architecture, ensuring **fault tolerance and reliability**. The infrastructure is deployed across **two Availability Zones (AZs)**, following a **three-tier architecture** (Web, App, and Data).

---

## 🏗 Infrastructure Design

### 1️⃣ Networking & Subnets  
The architecture is deployed within an **Amazon VPC** with the following subnets:
- **1 Public Subnet**:
  - Hosts the **NAT Gateway**, allowing private instances to access the internet.
- **3 Private Subnets**:
  - **Web Tier**: Hosts **EC2 instances** serving as web servers.
  - **App Tier**: Hosts **EC2 instances** for business logic processing.
  - **Data Tier**: Hosts **Amazon RDS** with **Multi-AZ replication** for failover support.
  
---

### 2️⃣ Traffic Routing & Load Balancing  
- **Amazon Route 53** directs incoming requests to **AWS WAF** for security.  
- Requests are then routed either to:
  - **Amazon CloudFront** for static/media content.  
  - **Application Load Balancer (ALB)** for dynamic requests.  
- **ALB** distributes web traffic among EC2 instances in the Web Tier to ensure high availability.

---

### 3️⃣ Compute & Database Layer  
- **Web Tier**: Each AZ contains **one EC2 instance** to handle frontend requests.  
- **App Tier**: Each AZ contains **two EC2 instances** to process business logic.  
- **Data Tier**:
  - **Amazon RDS (Multi-AZ Deployment)**:
    - **Primary instance** in **AZ1**.
    - **Standby instance** in **AZ2** for failover support.
  - **Amazon ElastiCache** improves performance by caching frequently accessed data.

---

### 4️⃣ Security  
- **AWS Shield**: Protects **CloudFront** & **ALB** from **DDoS attacks**.  
- **AWS WAF**: Secures **ALB** by filtering **malicious requests**.  
- **IAM Roles & Policies**: Enforce **least-privilege access** across services.  
- **Amazon S3**: Used for **secure file storage**, accessed by **EC2 instances**.

---

### 5️⃣ Security Groups & Network Access Control  
Security Groups (SGs) act as **virtual firewalls** to control traffic:  
- **ALB SG**: Allows **HTTP/HTTPS** traffic from the internet.  
- **Web Tier SG**: Accepts traffic **only from ALB SG**.  
- **App Tier SG**: Accepts traffic **only from Web Tier SG**.  
- **Database SG**: Accepts traffic **only from App Tier SG**.  
- **NAT Gateway SG**: Allows **outbound** traffic for private instances.  

---

### 6️⃣ Monitoring & Notifications  
- **Amazon CloudWatch**:
  - Collects **logs** and **performance metrics** from all services.  
- **Amazon SNS**:
  - Sends **real-time notifications** to **admins and developers** about:
    - **System health**  
    - **Security alerts**  
    - **Performance issues**  


## 🔹 Author  
**Dina Abdelgalil Mohamed**  
