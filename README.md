# Terraform_Cheatsheet
Terraform is the infrastructure as code tool from HashiCorp. It is a tool for building, changing, and managing infrastructure in a safe, repeatable way. Operators and Infrastructure teams can use Terraform to manage environments with a configuration language called the HashiCorp Configuration Language (HCL) for human-readable, automated deployments.

TO download:
https://www.terraform.io/downloads.html

Download the binary and move it to the executable path - mv ~/Downloads/terraform /usr/local/bin/

>> terraform -h - To check the installation.

main.tf - It contains the configuration of terraform.

terraform init - To initialize the project

terraform apply - To start the provision process with the given config file.

terraform destroy - For destroying the launched instance.

# Terraform with AWS:

Create a folder Aws-Instance-Example

Create an Example coonfig file ==> example.tf

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  profile = "default"
  region  = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"
}
```

Terraform loads all the files with .tf in the working directory.

terraform validate - To check the config is valid or not!.

Use commands - terraform init && teraform apply

terraform show - To inspect current state.

Changing infrastructure:

Change the amazon machine image value in example.tf.

```
resource "aws_instance" "example" {
- ami           = "ami-830c94e3"
+ ami           = "ami-08d70e59c07c61a3a"
  instance_type = "t2.micro"
}
```
We changed the image instance.

terraform apply - Use to apply the recent changes.

As per the execution plan, Terraform first destroyed the existing instance and then created a new one in its place. You can use terraform show again to see the new values associated with this instance.

Destroying infrastructure:

Terraform destroy - Destroys the infrastructure.

Input variables:

variables.tf
```
variable "region"{
  default ="us-west-2"
}
```
example.tf
```
provider "aws" {
  profile = "default"
  region  = var.region
}
```

We can declare and use variables like the above exmaple.

terraform -var="region-ap-south-1" - We can also use the var flag to define variable in command line.

To include variables,we can create a file named sensitive.tfvars.

terraform output ip - It will output the ip address of the instance created.

Store Remotely:

you can also store the state of the instance in terraform cloud.

```
terraform {
  backend "remote" {
    organization = "<ORG_NAME>"

    workspaces {
      name = "Example-Workspace"
    }
  }
}
credentials "app.terraform.io" {
  token = "REPLACE_ME"
}
```
We need to add the generated api token in the above field and the respective organisation and workspace name given during the signup.
