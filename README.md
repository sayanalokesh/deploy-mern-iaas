# deploy-mern-iaas
This project demonstrates how to automate the deployment of a MERN (MongoDB, Express, React, Node.js) application on AWS using Infrastructure as Code (Terraform) and configuration management (Ansible).

# Terraform Infrastructure Setup Documentation

## Table of Contents
[Part 1: Infrastructure Setup with Terraform](#part-1-infrastructure-setup-with-terraform)
1. [AWS Setup and Terraform Initialization](#1-aws-setup-and-terraform-initialization)
2. [VPC and Network Configuration](#2-vpc-and-network-configuration)
3. [EC2 Instance Provisioning](#3-ec2-instance-provisioning)
4. [Security Groups and IAM Roles](#4-security-groups-and-iam-roles)
5. [Resource Output](#5-resource-output)

[Part 2: Configuration and Deployment with Ansible](#part-2-Configuration-and-deployment-with-ansible)

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
Paste this [code](https://github.com/sayanalokesh/deploy-mern-iaas/blob/main/terraform/main.tf) in main.tf and use the below commands to amend the changes

```
terraform plan
```
```
terraform apply
```
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/4d9fcfc7-60d5-4d2e-9b5d-18f80d8682b0)

### 2. VPC and Network Configuration:
- You can find all the steps in the main.tf file.
- **Create an AWS VPC with two subnets: one public and one private.**

- **Set up an Internet Gateway and a NAT Gateway.**
- **Configure route tables for both subnets.**
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/88d92c74-c03c-46d7-a4c9-e397f3cc24fb)


### 3. EC2 Instance Provisioning:
- You can find all the steps in the main.tf file.
- **Launch two EC2 instances: one in the public subnet (for the web server) and another in the private subnet (for the database).**
- **Ensure both instances are accessible via SSH (public instance only accessible from your IP).**

### 4. Security Groups and IAM Roles:
- You can find all the steps in the main.tf file.
- **Create necessary security groups for web and database servers.**
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/ea32b966-3ca1-4c46-9fc5-80ff4b1e4d99)
- **Set up IAM roles for EC2 instances with required permissions.**
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/539e9d09-8686-4407-a54f-0d0187f4b281)

### 5. Resource Output:

- **Output the public IP of the web server EC2 instance.**
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/41a91dda-a2ee-4922-9e61-031f377d2ad4)

## Part 2: Configuration and Deployment with Ansible

### 1. Ansible Configuration

Configure Ansible to communicate with the AWS EC2 instances.

Create ec2 instance t2.micro which will act as a control node of Ansible from this particular instance, and Ansible will control all other instances.

## Activities

Some activities will be performed on the Control node and some on the worker nodes Installing Ansible on Ubuntu.

### Activities to be done on the Control node

```bash
sudo apt update
sudo apt install openssh-server
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
ansible --version
cd /etc/ansible/
nano hosts
```
In the host file, enter the private IP of the worker nodes and create a group.
```
[Demo]
172.31.40.147 #private IP of the worker nodes
```
```
nano ansible.cfg
```
enter the code below
```
[default]

inventory      = /etc/ansible/hosts
sudo_user      = root
remote_port    = 22
host_key_checking = False
```
Create a root password in all worker nodes using passwd root.
```
passwd root
```
### On Worker nodes
```
nano /etc/ssh/sshd_config
set as shown below

#LoginGraceTime 2m
PermitRootLogin yes

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes

```
### Now on Control Node
```
ssh-keygen  # to generate the public key of the control node
ssh-copy-id root@<private ip of workernode>   # copy the generated key to the worker nodes

```
After all the configuration, let's test it:
```
ansible all -m ping

```
### 2. Web Server Setup

Write an Ansible playbook and you can find the playbook [here](https://github.com/sayanalokesh/deploy-mern-iaas/blob/main/ansible/deployments.yml) to install Node.js and NPM on the web server.

Clone the MERN application repository and install dependencies.

Configure environment variables and start the Node.js application.

Ensure the React frontend communicates with the Express backend.

### 3. Security Hardening

Harden the security by configuring firewalls and security groups. You can find the file [here](https://github.com/sayanalokesh/deploy-mern-iaas/blob/main/ansible/security.yml)

Implement additional security measures as needed (e.g., SSH key pairs, disabling root login).

For connecting Ansible to AWS resources, we need to install the following packages on the Ansible control node.
```
apt install python3-pip
pip install awscli 
pip install boto
pip install boto3
pip install bs4
ansible-galaxy collection install community.aws
ansible-galaxy collection install amazon.aws:==3.3.1 --force
```
![image](https://github.com/sayanalokesh/deploy-mern-iaas/assets/105637305/ea32b966-3ca1-4c46-9fc5-80ff4b1e4d99)
