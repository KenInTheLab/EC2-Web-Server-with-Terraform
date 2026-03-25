# Project 02 — EC2 Web Server with Terraform

![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Terraform](https://img.shields.io/badge/IaC-Terraform-7B42BC?logo=terraform)
![Status](https://img.shields.io/badge/Status-Live%20Demo-brightgreen)

## Overview

Provisions a fully functional Linux web server on AWS EC2 using Terraform. Apache is installed and a custom web page is served automatically on boot via a `user_data` script — zero manual SSH, zero console setup. Everything is defined as code.

## Architecture

```
Browser → Security Group (port 80) → EC2 t2.micro (Apache) → index.html
```

Terraform provisions:
- Security group allowing HTTP (80) and SSH (22)
- EC2 instance using the latest Amazon Linux 2 AMI (dynamic lookup)
- Apache installed and started automatically via `user_data`
- Custom HTML page served on boot

## AWS Services Used

| Service | Purpose |
|---------|---------|
| Amazon EC2 | Virtual machine / web server |
| VPC Security Group | Firewall rules for HTTP + SSH |

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.3.0
- [AWS CLI](https://aws.amazon.com/cli/) configured (`aws configure`)
- AWS account (free tier eligible)

## Repo Structure

```
project-02-ec2-web-server/
├── main.tf          # AMI lookup, security group, EC2 instance, user_data
├── variables.tf     # Region, instance type, tags
├── outputs.tf       # Public IP, website URL, instance ID
├── provider.tf      # AWS provider + Terraform version
└── README.md
```

## Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/project-02-ec2-web-server
cd project-02-ec2-web-server

# 2. Deploy
terraform init
terraform plan
terraform apply -auto-approve

# 3. Wait ~60 seconds for Apache to start
# Then open the URL from the output
```

## Outputs

| Output | Description |
|--------|-------------|
| `public_ip` | EC2 public IP address |
| `website_url` | Direct URL to the running web server |
| `instance_id` | EC2 instance ID |

## Cost

| Resource | Monthly Cost |
|----------|-------------|
| EC2 t2.micro | Free tier (750 hrs/month) |
| Data transfer | ~$0.00 for demo |
| **Total** | **Free tier** |

## Cleanup

```bash
terraform destroy -auto-approve
```

## What This Demonstrates

- EC2 provisioning with Terraform
- Dynamic AMI lookup using `data` source
- Security group design (ingress/egress rules)
- Automated server configuration via `user_data`
- Real-world IaC practices: tagging, variables, outputs

## Production Improvements

- Restrict SSH ingress to a specific CIDR (`your_ip/32`)
- Store Terraform state in S3 with DynamoDB locking
- Add an Elastic IP for a stable public address

---

*Built as part of a Cloud Engineer portfolio — provisioned 100% with Terraform.*
