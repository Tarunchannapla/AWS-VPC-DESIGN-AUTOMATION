# 🏗️ AWS VPC Design & Automation using Terraform

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws&style=for-the-badge)
![Terraform](https://img.shields.io/badge/Terraform-IaC-purple?logo=terraform&style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner--Friendly-blue?style=for-the-badge)

> Designed and automated a **production-grade AWS VPC** from scratch using Terraform — the same architecture pattern used in real enterprise environments.

---

## 📌 Project Overview

This project covers two key areas:

1. **VPC Design** — Understanding how to design a multi-tier network architecture for a real application stack
2. **VPC Automation** — Using Terraform (Infrastructure as Code) to create and manage the entire VPC with a single command

---

## 🏛️ Architecture Diagram

![VPC Architecture](architecture/vpc-architecture.png)

The architecture supports a full production application stack:

| Layer | Component | Subnet Type |
|---|---|---|
| Web / Traffic | Application Load Balancer, Route 53 | Public |
| Application | Java App (Auto Scaling Group) | Private - App |
| Database | RDS (MySQL / PostgreSQL) | Private - DB |
| CI/CD | Jenkins, Ansible, Terraform | Private - Management |
| Monitoring | Prometheus, Grafana, Consul | Private - Platform |
| Security | AWS Secrets Manager, NACLs | Private |

---

## 🌐 VPC Configuration

| Parameter | Value |
|---|---|
| **CIDR Block** | `10.0.0.0/16` |
| **Region** | `us-west-2` (Oregon) |
| **Availability Zones** | `us-west-2a`, `us-west-2b`, `us-west-2c` |
| **Total Subnets** | 15 (5 types × 3 AZs) |
| **Internet Gateway** | ✅ Yes (for public subnets) |
| **NAT Gateway** | ✅ Yes (for private subnet outbound) |
| **VPC Endpoints** | S3, CloudWatch, Secrets Manager |
| **Network ACLs** | 4 dedicated sets |

---

## 🗂️ Subnet Architecture

| Subnet Type | CIDR Range | Access | Purpose |
|---|---|---|---|
| **Public** (×3) | `10.0.1.0/24` – `10.0.3.0/24` | Internet-facing | Load Balancer, Bastion Host |
| **App** (×3) | `10.0.4.0/24` – `10.0.6.0/24` | Private (via NAT) | Java Application Servers |
| **DB** (×3) | `10.0.7.0/24` – `10.0.9.0/24` | Private (isolated) | RDS Database Instances |
| **Management** (×3) | `10.0.10.0/24` – `10.0.12.0/24` | Private (via NAT) | Jenkins, Ansible, CI/CD Tools |
| **Platform** (×3) | `10.0.13.0/24` – `10.0.15.0/24` | Private (via NAT) | Prometheus, Grafana, Consul |

---

## 📁 Repository Structure

```
AWS-VPC-DESIGN-AUTOMATION/
├── README.md
├── architecture/
│   └── vpc-architecture.png
├── terraform/
│   ├── modules/
│   │   └── vpc/
│   │       ├── vpc.tf
│   │       ├── subnet.tf
│   │       ├── internet-gateway.tf
│   │       ├── nat-gateway.tf
│   │       ├── route-tables.tf
│   │       ├── nacl.tf
│   │       ├── endpoint.tf
│   │       ├── outputs.tf
│   │       └── variables.tf
│   ├── infra/
│   │   └── vpc/
│   │       ├── main.tf
│   │       └── variables.tf
│   └── vars/
│       └── dev/
│           └── vpc.tfvars
├── docs/
│   └── step-by-step.md
└── screenshots/
    ├── 01-terraform-apply.png
    ├── 02-vpc-console.png
    ├── 03-subnets.png
    └── 04-route-tables.png
```

---

## ⚙️ Prerequisites

Before running this project, make sure you have:

- [ ] AWS Account with IAM user (Access Key + Secret Key)
- [ ] [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed and configured
- [ ] [Terraform](https://developer.hashicorp.com/terraform/install) installed (v1.0+)
- [ ] [Git](https://git-scm.com/downloads) installed

---

## 🚀 How to Run This Project

### Step 1 — Clone the Repository
```bash
git clone https://github.com/Tarunchannapla/AWS-VPC-DESIGN-AUTOMATION.git
cd AWS-VPC-DESIGN-AUTOMATION
```

### Step 2 — Configure AWS CLI
```bash
aws configure
# Enter: Access Key ID, Secret Access Key, Region (us-west-2), Output (json)
```

### Step 3 — Edit Variables (Optional)
Open `terraform/vars/dev/vpc.tfvars` and update as needed:
```hcl
region         = "us-west-2"
vpc_cidr_block = "10.0.0.0/16"
owner          = "your-name"
environment    = "dev"
```

### Step 4 — Navigate to Infra Folder
```bash
cd terraform/infra/vpc
```

### Step 5 — Initialize Terraform
```bash
terraform init
```
✅ Expected: `Terraform has been successfully initialized!`

### Step 6 — Preview Resources
```bash
terraform plan -var-file="../../vars/dev/vpc.tfvars"
```
✅ Expected: List of 40+ resources planned for creation

### Step 7 — Create the VPC
```bash
terraform apply -var-file="../../vars/dev/vpc.tfvars"
```
Type `yes` when prompted.

✅ Expected: `Apply complete! Resources: X added`

### Step 8 — Verify in AWS Console
Go to **AWS Console → VPC → us-west-2 region** and verify:
- Your VPC (`10.0.0.0/16`) is listed
- 15 subnets are created
- Internet Gateway is attached
- Route Tables and NACLs are configured

### Step 9 — Cleanup (Avoid AWS Charges!)
```bash
terraform destroy -var-file="../../vars/dev/vpc.tfvars"
```

---

## 📸 Screenshots

### Terraform Apply — Git Bash
![Terraform Apply](screenshots/01-terraform-apply.png)

### VPC Created — AWS Console
![VPC Console](screenshots/02-vpc-console.png)

### 15 Subnets Created
![Subnets](screenshots/03-subnets.png)

### Route Tables & Internet Gateway
![Route Tables](screenshots/04-route-tables.png)

---

## 💡 Key Concepts Learned

**Cloud Networking:**
- VPC design and CIDR block planning
- Public vs Private subnet separation
- Internet Gateway vs NAT Gateway
- Route table associations
- Network ACLs as a subnet-level firewall
- VPC Endpoints for private AWS service access
- High Availability across multiple Availability Zones

**DevOps / IaC:**
- Terraform `init` → `plan` → `apply` → `destroy` workflow
- Terraform module structure (reusable infrastructure components)
- Using `.tfvars` for environment-specific configuration
- Infrastructure as Code vs manual provisioning
- AWS CLI configuration and IAM access management
- Resource tagging for cost tracking and ownership

---

## 🐛 Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| VPC not visible in AWS Console | Checking wrong region | Switch console to `us-west-2` — always match region in `vpc.tfvars` |
| `terraform init` fails | AWS provider not reachable | Check internet connection |
| `terraform apply` fails with auth error | AWS CLI not configured | Run `aws configure` and verify with `aws sts get-caller-identity` |
| NAT Gateway charges | `create_nat_gateway = true` | Set to `false` in `vpc.tfvars` for practice |

---

## 📚 References

- [AWS VPC Design Guide — DevOpsCube](https://devopscube.com/aws-vpc-design/)
- [Terraform AWS VPC Tutorial — DevOpsCube](https://devopscube.com/terraform-aws-vpc/)
- [Original Project Source — techiescamp](https://github.com/techiescamp/terraform-aws)
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

---

## 👤 Author

**Tarun Channapla**
- GitHub: [github.com/Tarunchannapla](https://github.com/Tarunchannapla)
- LinkedIn: *(Add your LinkedIn URL here)*

---

⭐ **If this project helped you, please give it a star!**
