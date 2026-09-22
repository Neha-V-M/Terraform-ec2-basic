# Terraform EC2 Instance with Public IP

A simple Terraform configuration that provisions an AWS EC2 instance in a specified subnet with an auto-assigned public IP address.

## What this does

- Launches an EC2 instance using a configurable AMI and instance type
- Places the instance in a specified subnet
- Associates a public IP address with the instance
- Outputs the instance's public IP after deployment

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/downloads) installed (v1.x recommended)
- An AWS account
- AWS CLI configured with valid credentials (`aws configure`)
- An existing VPC and **public subnet** (with a route to an Internet Gateway) in your target region

## Project structure

```
.
├── main.tf                    # Provider and EC2 instance resource
├── variables.tf                # Input variable declarations
├── outputs.tf                  # Output values (public IP)
├── terraform.tfvars.example    # Example variable values (copy to terraform.tfvars)
└── README.md
```

## Setup

1. Clone this repository:
   ```bash
   git clone <your-repo-url>
   cd <repo-folder>
   ```

2. Copy the example variables file and fill in your own values:
   ```bash
   cp terraform.tfvars.example terraform.tfvars
   ```

3. Edit `terraform.tfvars` with your actual AMI ID, instance type, and subnet ID:
   ```hcl
   ami_value     = "ami-xxxxxxxxxxxxxxxxx"
   instance_type = "t3.micro"
   subnet_id     = "subnet-xxxxxxxxxxxxxxxxx"
   ```

## Usage

Initialize Terraform (downloads the AWS provider):
```bash
terraform init
```

Preview the changes Terraform will make:
```bash
terraform plan
```

Apply the configuration to create the instance:
```bash
terraform apply
```

After it completes, the instance's public IP will be printed as an output. You can also retrieve it anytime with:
```bash
terraform output public_ip_address
```

When you're done, destroy the resources to avoid ongoing charges:
```bash
terraform destroy
```

## Variables

| Name            | Description                          | Type   | Required |
|-----------------|---------------------------------------|--------|----------|
| `ami_value`     | AMI ID to launch the instance from   | string | Yes      |
| `instance_type` | EC2 instance type (e.g. `t3.micro`)  | string | Yes      |
| `subnet_id`     | ID of the subnet to launch into      | string | Yes      |

## Outputs

| Name                 | Description                        |
|----------------------|-------------------------------------|
| `public_ip_address`  | Public IP address of the instance  |

## Notes

- The target subnet must be public (its route table must include a route to an Internet Gateway) for the public IP to be reachable.
- Free Tier eligibility for instance types varies by account — check with:
  ```bash
  aws ec2 describe-instance-types --filters Name=free-tier-eligible,Values=true --query "InstanceTypes[*].InstanceType" --output text
  ```

## License

MIT (or update as needed)
