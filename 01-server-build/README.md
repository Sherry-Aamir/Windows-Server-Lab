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
The initial dual-adapter configuration (NAT plus Internal Network) caused intermittent connectivity issues, including failed ping tests despite valid IP configuration. This is likely linked to VirtualBox NAT behaviour on this host setup. To be revisited in a future rebuild using a single NAT Network adapter, which provides both internet access and inter-VM connectivity within a shared subnet.

---

## Notes
In this lab environment, multiple infrastructure roles (AD DS, DNS, DHCP) are hosted on a single server for simplicity. In production environments, these services are typically distributed across multiple systems for scalability, security, and fault tolerance.

---

## Screenshots


### VirtualBox setup
<img width="801" height="444" alt="VirtualBox-Setup-1" src="https://github.com/user-attachments/assets/a735505e-263a-45e8-8d07-940a60a24f02" />

*Caption here.*

<img width="806" height="440" alt="VirtualBox-Setup-2" src="https://github.com/user-attachments/assets/30ae983d-51db-4be6-9c3b-c1a0596fe9f8" />

*Caption here.*

<img width="799" height="441" alt="VirtualBox-Setup-3" src="https://github.com/user-attachments/assets/d5259355-15b8-441b-be00-037315364cf3" />

*Caption here.*

<img width="798" height="442" alt="VirtualBox-Setup-4" src="https://github.com/user-attachments/assets/1514079b-a809-4606-9ef8-0284c50b2ee0" />

*Caption here.*

<img width="993" height="1030" alt="VirtualBox-Setup-5" src="https://github.com/user-attachments/assets/819ed4ba-3f9f-4b46-9363-367a658a15ed" />

*Caption here.*




### Network Adapter settings



<img width="762" height="479" alt="VirtualBox-Setup-6" src="https://github.com/user-attachments/assets/1f8e2f31-11bc-4b03-9243-805bd786e793" />

*Caption here.*


<img width="764" height="473" alt="VirtualBox-Setup-7" src="https://github.com/user-attachments/assets/09c4853f-88ec-4ec5-b40f-5e8075d45da7" />

*Caption here.*

### Windows Server Installation

<img width="602" height="449" alt="05-Windows-Server-install-edition" src="https://github.com/user-attachments/assets/7d6809e7-c17b-45ef-b157-8a3cef3a33aa" />

*Caption here.*


<img width="602" height="449" alt="06-Windows-Server-install-edition" src="https://github.com/user-attachments/assets/970cfb11-7fa4-4ed0-8ebd-4b2f199e4484" />

*Caption here.*

<img width="602" height="449" alt="07-Windows-Server-install" src="https://github.com/user-attachments/assets/da199515-85bc-49ae-b94b-a73355c6eeb6" />

*Caption here.*

<img width="602" height="447" alt="08-Windows-Server-install" src="https://github.com/user-attachments/assets/82cfade8-851c-4f58-8c31-c4cb10a2b014" />

*Caption here.*

<img width="602" height="448" alt="09-Windows-Server-install" src="https://github.com/user-attachments/assets/e472be13-075d-4214-ac7e-5d28ac92ac05" />

*Caption here.*

<img width="602" height="445" alt="10-Windows-Server-install" src="https://github.com/user-attachments/assets/2fd8c4c8-9bb8-4af8-b845-98bb612efcdf" />

*Caption here.*

<img width="1023" height="765" alt="11-Windows-Server-install" src="https://github.com/user-attachments/assets/4cadf19c-a0b9-4d2a-a0f9-7cb06a87bee9" />

*Caption here.*

<img width="681" height="424" alt="12-Windows-Setup" src="https://github.com/user-attachments/assets/b5f8fe7d-99c7-4878-bc8a-0d76a80c92a6" />

*Caption here.*



