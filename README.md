<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up Resources in Azure
- Install Active Directory
- Join Client-1 to the Domain
- Configure Remote Desktop for Non-Administrative Users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://imgur.com/4GBI1zE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Section 1: Setup Resources in Azure

Create the Domain Controller VM:

Launch a new virtual machine in Azure with Windows Server 2022 as the operating system.
Name the VM "DC-1".
Note down the Resource Group and Virtual Network (Vnet) created during this process.
Set the Domain Controller's NIC Private IP address to be static.
Create the Client VM:

Create another virtual machine in Azure with Windows 10 as the operating system.
Use the same Resource Group and Vnet from step 1.
Ensure that both VMs are in the same Vnet.
Verify Connectivity between the client and Domain Controller:

Log in to "Client-1" using Remote Desktop and open Command Prompt.
Ping the private IP address of "DC-1" using the command: ping -t <DC-1 IP address>.
If the ping is successful, proceed to the next step.
Enable ICMPv4 on the Domain Controller:

Log in to "DC-1" using Remote Desktop.
Open Windows Firewall settings and allow ICMPv4.
Ensure that the firewall rule allows ping requests.
Verify that the ping from "Client-1" to "DC-1" succeeds.
</p>
<br />

<p>
<img src="https://imgur.com/9Mqi6eg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Section 2: Install Active Directory

Install Active Directory Domain Services on the Domain Controller:

Log in to "DC-1" using Remote Desktop.
Install Active Directory Domain Services role.
Set up a new forest with a domain name (e.g., "mydomain.com").
Restart "DC-1" and log back in as user: "mydomain.com\labuser".
Create Admin and Normal User Accounts in AD:

Open Active Directory Users and Computers (ADUC).
Create an Organizational Unit (OU) named "_EMPLOYEES".
Create another OU named "_ADMINS".
Create a new employee named "Jane Doe" with the username "jane_admin" (same password).
Add "jane_admin" to the "Domain Admins" Security Group.
Log out/close the Remote Desktop connection to "DC-1" and log back in as "mydomain.com\jane_admin".
Use "jane_admin" as the admin account from now on.
</p>
<br />

<p>
<img src="https://imgur.com/Wc0ijUv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Section 3: Join Client-1 to the Domain and Configure Remote Desktop

Join Client-1 to the domain (mydomain.com):

In the Azure Portal, set Client-1's DNS settings to the DC's Private IP address.
Restart Client-1 from the Azure Portal.
Log in to Client-1 using Remote Desktop as the original local admin ("labuser").
Join Client-1 to the domain and wait for the computer to restart.
Log in to the Domain Controller using Remote Desktop and verify that Client-1 appears in ADUC inside the "Computers" container in the root of the domain.
Create a new OU named "_CLIENTS" and move Client-1 into it.
Set up Remote Desktop for non-administrative users on Client-1:

Log in to Client-1 as "mydomain.com\jane_admin" using Remote Desktop.
Open System Properties on Client-1.
Click "Remote Desktop" and allow "domain users" access to Remote Desktop.
Non-administrative users can now log into Client-1 using Remote Desktop.
Create Additional Users and Test Login on Client-1:

Log in to "DC-1" as "jane_admin" using Remote Desktop.
Open PowerShell_ise as an administrator.
Create a new file and paste the script contents from this link: GitHub Script.
Run the script and observe the accounts being created.
Open ADUC and verify that the accounts are in the appropriate OU.
Attempt to log into Client-1 with one of the newly created user accounts (note the password from the script).</p>
<br />
