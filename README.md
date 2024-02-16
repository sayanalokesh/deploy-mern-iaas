# deploy-mern-iaas
This project demonstrates how to automate the deployment of a MERN (MongoDB, Express, React, Node.js) application on AWS using Infrastructure as Code (Terraform) and configuration management (Ansible).

# Terraform Infrastructure Setup Documentation

## Table of Contents
1. [AWS Setup and Terraform Initialization](#1-aws-setup-and-terraform-initialization)
2. [VPC and Network Configuration](#2-vpc-and-network-configuration)
3. [EC2 Instance Provisioning](#3-ec2-instance-provisioning)
4. [Security Groups and IAM Roles](#4-security-groups-and-iam-roles)
5. [Resource Output](#5-resource-output)

# Installing Terraform on Ubuntu:
Ubuntu:

    # Update apt package index
    ```
    sudo apt update

    ```

    # Install unzip (required to extract Terraform zip)
    ```
    sudo apt install unzip
    
    ```

    # Download the latest version of Terraform for Linux
    ```
    wget https://releases.hashicorp.com/terraform/<VERSION>/terraform_<VERSION>_linux_amd64.zip

    wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip

    ```

    # Unzip the downloaded package
    ```
    unzip terraform_<VERSION>_linux_amd64.zip

    unzip terraform_1.6.6_linux_amd64.zip

    ```

    # Move the Terraform binary to /usr/local/bin
    ```
    sudo mv terraform /usr/local/bin/
    
    ```

    # Verify installation
    ```
    terraform --version

    ```

Replace <VERSION> with the latest version available on the HashiCorp website.

## Part 1: Infrastructure Setup with Terraform

This document outlines the setup and configuration of infrastructure using Terraform on AWS.

### 1. AWS Setup and Terraform Initialization:

- **Configure AWS CLI and authenticate with your AWS account.**
- **Initialize a new Terraform project targeting AWS.**

### 2. VPC and Network Configuration:

- **Create an AWS VPC with two subnets: one public and one private.**
- **Set up an Internet Gateway and a NAT Gateway.**
- **Configure route tables for both subnets.**

### 3. EC2 Instance Provisioning:

- **Launch two EC2 instances: one in the public subnet (for the web server) and another in the private subnet (for the database).**
- **Ensure both instances are accessible via SSH (public instance only accessible from your IP).**

### 4. Security Groups and IAM Roles:

- **Create necessary security groups for web and database servers.**
- **Set up IAM roles for EC2 instances with required permissions.**

### 5. Resource Output:

- **Output the public IP of the web server EC2 instance.**

## Terraform Configuration

```hcl
# Creates an EC2 instance with the default security group
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.32.1"
    }
  }
}

provider "aws" {
  region = "ap-south-1"  # Change to your desired region
}

# VPC and Network Configuration
...

# Security Groups and IAM Roles
...

# Instances Provisioning
...

# IAM Policies
...

# Displaying the Output
output "lokesh_terraform_instance_ip" {
  value = aws_instance.lokesh_terraform.public_ip
}

output "lokesh_database_terraform_instance_ip" {
  value = aws_instance.lokesh_terraform.public_ip
}

