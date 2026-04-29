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
- A single DC is sufficient for a lab environment, but in production a second DC would be deployed for redundancy and replication.

---

## Screenshots

### Installing the AD DS Role

<img width="1299" height="879" alt="1-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/ec2da47d-c423-415e-98e8-276ab1a062b1" />

*Locate onto the server manager and selected the Add Roles and Features menu. *

---

<img width="749" height="533" alt="2-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/0e0c2479-0bb1-49c2-ba4c-00d0f3aedc64" />

*Selected Role-based or the standard option for adding roles to the server*

---

<img width="749" height="531" alt="3-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/d17ac287-421c-4a0e-820d-195e69d6c281" />

* On the Server selection, The local server (DC01) is automatically selected. Click Next to continue.*

---

<img width="750" height="536" alt="4-Server Manager-Installing AD selecting AD, DHCP and DNS" src="https://github.com/user-attachments/assets/28a0f43e-ddac-4c43-9419-b24d3f8ea8f7" />

*Selecting Active Directory Domain Services (AD DS) from the Server Roles list.*

---

<img width="749" height="534" alt="5-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/b0917c7d-67c8-4e95-986d-6710a3775da5" />

*After selecting AD DS, a pop-up appears prompting to add required features. I will click Add features to continue.*

---

<img width="751" height="525" alt="6-Server Manager-Installing AD selecting AD, DHCP and DNS" src="https://github.com/user-attachments/assets/6941164c-90f8-4300-99a0-4f09dfcf1056" />

*I will also select DHCP and DNS which will be configuring later*

---

<img width="752" height="537" alt="7-Server Manager-Installing AD selecting AD, DHCP and DNS" src="https://github.com/user-attachments/assets/9849ed75-4714-457e-83fd-be387af291dc" />

*After selecting these roles, a pop-up appears again prompting to add required features. I will click again onto Add features to continue.*

---

<img width="747" height="532" alt="8-Server Manager-Installing AD selecting AD, DHCP and DNS" src="https://github.com/user-attachments/assets/283d0fac-0443-404e-9c51-074c3ce2ea11" />

*The same will appear for DNS click Add Features to continue*

---

<img width="751" height="528" alt="9-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/d2877c3c-4c03-4480-ac47-3f5ae24b321c" />

*The Server roles have now all been selected as mentioned. I will click on next to continue*

---

<img width="747" height="533" alt="10-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/02806d75-c52f-4a2b-833d-11986f71c489" />


*We will not add anymore features so click next to continue*

---

<img width="747" height="535" alt="11-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/1b7192ec-2c1d-46e3-8bce-ae5e8be1da5b" />

*On the AD DS part of the wizard just click next and continue*
---

<img width="746" height="528" alt="12-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/99590174-e2d3-4e11-90e7-4efe15939ff9" />
*On the DHCP menu click next to continue.*


---

<img width="747" height="530" alt="13-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/7925a631-4616-4447-8901-e4d633d6d91d" />

*Same thing for DNS click next to continue.*

---

<img width="746" height="530" alt="14-Server Manager-Installing AD" src="https://github.com/user-attachments/assets/31b20ab3-9ecd-482b-8e71-220b83e43294" />
*Roles and features successfully installed. The server is now ready for promotion to a domain controller.*

---

<img width="747" height="531" alt="15 - Roles and features successfully installed" src="https://github.com/user-attachments/assets/521194e7-b566-4f56-ad6d-3050b9dfcb68" />



---

### Promoting to Domain Controller

<img width="1830" height="866" alt="16 - AD config from flag yellow icon" src="https://github.com/user-attachments/assets/fc266d7d-cc10-4657-8018-cd6a06eeb53f" />

*Post-deployment configuration notification. Click "Promote this server to a domain controller" from the flag in Server Manager.*

---

<img width="722" height="531" alt="17 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/020eeb8d-8532-47a9-a30a-cd5efa08ffdb" />

*Selecting "Add a new forest" and entering the root domain name `sherry.local`.*

---

<img width="724" height="531" alt="18 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/91f84bf8-568d-4c42-9f53-e11d489de308" />

*Domain Controller Options. Setting functional levels, enabling DNS and Global Catalog, and entering the DSRM password.*

---

<img width="725" height="530" alt="19 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/b59237ee-4a0d-41a2-856e-152b2ea1f634" />

*DNS Options page. The delegation warning can be ignored in a new forest.*

---

<img width="727" height="528" alt="20- Wizard AD DS setup" src="https://github.com/user-attachments/assets/3bcf9991-503c-4498-ba30-8bd560c21511" />

*Additional Options. NetBIOS name auto-populates as `SHERRY`.*

---

<img width="722" height="532" alt="21 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/90dd7a6b-2a47-4e7a-86b2-0704914def94" />

*Paths page. Default locations for database, logs, and SYSVOL are retained.*

---

<img width="721" height="530" alt="22 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/1eedc9a6-f831-43e0-b648-78af1a74976a" />

*Review Options. Final configuration summary.*

---

<img width="724" height="529" alt="23 - Wizard AD DS setup" src="https://github.com/user-attachments/assets/77d65b24-f714-4383-9e5a-11c847d7e417" />

*Prerequisites Check. Warnings are expected and can be ignored in a lab environment.*

---

<img width="725" height="531" alt="24 - Wizard AD DS setup installation" src="https://github.com/user-attachments/assets/de8bb437-2f0f-415e-8524-fa2f6b55b1d0" />

*Promotion in progress. AD DS and DNS are being configured.*

---

<img width="727" height="533" alt="25 - Wizard AD DS setup installed and rebooting on its own" src="https://github.com/user-attachments/assets/8229472c-1dfe-43b9-ad01-c63288ccfee0" />

*Promotion complete. The server reboots automatically.*

---

### Verification

<img width="962" height="876" alt="26 - AD successfully installed and showing login screen" src="https://github.com/user-attachments/assets/493cb8f1-c71f-4a16-aa30-e35d56bed95d" />

*Login screen displays `SHERRY\Administrator`, confirming DC01 is now a domain controller.*
