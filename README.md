# deploy-mern-iaas
This project demonstrates how to automate the deployment of a MERN (MongoDB, Express, React, Node.js) application on AWS using Infrastructure as Code (Terraform) and configuration management (Ansible).

# Terraform Infrastructure Setup Documentation

## Table of Contents
1. [AWS Setup and Terraform Initialization](#1-aws-setup-and-terraform-initialization)
2. [VPC and Network Configuration](#2-vpc-and-network-configuration)
3. [EC2 Instance Provisioning](#3-ec2-instance-provisioning)
4. [Security Groups and IAM Roles](#4-security-groups-and-iam-roles)
5. [Resource Output](#5-resource-output)

## Part 1: Infrastructure Setup with Terraform

This document outlines the setup and configuration of infrastructure using Terraform on AWS.

### 1. AWS Setup and Terraform Initialization:

- **Configure AWS CLI and authenticate with your AWS account. Below command is to install on Ubuntu machine**
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
```
Use the below command to authenticate AWS via CLI
```
aws configure
```
- **Initialize a new Terraform project targeting AWS.**
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

Creat a new directory using the below commands
```
mkdir terraform
cd terraform
touch main.tf
```
Initialize the Terraform usign the below command
```
terraform init

```
Paste this [code](main.tf) in main.tf and use the below commands to amend the changes

```
terraform plan
```
```
terraform apply
```
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
