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

<h2>Steps in Creating Resource Group, Virtual Network, and Virtual Machines</h2>

**1. Setup Domain Controller VM in Azure**
---
1. Open the [Azure](https://portal.azure.com/) Portal
2. Create a new **Resource Group**
3. Create a **Virtual Network (VNet)** and add a subnet
4. Deploy a **Windows Server 2022** Virtual Machine named `DC-1`
5. Configure the administrator credentials: **Username/Password**

`DC-1` has been successfully deployed in Azure within the configured Resource Group and Virtual Network. It is now running and properly connected to the designated subnet, making it ready for network configuration and further Active Directory setup.

> [!NOTE]
> Ensure the VM is deployed in the selected region and connected to the created Virtual Network
<img width="1174" height="226" alt="image" src="https://github.com/user-attachments/assets/4cd40abd-9120-443e-b930-57e18826b1e6" />


**2. Domain Controller VM configuration**
---
1. Navigate to `DC-1` in the Azure Portal
2. Open the `Network Interface Card` (NIC) settings
3. Under `IP configurations`, change the Private IP from **Dynamic** to **Static** and save

The Domain Controllers' IP is configured as static to ensure consistent network communication. It must have a static IP so clients can reliably locate it for DNS and authentication services.

 <img width="572" height="621" alt="Screenshot 2026-03-02 183115" src="https://github.com/user-attachments/assets/2344b303-daae-423e-b7be-3ea99c58717b" />

<br>
<br>

**Disable Firewall (Temporary for Testing)**
**Steps**:
1. Connect to `DC-1` using Remote Desktop
2. Open Windows Firewall settings
3. Temporarily disable the **Windows Firewall** for testing connectivity

Firewall is temporarily disabled to allow connectivity testing between virtual machines, allowing `ICMP` ping traffic for testing. `DC-1` has been successfully deployed, connected to the correct Virtual Network and subnet, and is now ready for further Active Directory configuration and role installation.

> [!NOTE]
> In a production environment, firewall rules would be properly configured rather than disabled.


<img width="762" height="669" alt="Screenshot 2026-03-02 183943" src="https://github.com/user-attachments/assets/e7a15d53-6d87-4875-8fcc-96ad3d78415c" />


**3. Setup Client-1 VM in Azure**
---
1. Deploy a **Windows 10** Virtual Machine named `Client-1`
2. Configure the local administrator credentials: **Username/Password**
3. Ensure `Client-1` is deployed in the **same region** and connected to the **same Virtual Network and subnet** as `DC-1`

**`Client-1` VM Configurations:**

1. After deployment, navigate to `Client-1` **Network Interface (NIC)** settings in Azure
2. Under **DNS settings**, change from default to **Custom** and enter `DC-1` Private IP address (`10.0.0.4`)
3. Save the configuration changes
4. Restart `Client-1` from the Azure Portal to apply the updated DNS settings

`Client-1` is now properly configured to use `DC-1` for DNS resolution and is ready for domain connectivity. 

<img width="757" height="160" alt="Screenshot 2026-03-02 185145" src="https://github.com/user-attachments/assets/d211fd43-480d-4e70-8488-c1edfb500633" />
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



