# Windows Server Lab

A home lab building a Windows Server 2022 domain with Windows 11 clients. Documents the design, configuration, and troubleshooting of a small Active Directory environment.

## Lab environment

- Host: Windows 11 Pro, AMD Ryzen 5 7600X, 32GB DDR5
- Hypervisor: VirtualBox
- Server: Windows Server 2022 Standard Evaluation (Desktop Experience)
- Clients: Windows 11 Pro

## Network design

- Subnet: 192.168.10.0/24
- DC01 (Domain Controller): 192.168.10.10 (static)
- DHCP scope: 192.168.10.100 to 192.168.10.110
- DNS: hosted on DC01, authoritative for lab.local

## Stages

1. [Server Build](./01-server-build/) — VM creation and base OS install
2. [Active Directory](./02-active-directory/) — AD DS install, forest creation, DC promotion
3. [DHCP and DNS](./03-dhcp-dns/) — DHCP scope and options, DNS verification
4. [Clients](./04-clients/) — Windows 11 build and domain join
5. [Group Policy](./05-group-policy/) — OUs, users, groups, GPOs

## Skills demonstrated

- Windows Server 2022 installation and configuration
- Active Directory Domain Services
- DNS and DHCP server setup
- Group Policy management
- Network troubleshooting
- VirtualBox configuration and virtual networking

## Notes

This lab was built from scratch as a learning exercise to refresh skills last used during a college placement. Documentation includes troubleshooting steps and lessons learned where relevant.
