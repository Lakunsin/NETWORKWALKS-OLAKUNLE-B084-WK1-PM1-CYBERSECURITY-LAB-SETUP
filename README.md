# Cybersecurity Lab Environment Setup

## Project Overview
This project documents the complete configuration, deployment, and verification of a dedicated cybersecurity testing lab. The environment is built entirely on local virtualization infrastructure to provide a safe, isolated workspace for security assessment and ethical hacking exercises using VirtualBox and Kali Linux.

## Objectives
- Install and configure Oracle VirtualBox
- Install and configure Kali Linux as a primary security auditing and hacking workstation
- Create a private NAT Network for the cybersecurity lab
- Configure network connectivity for Kali Linux
- Assign a consistent IP address to the Kali Linux Virtual Machine
- Take a VM snapshot for recovery
- Validate network stability, internet connectivity, and execute a full system update and upgrade
- Enable Clipboard & File Drag-and-Drop
- Enable Shared Folders with Host system

## Purpose of the Lab
The purpose of this lab is to build a reliable, repeatable, and isolated environment for practicing ethical hacking methodologies, analyzing network traffic, testing security tools, and performing administrative tasks without risking host system integrity or exposing the primary operating system to experimental security configurations.

> **NOTE:** This lab is for educational purposes only.

## Lab Architecture
- **Host System:** Local laptop running the primary operating system and hosting the virtualization software.
- **Hypervisor:** Oracle VirtualBox managing virtual hardware, storage controllers, and network switches.
- **Guest Operating System:** Kali Linux installed on a virtual hard disk with a dedicated static IP address (`10.0.0.2/24`).
- **Network Topology:** A private NAT Network (`10.0.0.0/24`) routing traffic through the host gateway (`10.0.0.1`) with external internet access via public DNS (`8.8.8.8`).
- **Resource Sharing:** A direct folder bridge mapping the host's `/downloads` directory into the Kali Linux environment.

## Lab Configuration

| Components | Configuration |
| :--- | :--- |
| **Host OS** | Windows 11 |
| **Host RAM** | 16 GB |
| **Processor** | Intel Core i5 |
| **Hypervisor** | Oracle VirtualBox 7.2.6 |
| **Guest OS** | Kali Linux 2026.3 |
| **Kali RAM** | 4096 MB |
| **Virtual Disk Size** | 50 GB |
| **Virtual Network** | NAT Network |
| **Network Address** | 10.0.0.0/24 |
| **Kali IP Address** | 10.0.0.2/24 |
| **Default Gateway** | 10.0.0.1 |
| **DNS Server** | 8.8.8.8 |

## Lab Setup Procedure

### Step 1: Host Prerequisites & BIOS Configuration
Hardware virtualization (Intel VT-x or AMD-V) was enabled in the BIOS/UEFI while Windows Hyper-V was disabled to prevent hypervisor conflicts, ensuring third-party tools like VirtualBox run smoothly.

### Step 2: Install VirtualBox
Oracle VirtualBox 7.2.6 was downloaded and installed.

### Step 3: Created a NAT Network
Configured a custom VirtualBox NAT Network named `NATNetworkwalks` with the IPv4 prefix `10.0.0.0/24`. DHCP was enabled. The NAT Network was selected to enable inter-VM communication and outbound internet access, laying the groundwork for integrated attacker-target testing within the lab.

### Step 4: Kali Linux Installation
Downloaded 7-Zip and the Kali Linux installer ISO. 7-Zip was installed and used to extract/manage files. Ran the Kali Linux graphical installation wizard, configured user accounts, allocated 4096 MB RAM, partitioned a 50 GB virtual hard disk, and completed the base OS setup.

### Step 5: Configure the Kali Linux Network
The Kali Linux network configuration was set up with a static IPv4 address to ensure consistency during lab sessions:
- **Address:** `10.0.0.2`
- **Netmask:** `24`
- **Gateway:** `10.0.0.1`
- **DNS Servers:** `8.8.8.8`

### Step 6: Took a VM Snapshot
Captured a working state snapshot of the virtual machine to serve as a reliable recovery point if lab testing causes system instability.

### Step 7: Clipboard & File Drag-and-Drop
Enabled bidirectional Clipboard & File Drag-and-Drop in VM settings.

### Step 8: Shared Folders with Host System
Enabled a shared folder in VirtualBox pointing to the host's `/downloads` path with Auto-mount enabled, generating the desktop mount point (`sf_downloads`).

## Lab Verification

| Test | Command | Expected Result |
| :--- | :--- | :--- |
| **Checked IP address** | `ip a` | Displayed the correct Kali IP |
| **Test internet connectivity** | `ping -c 4 8.8.8.8` | 0% packet loss, successful |
| **Test gateway** | `ping -c 4 10.0.0.1` | 0% packet loss, successful |

## Problems Encountered & Solutions
After manually configuring static IPv4 settings, internet connectivity failed. Reconfigured and restarted the network connection using NetworkManager:  

sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0  
sudo nmcli connection down "Wired connection 1"  
sudo nmcli connection up "Wired connection 1"

## Key Takeaways

1. **NAT vs. NAT Network**  
   A standard NAT configuration and a NAT Network serve different purposes. I configured a custom NAT Network to allow future inter-VM communication and external connectivity, establishing the foundation for a multi-machine cybersecurity laboratory.

2. **Virtual Machine Networking**  
   I learned how VirtualBox virtual network adapters connect virtual machines to custom subnets (`10.0.0.0/24`) and how correct network configuration ensures seamless communication between the host gateway and the guest machine.

3. **Static IP Configuration**  
   I configured and verified static IPv4 addressing (`10.0.0.2/24`), subnet masks, gateways (`10.0.0.1`), and DNS settings (`8.8.8.8`) in Kali Linux, while successfully troubleshooting and resolving timeouts using NetworkManager.

4. **VM Snapshots**  
   I captured a clean baseline snapshot of the virtual machine after initial setup, providing a reliable recovery point for future cybersecurity testing and lab exercises.

5. **Documentation**  
   I documented commands, configurations, encountered problems, and exact solutions (such as resolving the DAD timeout), recognizing its vital role in a professional cybersecurity project.

6. **Hypervisor Foundation & Resource Allocation**  
   - **Hardware Virtualization:** Ensuring Intel VT-x/AMD-V is enabled and Windows Hyper-V is disabled prevents hypervisor resource conflicts and performance bottlenecks.
   - **Resource Balancing:** Allocated optimal CPU and RAM to keep the host responsive to run security tools.

7. **Host-Guest Integration & Workflow**  
   - **Seamless Interoperability:** Enabled bidirectional clipboard and drag-and-drop to eliminate friction when moving text and files between the host and Kali.
   - **Shared Storage:** Mapped the host `/downloads` directory (`sf_downloads`) to streamline importing wordlists, scripts, and tools.

## Tools & Resources
- [7-Zip](https://7-zip.org/download.html)
- [VirtualBox](https://virtualbox.org/wiki/Downloads)
- [Kali Linux](https://www.kali.org/get-kali/#kali-installer-images)

## Author
**Olakunle Odesanya**  

**Role:** Aspiring SOC Analyst  
**Github:** <https://github.com/Lakunsin>  
**LinkedIn:** <https://www.linkedin.com/in/olakunle-o-04a51a257>

## Project Information
**Program Name:** Cybersecurity at Networkwalks | **Week:** 01**Project:** Cybersecurity & Pentesting Lab Setup **Repository:** GitHub
