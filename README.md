Project Overview
This project covers two key areas:

VPC Design — Understanding how to design a multi-tier network architecture for a real application stack
VPC Automation — Using Terraform (Infrastructure as Code) to create and manage the entire VPC with a single command

<img width="2048" height="1241" alt="architecture" <img width="953" height="986" alt="Destoy1" src="https://github.com/user-attachments/assets/a07b29cf-a5d3-4655-9a46-226253b41d2f" /><img width="1576" height="312" alt="Subents" src="https://github.com/user-attachments/assets/b864caab-5c6b-461c-8afc-f59daa85e1ab" />

="https://github.com/user-attachments/assets/61039abd-fcdb-4b06-954d-bfbda2432b58" />
The architecture supports a full production application stack:
LayerComponentSubnet TypeWeb/TrafficApplication Load Balancer, Route 53PublicApplicationJava App (Auto Scaling Group)Private - AppDatabaseRDS (MySQL/PostgreSQL)Private - DBCI/CDJenkins, Ansible, TerraformPrivate - ManagementMonitoringPrometheus, Grafana, ConsulPrivate - PlatformSecurityAWS Secrets Manager, NACLsPrivate

🌐 VPC Configuration
ParameterValueCIDR Block10.0.0.0/16Regionus-west-2 (Oregon)Availability Zonesus-west-2a, us-west-2b, us-west-2cTotal Subnets15 (5 types × 3 AZs)Internet Gateway✅ Yes (for public subnets)NAT Gateway✅ Yes (for private subnet outbound)VPC EndpointsS3, CloudWatch, Secrets ManagerNetwork ACLs4 dedicated sets

🗂️ Subnet Architecture
Subnet TypeCIDR RangeAccessPurposePublic (×3)10.0.1.0/24 – 10.0.3.0/24Internet-facingLoad Balancer, Bastion HostApp (×3)10.0.4.0/24 – 10.0.6.0/24Private (via NAT)Java Application ServersDB (×3)10.0.7.0/24 – 10.0.9.0/24Private (isolated)RDS Database InstancesManagement (×3)10.0.10.0/24 – 10.0.12.0/24Private (via NAT)Jenkins, Ansible, CI/CD ToolsPlatform (×3)10.0.13.0/24 – 10.0.15.0/24Private (via NAT)Prometheus, Grafana, Consul

📁 Repository Structure
aws-vpc-terraform/
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

⚙️ Prerequisites
Before running this project, make sure you have:

 AWS Account with IAM user (Access Key + Secret Key)
 AWS CLI installed and configured
 Terraform installed (v1.0+)
 Git installed


🚀 How to Run This Project
Step 1 — Clone the Repository
bashgit clone https://github.com/YOUR_USERNAME/AWS-VPC-DESIGN-AUTOMATION.git
cd AWS-VPC-DESIGN-AUTOMATION
Step 2 — Configure AWS CLI
bashaws configure
# Enter: Access Key ID, Secret Access Key, Region (us-west-2), Output (json)
Step 3 — Edit Variables (Optional)
Open terraform/vars/dev/vpc.tfvars and update values as needed:
hclregion         = "us-west-2"
vpc_cidr_block = "10.0.0.0/16"
owner          = "your-name"
environment    = "dev"
Step 4 — Navigate to Infra folder
bashcd terraform/infra/vpc
Step 5 — Initialize Terraform
bashterraform init
✅ Expected: Terraform has been successfully initialized!
Step 6 — Preview Resources
bashterraform plan -var-file="../../vars/dev/vpc.tfvars"
✅ Expected: List of 40+ resources to be created
Step 7 — Create the VPC
bashterraform apply -var-file="../../vars/dev/vpc.tfvars"
Type yes when prompted.
✅ Expected: Apply complete! Resources: X added
Step 8 — Verify in AWS Console
Go to AWS Console → VPC → us-west-2 region and verify:

Your VPC (10.0.0.0/16) is listed
15 subnets are created
Internet Gateway is attached
Route Tables and NACLs are configured

Step 9 — Cleanup (Avoid AWS Charges!)
bashterraform destroy -var-file="../../vars/dev/vpc.tfvars"

Screenshots:
<img width="1567" height="522" alt="Screenshot 2026-03-04 140040" src="https://github.com/user-attachments/assets/76a6f1b8-c190-4ec1-b510-269fbff54868" />
<img width="1583" height="416" alt="RT&#39;s" src="https://github.com/user-attachments/assets/f0ef9ad7-58e1-4b50-9b01-004a062c4e13" />
<img width="1598" height="611" alt="IGW" src="https://github.com/user-attachments/assets/a7318141-fd1b-408d-a8dc-2f8d304dfda6" />
<img width="802" height="978" alt="Git-TF3" src="https://github.com/user-attachments/assets/6c4b66ef-dda2-43f2-9a46-b1310fd22728" />
<img width="756" height="923" alt="Git-TF2" src="https://github.com/user-attachments/assets/3f5c667a-a01c-4620-b9e5-98ed6d8365c9" />
<img width="822" height="785" alt="Git-TF" src="https://github.com/user-attachments/assets/badffb3e-40b4-482b-aa40-c38d221038a9" />
<img width="1036" height="982" alt="Destroy2" src="https://github.com/user-attachments/assets/fe75fc51-7179-4f25-b900-094de7210677" />
<img width="838" height="987" alt="Destroy" src="https://github.com/user-attachments/assets/80722f89-cda6-41ff-ae62-839d5de50e46" />

Key Concepts Learned
Cloud Networking:

VPC design and CIDR block planning
Public vs Private subnet separation
Internet Gateway vs NAT Gateway
Route table associations
Network ACLs as a subnet-level firewall
VPC Endpoints for private AWS service access
High Availability across multiple Availability Zones

DevOps / IaC:

Terraform init → plan → apply → destroy workflow
Terraform module structure (reusable infrastructure components)
Using .tfvars for environment-specific configuration
Infrastructure as Code vs manual provisioning
AWS CLI configuration and IAM access management
Resource tagging for cost tracking and ownership


