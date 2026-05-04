# Stage 5: Active Directory Organisation and Group Policy

## Objective
- Build a proper OU structure to separate computers from users and admins from standard users
- Create domain users and assign IT Admin to Domain Admins
- Create and link Group Policy Objects to apply different policies to different OUs
- Demonstrate Computer Configuration policies on workstations and User Configuration policies on standard users
- Prove the policies work by testing two different users on the same machine

---

## OU and Group Policy Configuration
- Top-level OUs: `Workstations` and `Domain Users`
- Sub-OUs under Domain Users: `Standard Users` and `IT Admins`
- Standard user: Joe Bloggs (`j.bloggs`) in Standard Users OU
- Admin user: IT Admin (`it.admin`) in IT Admins OU, member of Domain Admins
- GPO 1: `Workstation Baseline` linked to Workstations OU (Computer Configuration)
- GPO 2: `Standard User Restrictions` linked to Standard Users OU (User Configuration)

---

## Steps Completed
- Created the OU hierarchy in AD Users and Computers
- Moved Client01 and Client02 into the Workstations OU
- Created Joe Bloggs in Standard Users OU
- Created IT Admin in IT Admins OU and added to Domain Admins
- Created and linked the Workstation Baseline GPO with login screen restrictions
- Created and linked the Standard User Restrictions GPO with cmd, regedit, PowerShell scripts, and Control Panel blocked
- Ran gpupdate /force on the server to push the policies
- Tested both users on Client01 to confirm policies apply differently based on OU

---

## Verification
- Login screen on Client01 is clean with just username and password boxes, no cached user list
- j.bloggs (Standard User) is blocked from cmd, regedit, PowerShell scripts, and Control Panel
- it.admin (Domain Admin) can use cmd freely on the same machine
- Get-ExecutionPolicy -List confirms UserPolicy is set to Restricted on j.bloggs

---

## Notes
- Computer Configuration policies apply to computer objects in the OU. User Configuration policies apply to user objects in the OU. The GPO has to be linked to an OU containing the right object type or it won't work even if it's configured correctly.
- I caught this myself early on. The first time I tried blocking Control Panel I used User Configuration but linked the GPO to the Workstations OU, which only has computers in it. Took me a minute to realise why nothing was happening.
- The default Computers and Users containers in AD are NOT OUs. You can't link GPOs to them properly. That's the main reason to create your own OU structure.
- IT Admin is a member of Domain Admins so most restrictive policies don't apply to that account anyway. This mirrors how schools and businesses set up their actual admins.
- Adding comments to GPO settings (the comment field on each policy) makes it way easier to figure out why a setting was enabled six months later. Not a technical requirement but good practice.
- gpupdate /force only updates the policies on the machine you run it on. Running it on the server doesn't push to the clients. Clients pull policies on their own schedule (every 90-120 minutes by default), on reboot, or when a user logs in. To push to a specific client you can run Invoke-GPUpdate from PowerShell on the server.
- The PowerShell setting "Turn on Script Execution = Disabled" doesn't block the PowerShell console itself, just script execution. Standard users can still open PowerShell and run interactive commands but can't run .ps1 files. Blocking the console entirely would break a lot of Windows functionality.

---

## Screenshots

### Building the OU Structure

<img width="959" height="865" alt="1 - Opening AD U+C" src="https://github.com/user-attachments/assets/b5da536e-c3b1-422e-887a-9f0afc9e6ff8" />


*Opened Active Directory Users and Computers from Server Manager > Tools.*

---

<img width="718" height="506" alt="2 - Right clicking on sherry local, New, Organizational Unit" src="https://github.com/user-attachments/assets/08dfd17d-2b8b-43b3-a680-9260f40cf962" />


*Right clicked on sherry.local and chose New > Organizational Unit to start creating the structure.*

---

<img width="721" height="509" alt="3 - Naming the OU Workstations" src="https://github.com/user-attachments/assets/fb0a579d-3b23-438c-a606-95b3b94d95d5" />


*Named the first OU Workstations. This will hold the client computers.*

---

<img width="718" height="500" alt="4 - Same process again but creating an OU called Domain Users" src="https://github.com/user-attachments/assets/9f791851-a737-49db-a313-a1d0f30f632b" />


*Same process again but creating an OU called Domain Users to hold the user accounts.*

<img width="744" height="572" alt="5 - Creating another OU by clicking on Domain Users folder then new then Organization Unit" src="https://github.com/user-attachments/assets/3b4ff096-d7eb-44e9-89e0-4c2cfc81bf7d" />


<!-- Images 5-8: Standard Users and IT Admins sub-OUs being created (you may have these as separate shots) -->

### Creating the Standard User

<!-- Image 9 -->

*Right clicked on the Standard Users OU and chose New > User to create the test user.*

---

<!-- Image 10 -->

*Created Joe Bloggs with the logon name j.bloggs. Picked a generic name that looks realistic in a portfolio.*

---

<!-- Image 11 -->

*Set a password for Joe and unticked "User must change password at next logon" to save hassle when testing.*

---

<!-- Image 12 -->

*Confirmation page for Joe Bloggs. Clicked Finish to create the user.*

---

### Creating the IT Admin and Adding to Domain Admins

<!-- Images 13-16: IT Admin user creation if you've got those -->

<!-- Image 17 -->

*Right clicked on the IT Admin user and chose Properties to open the user account properties.*

---

<!-- Image 18 -->

*Went to the Member Of tab. By default the user is just a member of Domain Users. Clicked Add to give it admin rights.*

---

<!-- Image 19 -->

*Typed "Domain Admins" and clicked Check Names. The underline confirms it found the group. Clicked OK to add it.*

---

<!-- Image 20 -->

*IT Admin is now a member of both Domain Users and Domain Admins. Clicked OK to save the changes.*

---

### Creating the Workstation Baseline GPO

<!-- Image 21 -->

*Opened Group Policy Management from Server Manager > Tools.*

---

<!-- Image 22 -->

*Expanded sherry.local, right clicked on Workstations OU and chose "Create a GPO in this domain, and Link it here". This creates the GPO and links it in one step.*

---

<!-- Image 23 -->

*Named the GPO Workstation Baseline. Clicked OK.*

---

<!-- Image 24 -->

*GPO is now linked to Workstations. Right clicked on it and chose Edit to open the Group Policy Management Editor.*

---

<!-- Image 25 -->

*Inside the editor. For machine-wide settings I expanded Computer Configuration > Policies > Windows Settings > Security Settings.*

---

<!-- Image 26 -->

*Then clicked into Local Policies.*

---

<!-- Image 27 -->

*Then Security Options. This is where the login screen settings live.*

---

<!-- Image 28 -->

*Located the two interactive logon settings I wanted to enable.*

---

<!-- Image 29 -->

*Enabled "Don't display last signed-in" so the login screen doesn't show previous users. Clicked OK.*

---

<!-- Image 30 -->

*Same for "Don't display username at sign-in" so the next user doesn't see who logged in last. Clicked OK.*

---

### Creating the Standard User Restrictions GPO

<!-- Image 31 -->

*Back in Group Policy Management, right clicked on Standard Users OU and chose "Create a GPO in this domain, and Link it here".*

---

<!-- Image 32 -->

*Named this one Standard User Restrictions.*

---

<!-- Image 33 -->

*GPO is now linked to Standard Users. Right clicked it and chose Edit to configure the policies.*

---

<!-- Image 34 -->

*Navigated to User Configuration > Policies > Administrative Templates > System. Found "Prevent access to the command prompt".*

---

<!-- Image 35 -->

*Set it to Enabled and added a comment "Disabled CMD for Joe" so anyone reading the GPO later knows why it's there. Clicked Apply then OK.*

---

<!-- Image 36 -->

*Same for "Prevent access to registry editing tools" in the same System folder.*

---

<!-- Image 37 -->

*Enabled it with a short comment, clicked Apply then OK.*

---

<!-- Image 38 -->

*Confirmation showing both cmd and regedit are now Enabled with the comments visible.*

---

<!-- Image 39 -->

*Navigated to User Configuration > Policies > Administrative Templates > Windows Components > Windows PowerShell to block scripts.*

---

<!-- Image 40 -->

*Set "Turn on Script Execution" to Disabled. This blocks .ps1 files from running but doesn't block the PowerShell console itself. Clicked Apply and OK.*

---

<!-- Image 41 -->

*Last one, navigated to Administrative Templates > Control Panel.*

---

<!-- Image 42 -->

*Found "Prohibit access to Control Panel and PC settings" and clicked it.*

---

<!-- Image 43 -->

*Set it to Enabled, clicked Apply then OK.*

---

### Pushing the Policies

<!-- Image 44 (your gpupdate one, may be numbered 43 in your files) -->

*Ran gpupdate /force on the server to refresh the policies. This only updates the server's own policies. Clients pick up changes on their own schedule, on reboot, or when a user logs in.*

---

### Testing on Client01 as Standard User

<!-- Image 44 (clean login screen) -->

*Clean login screen on Client01 with just the username and password boxes. No cached user list at the bottom left. The Workstation Baseline GPO is doing its job. Typed in j.bloggs to log in as the standard user.*

---

<!-- Image 45 -->

*Tried to open cmd as j.bloggs. Got "The command prompt has been disabled by your administrator" exactly as expected. Standard User Restrictions GPO is applying.*

---

<!-- Image 46 -->

*Searched for Control Panel in the Start menu and clicked it.*

---

<!-- Image 47 -->

*Got the Restrictions popup: "This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator." Control Panel is fully blocked.*

---

<!-- Image 48 -->

*Tried to open regedit using the Run dialog.*

---

<!-- Image 49 -->

*Got the registry editing block message. Standard users can't make registry changes which is exactly what we want.*

---

<!-- Image 50 -->

*Opened PowerShell as j.bloggs. The console itself opens because we only blocked script execution, not the console. Ran Get-ExecutionPolicy -List to confirm the policy is in effect. UserPolicy shows as Restricted which means no scripts can run for this user. The MachinePolicy and other scopes are Undefined because the GPO only sets the user policy.*

---

### Testing on Client01 as IT Admin

<!-- Image 51 -->

*Logged out and logged back in as it.admin to compare. Same machine, different user. Gets through the login screen the same way (no cached list).*

---

<!-- Image 52 -->

*Opened cmd as it.admin and ran whoami. Cmd works fine because the Standard User Restrictions GPO is linked to the Standard Users OU. it.admin is in the IT Admins OU so none of those restrictions apply. Same machine, completely different experience based on which OU the user lives in.*
