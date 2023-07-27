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

<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
