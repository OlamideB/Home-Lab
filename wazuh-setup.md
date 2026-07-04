# Setting Up Wazuh SIEM on Ubuntu Server

## What is Wazuh?

Wazuh is a free open source Security Information 
and Event Management (SIEM) tool. It collects and 
analyzes security logs from all devices on your 
network, detects threats and suspicious activity, 
and sends alerts when something requires attention.

## Why Wazuh?

Wazuh was chosen for this home lab because it:

- Is free and open source
- Provides real world SIEM experience
- Collects logs from multiple devices
- Sends email alerts for suspicious activity
- Is directly relevant to SOC analyst work
- Integrates with Pi-hole for DNS monitoring

## System Requirements

### Host Machine
- 64-bit Operating System
- Minimum 16GB RAM (8GB allocated to Wazuh VM)
- 50GB free disk space
- VirtualBox installed

### Wazuh VM Specifications
- OS: Ubuntu Server 22.04 LTS
- RAM: 8GB
- CPU: 2 cores
- Storage: 50GB
- Network: NAT or Bridged Adapter

## Part 1: Creating the Wazuh VM

A new virtual machine was created in VirtualBox 
with the following specifications:

- **Name:** Wazuh-SIEM
- **OS:** Ubuntu Server 22.04 LTS
- **RAM:** 8GB
- **CPU:** 2 cores
- **Storage:** 50GB
- **Hard Disk Format:** VDI

> Note: For detailed steps on creating a VM in 
> VirtualBox refer to the 
> [Ubuntu VM Installation Guide](ubuntu-vm-install.md)