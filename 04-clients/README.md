# Stage 4: Client Build and Domain Join

## Objective
- Build two Windows 11 client VMs in VirtualBox
- Install Windows 11 Pro on both clients
- Verify the clients pick up an IP from the DHCP scope set up in Stage 3
- Join both clients to the sherry.local domain
- Verify both clients appear in Active Directory

---

## Client Configuration
- Hypervisor: VirtualBox
- OS: Windows 11 Pro 25H2
- Base memory: 8192 MB (8 GB)
- Processors: 2 cores
- Disk: 50 GB VDI (dynamic)
- Network: Internal network (same as DC01)
- Local accounts: Josh (Client01), Shez (Client02)

---

## Steps Completed
- Created two VMs in VirtualBox with matching specs
- Installed Windows 11 Pro on both, picking Pro for domain join support
- Used the OOBE bypass trick (`OOBE\BYPASSNRO`) to skip the Microsoft account requirement
- Created local accounts on each client
- Verified both clients pulled IPs from the DHCP scope (192.168.10.20 and 192.168.10.21)
- Renamed the PCs to Client01 and Client02 and joined them to sherry.local
- Restarted the clients to finish the join
- Verified both clients show up in Active Directory on DC01

---

## Verification
- `ipconfig` on both clients shows IPs in the DHCP scope range with the connection-specific DNS suffix sherry.local
- "Welcome to the sherry.local domain" message appeared on both clients
- `Get-ADComputer -Filter "Name -like 'CLIENT*'"` on DC01 returns Client01 and Client02
- Both clients visible in the Computers container in Active Directory Users and Computers

---

## Notes
- I picked Windows 11 Pro because Home edition can't join a domain. Worth knowing if you ever buy a laptop for a homelab.
- Windows 11 setup forces you to use a Microsoft account by default. The `OOBE\BYPASSNRO` command run from a hidden cmd window (SHIFT+F10) gets round this and lets you create a local account instead. Useful for lab work where you don't want everything tied to a Microsoft account.
- The clients picked up the right DNS suffix automatically because of the scope option I set in Stage 3. That's what makes the domain name resolve from the client without any manual config.
- I hit a problem during the first domain join because the DC had a NAT adapter registering its IP in DNS alongside the proper LabNet IP. I'll write that up in a separate troubleshooting section later.
- Both clients are on the same internal network as DC01 in VirtualBox so they can talk to the DC directly without needing a router.

---

## Screenshots

### VM Setup

<img width="743" height="528" alt="1 - Creating virtual machine for win 11 on VirutalBox  " src="https://github.com/user-attachments/assets/57d9c6aa-6939-47a3-8db2-03d931f6ead6" />

*Created the first VM in VirtualBox. Named it Client 1, pointed it at the Windows 11 ISO and let VirtualBox detect the OS.*

---

<img width="740" height="527" alt="2 - I have selected the CPU cores and base memory" src="https://github.com/user-attachments/assets/bc37e9ee-7f64-48ea-9beb-f7ff8859a714" />

*Set the base memory to 8 GB and gave it 2 CPU cores. Plenty for a lab client.*

---

<img width="742" height="529" alt="3 - Configured VDI 50gb and clicked finish" src="https://github.com/user-attachments/assets/bbbacd0a-7447-46c6-a2b0-7ecab2bd3ccc" />

*Created a 50 GB virtual hard disk in VDI format. Dynamic allocation so it only grows as needed.*

---

### Installing Windows 11

<img width="980" height="732" alt="4 - Windows 11 setup begins starting langauge I click next leave as default" src="https://github.com/user-attachments/assets/154141d2-ce0a-4db4-8691-1fb8c4de35ef" />

*Booted into the Windows 11 setup. Left the language as English (United States) and clicked next.*

---

<img width="977" height="737" alt="5 - Windows 11 setup US keyboard language then press next" src="https://github.com/user-attachments/assets/0fe75b80-3e99-450b-9e0c-e7b9d536dd02" />

*Kept the keyboard layout on US.*

---

<img width="979" height="731" alt="6 - Selecting Install Windows 11 and tick the box" src="https://github.com/user-attachments/assets/1f2fd6ad-7e0e-4086-9ac5-cf2200972214" />

*Picked "Install Windows 11" and ticked the box to confirm files would be deleted. Clean install since this is a fresh VM anyway.*

---

<img width="975" height="734" alt="7 - Selecting I don&#39;t have a product key" src="https://github.com/user-attachments/assets/56fc69d0-7091-4650-aa9a-dd7943417781" />

*Selected "I don't have a product key" since this is a lab VM.*

---

<img width="977" height="732" alt="8 - Selecting Windows 11 Pro for domain connection and extra features" src="https://github.com/user-attachments/assets/9479520c-248b-47d0-8141-7f67c605f7ca" />

*Selected Windows 11 Pro from the edition list. Pro is needed for domain join, Home can't do it.*

---

<img width="970" height="733" alt="9 - Accepting license agreement" src="https://github.com/user-attachments/assets/84a7b8dc-d7aa-4f46-86a1-cc91dd5e2bd4" />

*Accepted the license agreement to continue.*

---

<img width="973" height="731" alt="10 - Selecting parition then click next" src="https://github.com/user-attachments/assets/30c84ecc-1024-4a19-8e86-a91b7596202e" />

*Selected the unallocated 50 GB disk and clicked next. Setup handled the partitioning automatically.*

---

<img width="972" height="730" alt="11 - Ready to install Windows 11 " src="https://github.com/user-attachments/assets/eb4ce6eb-86a6-4eb2-8116-7afac34c35da" />

*Confirmation page showing what's about to be installed. Clicked Install.*

---

<img width="971" height="733" alt="12 - Picture showing win 11 installing it will restart multiple times" src="https://github.com/user-attachments/assets/9fa20fd4-fe76-46a3-9d2d-0f0768904ad6" />

*Windows 11 installation in progress. The VM rebooted a few times during this stage.*

---

### Out-of-Box Experience and OOBE Bypass

<img width="972" height="735" alt="13 - Booted into windows setup after reboot I will select next on country" src="https://github.com/user-attachments/assets/8a35dba6-2958-4c55-9bd1-fb48c89e2ced" />

*Booted into the OOBE after the install finished. Confirmed the country.*

---

<img width="975" height="738" alt="14 - keyboard layout stays the same clicking yes" src="https://github.com/user-attachments/assets/edf63adf-ec29-4969-a2b5-abdf068a12c0" />

*Confirmed the primary keyboard layout as US.*

---

<img width="972" height="730" alt="15 - Skipping keyboard layout" src="https://github.com/user-attachments/assets/ef80c5a1-c8f4-4fce-9cb9-57ca3c92a242" />

*Skipped adding a second keyboard layout, only need US.*

---

<img width="970" height="729" alt="16 - As its a lab no internet I have to bypass  " src="https://github.com/user-attachments/assets/1fce0e9b-43e6-407f-9274-ed7b93f947f0" />

*Got to the "Let's connect you to a network" page. The lab clients have no internet so I needed to bypass this.*

---

<img width="974" height="728" alt="17 - Bypassing my pressing SHIFT + F10 key and entering the command" src="https://github.com/user-attachments/assets/5c384a15-46f8-45c4-8488-988bfc0d22ce" />

*Pressed SHIFT+F10 to open a hidden command prompt and ran `OOBE\BYPASSNRO`. This restarts the OOBE with the option to skip the network requirement.*

---

<img width="976" height="733" alt="18 - After the device restarts you need to repeat processes and now you can see we can select I don&#39;t have internet" src="https://github.com/user-attachments/assets/389f66d6-d1cc-41e7-bbe5-2599c2633b63" />

*After the restart the network page now shows an "I don't have internet" link in the bottom right. Clicked it to carry on without connecting.*

---

### Local Account Creation

<img width="976" height="731" alt="19 - Entering name for PC" src="https://github.com/user-attachments/assets/b1ca60f0-8548-4dce-be13-869ce1a193be" />

*Entered a name for the local account. This will be the local user on the client before it's joined to the domain.*

---

<img width="979" height="734" alt="20 - Entering a password for PC" src="https://github.com/user-attachments/assets/cf95bec4-61a8-4ab6-89bd-e07c933fbddd" />

*Set a password for the local account.*

---

<img width="976" height="725" alt="21 - Re-Entering password to confirm" src="https://github.com/user-attachments/assets/de662dda-a766-4522-8b45-eee717873b33" />

*Confirmed the password.*

---

<img width="974" height="729" alt="22 - Filling in all security questions once done I will go onto next step" src="https://github.com/user-attachments/assets/eacab4e5-05e2-4796-8fd7-c528e2781883" />

*Filled in the three security questions. Required when creating a local account on Windows 11.*

---

<img width="975" height="734" alt="23 - Turned the privacy options off for the first 3 and the last to are still checked" src="https://github.com/user-attachments/assets/9b70d3c0-e152-47b8-8725-9f8d8d9f413b" />

*Turned off the first three privacy options (location, find my device, send diagnostic data) and left the rest as default.*

---

<img width="976" height="727" alt="24 - Windows finishing up message" src="https://github.com/user-attachments/assets/3b1bc58f-200c-4d8e-a4da-99acd37a8028" />

*"This might take a few minutes" message while Windows finishes setting up. Couple of minutes later the desktop loaded.*

---

### DHCP Verification

<img width="976" height="734" alt="25 - Booted into windows and went on cmd typed ipconfig the lease has been given out by server on client 1" src="https://github.com/user-attachments/assets/775f0bb5-6498-4d56-a432-5ecde6caba31" />

*Opened cmd on Client 1 and ran `ipconfig`. Got 192.168.10.20 with the connection-specific DNS suffix sherry.local. That's the lease handed out by the DHCP server set up in Stage 3.*

---

<img width="978" height="736" alt="26 - Booted into windows and went on cmd typed ipconfig the lease has been given out by server on client 2" src="https://github.com/user-attachments/assets/d6375524-9b0a-4f84-9aba-257f1cf3794f" />

*Same check on Client 2. Got 192.168.10.21 and the same sherry.local DNS suffix. Both clients pulling addresses cleanly from the DHCP scope.*

---

### Domain Join

<img width="1018" height="765" alt="27 - Locate onto system settings on client one and click on domain and workgroup" src="https://github.com/user-attachments/assets/1122bc4a-4891-4f25-a446-8c1507d71042" />

*Opened Settings > System > About on Client 1. Currently sitting in WORKGROUP with the default Windows-generated PC name. Clicked "Domain or workgroup" to open the System Properties dialog.*

---

<img width="1019" height="760" alt="28 - Click on Rename " src="https://github.com/user-attachments/assets/889d9401-c1e5-4e45-ac38-60e981c061bd" />

*Clicked the Change button in System Properties to open the Computer Name/Domain Changes dialog.*

---

<img width="1021" height="763" alt="29 - Chaged computer name to client01 + added to domain then I will click OK" src="https://github.com/user-attachments/assets/dd71f4d5-6fd6-4c51-a08a-628dd9829494" />

*Renamed the PC to Client01 and switched the Member of radio button from Workgroup to Domain. Typed sherry.local in the domain box.*

---

<img width="1025" height="765" alt="30 - A pop up for prompting server username and password" src="https://github.com/user-attachments/assets/a921173e-a9df-4302-9f1e-3404a7786ddf" />

*Windows Security prompt asking for credentials to join the domain. Used the SHERRY\Administrator account.*

---

<img width="1025" height="776" alt="31 - Domain connected! (Also had errors before until cmd and disabled NAT)" src="https://github.com/user-attachments/assets/47c003a3-2ec9-4cd2-85cc-a965b96bd582" />

*"Welcome to the sherry.local domain" confirmation. Domain join successful.*

---

<img width="1025" height="762" alt="32 - prompted to restart computer" src="https://github.com/user-attachments/assets/2e41e146-dace-4094-8a3e-d7bbe7461bce" />

*Prompted to restart for the changes to take effect.*

---

<img width="1035" height="767" alt="33 - Clicked the restart now button" src="https://github.com/user-attachments/assets/49c81582-b74b-419a-b741-7908f139fb57" />

*Final restart prompt. Notice in the System info behind it the Full device name is now Client01.sherry.local. Clicked Restart Now.*

---

### Verification on DC01

<img width="971" height="412" alt="34 - Verification 1 on server" src="https://github.com/user-attachments/assets/9cf9a6f4-a0cd-4585-a754-ca02646a684e" />

*Ran `Get-ADComputer -Filter "Name -like 'CLIENT*'"` in PowerShell on DC01. Both Client01 and Client02 returned with their full AD details, including DNSHostName values pointing at sherry.local. This proves the clients are properly registered in AD and DNS.*

---

<img width="753" height="531" alt="35 - Verification on server via AD users + computers" src="https://github.com/user-attachments/assets/60af9601-bf00-4901-adcb-1d19562bacdb" />

*Same check via the GUI. Active Directory Users and Computers on DC01 showing Client01 and Client02 in the Computers container under sherry.local. Both clients are now full domain members.*
