<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Preparing Active Directory Infrastructure in (Azure)</h1>
This repository demonstrates the deployment of an on-premises-style Active Directory infrastructure using Microsoft Azure Virtual Machines (VMs). The project walks through provisioning a Domain Controller (DC) and a client machine, configuring network connectivity, and implementing the required DNS settings to support Active Directory Domain Services.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

**1. Setup Domain Controller VM in Azure**
---
***CREATE RESOURCE GROUP.***

Created a resource group to organize and manage all Azure resources for the Active Directory lab. I begin by logging in to the [Azure](https://portal.azure.com/) Portal, typing "Resource Group" in the search bar, selecting it, and pressing **Create** to create a resource group. I named it "rg-active-directory-lab", selected **Review + Create**, and then **Create**. 

<img width="1119" height="709" alt="Screenshot 2026-04-10 202145" src="https://github.com/user-attachments/assets/5785d2fe-6cd6-4c2a-bdec-b9283fe8d78b" />
<img width="813" height="703" alt="Screenshot 2026-04-10 211121" src="https://github.com/user-attachments/assets/1cbbe209-dfdd-499a-bb5a-bcf041ad6306" />

<br>
<br>
<br>

***CREATE VIRTUAL NETWORK.***

In Azure, typed "Virtual Network" in the search bar, pressed **Virtual Network**, and selected **create**. During creation, I selected the previously created Resource Group "rg-active-directory-lab", and named the Virtual Network "vnet-active-directory-lab". A Virtual network enables secure communication between Azure VMs.

<img width="816" height="703" alt="Screenshot 2026-04-10 211247" src="https://github.com/user-attachments/assets/f7842141-c51d-4625-a391-655d854b45b6" />

<br>
<br>
<br>

***CREATE DOMAIN CONTROLLER VM***

In Azure, typed "Virtual Machine" in the search bar and selected **+ Create** to start VM creation. Configured the virtual machine by selecting the resource group "rg-active-directory-lab", naming the VM "DC-1", selecting **Windows Server 2022** for **Image**, and assigning an appropriate VM size. Configured the administrator credentials, **username**, and **password** in the Admin Account section.

<img width="814" height="1289" alt="Screenshot 2026-04-10 211602" src="https://github.com/user-attachments/assets/ec288a74-6741-4888-99ab-7d4f5473604e" />

<br>
<br>
<br>

Navigated to the Networking tab. For Virtual Network, I selected the previously created vnet "rg-active-directory-lab", assigned a public IP address, and selected 'default' for Subnet. Once the networking settings are configured, select "Review + Create," then "Create". 

<img width="848" height="1302" alt="Screenshot 2026-04-10 211624" src="https://github.com/user-attachments/assets/b2943ae0-13d7-4d14-ab5f-7b23da956bd0" />

<img width="1319" height="388" alt="Screenshot 2026-04-10 212231" src="https://github.com/user-attachments/assets/ff30e45b-65d3-4e82-9f36-616d9e7a63d6" />

<br>
<br>
<br>

***DOMAIN CONTROLLER VM CONFIGURATION***

Once the DC-1 VM is created, view the VM by typing "virtual Machine" in the search bar and navigating to > **Virtual Machines** > **DC-1**. On the left panel, located **Networking** > **Network Settings**, and select the VM's **Network Interface** under **Essentials**. On the left pane, in the drop-down select **Settings** > **IP Configurations** > **ipconfig1**, and set Private IP
Allocation settings to **Static**. Once I selected **Save**, I went back to the listed DC-1 VM and noted and copied the Domain Controller's Public IP. 
The Domain Controllers' IP is configured as static to ensure consistent network communication. It must have a static IP so clients can reliably locate it for DNS and authentication services.

<img width="1048" height="819" alt="Screenshot 2026-04-10 212605" src="https://github.com/user-attachments/assets/2c2f1736-b96e-4c63-9e35-58c914329ab5" />

<br>
<br>
<br>

****Disable Firewall on DC-1 VM (Temporary for Testing)****

On my local computer, I used Remote Desktop Client to connect to DC-1 VM using its public IP address (20.83.153.138). To connect, I enetred the correct credentials created when the VM was created.   

<img width="1381" height="613" alt="Screenshot 2026-04-10 213001" src="https://github.com/user-attachments/assets/25ad1938-1ef6-4450-ab8c-85e5e596b9bc" />

<br>
<br>
<br>

Next, select **Yes** to proceed—your Domain Controller will begin booting. Once it has fully started, open **Windows Defender Firewall with Advanced Security**, navigate to **Windows Defender Firewall Properties**, and set the **Firewall State** to **Off** for the Domain, Public, and Private profiles. Click **Apply** to save the changes.

**Reason:**

The firewall is temporarily disabled to allow connectivity testing between virtual machines, permitting `ICMP` ping traffic. DC-1 has been successfully deployed, connected to the correct Virtual Network and subnet, and is now ready for further Active Directory configuration and role installation.


<img width="911" height="736" alt="Screenshot 2026-04-10 213841" src="https://github.com/user-attachments/assets/22168902-e12c-4377-a31f-c8973e2f4082" />

> [!NOTE]
> In a production environment, firewall rules would be properly configured rather than disabled.

***CREATE CLIENT VM***

In the Azure portal, navigate to Virtual Machine creation by typing "Virtual Machine" in the search bar. We select **+ Create** to create a Virtual Machine and configure the virtual machine by selecting your subscription and the "rg-active-directory-lab" resource group, naming the VM Client-1, selecting the Windows 10 Enterprise version 22H2 (x64 Gen2) image with x64 architecture, and using the Standard D2s_v3 (2 vCPUs, 8 GiB RAM) size.

<img width="883" height="1299" alt="Screenshot 2026-04-10 214235" src="https://github.com/user-attachments/assets/51f34805-3fdc-40fc-b8ad-197999d63147" />


Ensure `Client-1` is deployed in the same **region** and connected to the same **Virtual Network and subnet** as `DC-1`

**`Client-1` VM Configurations:**

1. After deployment, navigate to `Client-1` **Network Interface (NIC)** settings in Azure
2. Under **DNS settings**, change from **default** to **Custom** and enter `DC-1` Private IP address (`10.0.0.4`)

To locate `DC-1` Private IP address:

- Navigate to Virtual Machines in the Azure Portal
- Select the DC-1 virtual machine
- View the Overview page to identify networking details
- Locate the assigned Private IP Address (10.0.0.4)

<img width="757" height="160" alt="Screenshot 2026-03-02 185145" src="https://github.com/user-attachments/assets/d211fd43-480d-4e70-8488-c1edfb500633" />

<br>
<br>

4. Save the configuration changes
5. Restart `Client-1` from the Azure Portal to apply the updated DNS settings

`Client-1` is now properly configured to use `DC-1` for DNS resolution and is ready for domain connectivity. 

<img width="1224" height="288" alt="Screenshot 2026-03-02 185024" src="https://github.com/user-attachments/assets/840890bd-ab1b-416b-8cbd-f4d38021a024" />

 
**4. Validate Connectivity and Configuration**
---

**Test connectivity to `DC-1`**

**Steps**:

1. Log into `Client-1`
2. Open Command Prompt
3. Ping `DC-1` private IP `(10.0.0.4)`

**Successful replies confirm:**
  * Network connectivity is functioning properly
  * The firewall is not blocking ICMP traffic
  * Both VMs are on the same Virtual Network and subnet

> [!NOTE]
>  If ping fails, verify that both VMs are in the same VNet and that firewall or NSG rules are not blocking ICMP traffic.

<img width="567" height="396" alt="Screenshot 2026-03-02 185645" src="https://github.com/user-attachments/assets/254be868-949c-493b-9a3e-6c74a2e0c960" />

<br>
<br>

**Verify DNS settings on `Client-1`**
- Open PowerShell and run: `ipconfig /all`

> [!NOTE]
> Active Directory relies on DNS to locate domain services, making this step essential for domain communications.

<img width="614" height="439" alt="Screenshot 2026-03-02 185813" src="https://github.com/user-attachments/assets/65b8ca9c-40c9-4e07-9678-76fbdaa204af" />


## Verification Results

- `Client-1` can reach `DC-1`
- DNS is correctly pointing to `DC-1`
- Network configuration is properly set
- The environment is ready for domain join

<h2>Purpose</h2>

The purpose of this repository is to document and demonstrate the process of preparing an Active Directory infrastructure within Microsoft Azure. It provides a structured reference for building a cloud-based environment that simulates a traditional on-premises Active Directory setup.
This project outlines the key steps for configuring virtual machines, networking, DNS settings, and domain services to establish a functional Active Directory environment. It is intended to support learning, testing, and experimentation with identity and infrastructure management in a cloud environment.



