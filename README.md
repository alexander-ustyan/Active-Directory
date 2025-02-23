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
</p>
<br />

<p>
  <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Installation and Promotion"/>
</p>
<p>
  <strong>Step 2: Installing Active Directory Domain Services and Promoting the Server</strong><br>
  On the Windows Server 2022 VM, open Server Manager and use the Add Roles and Features Wizard to install the "Active Directory Domain Services" role along with any required features. Once installed, launch the Active Directory Domain Services Configuration Wizard to promote the server to a Domain Controller. Choose to create a new forest (for example, <code>mydomain.local</code>), configure DNS settings, and complete the wizard, which will automatically restart the server.
</p>
<br />

<p>
  <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client Domain Join and Validation"/>
</p>
<p>
  <strong>Step 3: Joining Windows 10 Client to the Domain and Validating the Setup</strong><br>
  On the Windows 10 VM, adjust the network settings to designate the Windows Server's IP address as the primary DNS server. Open the System Properties, select "Change settings" under Computer Name, and join the machine to the newly created domain by entering the domain name and appropriate credentials. After a reboot, verify the domain join by logging in with a domain account and using PowerShell (e.g., running <code>Get-ADUser</code>) to confirm connectivity with Active Directory.
</p>
<br
