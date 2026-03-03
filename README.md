<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Preparing Active Directory Infrastructure in (Azure)</h1>
This repository demonstrates the implementation of an on-premises-style Active Directory infrastructure within Azure Virtual Machines (VMs). The steps outline the process of creating a Domain Controller (DC) and a client machine, establishing connectivity, and configuring the necessary network and DNS settings for Active Directory Domain Services.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Steps in Creating Resource Group, Virtual Network, and Virtual Machines</h2>

**1. Setup Domain Controller in Azure**

- Create a Resource Group
  - Navigate to the Azure Portal and create a new Resource Group.
- Create a Virtual Network and Subnet
  - Set up a Virtual Network (VNet) and define a subnet within the VNet.
- Create the Domain Controller VM
  - Use the following settings to create a Windows Server 2022 Virtual Machine named `DC-1`:
  - Username: 
  - Password:
 
<img width="1224" height="288" alt="Screenshot 2026-03-02 185024" src="https://github.com/user-attachments/assets/840890bd-ab1b-416b-8cbd-f4d38021a024" />

**2. Domain Controller VM configuration**

- Set the Domain Controller's NIC Private IP Address to Static
  - Navigate to the Network Interface Card (NIC) settings for `DC-1` and configure the Private IP Address to be static.
- Log into the VM and Disable the Windows Firewall
  - For testing connectivity, disable the Windows Firewall temporarily within `DC-1`.

<img width="572" height="621" alt="Screenshot 2026-03-02 183115" src="https://github.com/user-attachments/assets/2344b303-daae-423e-b7be-3ea99c58717b" />
<img width="762" height="669" alt="Screenshot 2026-03-02 183943" src="https://github.com/user-attachments/assets/e7a15d53-6d87-4875-8fcc-96ad3d78415c" />


**3. Setup Client-1 in Azure**

- Create the Client VM
  - Use the following settings to create a Windows 10 Virtual Machine named `Client-1`:
  - Username: 
  - Password: 
- Attach Client-1 to the Same Region and Virtual Network as DC-1
  - Ensure `Client-1` is in the same region and Virtual Network as `DC-1`.
- Set Client-1's DNS Settings to DC-1's Private IP Address
  - Update the DNS settings for `Client-1` to use `DC-1's Private IP address.
- Restart Client-1 from the Azure Portal
  - After updating the DNS settings, restart the `Client-1` VM from the Azure Portal.
 
<img width="757" height="160" alt="Screenshot 2026-03-02 185145" src="https://github.com/user-attachments/assets/d211fd43-480d-4e70-8488-c1edfb500633" />

- Login to Client-1
  - Use the credentials provided to log in.
- Test Connectivity to DC-1
  - Open a command prompt and attempt to ping `DC-1`'s Private IP address.
  - Verify that the ping succeeds.
  - confirms: Network connectivity is good. Firewall isn’t blocking ICMPVMs. They are in the same VNet/Subnet
- Verify DNS Settings on Client-1
  - Open PowerShell on `Client-1` and run the command:
    - `ipconfig /all`
  - confirmed:
✔ Client-1 can reach DC-1
✔ DNS is pointing to DC-1
✔ Network configuration is correct
✔ Ready for domain join
 
<img width="567" height="396" alt="Screenshot 2026-03-02 185645" src="https://github.com/user-attachments/assets/254be868-949c-493b-9a3e-6c74a2e0c960" />
<img width="614" height="439" alt="Screenshot 2026-03-02 185813" src="https://github.com/user-attachments/assets/65b8ca9c-40c9-4e07-9678-76fbdaa204af" />



<h2>Purpose</h2>
The purpose of this repository is to document and showcase the preparation of an Active Directory infrastructure in Azure. This serves as a reference for implementing a similar setup for learning, testing, or production environments.

