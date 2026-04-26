# Stage 1: Server Build

## Objective
- Create a Windows Server 2022 VM in VirtualBox and complete the base OS installation  
- Configure foundational services: Active Directory, DHCP, and DNS  
- Prepare the system for promotion to a domain controller  

---

## VM Specifications
- Name: DC01 (designated domain controller)  
- OS: Windows Server 2022 Standard Evaluation (Desktop Experience)  
- RAM: 8192 MB  
- CPU: 3 cores  
- Disk: 65GB VDI (dynamically allocated)  

---

## Steps Completed
- Created the VM in VirtualBox  
- Installed Windows Server 2022 from the official Microsoft evaluation ISO:  
  https://www.microsoft.com/en-gb/evalcenter/evaluate-windows-server-2022  
- Set local Administrator credentials  
- Installed VirtualBox Guest Additions for improved performance and usability  
- Renamed the machine to **DC01**  
- Configured a static IP address on the internal lab network adapter  

---

## Network Configuration
- Adapter 1: NAT (used for internet access, updates, and package installation)  
- Adapter 2: Internal Network (LabNet)  

Internal network configuration:
- IP Address: 192.168.10.10  
- Subnet Mask: 255.255.255.0  
- Default Gateway: Not configured (internal-only network at this stage)  
- Preferred DNS: 127.0.0.1 (loopback address, as the server hosts DNS after AD DS installation)  

---

## Verification
- `hostname` returns **DC01**  
- `ipconfig` confirms correct IP configuration on the LabNet adapter  

---

## Issues Encountered
Initial dual-adapter configuration (NAT + Internal Network) caused connectivity confusion during early setup, including failed ping tests despite valid addressing.

This was later resolved through correct routing design, including the implementation of RRAS NAT and proper DHCP gateway configuration.

---

## Notes
In this lab environment, multiple infrastructure roles (AD DS, DNS, DHCP) are hosted on a single server for simplicity. In production environments, these services are typically distributed across multiple systems for scalability, security, and fault tolerance.

---

## Screenshots


### Network Adapters
