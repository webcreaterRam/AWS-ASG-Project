# 🚀 AWS Auto Scaling Group (ASG) Project  
### High Availability | Load Balancing | Auto Healing | Scalable Architecture

This project demonstrates a **production-style AWS architecture** built manually using the AWS Management Console.  
It showcases **real-world AWS infrastructure design**, focusing on high availability, scalability, auto-healing, and load-balanced web applications.

---

## 📌 **1. Architecture Overview**

This architecture deploys a **highly available web application** across multiple Availability Zones using:  
- **VPC with public subnets**  
- **Internet Gateway + Route Tables**  
- **Security Groups with least privilege**  
- **EC2 instance with Apache (via user-data script)**  
- **Custom AMI creation**  
- **Launch Template based on AMI**  
- **Application Load Balancer (ALB)**  
- **Target Group (TG)**  
- **Auto Scaling Group (ASG)**  

The system automatically:  
- distributes traffic  
- scales based on load  
- replaces unhealthy EC2 instances  
- maintains desired capacity  

---

## 📊 **2. Architecture Diagram**

Example:

```
![AWS ASG Architecture](architecture-diagram.png)
```

---

## 🧩 **3. Step-by-Step Implementation Flow**

### **🔹 Step 1 — Create Networking Layer (VPC)**
- Custom VPC created (CIDR example: `10.0.0.0/16`)
- **Two public subnets** in two different AZs  
- Public Route Table associated with both public subnets  
- Internet Gateway attached  

**Purpose:** Multi-AZ setup ensures **high availability**.

---

### **🔹 Step 2 — Create Security Groups**
1. **ALB Security Group**
   - Inbound: HTTP (80) from anywhere  
2. **EC2 Security Group**
   - Inbound: HTTP (80) only from **ALB SG**  
   - More secure approach: no public access to EC2

**Purpose:** Enforces **least-privilege traffic flow**.

---

### **🔹 Step 3 — Create an EC2 Instance**
- Amazon Linux / Ubuntu instance  
- EC2 SG attached  
- User Data used to install Apache + custom HTML:

```bash
#!/bin/bash
sudo apt update -y
sudo apt install apache2 -y
echo "<h1>Welcome to my Auto Scaling Project</h1>" > /var/www/html/index.html
sudo systemctl start apache2
sudo systemctl enable apache2
```

**Result:** A running web server in your custom VPC.

---

### **🔹 Step 4 — Create AMI (Golden Image)**
- A custom AMI created from the EC2 instance  
- This AMI contains pre-installed Apache + app files  

**Purpose:** Enables fast, consistent, identical environment deployment.

---

### **🔹 Step 5 — Create Launch Template**
- Launch Template created using:
  - Custom AMI  
  - Instance type  
  - EC2 Security Group  
  - User data (optional, since AMI already configured)  

**Purpose:** ASG uses this template to create new instances automatically.

---

### **🔹 Step 6 — Create Target Group**
- Target type: **Instances**  
- Health check: `/` on port 80  
- Will dynamically register EC2 instances launched by ASG  

---

### **🔹 Step 7 — Create Application Load Balancer**
- ALB created in **both public subnets**  
- Listener: HTTP (80)  
- Target Group attached  

**Result:** ALB distributes traffic across all healthy EC2s.

---

### **🔹 Step 8 — Create Auto Scaling Group (ASG)**
- ASG configured using Launch Template  
- VPC Subnets: Both public subnets  
- Desired capacity: **1**  
- Min: **1**, Max: **2**  
- Health checks: ELB  
- Automatically registers instances to Target Group  

**Outcome:**  
- If traffic increases → ASG scales out  
- If instance fails → ASG replaces it  
- If load decreases → ASG scales in  

---

## 🏆 **4. Skills Demonstrated (Interview-Focused)**

### ✅ **Core AWS Skills**
- VPC design  
- Multi-AZ architecture  
- Security Group modeling  
- EC2 provisioning  
- Application Load Balancer  
- Auto Scaling Group  
- Launch Templates  
- AMI lifecycle  

### ✅ **Production Concepts**
- High Availability (HA)  
- Auto Healing  
- Horizontal Scaling  
- Elastic Load Balancing  
- Golden Image pattern  
- Infrastructure segmentation  

### ✅ **DevOps Strength**
- Clear understanding of cloud infrastructure  
- Ability to design scalable systems  
- Knowledge of traffic flow & security boundaries  
- Understanding of stateless architecture  

---

## 🎯 **5. Real-World Use Cases**
- High traffic websites  
- Auto scaling web apps  
- Disaster recovery setups  
- Fault-tolerant systems  
- Production-grade backend servers  

---

## 🚀 **6. Future Enhancements**
- Infrastructure as Code (Terraform)  
- Configuration Management (Ansible)  
- CI/CD with Jenkins + GitHub Actions  
- Containerization (Docker & Kubernetes)  
- ALB + ASG + RDS multi-tier architecture  

---

## 👨‍💻 Author
**Ramkumar Baghel**  
Cloud & DevOps Enthusiast | AWS | Terraform | Linux | Docker | CI/CD  

---
