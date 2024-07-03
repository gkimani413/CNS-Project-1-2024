# CNS-Project-1-2024
# Domain Controller and Client Virtual Network Setup

This project demonstrates the setup of a virtual network using VirtualBox, featuring a Domain Controller running Windows Server 2019, Active Directory, and a client machine running Windows 10. The Domain Controller will manage the internal network, while also allowing the client machine to access the internet.

## Project Overview

The project involves the following steps:

1. **Setup the Domain Controller (DC) Virtual Machine**:
    - Install Windows Server 2019 on the DC.
    - Configure two network adapters:
        - One for connecting to the external network (home network/router) for internet access.
        - One for connecting to the internal VirtualBox private network.
    - Assign static IP addresses to the internal network adapter, and allow the external adapter to receive an IP address via DHCP from the home network/router.

2. **Configure Active Directory**:
    - Install Active Directory Domain Services (AD DS) on the DC.
    - Promote the DC to a domain controller and create a new domain.

3. **Setup Network Address Translation (NAT) and Routing**:
    - Configure NAT and routing on the DC to allow clients on the private network to access the internet through the DC.

4. **Setup DHCP on the Domain Controller**:
    - Install and configure DHCP on the DC to assign IP addresses to client machines in the internal network.

5. **Automate User Creation in Active Directory**:
    - Run a PowerShell script on the DC to automatically create 1000 user accounts in Active Directory.

6. **Setup the Client Virtual Machine**:
    - Create a new virtual machine and install Windows 10.
    - Join the Windows 10 client to the domain.
    - Log into the client using one of the created domain accounts.

## Prerequisites

- VirtualBox installed on your host machine.
- Windows Server 2019 ISO.
- Windows 10 ISO.
- PowerShell script for user creation.

## Step-by-Step Instructions

### 1. Setting up the Domain Controller (DC)

1. **Create a new virtual machine in VirtualBox**:
    - Name: `DomainController`
    - Type: `Microsoft Windows`
    - Version: `Windows 2019 (64-bit)`
    - Memory: Allocate appropriate memory (e.g., 4096 MB)
    - Hard disk: Create a virtual hard disk (e.g., 50 GB)

2. **Configure network adapters**:
    - Adapter 1: NAT (for external internet access)
    - Adapter 2: Host-Only Adapter (for internal network)

3. **Install Windows Server 2019**:
    - Attach the Windows Server 2019 ISO and proceed with the installation.
    - Assign static IP address to the internal network adapter.

4. **Configure Active Directory and DNS**:
    - Open Server Manager and add the AD DS role.
    - Promote the server to a domain controller and create a new domain (e.g., `example.local`).

### 2. Configuring NAT and Routing

1. **Enable Routing and Remote Access**:
    - Open Server Manager, add the `Remote Access` role, and configure routing and NAT.

2. **Configure NAT**:
    - Set up NAT on the external network adapter to provide internet access to the internal network.

### 3. Setting up DHCP

1. **Install DHCP Server Role**:
    - Open Server Manager and add the `DHCP` role.
    - Configure a new DHCP scope for the internal network.

### 4. Automating User Creation

1. **Run PowerShell Script**:
    - Create and run a PowerShell script to generate 1000 user accounts in Active Directory.

```powershell
for ($i=1; $i -le 1000; $i++) {
    $username = "user$i"
    New-ADUser -Name $username -AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) -PasswordNeverExpires $true -Enabled $true -Path "OU=Users,DC=example,DC=local"
}

