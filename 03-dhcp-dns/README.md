# Stage 3: DHCP Configuration

## Objective
- Complete the post-install configuration for the DHCP role from Stage 2
- Authorise the DHCP server in Active Directory
- Create a DHCP scope for the lab
- Set scope options for DNS so clients pick up the right settings
- Activate the scope ready for clients in Stage 4

---

## DHCP Configuration
- Scope name: Workstations
- IP range: 192.168.10.20 to 192.168.10.30
- Subnet mask: 255.255.255.0
- Default gateway: not set (no router in this virtual lab)
- DNS server: 192.168.10.10 (DC01)
- DNS domain name: sherry.local
- Lease duration: 8 days (default)

---

## Steps Completed
- Opened the post-install DHCP configuration wizard from Server Manager
- Authorised DHCP in Active Directory using the domain admin account
- Opened the DHCP console from the Windows search bar
- Created a new scope called Workstations with a small IP range
- Set DNS server and parent domain so clients get the right resolver
- Skipped the gateway and WINS pages as they aren't needed in this lab
- Activated the scope so it's ready to lease addresses

---

## Verification
- Post-install wizard summary shows "Authorizing DHCP server: Done"
- New Scope wizard completed without errors
- Scope is active and waiting for clients (will be tested in Stage 4)

---

## Notes
- DHCP needs to be authorised in AD before it'll hand out addresses on a domain network. This stops rogue DHCP servers from running on the network without permission.
- I picked a small IP range (only 11 addresses) since this is a lab and I'm not running many clients. Easy to expand the scope later if I need more.
- No default gateway was set because there's no router in this virtual lab. If I add one in a later stage I'll come back and update the scope options.
- The DNS server is set to DC01's IP (192.168.10.10) which is also acting as the DNS server for sherry.local.
- The actual lease test happens in Stage 4 when I join a client to the domain.

---

## Screenshots

### Completing the Post-Install Configuration

<img width="1833" height="873" alt="1 - Showing server manager and selecting the yellow flag to the DHCP wizard" src="https://github.com/user-attachments/assets/1468af35-1b19-4444-a4f4-2b08191fbc67" />


*Server Manager showing the yellow flag at the top right. Clicked it and chose "Complete DHCP configuration" to finish setting up the role from Stage 2.*

---

<img width="726" height="533" alt="2 - Description post-install config wizard" src="https://github.com/user-attachments/assets/3882cd42-3f92-4d98-90ac-ff8901525444" />


*Description page of the post-install wizard. Just a summary of what's about to happen, including creating the DHCP Administrators and Users security groups and authorising the server in AD.*

---

<img width="725" height="534" alt="3 - Authorization part of the DHCP wizard I select use default User&#39;s credentials" src="https://github.com/user-attachments/assets/6f01474e-a000-4a8f-b558-da33f5364bf1" />


*Authorisation page. Left it on "Use the following user's credentials" with SHERRY\Administrator since I'm logged in as a domain admin. This is what authorises the DHCP server in AD.*

---

<img width="725" height="533" alt="4 - Summary of the wizard then I click close" src="https://github.com/user-attachments/assets/8234b6fd-3a4d-47e3-b54e-34abe5a3a160" />


*Summary page showing both steps came back as Done. Security groups created and DHCP server authorised. Clicked Close.*

---

### Creating the Scope


<img width="752" height="604" alt="5 - I go on the windows search bar and locate DHCP" src="https://github.com/user-attachments/assets/cc6e63e6-c35e-4ccd-8a81-10a3a61c77e7" />


*Opened the DHCP console from the Windows search bar. Alternatively it can also be opened from Server Manager.*

---

<img width="1178" height="867" alt="6 - On dhcp right click on IPV4 and then select New scope" src="https://github.com/user-attachments/assets/cd094b94-92e3-4a2e-8e55-f602e086f7d8" />


*Right clicked on IPv4 in the DHCP console and chose New Scope to start the wizard.*

---

<img width="489" height="407" alt="7 - New scope wizard clicking next to begin" src="https://github.com/user-attachments/assets/8c60110e-c4c8-419d-a730-300ae7d14dc8" />


*Welcome page of the New Scope Wizard. Clicked next to begin.*

---

<img width="495" height="401" alt="8 - Scope name on wizard is workstations" src="https://github.com/user-attachments/assets/133a436e-b443-4790-a4dd-3a892e4f3241" />


*Named the scope Workstations so it's easy to identify what it's for.*

---

<img width="494" height="408" alt="9 - IP address range entered on wizard" src="https://github.com/user-attachments/assets/1a0050f4-5dc4-4694-a6e1-5be5d3d9edc1" />


*Set the IP range to 192.168.10.20 to 192.168.10.30 with a /24 subnet mask. Small range but enough for the lab clients I'll be adding in Stage 4.*

---

<img width="492" height="407" alt="10 - I am not adding any exclusions or delays clicking next" src="https://github.com/user-attachments/assets/ea68b749-8745-4a9c-a50d-d26ba9f68708" />

*No exclusions or delays needed since the range is clean and doesn't overlap with anything static.*

---

<img width="490" height="404" alt="11 - Leaving the lease duration on default so I click next" src="https://github.com/user-attachments/assets/65a9fb90-a256-467e-9afb-f2c135e548fc" />


*Left the lease duration on the default 8 days. Fine for a lab.*

---

<img width="491" height="406" alt="12 - Clicking next to configure these options now (YES)" src="https://github.com/user-attachments/assets/aa3b928c-dd70-4bdb-ae91-2f4649c80630" />


*Chose "Yes, I want to configure these options now" so I could set DNS and the parent domain in the same wizard.*

---

<img width="496" height="408" alt="13 - As this is a virtual lab no router default gateway so I click next" src="https://github.com/user-attachments/assets/7f7d4ee3-3f9e-4076-a2fc-2551ab5943a0" />


*Left the gateway blank since there's no router in this virtual lab. I'll come back and set this if I add a router in a later stage.*

---

<img width="492" height="407" alt="14 - Selecting the DNS server which is my main server IP 192 168 10 10" src="https://github.com/user-attachments/assets/16d19c33-a0a2-42da-9a0a-e401b870c3e8" />


*Set the parent domain to sherry.local and the DNS server to 192.168.10.10 which is DC01. Clients will use this for DNS lookups.*

---

<img width="492" height="408" alt="15 - No wins servers required I click next" src="https://github.com/user-attachments/assets/d6b58228-ba3d-4bb6-9925-c7d709596701" />


*Skipped WINS as it's not needed in a modern AD environment.*

---

<img width="495" height="406" alt="16 - I click next to activate the scope now" src="https://github.com/user-attachments/assets/002b84ab-50da-4a03-a166-b799323b5b70" />


*Chose "Yes, I want to activate this scope now" so it starts handing out addresses straight away.*

---

<img width="489" height="404" alt="17 - I click finish on the wizard" src="https://github.com/user-attachments/assets/a7f4f0b4-9ec9-4ca2-8e24-135b240c2e1c" />


*Wizard finished. Scope is active and ready for clients in Stage 4.*
