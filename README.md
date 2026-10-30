# Kali Linux Virtual Lab Deployment

<p align="center">
  **A practical guide to building a virtual cybersecurity workspace**
</p>

## 01. Project Introduction

This repository documents the preparation of a virtual cybersecurity environment using Oracle VirtualBox and Kali Linux.

The project involved deploying a Kali Linux virtual machine, establishing a dedicated virtual network, and configuring a fixed IPv4 address to maintain consistent communication settings.

The purpose of this initial build was to create a working environment that can support future cybersecurity learning activities and controlled laboratory exercises.

## 02. Project Objectives

The project was guided by the following objectives:

- Prepare a virtualization platform on a Windows host.
- Deploy Kali Linux as the primary laboratory workstation.
- Establish a dedicated NAT Network within VirtualBox.
- Configure a static IPv4 address for the Kali virtual machine.
- Validate external network access and DNS functionality.
- Maintain consistent network settings across system restarts.
- Prepare a reusable environment for future security exercises.

## 03. Purpose and Scope

### Project Purpose

The aim was to establish a virtual workspace where cybersecurity concepts can be explored through practical experimentation.

Using a virtual machine makes it possible to work with a dedicated Linux environment while keeping the laboratory operating separately from the host operating system.

### Scope of This Build

This stage concentrates on:

- Virtualization platform preparation.
- Kali Linux deployment.
- Virtual network creation.
- Static IP configuration.
- Network connectivity testing.
- Preparation of a clean VM baseline.

Activities such as vulnerability scanning, exploitation, penetration testing, and SIEM monitoring are reserved for later projects.

## 04. Virtual Lab Architecture

The environment follows a host-and-guest virtualization model.

A Windows computer serves as the host system, while Oracle VirtualBox manages the Kali Linux guest machine. The guest connects to a custom NAT Network configured within VirtualBox.

### Network Layout
The diagram below illustrates the relationship between the Windows host,
Oracle VirtualBox, the NAT Network, and the Kali Linux virtual machine.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/5568b61a-03f9-4323-bb92-8cb7dfd6b2ae" />

### Network Address Plan

| Component | Configuration |
|---|---|
| Virtual Network | `NatNetwork` |
| Network Range | `10.0.0.0/24` |
| Gateway | `10.0.0.1` |
| Kali IPv4 Address | `10.0.0.2` |
| Subnet Mask | `255.255.255.0` |
| DNS Server | `8.8.8.8` |
| DHCP | Disabled |
| IPv6 | Disabled |

The NAT Network allows connected virtual machines to communicate within the configured virtual network while supporting outbound connectivity. <https://docs.oracle.com/en/virtualization/virtualbox/7.2/user/networkingdetails.html>

## 05. Platform and Environment Details

### Software Components

| Software | Function |
|---|---|
| Windows | Host operating system |
| Oracle VirtualBox | Virtualization and VM management |
| Kali Linux | Laboratory workstation |
| NetworkManager | Linux network configuration |

### Kali Virtual Machine Details

| Parameter | Value |
|---|---|
| Operating System | Kali Linux |
| Virtualization Platform | Oracle VirtualBox |
| Network Adapter | Adapter 1 |
| Network Mode | NAT Network |
| Connected Network | `NatNetwork` |
| Interface | `eth0` |
| MAC Address | `08:00:27:1c:7d:5e` |

## 06. Lab Deployment Procedure

### Step 1. Prepare Oracle VirtualBox

Oracle VirtualBox was installed on the Windows host to provide the virtualization platform required for the laboratory.

It was used to manage the virtual machine and configure its networking environment.

**Tool:** Oracle VirtualBox

**Evidence:**


<img width="746" height="688" alt="image" src="https://github.com/user-attachments/assets/abc8cb3c-eaf7-489d-873b-c1d2936591d4" />


### Step 2. Import the Kali Linux Virtual Machine

The Kali Linux virtual machine was imported into VirtualBox and prepared as the primary workstation for the lab.

After importing the machine, its settings were reviewed to ensure that the correct virtual network could be selected.

**Evidence:**

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/bc3e3165-8588-4a6e-af00-43c6ac7b7ff8" />


### Step 3. Establish the NAT Network

A custom NAT Network was created through the VirtualBox Network Manager.

The network was configured with the following values:

```text
Network Name:  NatNetwork
IPv4 Prefix:   10.0.0.0/24
DHCP Server:   Disabled
IPv6:          Disabled
```

DHCP was disabled because the Kali workstation would use a manually assigned IPv4 address.

**Evidence:**

<img width="746" height="688" alt="image" src="https://github.com/user-attachments/assets/cba21e87-63f7-48d8-a64b-fa091993081e" />


### Step 4. Connect Kali Linux to the Virtual Network

The Kali virtual machine's first network adapter was configured to use the newly created NAT Network.

```text
Adapter:       Adapter 1
Attached To:   NAT Network
Network:       NatNetwork
Cable Status:  Connected
```

This connected the guest operating system to the intended virtual network.

**Evidence:**


<img width="781" height="486" alt="image" src="https://github.com/user-attachments/assets/1a3021f1-dfcb-4c46-8631-ddf438b78225" />



### Step 5. Configure Static Network Addressing

A fixed IPv4 configuration was applied to the Kali Linux network interface using NetworkManager.

The selected parameters were:

```text
Interface:     eth0
IP Address:    10.0.0.2/24
Subnet Mask:   255.255.255.0
Gateway:       10.0.0.1
DNS Server:    8.8.8.8
```

The connection was configured to start automatically, helping maintain the same addressing information after restarting the virtual machine.

**Evidence:**

<img width="1366" height="205" alt="Kali   Running  - Oracle VirtualBox 30_10_2026 15_10_09" src="https://github.com/user-attachments/assets/6c4a4949-ecfa-47f7-9e42-eb64b0471e6c" />



### Step 6. Prepare a Baseline Snapshot

A VirtualBox snapshot will be used to preserve the initial laboratory configuration.

The intended snapshot name is:

```text
Kali - Clean Static Network
```

This baseline will provide a restoration point before future exercises introduce additional software, network changes, or security-testing activities.


## 07. Network Verification

After completing the configuration, connectivity tests were performed to check the assigned IP address and access to external network resources.

### Verification Commands

| Check | Command | Purpose |
|---|---|---|
| Interface details | `ip a show eth0` | Inspect assigned IP address |
| Routing information | `ip route` | Review the default route |
| Gateway reachability | `ping -c 4 10.0.0.1` | Test gateway communication |
| External connectivity | `ping -c 4 8.8.8.8` | Test external IP connectivity |
| DNS functionality | `ping -c 4 google.com` | Check name resolution and connectivity |

### Observed Results

| Test | Result |
|---|---|
| Static IP assignment | Successful |
| External IP connectivity | Successful |
| Domain name resolution | Successful |
| IP persistence after reboot | Confirmed |

The connectivity tests to `8.8.8.8` and `google.com` completed successfully with zero packet loss.

The assigned Kali address remained consistent after restarting the virtual machine.

### Supporting Evidence

**IP Address Verification**

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/7fb0c2dd-58d0-45e8-ac90-6c857156c292" />


**Routing Table**

<img width="1366" height="322" alt="Kali   Running  - Oracle VirtualBox 30_10_2026 15_18_03" src="https://github.com/user-attachments/assets/2bcbe8c9-9212-4bdb-b9f5-8f7a8afb955e" />


**Internet Connectivity**

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/ad48fd6c-f834-4d51-88ec-8d3af5a4718e" />


**DNS Resolution**

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/71d7fdfb-9c6e-4e5a-bd91-3e0d56a9318c" />


## 08. Troubleshooting and Configuration Notes

During the network preparation process, maintaining a consistent IP address required attention to the VirtualBox network configuration and Kali's connection settings.

The following checks were important:

- Confirming that the correct NAT Network was selected.
- Ensuring that the virtual network adapter was connected.
- Checking the interface configuration through `ip a`.
- Reviewing network connection settings through NetworkManager.
- Confirming that external connectivity and DNS resolution were working.

These checks helped establish a consistent network configuration for the virtual machine.

## 09. Lessons and Technical Takeaways

This setup provided practical experience with several infrastructure concepts:

- Understanding the role of a hypervisor in managing guest operating systems.
- Configuring NAT Networks in a virtualization environment.
- Working with IPv4 addressing and subnet masks.
- Applying static network settings through NetworkManager.
- Using command-line tools to inspect network interfaces and connectivity.
- Understanding the importance of maintaining a clean baseline before laboratory exercises.

## 10. Tools and Technologies

- Oracle VirtualBox
- Kali Linux
- Windows
- NetworkManager
- IPv4 networking
- ICMP connectivity testing

## 11. Responsible Laboratory Use

This environment is intended for educational purposes and authorized cybersecurity experimentation.

Future testing activities should be performed only against systems owned by the learner or where explicit permission has been granted.

## 12. Project Status and Future Work

The initial Kali Linux environment and its static network configuration have been established.

Potential next stages include:

- Introducing additional virtual machines.
- Building a dedicated target environment.
- Practising network discovery and service enumeration.
- Conducting controlled vulnerability assessments.
- Exploring defensive monitoring and security analysis.

## 13. Project Maintainer

**Maintained by:** Amos Mbevo

**Focus:** Cybersecurity and Virtual Lab Development

**Repository:** `NETWORKWALKS-B084F-WK1-PM1-CYBERSECURITY-LAB-SETUP-SAMPLE`
