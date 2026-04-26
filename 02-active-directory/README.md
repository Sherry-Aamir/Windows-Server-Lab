# Stage 2: Active Directory

## Objective
- Install the Active Directory Domain Services (AD DS) role on DC01
- Promote the server to a domain controller
- Create a new forest and domain (sherry.local)
- Verify the domain is functional and operational

---

## Domain Configuration
- Forest root domain: sherry.local
- NetBIOS name: SHERRY
- Forest functional level: Windows Server 2016
- Domain functional level: Windows Server 2016
- DNS: installed automatically as part of AD DS promotion
- Global Catalog: enabled (required on first domain controller)

---

## Steps Completed
- Installed the Active Directory Domain Services role via Server Manager
- Promoted DC01 to a domain controller using the AD DS Configuration Wizard
- Created a new forest with the root domain `sherry.local`
- Set the Directory Services Restore Mode (DSRM) recovery password
- Allowed the server to reboot to complete promotion
- Logged back in using the new domain credentials (SHERRY\Administrator)

---

## Verification
- Login screen displays `SHERRY\Administrator` after reboot, confirming domain membership
- PowerShell `Get-ADDomain` returns full domain details:
  - DNSRoot: sherry.local
  - NetBIOSName: SHERRY
  - PDCEmulator: DC01.sherry.local
  - InfrastructureMaster: DC01.sherry.local

---

## Notes
- DNS is automatically installed and configured as part of AD DS promotion. The DC becomes the authoritative DNS server for `sherry.local`.
- The DSRM password is a recovery credential used only when starting the DC in safe mode for directory repair. It is separate from the standard domain administrator password.
- A single DC is sufficient for a lab environment but in production a second DC would be deployed for redundancy and replication.

---

## Screenshots

### Installing the AD DS Role

(Add screenshots showing Server Manager → Add Roles and Features → Active Directory Domain Services selection and install completion.)

*Caption here.*

---

### Promoting to Domain Controller

(Add screenshots showing the Promote this server to a domain controller wizard, the Add a new forest option, the sherry.local domain name entry, the DSRM password screen, and the Prerequisites Check.)

*Caption here.*

---

### Verification

(Add a screenshot of the SHERRY\Administrator login screen post-reboot, and the `Get-ADDomain` PowerShell output.)

*Caption here.*
