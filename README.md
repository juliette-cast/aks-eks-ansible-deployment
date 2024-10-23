Ansible to deploy EKS and AKS clusters
# AKS & EKS Ansible Deployment

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup and Installation](#setup-and-installation)
- [Usage](#usage)
  - [Deploying EKS Cluster](#deploying-eks-cluster)
  - [Deploying AKS Cluster](#deploying-aks-cluster)
- [Cleaning Up Resources](#cleaning-up-resources)
- [Connecting Clusters to Lens](#connecting-clusters-to-lens)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

Welcome to the **AKS & EKS Ansible Deployment** repository! This project provides a set of Ansible playbooks designed to automate the deployment and management of Kubernetes clusters on both Azure Kubernetes Service (AKS) and Amazon Elastic Kubernetes Service (EKS). By leveraging Infrastructure as Code (IaC) principles, you can efficiently set up, configure, and tear down your Kubernetes environments with ease.

## Features

- **Automated Deployment:** Seamlessly deploy AKS and EKS clusters using Ansible playbooks.
- **Modular Playbooks:** Organized playbooks for creating necessary resources like VPCs, IAM roles, and Kubernetes clusters.
- **Cleanup Scripts:** Easily remove deployed resources to avoid unnecessary costs.
- **Integration with Lens:** Connect your Kubernetes clusters to Lens for visual management and monitoring.
- **Version Control:** Keep track of your infrastructure changes with Git and GitHub.

## Prerequisites

Before you begin, ensure you have met the following requirements:

### General

- **Operating System:** macOS (tested on Catalina and later)
- **Git:** Installed and configured with SSH authentication.
- **Ansible:** Installed on your local machine.

### AWS (EKS)

- **AWS CLI:** Installed and configured with appropriate permissions.
- **`eksctl`:** Installed for managing EKS clusters.
- **IAM Permissions:** Ability to create IAM roles and policies.

### Azure (AKS)

- **Azure CLI:** Installed and logged in to your Azure account.
- **kubectl:** Installed for interacting with Kubernetes clusters.
- **Azure Permissions:** Ability to create resource groups and manage AKS clusters.

### Optional

- **Lens:** Installed for Kubernetes cluster visualization and management.

## Project Structure

Ansible/
├── README.md
├── .gitignore
├── eks/
│   ├── deploy_eks.yml
│   ├── create_iam.yml
│   ├── create_vpc.yml
│   ├── cleanup.yml
│   ├── eks_service_role_policy.json
│   └── ...
├── agent-latest.yaml
└── cleanup.yml

## Run the EKS deployment playbooks:

ansible-playbook create_vpc.yml

ansible-playbook create_iam.yml

ansible-playbook deploy_eks.yml


## Verify the cluster creation:

aws ec2 describe-vpcs --region us-east-2

aws eks describe-cluster --name my-eks-cluster --region us-east-2

## Cleaning Up Resources

ansible-playbook cleanup.yml

## Add to lens

aws eks update-kubeconfig --name your-cluster-name --region your-region


Link to Document: https://castai.atlassian.net/wiki/spaces/~712020360d7d5fb5d743958259ca9f06394a96/pages/edit-v2/2620293262?draftShareId=9fefb3eb-42bd-4d45-9cbb-63c4ecf10dea
