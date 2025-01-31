<p align="center">
<img src="https://logos-world.net/wp-content/uploads/2021/02/Microsoft-Azure-Symbol.png" alt="Azure AD Logo" />
</p>

<h1 align="center">Microsoft Azure / Entra ID: Configuring On-premises Active Directory within Azure VMs</h1>

The goal of this exercise is to capture SSH (SecureShell) traffic via Wireshark, connecting via SSH to our Linux VM via PowerShell, and finally experimenting with DHCP (Dynamic Host Configuration Protocol).
---

## **Environments and Technologies Used**

- **Microsoft Azure (Virtual Machines/Compute)**
- **Remote Desktop Protocol (RDP)**
- **Server Manager**
- **Group Policy Management (GPM)**
- **Active Directory Users and Computers (ADUC)**
- [**WireShark**](https://www.wireshark.org/#downloadLink)

---

## **Operating System Used**

- **Windows 10** (21H2)
- **Windows Server 2022 Datacenter: Azure Edition**
- **Ubuntu Server 22.04**

---

## **Prerequisites**

Before starting, make sure you've set up an Azure account.
---

## **Creating Virtual Machines**

---

### **1. Creation of Resource Group **
1. Create a **Resource Group** with a unique name.
2. **Ensure everything you create is in the same Region.**
![RSG](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/1CreationOfGroup.png?raw=true)

---

### **2. Creation of Virtual Machine **
1. Create a **Virtual Machine** with a unique name that runs **Windows 10 Pro**.
2. **Ensure everything you create is in the same Region.**
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/4CreationOfVMs.png?raw=true)
3. Select **Networking**, then **Create New**, and name it accordingly.
![Netwkr](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/6CreationOfSubnetNetwork.png?raw=true)
4. **Create**.

---

### **4. Creation of Second Virtual Machine **
1. Create a **Virtual Machine** with a unique name that runs **Ubuntu Server**.
2. **Ensure everything you create is in the same Region.**
3. Make sure you join your **Virtual Network**.
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/12Finalization.png?raw=true)
4. **Create**.

---

### **5. Connect via RDP to Windows VM **
1. Sign in to your **Windows Machine** via **RDP** with your **credentials** you created previously.
2. Inside your VM, open your browser, and download **WireShark Windows 64x Installer** if you haven't already.
3. In **WireShark**, click on your **Ethernet**, then click **Start Capturing Packets**.
![WS](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/26RDPWireSharkBeginCapture.png?raw=true)
4. In the **Search Bar**, filter by **IMCP**.
5. Find your **Ubuntu Machine's Private IP** in the **Azure Virtual Machines Dashboard** by clicking on it, and scrolling down under **Properties**.
6. Back in your **Windows VM**, open **PowerShell**, and ping the private IP.
![WS](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/30PowershellPing.png?raw=true)
7. Back in **WireShark**, you should clearly see the **IMCP** packets.
![WS](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/31ConfirmationOnWiresharkAndPowershell.png?raw=true)
8. Click on the most recent packet, and inspect **Internet Control Message Protocol**.
![WS](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/33WhatWasSent.png?raw=true)

---

### **6. Inbound Security Rules **
1. In **Powershell**, type **ping 10.0.0.X -t** to begin constant pinging.
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/34EndlessPinging.png?raw=true)
2. Back in the **Azure Virtual Machines Dashboard**, click your **Ubuntu Virtual Machine**, then **Network Settings**.
3. Inside of the **Network Security Group (NSG)**, click **Settings**, then **Inbound security rules**
4. Make the source **Any**, and the Destination port range an **asterisk** to block all traffic.
5. Make the Priority **290**, with a custom name.
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/36LinuxNSGRule.png?raw=true)

---

### **7. Observe the changes in traffic **
1. In your **Windows Virtual Machine**, you should notice the timing out of requests.
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/37LinuxNSGResults.png?raw=true)
2. Return to your **Azure Virtual Machines Dashboard**, and click your **Ubuntu Virtual Machine**, then **Network Settings**.
3. Inside of the **Network Security Group (NSG)**, click **Settings**, then **Inbound security rules**, and **delete your newly created rule**.
![VMs](https://github.com/michael-L2/activeDirectoryGP/blob/main/2AzureComputeExercise/39DisabledNSGResult.png?raw=true)

---

### **Congratulations!**
You've created two Virtual Machines within a Virtual Network, and configured a Network Security Group to control the traffic, and monitored all the changes in real-time with WireShark! 🎉
