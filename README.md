# Ansible to Deploy EKS and AKS Clusters

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Prerequisites](#prerequisites)
  - [General](#general)
  - [AWS (EKS)](#aws-eks)
  - [Optional](#optional)
- [Project Structure](#project-structure)
- [Setup and Installation](#setup-and-installation)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Configure AWS Credentials](#2-configure-aws-credentials)
  - [3. Install Dependencies](#3-install-dependencies)
  - [4. Set Up IAM Roles](#4-set-up-iam-roles)
  - [5. Create SSH Key Pair](#5-create-ssh-key-pair)
  - [6. Update Variables](#6-update-variables)
- [Usage](#usage)
  - [Deploying EKS Cluster](#deploying-eks-cluster)
  - [Verifying the Cluster Creation](#verifying-the-cluster-creation)
  - [Cleaning Up Resources](#cleaning-up-resources)
  - [Connecting Clusters to Lens](#connecting-clusters-to-lens)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

Welcome to the **AKS & EKS Ansible Deployment** repository! This project provides a set of Ansible playbooks designed to automate the deployment and management of Kubernetes clusters on both Azure Kubernetes Service (AKS) and Amazon Elastic Kubernetes Service (EKS). By leveraging Infrastructure as Code (IaC) principles, you can efficiently set up, configure, and tear down your Kubernetes environments with ease.

## Features

- **Automated Deployment**: Seamlessly deploy AKS and EKS clusters using Ansible playbooks.
- **Modular Playbooks**: Organized playbooks for creating necessary resources like VPCs, IAM roles, and Kubernetes clusters.
- **Cleanup Scripts**: Easily remove deployed resources to avoid unnecessary costs.
- **Integration with Lens**: Connect your Kubernetes clusters to [Lens](https://k8slens.dev/) for visual management and monitoring.
- **Version Control**: Keep track of your infrastructure changes with Git and GitHub.

## Prerequisites

Before you begin, ensure you have met the following requirements:

### General

- **Operating System**: macOS (tested on Catalina and later) or Linux.
- **Git**: Installed and configured with SSH authentication.
  - [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- **Ansible**: Installed on your local machine.
  - [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- **Python Packages**: `boto3`, `botocore`, and `requests`.
  - Install using `pip install boto3 botocore requests`
- **Ansible Collections**: Install `community.aws`.
  - Install using `ansible-galaxy collection install community.aws`

### AWS (EKS)

- **AWS CLI**: Installed and configured with appropriate permissions.
  - [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- **IAM Permissions**: Ability to create IAM roles and policies.
- **AWS Account ID**: Your AWS account ID.
- **CAST AI API Token**: Obtain from your [CAST AI account](https://cast.ai/).
- **kubectl**: Installed for interacting with the EKS cluster.
  - [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Optional

- **Lens**: Installed for Kubernetes cluster visualization and management.
  - [Download Lens](https://k8slens.dev/)

## Project Structure

eks-ansible-deployment/
├── README.md
├── hosts.ini
├── vars.yml
├── eks-trust-policy.json
├── eks-node-trust-policy.json
├── create_vpc.yml
├── deploy_eks.yml
├── deploy_cast_ai.yml
├── cleanup.yml


- **README.md**: This file containing project documentation.
- **hosts.ini**: Ansible inventory file.
- **vars.yml**: Variables file containing configuration parameters.
- **eks-trust-policy.json**: IAM trust policy for the EKS cluster role.
- **eks-node-trust-policy.json**: IAM trust policy for the EKS node group role.
- **create_vpc.yml**: Playbook to create VPC and networking components.
- **deploy_eks.yml**: Playbook to deploy the EKS cluster.
- **deploy_cast_ai.yml**: Playbook to integrate CAST AI components.
- **cleanup.yml**: Playbook to clean up all resources.

## Setup and Installation

### 1. Clone the Repository

Clone the repository to your local machine:

```bash
git clone <repo url>
cd <repo>

2. Configure AWS Credentials (If needed)
Ensure your AWS CLI is configured with credentials that have the necessary permissions.

3. Install Dependencies
##Install the required Ansible collections and Python packages.

##Install Ansible Collection

- ansible-galaxy collection install community.aws

##Install Python Packages

- pip install boto3 botocore requests

##Create the EKS Node Group Role (EKSNodeRole)
File: eks-node-trust-policy.json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

##Run the following commands:

- aws iam create-role --role-name EKSNodeRole --assume-role-policy-document file://eks-node-trust-policy.json

- aws iam attach-role-policy --role-name EKSNodeRole --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy

- aws iam attach-role-policy --role-name EKSNodeRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly

- aws iam attach-role-policy --role-name EKSNodeRole --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy

5. Create SSH Key Pair
##Generate an SSH key pair for EC2 instances.
- aws ec2 create-key-pair --key-name my-ec2-keypair --query 'KeyMaterial' --output text > my-ec2-keypair.pem
- chmod 400 my-ec2-keypair.pem

## Run the EKS deployment playbooks:

- ansible-playbook -i hosts.ini create_vpc.yml
- ansible-playbook -i hosts.ini deploy_eks.yml
- ansible-playbook -i hosts.ini deploy_cast_ai.yml

##Verifying the Cluster Creation

- aws ec2 describe-vpcs --region us-east-2

- aws eks describe-cluster --name <cluster name> --region us-east-2

##Cleaning Up Resources
- ansible-playbook -i hosts.ini cleanup.yml


## Add to lens

- aws eks update-kubeconfig --name your-cluster-name --region your-region



