<h1>Azure Sentinel (SIEM) Honeypot Home Lab</h1>

<img src="https://i.imgur.com/uQMrbdI.png" height="80%" width="80%" alt="Azure Homelab Cover"/>

<h2>Description</h2>
Welcome to the Azure Sentinel Honeypot Homelab walkthrough! In this guide, we will explore how to set up and utilize a powerful and educational Homelab using Microsoft Azure Sentinel. Honeypots are decoy systems designed to attract and monitor malicious activity, providing valuable insights into potential threats and attackers' tactics. A SIEM (Security Information and Event Management) is a comprehensive security solution that helps organizations collect, analyze, and respond to security events in real-time. With Azure Sentinel, Microsoft's cloud-native SIEM (Security Information and Event Management) solution, we can gain a comprehensive view of security events and automate threat detection and response. Unleash the power of our homelab where cybersecurity meets innovation! Track and log attacks from around the globe and witness our mesmerizing attack map take shape. Discover the thrilling world of cyber warfare with us!
<br/>

<h2>Learning Objectives </h2>

- Setting up and rolling out various Azure components including Virtual Machines (VMs), Log Analytics Workspaces, and Azure Sentinel 
- Competence and experience with Microsoft Azure Sentinel, a SIEM (Security Information and Event Management) Log Management Tool
- Third-party API Calls
- Using KQL to query logs
- Learn how to read the Security Event Logs in Windows
- Utilize Workbooks (World Map) to make an interactive map showing attack statistics



<h2>Technologies + Requirements </h2>

- Microsoft Azure + Account 
- Azure Services: Sentinel, Log Analytics Workspace, Workbooks, Network Security Groups
- Powershell
- Remote Desktop Protocol (RDP)
- Third-party API: <a href="https://ipgeolocation.io/"> ipgeolocation.io </a>
- Customized <a href="https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1"> Powershell Script </a> authored by Josh Madakor

<h2>Overview:</h2>
<img src="https://i.imgur.com/Ib6Deyt.png" height="100%" width="100%" alt="Azure Homelab Schematic"/>

<h2>Step 1: Create a Microsoft Azure Account: <a href="https://azure.microsoft.com/en-us/free/"> Azure </a></h2>

> Microsoft offers $200 in Azure credit for 30 days when you initially sign up

<img src="https://i.imgur.com/BzmjVa4.png" height="100%" width="100%" alt="Azure Sign Up"/>

<h2>Step 2: Setup our honey pot virtual machine</h2>

> Vulnerable Windows VM

<img src="https://i.imgur.com/nUDrnbz.png" height="100%" width="100%" alt="Honeypot VM"/>

<h3>Basics</h3>

- After signing up, click "Go to the Azure Portal" , or visit `portal.azure.com`
- In the search bar type "`virtual machines`"
- Under Create tab click on Azure virtual machine

<h4>Project Details</h4>

- Create a new resource group and give it a name (honeypot-lab)

> A resource group is a container that helps organize and manage related cloud resources.

<h4>Instance Details</h4>

- Give your virtual machine a name (honeypot-vm)
- Choose a recommended region: ((US) West 3)
- Availability options: No infrastructure redundancy required
- Security type: Standard
- Image: Windows 10 Pro, version 22H2 - x64 Gen2
- VM Architecture: x64
- Size: Default is fine (Standard_D2s_v3 – 2vcpus, 8 GiB memory)

<h4>Administrator account</h4>

- Set up a username and password for the virtual machine.

> IMPORTANT: these identification details will be used to log into the virtual machine. (Make sure to keep them in mind) 

<h4>Inbount port rules</h4>

- Public inbound ports -> Allow selected ports: RDP (3389)

<h4>Licensing</h4>

- Confirm Licensing
- Select <b>Next : Disks ></b>

<img src="https://i.imgur.com/EvvnrNQ.png" height="80%" width="80%" alt="Honeypot VM Config"/>

<h3>Disks</h3>

- Leave everything as is
- Select <b>Next : Networking > </b>

<h3>Networking</h3>

<b>Network interface</b>
- NIC network security group: Advanced -> Create new

> A Network Security Group (NSG) in Azure is a virtual firewall that filters and controls network traffic to protect Azure resources.

- By clicking the three dots, delete Inbound rules (1000: default-allow-rdp)
- Add an inbound rule
- Destination port ranges: * (wildcard for anything)
- Protocol: Any
- Action: Allow
- Priority: 100 (low)
- Name: Anything (allow-any-inbound)
- Select <b>Review + Create</b>

<img src="https://i.imgur.com/jMlr4UE.png" height="100%" width="100%" alt="Network Security Group"/>

> The VM will be simple to detect if the firewall is set up to accept traffic from anywhere.

<h2>Step 3 : Provisioning a Log Analytics Workspace</h2>

- Search for "Log analytics workspaces"
- Select <b>Create Log Analytics workspace</b>
- Place it in the identical resource group as the VM (honeypot-lab) 
- Give it the name you choose (honeypot-law) 
- Add to the same region (West US 3)
- Select <b>Review + Create</b>

<img src="https://i.imgur.com/N3fwTkR.png" height="70%" width="70%" alt="Log Analytics workspace"/>

> Custom logs with geographic information to map the locations of attackers will be absorbed into Log Analytics workspaces together with the Windows Event Viewer logs.

<h2>Step 4 : Setup Microsoft Defender for Cloud</h2>

- Search for "Microsoft Defender for Cloud"
- Under <b>Management</b> click on "Environment settings" -> Subscription Name -> Log Analytics Workspace Name (honeypot-law)

<img src="https://i.imgur.com/yXxSvyR.png" height="100%" width="100%" alt="Microsoft Defender"/>

<b>Settings | Defender plans</b>

- Foundational CSPM (Cloud Security Posture Management): ON
- Servers: ON
- SQL servers on machines: OFF
- Click <b>Save</b>

<img src="https://i.imgur.com/ony2xay.png" height="100%" width="100%" alt="Microsoft Defender 2"/>

<b>Settings | Data collection</b>

- Select "All Events"
- Click <b>Save</b>

<h2>Step 5 : Link Virtual Machine to the Log Analytics Workspace</h2>

- Look for "Log Analytics workspaces"
- Select workspace name (honeypot-law) -> "Virtual machines" -> virtual machine name (honeypot-vm)
- Hit <b>Connect</b>

<img src="https://i.imgur.com/I0xy678.png" height="100%" width="100%" alt="Link LAW to VM"/>

<h2>Step 6 : Setup Microsoft Sentinel</h2>

- Look for "Microsoft Sentinel"
- Hit <b>Create Microsoft Sentinel</b>
- Choose Log Analytics Workspace name (honeypot-law)
- Hit <b>Add</b>

<img src="https://i.imgur.com/t6jbVLT.png" height="100%" width="100%" alt="Microsoft Sentinel"/>

<h2>Step 7 : Turn OFF the Virtual Machine's firewall</h2>

> Testing Firewall - first we can test the firewall by pinging the VM's IP address. The purpose of this is to check if the target device with that specific IP address is reachable and responsive on the network. <b>"PING"</b> is a basic network troubleshooting tool used to test connectivity between your device and the target device.

<h4>For Windows:</h4>
<ol>
<li>Open the Command Prompt: Press the Windows key, type "cmd," and press Enter to open the Command Prompt</li>
<li>Type the Ping Command: In the Command Prompt window, type the following command and press Enter:</li>
</ol>
   
```ping (ip-adress)```

3. Replace <IP-address> with the actual IP address you want to ping. For example:

```ping 20.163.108.170```

<h4>For macOS and Linux</h4>
<ol>
<li>Open the Terminal: On macOS, you can find the Terminal in Applications > Utilities. On Linux, press Ctrl + Alt + T to open the Terminal.</li>
<li>Type the Ping Command: In the Terminal window, type the following command and press Enter:</li>
</ol>
   
```ping (ip-adress)```

3. Replace <IP-address> with the actual IP address you want to ping. For example:

```ping 20.163.108.170```

<h4>Both :</h4>

- Observe the Results: the ping command will start sending packets to the specified IP address, and you'll see the results with round-trip times and other statistics. To stop the ping process on both Windows and macOS/Linux, you can press Ctrl + C or Ctrl + Z
  
<img src="https://i.imgur.com/myGs1oV.png" height="100%" width="100%" alt="Microsoft Sentinel"/>

> As you can see above our firewall is set up to block out incoming ping requests. Next we will continue to disable the firewall...

- Look for "Microsoft Sentinel
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"
- Look for "Microsoft Sentinel"

<img src="https://i.imgur.com/D4uFicX.png" height="100%" width="100%" alt="Microsoft Sentinel"/>
