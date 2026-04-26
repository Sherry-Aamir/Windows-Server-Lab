# Stage 1: Server Build

## Objective
Create a Windows Server 2022 VM in VirtualBox and complete the base OS install. Prepare the machine for promotion to a domain controller.

## VM specifications
- Name: DC01 (unique name for domain controller)
- OS: Windows Server 2022 Standard Evaluation (Desktop Experience)
- RAM: 4096 MB
- CPU: 2 cores
- Disk: 60GB VDI, dynamically allocated

## Steps completed
- Created the VM in VirtualBox
- Installed Windows Server 2022 from evaluation ISO
- Set local administrator password
- Installed VirtualBox Guest Additions for clipboard sharing and improved display
- Renamed the machine from default to DC01
- Configured static IP on the internal lab adapter

## Network configuration
- LabNet adapter: 192.168.10.10 / 255.255.255.0
- Default gateway: not set (no gateway on the internal lab network)
- Preferred DNS: 127.0.0.1 (server is its own DNS once AD is promoted)

## Verification
- `hostname` returns DC01
- `ipconfig` shows correct IP on LabNet adapter

## Issues encountered
Initial dual adapter setup (Internal Network plus NAT) caused VirtualBox NAT routing issues, with pings failing despite a valid configuration. Resolved by simplifying the network design.

## Screenshots
(to be added)
