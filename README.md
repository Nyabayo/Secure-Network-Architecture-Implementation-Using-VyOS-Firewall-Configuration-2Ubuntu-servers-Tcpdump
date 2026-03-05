# Secure Network Architecture Implementation Using VyOS
<img width="960" height="506" alt="image" src="https://github.com/user-attachments/assets/d20bd2b1-ebba-446e-8d26-c8baa75070f9" />

## Overview
This repository contains the implementation of **Network Segmentation** and **Firewall Configuration** using **VyOS** to isolate the **Finance** and **HR** departments at Acme Corp. It involves blocking ping traffic (ICMP), validating network segmentation using **tcpdump**, and ensuring compliance with key security standards like **PCI DSS** and **CIS Benchmarks**.

## Scenario

**Acme Corp**, a growing company handling sensitive financial and human resources data, identified a major security risk: all internal network devices are on a flat network, making it easy for attackers to move laterally between departments. To mitigate this, network segmentation is implemented, dividing the network into two secure zones—**Finance** and **HR**—with strict traffic isolation.

The task involves configuring a **VyOS Router** to create two separate subnets: one for Finance and one for HR. The goal is to prevent unauthorized access between these subnets, ensuring strict data isolation while allowing necessary internal communication. This segmentation reduces the risk of lateral movement if an attacker compromises one system.

### **Tools and Resources Used**
- **VyOS Router**: A network operating system to configure routing and firewall rules.  
  [VyOS Download](https://drive.google.com/file/d/18ujHjtooze9-BFuaeMojGV54EEoHxc5t/view?usp=drive_link)
  
- **tcpdump**: A command-line packet capture tool to monitor and analyze network traffic.
  
- **Ping Command**: A built-in tool to test connectivity between devices.
  
- **Ubuntu Server 24.04 ISO**: This will be used for creating the Finance and HR VMs.  
  [Ubuntu 24.04 Server ISO](https://releases.ubuntu.com/24.04/)

- **VyOS OVA**: The VyOS OVA provided in the course materials for creating the VyOS router VM.

- **Report Template**:  
  [Report Template - Network Segmentation Implementation](https://docs.google.com/document/d/10Tlo4SaB8ONGgiNXQm_cu2k3y5A_mr_iftgAhbgu8T4/copy)

---

## Steps Covered in This Implementation

### Task 1: Download the Required Resources
- Download the necessary files to set up your environment.

  1. Go to the [Ubuntu 24.04 Releases](https://releases.ubuntu.com/24.04/) and download the server 64-bit PC (AMD64) install image.
  2. Access the [VyOS OVA provided in the course materials](https://drive.google.com/file/d/18ujHjtooze9-BFuaeMojGV54EEoHxc5t/view?usp=drive_link).

---

### Task 2: Set Up the Virtual Machines (VMs)

1. **Create the Virtual Machine in VirtualBox**:
   - Open VirtualBox and click **New** to create a new VM.
   - Name the first VM `Finance-VM` (and the second VM `HR-VM`) and select the Ubuntu Server ISO (24.04) as the ISO image.
  
2. **Install Ubuntu Server**:
   - Set RAM to 2 GB and HDD to 80 GB.
   - Keep the default Ubuntu Server.
   - Edit the IPv4 in the Network Configuration and set the method to Manual:
     - **Finance-VM:**
       - Subnet: `192.168.1.0/24`
       - Address: `192.168.1.2`
       - Gateway: `192.168.1.1`
     - **HR-VM:**
       - Subnet: `192.168.2.0/24`
       - Address: `192.168.2.2`
       - Gateway: `192.168.2.1`

3. **Change to Internal Networks in VirtualBox**:
   - Shut down both VMs after installation is complete.
   - Open **VirtualBox → Settings → Network** for `Finance-VM` and change **Adapter 1** to **Internal Network** named "Finance".
   - Open **VirtualBox → Settings → Network** for `HR-VM` and change **Adapter 1** to **Internal Network** named "HR".
   - Start the VMs again.

---

### Task 3: Configure the VyOS Router

1. **Import the VyOS OVA into Your Virtualization Tool**:
   - Assign **Network Adapter 1** to **Finance** and **Adapter 2** to **HR**.
   - Start the VyOS VM.
   
2. **Configure Interfaces for Network Segments**:
   - Log into VyOS router (username/password: `vyos/vyos`) and access the VyOS CLI through the console using the command:  
     ```bash
     configure
     ```

3. **Assign IPs and Descriptions**:
   - Assign **eth3** to **Finance Subnet**:
     ```bash
     set interfaces ethernet eth3 address 192.168.1.1/24
     set interfaces ethernet eth3 description 'Finance Subnet'
     ```

   - Assign **eth2** to **HR Subnet**:
     ```bash
     set interfaces ethernet eth2 address 192.168.2.1/24
     set interfaces ethernet eth2 description 'HR Subnet'
     ```

4. **Apply and Save the Configuration**:
   ```bash
   commit
   save
