<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. <br/>

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
  
<h2> Start Here: </h2>

<strong> Begin by logging into the Azure portal and creating a new resource group. Deploy two Virtual Machines: one running Windows Server 2022 to host Active Directory and another running Windows 10 for client testing. Configure the virtual network and subnet settings to ensure that both VMs can communicate, and enable Remote Desktop access on each machine. </strong>
  
<p>
  <img src="https://i.imgur.com/VlOOQH5.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

<strong> Make sure to set the configurations for the (DC-1) VM like this: </strong>
• Image: Windows Server 2022 Datacenter → Specs: 2 vCPUs → Set username & password → Connect to VNet & review + create.

<p>
  <img src="https://i.imgur.com/mr8Ofex.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

<strong> Create a Second Virtual Machine: </strong>
• Create VM2 - client-1 in the same Resource Group:

  <img src="https://i.imgur.com/lEUurYD.png" height="80%" width="80%" alt="Azure VM Provisioning"/>

<strong> Make sure to set the configurations for VM2 (Client-1) like this: </strong>

• Image: Windows 10 Pro → Specs: 2 vCPUs →Set username & password → Connect to VNet & review + create.

<strong> Configure Networking for DC-1: </strong>

• Open Azure Home → Go to VM dc-1 → Networking. → Go to Network Settings and disable NIC 1690. → Change IP settings: → Switch from Dynamic to Static.
• Restart VM through Azure Portal.
• CTRL + C (Save the Private IP/should remain unchanged)

<p>
  <img src="https://i.imgur.com/vVN8ADF.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

<Strong> Configuring Firewall and Network Settings: </strong> 

• Disable Windows Firewall on dc-1 by logging into DC-1 through Microsoft Remote Desktop:

<p>
  <img src="https://i.imgur.com/zK6ct4v.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

• Open wf.msc → Turn OFF Firewall → Apply & OK.

<p>
  <img src="https://i.imgur.com/pJZpJZZ.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

• Set Client-1 DNS to DC-1’s Private IP: → Get dc-1 Private IP from Azure → Go to VM2 (Client-1) Network Settings → Interface Card → Change DNS Servers → Custom → Paste dc-1 Private IP.

<p>
  <img src="https://i.imgur.com/Wl9iODY.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

• Restart Client-1.

<p>
  <img src="https://i.imgur.com/tZFML4A.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

<strong> Now we will Test Connection! </strong>
• Log in to Client-1 → Open PowerShell and test connectivity → ping 10.0.0.4 (Private IP of dc-1) → Ensure the ping succeeds.

<p>
  <img src="https://i.imgur.com/IiPHdQu.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

• Run: ipconfig /all to check DNS server settings.

<p>
  <img src="https://i.imgur.com/qUpRgei.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

Now we are going to Install Active Directory Domain Services:

• Log into dc-1 → and open Server Manager → Add Roles & Features.

<p>
  <img src="https://i.imgur.com/7OMa4i9.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

• Select dc-1 and install Active Directory Domain Services.

• Enable Add Feature → Check Restart if required → Install.

<p>
  <img src="https://i.imgur.com/uRZOI2V.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>
:)
<p>
  <img src="https://i.imgur.com/noJ5Aoe.png" height="80%" width="80%" alt="Azure VM Provisioning"/>
</p>

Now we will Promote the Server to a Domain Controller:

• In dc-1, open Server Manager and click the flag → Promote the server to Domain Controller: → Set up a new forest →Enter a domain name (mydomain.com) → Set a password → Uncheck “Create DNS” and proceed → System will now auto-restart.

</p>

<br />

<p>
  <strong>Installing Active Directory Domain Services and Promoting the Server:</strong><br>

  <strong> On the Windows Server 2022 VM, open Server Manager and use the Add Roles and Features Wizard to install the "Active Directory Domain Services" role along with any required features. Once installed, launch the Active Directory Domain Services Configuration Wizard to promote the server to a Domain Controller. Choose to create a new forest (for example, <code>mydomain.local</code>), configure DNS settings, and complete the wizard, which will automatically restart the server. </strong>

   <p>
  <img src="https://i.imgur.com/1nDZ4jI.png" height="80%" width="80%" alt="AD Installation and Promotion"/>
</p>
  
Sign In with Domain User Credentials:

• Use mydomain.com\labuser and password to log in to dc-1.

• Confirm that users can sign in with their domain credentials.

<strong> Create Organizational Units (OU) </strong>

• Open Active Directory Users and Computers on dc-1.

• Navigate to mydomain.com.

• Right-click the domain → Select New → Organizational Unit (OU).

<p>
  <img src="https://i.imgur.com/S8YZZjq.png" height="80%" width="80%" alt="AD Installation and Promotion"/>
</p>

• Name it accordingly (e.g., Employees, Admins, Clients).

<strong> Create and Assign Users: </strong>

• Right-click Admins or Users OU → New → User → Enter username & password → Right-click the user account → Properties → Member of → Add to “Domain Admins” (if admin privileges are needed).

<p>
  <img src="https://i.imgur.com/toOhTSN.png" height="80%" width="80%" alt="AD Installation and Promotion"/>
</p>

<STRONG> Assign User Roles: </strong>

• Log out of dc-1 and log back in as the created user.

• Rename the PC if necessary.
  
</p>
<br />

<p>
  <strong>Joining Windows 10 Client to the Domain and Validating the Setup:</strong><br>
  
  On the Windows 10 VM, adjust the network settings to designate the Windows Server's IP address as the primary DNS server. Open the System Properties, select "Change settings" under Computer Name, and join the machine to the newly created domain by entering the domain name and appropriate credentials. After a reboot, verify the domain join by logging in with a domain account and using PowerShell (e.g., running <code>Get-ADUser</code>) to confirm connectivity with Active Directory.
  
<strong> Join Client Machine (VM2) to the Domain: </strong>

• Log into Client-1 (VM2).

• Open System Properties → Advanced System Settings.

• Under Computer Name → Click Change.

<p>
  <img src="https://i.imgur.com/hXn8rLA.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>

<p>
  <img src="https://i.imgur.com/P7aCcC3.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>


• Select Domain, enter mydomain.com, and provide admin credentials.

• Click OK and restart Client-1.

<p>
  <img src="https://i.imgur.com/Uhfm4sN.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>

<strong> Verify Domain Enrollment </strong>

<p>
  <img src="https://i.imgur.com/iUIUadc.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>

• Log back into dc-1.

• Open Active Directory Users and Computers.

• Navigate to mydomain.com → Expand Computers.

• Verify that Client-1 is listed.

<p>
  <img src="https://i.imgur.com/ciEW0hh.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>


• Right-click mydomain.com → Create a new OU named “Clients”.

<p>
  <img src="https://i.imgur.com/xCtxaZg.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>

• Drag Client-1 into the Clients OU.

<p>
  <img src="https://i.imgur.com/IRDDbYk.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>


<strong> Enable Remote Desktop Access for Domain Users: </strong>

• Log into Client-1 as user.mydomain.com → Open System Properties → Remote Desktop → Navigate to User Accounts → Select Users → Add → Enter Domain Users → Click OK → Now, all domain users can log into Client-1.

<p>
  <img src="https://i.imgur.com/Pa94H6X.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>

<p>
  <img src="https://i.imgur.com/0Q9mEnE.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>


<strong> Apply Group Policies via PowerShell: </strong>

• Log into dc-1 as a domain admin (user.domain.com (admin)).

• Open PowerShell.

• Paste and execute any required PowerShell scripts for automation, security, or user/group policies.
</p>
<br

<h1>And just like that, Active Directory is ready to use!</h1>
