<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Video Demonstration (coming soon)</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Provision and configure Azure Virtual Machines for Windows Server and Windows 10.
- Step 2: Install Active Directory Domain Services on the Windows Server 2022 VM.
- Step 3: Promote the server to a Domain Controller and create a new domain.
- Step 4: Join the Windows 10 client to the new domain and validate connectivity.

<h2>Deployment and Configuration Steps</h2>

<p>
  <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>
<p>
  <strong>Step 1: Provisioning Azure Virtual Machines and Network Configuration</strong><br>
  Begin by logging into the Azure portal and creating a new resource group. Deploy two Virtual Machines: one running Windows Server 2022 to host Active Directory and another running Windows 10 for client testing. Configure the virtual network and subnet settings to ensure that both VMs can communicate, and enable Remote Desktop access on each machine.
  CHECKLIST:
• Create a Resource Group (Set name and region).

• Create a Virtual Network inside the Resource Group.

• Create a Virtual Machine (VM1 - dc-1):

• Image: Windows Server 2022 Datacenter

• Specs: 2 vCPUs

• Set username & password

• Connect to VNet & review + create.

2. Create a Second Virtual Machine

• Create VM2 - client-1 in the same Resource Group:

• Image: Windows 10 Pro

• Specs: 2 vCPUs

• Set username & password

• Connect to VNet & review + create.

3. Configure Networking for DC-1

• Open Azure Home → Go to VM dc-1 → Networking.

• Go to Network Settings and disable NIC 1690.

• Change IP settings:

• Switch from Dynamic to Static.

• Save the Private IP (should remain unchanged).

4. Configure Firewall and Network Settings

• Restart VM.

• Disable Windows Firewall on dc-1:

• Open wf.msc → Turn OFF Firewall → Apply & OK.

• Set Client-1 DNS to DC-1’s Private IP:

• Get dc-1 Private IP from Azure.

• Go to VM2 (Client-1) Network Settings → Interface Card.

• Change DNS Servers → Custom → Paste dc-1 Private IP.

• Restart Client-1.

5. Test Connection

• Log in to Client-1.

• Open PowerShell and test connectivity:

• ping 10.0.0.4 (Private IP of dc-1).

• Ensure the ping succeeds.

• Run: ipconfig /all to check DNS server settings.

6. Install Active Directory Domain Services

• Log into dc-1.

• Open Server Manager → Add Roles & Features.

• Select dc-1 and install Active Directory Domain Services.

• Enable Add Feature → Check Restart if required → Install.

7. Promote Server to Domain Controller

• In dc-1, open Server Manager and click the flag.

• Promote the server to Domain Controller:

• Set up a new forest.

• Enter a domain name (mydomain.com).

• Set a password.

• Uncheck “Create DNS” and proceed.

• System will auto-restart.
</p>
<br />

<p>
  <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Installation and Promotion"/>
</p>
<p>
  <strong>Step 2: Installing Active Directory Domain Services and Promoting the Server</strong><br>

   On the Windows Server 2022 VM, open Server Manager and use the Add Roles and Features Wizard to install the "Active Directory Domain Services" role along with any required features. Once installed, launch the Active Directory Domain Services Configuration Wizard to promote the server to a Domain Controller. Choose to create a new forest (for example, <code>mydomain.local</code>), configure DNS settings, and complete the wizard, which will automatically restart the server.
  
  8. Sign In with Domain User Credentials

• Use mydomain.com\labuser and password to log in to dc-1.

• Confirm that users can sign in with their domain credentials.

9. Create Organizational Units (OU)

• Open Active Directory Users and Computers on dc-1.

• Navigate to mydomain.com.

• Right-click the domain → Select New → Organizational Unit (OU).

• Name it accordingly (e.g., Employees, Admins, Clients).

10. Create and Assign Users

• Right-click Admins or Users OU → New → User.

• Enter username & password.

• Right-click the user account → Properties → Member of → Add to “Domain Admins” (if admin privileges are needed).

11. Assign User Roles

• Log out of dc-1 and log back in as the created user.

• Rename the PC if necessary.
  
</p>
<br />

<p>
  <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>
<p>
  <strong>Step 3: Joining Windows 10 Client to the Domain and Validating the Setup</strong><br>
  On the Windows 10 VM, adjust the network settings to designate the Windows Server's IP address as the primary DNS server. Open the System Properties, select "Change settings" under Computer Name, and join the machine to the newly created domain by entering the domain name and appropriate credentials. After a reboot, verify the domain join by logging in with a domain account and using PowerShell (e.g., running <code>Get-ADUser</code>) to confirm connectivity with Active Directory.
  
  12. Join Client Machine (VM2) to the Domain

• Log into Client-1 (VM2).

• Open System Properties → Advanced System Settings.

• Under Computer Name → Click Change.

• Select Domain, enter mydomain.com, and provide admin credentials.

• Click OK and restart Client-1.

13. Verify Domain Enrollment

• Log back into dc-1.

• Open Active Directory Users and Computers.

• Navigate to mydomain.com → Expand Computers.

• Verify that Client-1 is listed.

• Right-click mydomain.com → Create a new OU named “Clients”.

• Drag Client-1 into the Clients OU.

14. Enable Remote Desktop Access for Domain Users

• Log into Client-1 as user.mydomain.com.

• Open System Properties → Remote Desktop.

• Navigate to User Accounts → Select Users → Add.

• Enter Domain Users → Click OK.

• Now, all domain users can log into Client-1.

15. Apply Group Policies via PowerShell

• Log into dc-1 as a domain admin (user.domain.com (admin)).

• Open PowerShell.

• Paste and execute any required PowerShell scripts for automation, security, or user/group policies.
</p>
<br
