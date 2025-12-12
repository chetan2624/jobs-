# 🚀 Complete Terraform Guide: From Zero to Hero

> **Your comprehensive guide to mastering Infrastructure as Code with Terraform**

## 📑 Table of Contents

1. [What is Terraform?](#what-is-terraform)
2. [Why Terraform?](#why-terraform)
3. [Core Concepts](#core-concepts)
4. [Terraform Architecture](#terraform-architecture)
5. [Installation & Setup](#installation--setup)
6. [Terraform Configuration Language (HCL)](#terraform-configuration-language-hcl)
7. [Terraform Workflow](#terraform-workflow)
8. [State Management](#state-management)
9. [Variables & Outputs](#variables--outputs)
10. [Data Sources](#data-sources)
11. [Modules](#modules)
12. [Provisioners](#provisioners)
13. [Workspaces](#workspaces)
14. [Backend Configuration](#backend-configuration)
15. [Best Practices](#best-practices)
16. [Advanced Topics](#advanced-topics)
17. [Common Interview Questions](#common-interview-questions)

---

## What is Terraform?

**Terraform** is an open-source **Infrastructure as Code (IaC)** tool created by HashiCorp. It allows you to define, provision, and manage infrastructure using declarative configuration files written in HashiCorp Configuration Language (HCL).

### Simple Analogy
Think of Terraform as a **blueprint system for buildings**. Instead of manually building each room (server), wiring (network), and plumbing (databases), you create a detailed blueprint (code), and Terraform automatically builds everything according to your specifications.

### What Problem Does It Solve?

**Traditional Way (Manual):**
- Login to AWS Console
- Click buttons to create EC2 instance
- Manually configure security groups
- Set up load balancers
- Repeat for 100s of resources
- **Result:** Time-consuming, error-prone, not reproducible

**Terraform Way (Automated):**
- Write infrastructure in code files
- Run `terraform apply`
- **Result:** Entire infrastructure created in minutes, reproducible, version-controlled

---

## Why Terraform?

### 1. **Infrastructure as Code (IaC)**
Your infrastructure becomes code files that can be:
- Version controlled (Git)
- Reviewed (Pull Requests)
- Shared across teams
- Reused multiple times

### 2. **Multi-Cloud Support**
Unlike CloudFormation (AWS only) or ARM templates (Azure only), Terraform works with:
- AWS
- Azure
- Google Cloud
- Kubernetes
- VMware
- Over 1000+ providers

**Real-World Example:** Your company wants to run workloads on both AWS and Azure. With Terraform, you use the same tool and workflow for both clouds.

### 3. **Declarative Syntax**
You declare **what** you want, not **how** to create it.

```hcl
# You write this
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}

# Terraform figures out HOW to create it
```

### 4. **Idempotency**
Running the same Terraform code multiple times produces the same result. If a resource exists, Terraform won't recreate it.

### 5. **Plan Before Apply**
Terraform shows you what will change **before** making changes (like a preview).

---

## Core Concepts

### 1. **Resources**
The most fundamental unit in Terraform. A resource represents an infrastructure component.

```hcl
resource "aws_instance" "my_server" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "MyWebServer"
  }
}
```

**Breaking it down:**
- `resource`: Keyword to define a resource
- `"aws_instance"`: Resource type (EC2 instance from AWS provider)
- `"my_server"`: Resource name (your custom identifier)
- Everything in `{}`: Resource configuration

**Real-World Example:** Just like ordering a pizza, you specify: type (aws_instance = pizza), size (t2.micro = medium), toppings (ami, tags = your preferences).

### 2. **Providers**
Providers are plugins that interact with APIs of cloud platforms or services.

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  features {}
}
```

**Why Needed?** Terraform is just a framework. Providers are the translators that convert your Terraform code into actual API calls to AWS, Azure, etc.

### 3. **State**
Terraform stores information about your infrastructure in a **state file** (`terraform.tfstate`).

**What's in the State File?**
- Current infrastructure configuration
- Resource IDs
- Metadata
- Resource dependencies

**Why Important?** Terraform compares your code with the state file to determine what needs to be created, updated, or destroyed.

**Analogy:** State file is like a blueprint tracker that knows what's already built vs. what needs to be built.

### 4. **Plan**
A preview of changes Terraform will make.

```bash
terraform plan
```

Shows:
- `+` Resources to be created
- `-` Resources to be destroyed
- `~` Resources to be modified

### 5. **Apply**
Executes the planned changes.

```bash
terraform apply
```

---

## Terraform Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Your Terraform Code                      │
│                      (.tf files in HCL)                      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │   Terraform Core     │
                │  (Graph, State Mgmt) │
                └──────────┬───────────┘
                           │
        ┏━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━━┓
        ▼                                      ▼
┌───────────────────┐              ┌───────────────────┐
│  AWS Provider     │              │  Azure Provider   │
│  (AWS API Calls)  │              │ (Azure API Calls) │
└────────┬──────────┘              └────────┬──────────┘
         │                                  │
         ▼                                  ▼
┌─────────────────┐                ┌─────────────────┐
│   AWS Cloud     │                │   Azure Cloud   │
│  (Your Infra)   │                │  (Your Infra)   │
└─────────────────┘                └─────────────────┘
```

**How It Works:**
1. You write `.tf` files
2. Terraform Core reads your configuration
3. Core creates a dependency graph
4. Providers execute API calls to create actual resources
5. State is updated with created resource information

---

## Installation & Setup

### Installation

**Linux/Mac:**
```bash
# Download Terraform
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip

# Unzip
unzip terraform_1.6.0_linux_amd64.zip

# Move to PATH
sudo mv terraform /usr/local/bin/

# Verify
terraform version
```

**Windows:**
Download from [terraform.io](https://www.terraform.io/downloads) and add to PATH.

### First Project Setup

```bash
# Create project directory
mkdir terraform-demo
cd terraform-demo

# Create main configuration file
touch main.tf
```

---

## Terraform Configuration Language (HCL)

HCL (HashiCorp Configuration Language) is human-readable and declarative.

### Basic Syntax

```hcl
# Comments start with #

# Block Structure
<BLOCK_TYPE> "<BLOCK_LABEL>" "<BLOCK_LABEL>" {
  # Arguments
  key = value
  
  # Nested block
  nested_block {
    setting = "value"
  }
}
```

### Arguments
```hcl
ami           = "ami-12345678"
instance_type = "t2.micro"
count         = 3
```

### Expressions

**String Interpolation:**
```hcl
name = "server-${var.environment}"
# Result: server-production
```

**Arithmetic:**
```hcl
count = var.instance_count * 2
```

**Conditional:**
```hcl
instance_type = var.environment == "prod" ? "t2.large" : "t2.micro"
```

**Lists:**
```hcl
availability_zones = ["us-east-1a", "us-east-1b"]
```

**Maps:**
```hcl
tags = {
  Name        = "WebServer"
  Environment = "Production"
}
```

### Functions

Terraform has 100+ built-in functions:

```hcl
# String functions
upper("hello")           # "HELLO"
lower("HELLO")           # "hello"
join(", ", ["a", "b"])   # "a, b"

# Numeric functions
max(5, 12, 9)            # 12
min(5, 12, 9)            # 5

# Collection functions
length(["a", "b", "c"])  # 3
concat([1, 2], [3, 4])   # [1, 2, 3, 4]

# File functions
file("config.json")      # Reads file content
```

---

## Terraform Workflow

The standard workflow has **4 main commands**:

### 1. `terraform init`

**Purpose:** Initialize a Terraform working directory.

**What it does:**
- Downloads provider plugins
- Sets up backend configuration
- Initializes modules

```bash
terraform init
```

**Output:**
```
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 4.0"...
- Installing hashicorp/aws v4.67.0...
```

**When to run:** 
- First time in a new project
- After adding new providers
- After modifying backend configuration

### 2. `terraform plan`

**Purpose:** Preview changes before applying them.

```bash
terraform plan
```

**Output:**
```
Terraform will perform the following actions:

  # aws_instance.web will be created
  + resource "aws_instance" "web" {
      + ami           = "ami-12345678"
      + instance_type = "t2.micro"
      ...
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

**Analogy:** Like checking your shopping cart before clicking "Buy".

### 3. `terraform apply`

**Purpose:** Create/update infrastructure.

```bash
terraform apply
```

**What happens:**
1. Shows plan
2. Asks for confirmation (`yes`)
3. Creates resources
4. Updates state file

**Auto-approve (use carefully):**
```bash
terraform apply -auto-approve
```

### 4. `terraform destroy`

**Purpose:** Delete all resources managed by Terraform.

```bash
terraform destroy
```

**Use case:** Clean up test environments, remove old infrastructure.

### Additional Useful Commands

```bash
# Format code nicely
terraform fmt

# Validate syntax
terraform validate

# Show current state
terraform show

# List resources in state
terraform state list

# Get specific output
terraform output
```

---

## State Management

### What is State?

The state file (`terraform.tfstate`) is a **JSON file** that maps your Terraform configuration to real-world resources.

**Example State File:**
```json
{
  "version": 4,
  "resources": [
    {
      "type": "aws_instance",
      "name": "web",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "attributes": {
            "id": "i-1234567890abcdef",
            "ami": "ami-12345678",
            "instance_type": "t2.micro"
          }
        }
      ]
    }
  ]
}
```

### Why State is Critical

**Without State:**
- Terraform wouldn't know what resources it created
- Every `apply` would try to create new resources
- No way to track dependencies
- Can't determine what changed

**With State:**
- Terraform knows what exists
- Can update existing resources
- Tracks dependencies
- Enables collaboration

### State Locking

**Problem:** Two people run `terraform apply` simultaneously.

**Solution:** State locking prevents concurrent modifications.

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"  # Enables locking
  }
}
```

### Local vs Remote State

**Local State (Default):**
- Stored on your computer
- `terraform.tfstate` file
- ❌ Not suitable for teams
- ❌ No locking
- ✅ Simple for learning

**Remote State (Production):**
- Stored in cloud (S3, Azure Blob, Terraform Cloud)
- ✅ Team collaboration
- ✅ State locking
- ✅ Encryption
- ✅ Versioning

**Example Remote Backend:**
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "global/s3/terraform.tfstate"
    region = "us-east-1"
    
    # Enable state locking
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```

### State Commands

```bash
# List all resources
terraform state list

# Show specific resource
terraform state show aws_instance.web

# Remove resource from state (doesn't delete actual resource)
terraform state rm aws_instance.web

# Move resource to different name
terraform state mv aws_instance.web aws_instance.app

# Pull remote state to local
terraform state pull

# Push local state to remote
terraform state push
```

---

## Variables & Outputs

### Input Variables

Variables make your code reusable and flexible.

**Defining Variables:**

```hcl
# variables.tf
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 1
}

variable "enable_monitoring" {
  description = "Enable CloudWatch monitoring"
  type        = bool
  default     = false
}

variable "availability_zones" {
  description = "List of AZs"
  type        = list(string)
  default     = ["us-east-1a", "us-east-1b"]
}

variable "tags" {
  description = "Resource tags"
  type        = map(string)
  default     = {
    Environment = "dev"
    Project     = "demo"
  }
}
```

**Using Variables:**

```hcl
# main.tf
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = var.instance_type
  count         = var.instance_count
  
  tags = var.tags
}
```

### Variable Types

```hcl
type = string        # "hello"
type = number        # 42
type = bool          # true/false
type = list(string)  # ["a", "b", "c"]
type = set(string)   # ["a", "b"] (unique values)
type = map(string)   # {key = "value"}
type = object({      # Complex structure
  name = string
  age  = number
})
type = tuple([string, number])  # ["text", 42]
type = any          # Any type
```

### Providing Variable Values

**Method 1: Command Line**
```bash
terraform apply -var="instance_type=t2.large" -var="instance_count=3"
```

**Method 2: Variable File (terraform.tfvars)**
```hcl
# terraform.tfvars
instance_type  = "t2.large"
instance_count = 3
tags = {
  Environment = "production"
  Owner       = "DevOps Team"
}
```

```bash
terraform apply  # Automatically loads terraform.tfvars
```

**Method 3: Custom Variable File**
```bash
terraform apply -var-file="prod.tfvars"
```

**Method 4: Environment Variables**
```bash
export TF_VAR_instance_type="t2.large"
terraform apply
```

### Variable Validation

```hcl
variable "instance_type" {
  type = string
  
  validation {
    condition     = contains(["t2.micro", "t2.small", "t2.medium"], var.instance_type)
    error_message = "Instance type must be t2.micro, t2.small, or t2.medium."
  }
}
```

### Output Values

Outputs display information after `terraform apply` and can be used by other configurations.

```hcl
# outputs.tf
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}

output "public_ip" {
  description = "Public IP address"
  value       = aws_instance.web.public_ip
}

output "instance_url" {
  description = "Full URL"
  value       = "http://${aws_instance.web.public_ip}:8080"
}

output "sensitive_data" {
  description = "Database password"
  value       = aws_db_instance.main.password
  sensitive   = true  # Won't display in logs
}
```

**Using Outputs:**
```bash
terraform output
terraform output instance_id
terraform output -json  # JSON format
```

**Real-World Use Case:** After creating infrastructure, output the load balancer URL so developers know where to deploy applications.

---

## Data Sources

Data sources allow you to **fetch information** about existing resources **not managed by Terraform**.

### Why Use Data Sources?

**Scenario:** You need to deploy an EC2 instance in an existing VPC created by another team.

**Without Data Source:** You manually look up VPC ID and hardcode it.

**With Data Source:** Terraform automatically fetches the VPC ID.

### Examples

**Fetch latest Ubuntu AMI:**
```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]  # Canonical (Ubuntu)

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id  # Use fetched AMI
  instance_type = "t2.micro"
}
```

**Fetch existing VPC:**
```hcl
data "aws_vpc" "existing" {
  tags = {
    Name = "production-vpc"
  }
}

resource "aws_subnet" "app" {
  vpc_id     = data.aws_vpc.existing.id  # Use existing VPC
  cidr_block = "10.0.1.0/24"
}
```

**Fetch availability zones:**
```hcl
data "aws_availability_zones" "available" {
  state = "available"
}

resource "aws_subnet" "public" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  availability_zone = data.aws_availability_zones.available.names[count.index]
}
```

### Resource vs Data Source

```hcl
# Resource = CREATE/MANAGE infrastructure
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

# Data Source = READ existing infrastructure
data "aws_vpc" "existing" {
  id = "vpc-12345678"
}
```

---

## Modules

Modules are **reusable packages** of Terraform configuration. Think of them as functions in programming.

### Why Modules?

**Without Modules:**
```
project/
  ├── main.tf (1000 lines)
  ├── variables.tf (200 lines)
  └── outputs.tf (100 lines)
```
- Hard to maintain
- Code duplication across projects
- Difficult to test

**With Modules:**
```
project/
  ├── main.tf (50 lines - calls modules)
  └── modules/
      ├── networking/
      ├── compute/
      └── database/
```
- Organized
- Reusable
- Easy to test

### Creating a Module

**Module Structure:**
```
modules/
  └── web-server/
      ├── main.tf       # Resources
      ├── variables.tf  # Input variables
      └── outputs.tf    # Output values
```

**modules/web-server/main.tf:**
```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  
  tags = {
    Name = var.server_name
  }
}

resource "aws_security_group" "web" {
  name = "${var.server_name}-sg"
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

**modules/web-server/variables.tf:**
```hcl
variable "ami_id" {
  type = string
}

variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "server_name" {
  type = string
}
```

**modules/web-server/outputs.tf:**
```hcl
output "instance_id" {
  value = aws_instance.web.id
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

### Using a Module

**main.tf:**
```hcl
module "frontend_server" {
  source = "./modules/web-server"
  
  ami_id        = "ami-12345678"
  instance_type = "t2.small"
  server_name   = "frontend"
}

module "backend_server" {
  source = "./modules/web-server"
  
  ami_id        = "ami-12345678"
  instance_type = "t2.medium"
  server_name   = "backend"
}

# Access module outputs
output "frontend_ip" {
  value = module.frontend_server.public_ip
}
```

### Module Sources

**Local Path:**
```hcl
module "vpc" {
  source = "./modules/vpc"
}
```

**Terraform Registry:**
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"
}
```

**GitHub:**
```hcl
module "vpc" {
  source = "github.com/terraform-aws-modules/terraform-aws-vpc"
}
```

**Git with specific branch:**
```hcl
module "vpc" {
  source = "git::https://github.com/org/repo.git?ref=v1.2.0"
}
```

### Real-World Example

**Creating standardized infrastructure across environments:**

```hcl
# Development environment
module "dev_infrastructure" {
  source = "./modules/full-stack"
  
  environment   = "dev"
  instance_type = "t2.micro"
  db_size       = "db.t3.small"
}

# Production environment
module "prod_infrastructure" {
  source = "./modules/full-stack"
  
  environment   = "prod"
  instance_type = "t2.large"
  db_size       = "db.r5.xlarge"
}
```

---

## Provisioners

Provisioners execute scripts or commands on resources **after creation**. They're used as a **last resort**.

### Types of Provisioners

#### 1. **local-exec** (Runs on your machine)

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  provisioner "local-exec" {
    command = "echo ${self.private_ip} >> private_ips.txt"
  }
}
```

**Use Case:** Save instance details to a local file for documentation.

#### 2. **remote-exec** (Runs on the remote resource)

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  key_name      = "my-key"
  
  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip
  }
  
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]
  }
}
```

**Use Case:** Install software immediately after instance creation.

#### 3. **file** (Upload files)

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  connection {
    type = "ssh"
    user = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
    host = self.public_ip
  }
  
  provisioner "file" {
    source      = "app.conf"
    destination = "/tmp/app.conf"
  }
  
  provisioner "remote-exec" {
    inline = [
      "sudo mv /tmp/app.conf /etc/app/app.conf"
    ]
  }
}
```

### Creation-Time vs Destroy-Time

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  # Runs when resource is created
  provisioner "local-exec" {
    command = "echo 'Instance created'"
  }
  
  # Runs when resource is destroyed
  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Instance destroyed'"
  }
}
```

### Failure Behavior

```hcl
provisioner "remote-exec" {
  inline = ["sudo apt-get update"]
  
  on_failure = continue  # Options: fail (default), continue
}
```

### ⚠️ Why Provisioners are Last Resort

**Problems:**
- Makes Terraform stateful operations unpredictable
- Hard to debug
- Breaks idempotency
- Not portable across platforms

**Better Alternatives:**
- **User Data** (for cloud-init scripts)
- **Configuration Management** (Ansible, Chef, Puppet)
- **Packer** (pre-baked AMIs)
- **Container Images** (Docker)

**When to Actually Use:**
- One-time setup tasks
- Bootstrapping configuration management
- Temporary workarounds

---

## Workspaces

Workspaces allow you to manage **multiple environments** (dev, staging, prod) using the **same configuration**.

### Default Workspace

Every Terraform project starts with a `default` workspace.

```bash
terraform workspace list
# * default
```

### Creating Workspaces

```bash
# Create dev workspace
terraform workspace new dev

# Create prod workspace
terraform workspace new prod

# List workspaces
terraform workspace list
# * prod
#   dev
#   default

# Switch workspace
terraform workspace select dev
```

### Using Workspaces in Code

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = terraform.workspace == "prod" ? "t2.large" : "t2.micro"
  
  tags = {
    Name        = "web-${terraform.workspace}"
    Environment = terraform.workspace
  }
}

# State file for each workspace
# terraform.tfstate.d/
#   ├── dev/
#   │   └── terraform.tfstate
#   └── prod/
#       └── terraform.tfstate
```

### Real-World Example

```hcl
variable "instance_count" {
  type = map(number)
  default = {
    dev  = 1
    prod = 3
  }
}

resource "aws_instance" "app" {
  count         = var.instance_count[terraform.workspace]
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name        = "app-${terraform.workspace}-${count.index + 1}"
    Environment = terraform.workspace
  }
}
```

**Deployment:**
```bash
# Deploy to dev
terraform workspace select dev
terraform apply  # Creates 1 instance

# Deploy to prod
terraform workspace select prod
terraform apply  # Creates 3 instances
```

### Workspaces vs Multiple Directories

**Workspaces:**
- ✅ Same code for all environments
- ✅ Easy to switch
- ❌ Can accidentally affect wrong environment
- **Use for:** Similar environments with small differences

**Separate Directories:**
- ✅ Complete isolation
- ✅ Different configurations possible
- ❌ Code duplication
- **Use for:** Environments with significant differences

---

## Backend Configuration

Backends determine where Terraform stores its **state** and how operations are executed.

### Local Backend (Default)

```hcl
# State stored in terraform.tfstate file
# No configuration needed
```

### S3 Backend (AWS)

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "prod/vpc/terraform.tfstate"
    region         = "us-east-1"
    
    # Enable state locking
    dynamodb_table = "terraform-state-locks"
    
    # Enable encryption
    encrypt = true
  }
}
```

**Setup Requirements:**
1. Create S3 bucket
2. Create DynamoDB table with `LockID` as primary key
3. Configure backend in Terraform
4. Run `terraform init` to migrate state

### Azure Backend

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-rg"
    storage_account_name = "tfstatestorage"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}
```

### Terraform Cloud Backend

```hcl
terraform {
  cloud {
    organization = "my-org"
    
    workspaces {
      name = "my-workspace"
    }
  }
}
```

**Benefits:**
- ✅ Remote execution
- ✅ State locking
- ✅ Web UI
- ✅ Team collaboration
- ✅ Policy as code

### Partial Backend Configuration

Keep sensitive info out of code:

```hcl
# backend.tf
terraform {
  backend "s3" {
    # bucket, region specified at runtime
  }
}
```

```bash
terraform init \
  -backend-config="bucket=my-bucket" \
  -backend-config="region=us-east-1"
```

Or use a config file:

```hcl
# backend.hcl
bucket = "my-terraform-state"
region = "us-east-1"
```

```bash
terraform init -backend-config=backend.hcl
```

---

## Best Practices

### 1. **Code Organization**

**Project Structure:**
```
terraform-project/
├── main.tf           # Main resources
├── variables.tf      # Input variables
├── outputs.tf        # Output values
├── provider.tf       # Provider configuration
├
