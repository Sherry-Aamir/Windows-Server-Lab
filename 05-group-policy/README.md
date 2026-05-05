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

---

<img width="744" height="572" alt="5 - Creating another OU by clicking on Domain Users folder then new then Organization Unit" src="https://github.com/user-attachments/assets/3b4ff096-d7eb-44e9-89e0-4c2cfc81bf7d" />

*Right clicked on the Domain Users OU and chose New > Organizational Unit.*

---

<img width="713" height="504" alt="6 - Creating the OU called Standard Users" src="https://github.com/user-attachments/assets/a3e96461-74ff-4190-be73-7c5eb3a57dbd" />

*I have named it Standard Users.*

---

<img width="725" height="501" alt="7 - using the same process as the 5, Another OU created called IT Admins" src="https://github.com/user-attachments/assets/344307d7-9229-47d9-b0aa-85fdfe1d7679" />

*Same process again then called it IT Admins.*

---

<img width="970" height="889" alt="8 - Dragging the clients from computers to Workstations Warning will come up just click yes on warning msg" src="https://github.com/user-attachments/assets/822de7b0-3da5-4342-9b59-e97449bc0c14" />

*From the computers folder I have dragged the clients into the workstations folder a pop message will come and I clicked yes.*

---

### Creating the Standard User

<img width="967" height="871" alt="9 - Right clicking on New on Standards User folder, then User" src="https://github.com/user-attachments/assets/4d2e71a0-c189-420e-983b-9f26ac79d20b" />

*Right clicked on the Standard Users OU and chose New > User to create the test user.*

---

<img width="724" height="506" alt="10 - Creating a new user called Joe bloggs, then I click Next" src="https://github.com/user-attachments/assets/f77cab84-8fc2-4c5f-ac73-c4aeecc90f0c" />

*Created Joe Bloggs with the logon name j.bloggs. Picked a generic name that looks realistic in a portfolio.*

---

<img width="715" height="506" alt="11 - I enter the password for Joe and untick the user must change password at next log on" src="https://github.com/user-attachments/assets/a67de4c6-25a7-4424-9f33-d87f2cc56af9" />

*Set a password for Joe and unticked "User must change password at next logon" to save hassle when testing.*

---

<img width="722" height="504" alt="12 - I click next and finish" src="https://github.com/user-attachments/assets/f2ea407c-c311-4b42-8126-d7e37f340c8f" />

*Confirmation page for Joe Bloggs. Clicked Finish to create the user.*

---

### Creating the IT Admin and Adding to Domain Admins

<img width="719" height="607" alt="13 - Right clicking on IT admins OU and selecting new then user" src="https://github.com/user-attachments/assets/b8f0eb0f-3cb9-49ad-8d6b-45b0d3be2448" />

*Now I right click on IT Admins OU, New > User.*

---

<img width="718" height="504" alt="14 - Filling the user details for IT admin then i will click next" src="https://github.com/user-attachments/assets/b12d6985-2a71-4268-ba2e-d31222557590" />

*I have filled in the Admin's info and gave a logon name then I will click next.*

---

<img width="725" height="503" alt="15 - I will enter the password for the IT admin and uncheck the user icon" src="https://github.com/user-attachments/assets/589fea64-416c-4ddf-9d71-aa1a97da4bec" />

*Entered a password for the user, unticked the User must change password at next logon.*

---

<img width="720" height="508" alt="16 - I will click finish and the user will be created" src="https://github.com/user-attachments/assets/e6ffca10-ae2f-4a4f-963f-8b50d7e2a279" />

*Final summary is listed and I will click finish.*

---

<img width="722" height="501" alt="17  - IT Admin needs admin access so I will right click on the user and select properties" src="https://github.com/user-attachments/assets/47136ec3-b596-4bb1-a37a-8fa449a294f0" />

*Right clicked on the IT Admin user and chose Properties to open the user account properties.*

---

<img width="722" height="506" alt="18 - Locate the Member Of tab and select Add as seen" src="https://github.com/user-attachments/assets/1749da65-e9e5-47cd-9035-ebbd91d65f16" />

*Went to the Member Of tab. By default the user is just a member of Domain Users. Clicked Add to give it admin rights.*

---

<img width="723" height="507" alt="19 - I will add the IT admin to Domain Admins by typing it in and clicking check Names as you see it has the line to confirm it then I will click Ok and Ok on both tabs" src="https://github.com/user-attachments/assets/408bf01c-de0e-407e-b195-30a0a4078a3d" />

*Typed "Domain Admins" and clicked Check Names. The underline confirms it found the group. Clicked OK to add it.*

---

<img width="716" height="507" alt="20 - Here is to confirm the changes now I will click OK" src="https://github.com/user-attachments/assets/cfe333c7-a3cb-458f-8752-4fb0c213942b" />

*IT Admin is now a member of both Domain Users and Domain Admins. Clicked OK to save the changes.*

---

### Creating the Workstation Baseline GPO

<img width="959" height="870" alt="21 - Now to apply policies and restrictions we head to group policy via server manager" src="https://github.com/user-attachments/assets/3dbb1b1d-5a40-4094-9ce1-4890a19828e1" />

*Opened Group Policy Management from Server Manager > Tools.*

---

<img width="973" height="641" alt="22 - Expanding my server name sherry local then I will start with the workstations I will Right click on workstations then click on Create a GPO in this domain and link it here" src="https://github.com/user-attachments/assets/a7bd793e-0dd4-4b69-bd86-46331720503e" />

*Expanded sherry.local, right clicked on Workstations OU and chose "Create a GPO in this domain, and Link it here". This creates the GPO and links it in one step.*

---

<img width="369" height="174" alt="23 - I have called it workstations baseline then I click ok" src="https://github.com/user-attachments/assets/32166d44-a1b0-4d40-a755-303fddf3d92c" />

*Named the GPO Workstation Baseline. Clicked OK.*

---

<img width="965" height="867" alt="24 - Once it created I will assign policies by right clicking on the Policy and clicking edit this opens a new windows" src="https://github.com/user-attachments/assets/4919df52-000f-4f73-9483-1c24ae4e1ca4" />

*GPO is now linked to Workstations. Right clicked on it and chose Edit to open the Group Policy Management Editor.*

---

<img width="962" height="838" alt="25 - Once on GPO editior we will assign computer config first to begin expand policies, click windows settings, then security settings" src="https://github.com/user-attachments/assets/49a8e527-f2e5-432d-bb81-4a477df6490f" />

*Inside the editor. For machine-wide settings I expanded Computer Configuration > Policies > Windows Settings > Security Settings.*

---

<img width="967" height="860" alt="26 - Then I click on Local Policies" src="https://github.com/user-attachments/assets/99afed4f-118f-4be0-ba43-41b13e0d831c" />

*Then clicked into Local Policies.*

---

<img width="963" height="876" alt="27 - Then I click on Security options " src="https://github.com/user-attachments/assets/9a7b39b0-444e-4f6c-9e69-b686aaf1bf7a" />

*Then Security Options. This is where the login screen settings live.*

---

<img width="538" height="45" alt="28 Locate the following in the screenshot (I am mentioning the 2 interactive logon things)" src="https://github.com/user-attachments/assets/dc13bcd5-f12c-4820-846c-23a73d4be27a" />

*Located the two interactive logon settings I wanted to enable.*

---

<img width="964" height="873" alt="29 - Once located do the following click on the checkbox and click enabled then click OK" src="https://github.com/user-attachments/assets/d9eef500-44d7-4850-a904-fea428def100" />

*Enabled "Don't display last signed-in" so the login screen doesn't show previous users. Clicked OK.*

---

<img width="395" height="483" alt="30 - Do the same for the 2nd one and click OK" src="https://github.com/user-attachments/assets/1238e04e-c31c-4c0c-8579-4f1d93243487" />

*Same for "Don't display username at sign-in" so the next user doesn't see who logged in last. Clicked OK.*

---

### Creating the Standard User Restrictions GPO

<img width="963" height="866" alt="31 - Locating back to the Domain Users OU then Standard Users OU  Right click then click on Create a GPO in this domain and link it here" src="https://github.com/user-attachments/assets/c8da89e6-2edc-49d6-8c02-4c9a4852dfa4" />

*Back in Group Policy Management, right clicked on Standard Users OU and chose "Create a GPO in this domain, and Link it here".*

---

<img width="376" height="178" alt="32 - I have named it Standard User Restrictions" src="https://github.com/user-attachments/assets/ee6529e0-b092-454f-b017-c0b23059c941" />

*Named this one Standard User Restrictions.*

---

<img width="969" height="867" alt="33 - Once that is created I will click on the policy and right click then click edit" src="https://github.com/user-attachments/assets/03c5e359-5827-47cc-be23-32e1306f995c" />

*GPO is now linked to Standard Users. Right clicked it and chose Edit to configure the policies.*

---

<img width="963" height="840" alt="34 - Locate onto User config - Policies - Administrative templates - System then click on prevent acccess to cmd" src="https://github.com/user-attachments/assets/e1b8bb99-de9c-4c4c-a123-1122f61bb2f3" />

*Navigated to User Configuration > Policies > Administrative Templates > System. Found "Prevent access to the command prompt".*

---

<img width="965" height="836" alt="34 - Locate onto User config - Policies - Administrative templates - System then right click and edit on prevent acccess to cmd" src="https://github.com/user-attachments/assets/4f8a584b-f789-4e46-a148-9e51631d93c6" />

*Set it to Enabled and added a comment "Disabled CMD for Joe" so anyone reading the GPO later knows why it's there. Clicked Apply then OK.*

---

<img width="653" height="607" alt="35 - I will enable this and make a short comment then apply - Then OK" src="https://github.com/user-attachments/assets/c9a92450-0b76-4470-8503-a2fdacdb975c" />

*Same for "Prevent access to registry editing tools" in the same System folder.*

---

<img width="660" height="608" alt="37 - Clicked enabled and made a comment then clicked apply then OK" src="https://github.com/user-attachments/assets/5c75a10e-b463-4702-8fbb-e2a6c582af47" />

*Enabled it with a short comment, clicked Apply then OK.*

---

<img width="583" height="532" alt="38 - Confirmed as both enabled" src="https://github.com/user-attachments/assets/c63cc1d3-6898-4686-8f1d-7fa217632c1c" />

*Confirmation showing both cmd and regedit are now Enabled with the comments visible.*

---

<img width="966" height="841" alt="39 - Locating onto user config, policies, admin templates, windows components  then windows powershell then hovering over it" src="https://github.com/user-attachments/assets/06b0d156-b1d8-45f4-a0b3-3f5990f0c220" />

*Navigated to User Configuration > Policies > Administrative Templates > Windows Components > Windows PowerShell to block scripts.*

---

<img width="652" height="610" alt="40 - Disabling Powershell scripts clicking apply and OK" src="https://github.com/user-attachments/assets/41209a09-40ba-4060-9e4e-27ec23ae63b0" />

*Set "Turn on Script Execution" to Disabled. This blocks .ps1 files from running but doesn't block the PowerShell console itself. Clicked Apply and OK.*

---

<img width="965" height="319" alt="41 - Going to the administrative templates then control panel" src="https://github.com/user-attachments/assets/e3a69b5d-20b8-48e5-a646-77055506f9b1" />

*Last one, navigated to Administrative Templates > Control Panel.*

---

<img width="970" height="581" alt="42 - Clicking on prohibit access to control panel and PC settings" src="https://github.com/user-attachments/assets/781bdfc6-0e16-4c5a-9e47-25f0000bd782" />

*Found "Prohibit access to Control Panel and PC settings" and clicked it.*

---

<img width="651" height="609" alt="42 - Clicking enabled on the setting then apply then OK" src="https://github.com/user-attachments/assets/2e323c8e-dc87-43b9-b0ef-9524c0fd22d4" />

*Set it to Enabled, clicked Apply then OK.*

---

### Pushing the Policies

<img width="916" height="482" alt="43 - Running gpupdate command on server to make sure policies are updated asap" src="https://github.com/user-attachments/assets/9547a025-aa42-400b-a0ef-ea4beba75aed" />

*Ran gpupdate /force on the server to refresh the policies. This only updates the server's own policies. Clients pick up changes on their own schedule, on reboot, or when a user logs in.*

---

### Testing on Client01 as Standard User

<img width="1025" height="768" alt="44 - Clean picture of login screen for client one and logging into J bloggs" src="https://github.com/user-attachments/assets/f993049b-8198-4192-a86a-3a05653564c9" />

*Clean login screen on Client01 with just the username and password boxes. No cached user list at the bottom left. The Workstation Baseline GPO is doing its job. Typed in j.bloggs to log in as the standard user.*

---

<img width="1024" height="767" alt="45 - Opned cmd on J bloggs its disabled by gpo" src="https://github.com/user-attachments/assets/2b6f2a6f-bf13-4857-ab77-e0719959bef7" />

*Tried to open cmd as j.bloggs. Got "The command prompt has been disabled by your administrator" exactly as expected. Standard User Restrictions GPO is applying.*

---

<img width="1022" height="760" alt="46 - Locating onto control panel" src="https://github.com/user-attachments/assets/cd1400b3-39c5-414b-83a0-19be5ebfeb01" />

*Searched for Control Panel in the Start menu and clicked it.*

---

<img width="1038" height="760" alt="47 - Control panel restriciton message has come up" src="https://github.com/user-attachments/assets/f7bfcaa7-46e5-492c-b711-e2b2d385b597" />

*Got the Restrictions popup: "This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator." Control Panel is fully blocked.*

---

<img width="392" height="204" alt="48 - Running regedit" src="https://github.com/user-attachments/assets/fe8c1452-7f7b-43c4-9306-68b8673d845b" />

*Tried to open regedit using the Run dialog.*

---

<img width="1020" height="719" alt="49 - Error msg confirming regedit has been blocked by admin" src="https://github.com/user-attachments/assets/e73b5cd5-0f91-4e45-90cb-344e544128e4" />

*Got the registry editing block message. Standard users can't make registry changes which is exactly what we want.*

---

<img width="1028" height="718" alt="50 - Powershell should be blocked but for testing purposes on j bloggs" src="https://github.com/user-attachments/assets/48616e20-6841-4d8b-9993-3384c9734632" />

*Opened PowerShell as j.bloggs. The console itself opens because we only blocked script execution, not the console. Ran Get-ExecutionPolicy -List to confirm the policy is in effect. UserPolicy shows as Restricted which means no scripts can run for this user. The MachinePolicy and other scopes are Undefined because the GPO only sets the user policy.*

---

### Testing on Client01 as IT Admin

<img width="1024" height="768" alt="51 - Logging on as ITAdmin" src="https://github.com/user-attachments/assets/fd3ff41c-dbb7-4355-a492-cd0b3131454c" />

*Logged out and logged back in as it.admin to compare. Same machine, different user. Gets through the login screen the same way (no cached list).*

---

<img width="1030" height="763" alt="52 - CMD prompt of whoami" src="https://github.com/user-attachments/assets/bea9f0ae-5e3f-42fa-86a9-54a12ed36041" />

*Opened cmd as it.admin and ran whoami. Cmd works fine because the Standard User Restrictions GPO is linked to the Standard Users OU. it.admin is in the IT Admins OU so none of those restrictions apply. Same machine, completely different experience based on which OU the user lives in.*
