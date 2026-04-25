# Scalable-Web-Infrastructure-on-Microsoft-Azure
Project Overview
This project demonstrates the end-to-end implementation of a secure and segmented cloud networking environment on Microsoft Azure. The architecture focuses on multi-tier network isolation, secure traffic management, and scalable compute provisioning in the Canada Central region.

Key Technical Milestones
Infrastructure Foundation: Provisioned a dedicated resource group and a Virtual Network (VNet) with a /16 address space.

Network Segmentation: Designed and implemented three distinct subnets (web-subnet, app-subnet, and db-subnet) to ensure logical isolation of workloads.

Security Configuration: Established a Network Security Group (NSG) with granular inbound security rules to permit HTTP traffic.

Compute Deployment: Provisioned a Linux Virtual Machine using Ubuntu 22.04 LTS, optimized via resource availability checks across different Azure SKU series.

Web Server Configuration: Deployed and verified an Nginx web server following secure SSH access establishment.

Repository Structure
/infrastructure: Azure CLI scripts for VNet, Subnets, and NSG creation.

/compute: Scripts and configurations for VM provisioning and SSH key management.

/scripts: Post-deployment configuration scripts (e.g., Nginx installation).

Deployment Steps
1. Networking Setup
Create the core networking components and define the subnets for the three-tier architecture:

Bash
# Create Resource Group
az group create --name rg-hybrid-network --location canadacentral

# Create VNet and Subnets
az network vnet create --name vnet-hybrid --resource-group rg-hybrid-network --address-prefix 10.0.0.0/16 --subnet-name web-subnet --subnet-prefix 10.0.1.0/24
2. Security Configuration
Deploy the Network Security Group to manage traffic flow to the web tier:

Bash
# Create NSG
az network nsg create --resource-group rg-hybrid-network --name nsg-web

# Allow HTTP traffic on Port 80
az network nsg rule create --resource-group rg-hybrid-network --nsg-name nsg-web --name AllowHTTP --priority 100 --destination-port-ranges 80 --access Allow --protocol Tcp
3. Virtual Machine Provisioning
Launch the web server instance using the Standard_D2s_v3 size:

Bash
# Deploy Linux VM
az vm create --resource-group rg-hybrid-network --name vm-web --image Ubuntu2204 --size Standard_D2s_v3 --generate-ssh-keys --vnet-name vnet-hybrid --subnet web-subnet --nsg nsg-web
4. Web Server Installation
Once the VM is accessible via SSH, update the package repository and install Nginx:

Bash
sudo apt update
sudo apt install nginx -y
Tools & Technologies
Cloud Provider: Microsoft Azure

CLI: Azure CLI

Operating System: Ubuntu 22.04 LTS

Web Server: Nginx

Networking: Azure VNet, NSG, Subnets
