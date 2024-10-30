<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Azure Virtual Machines</h1>
In this lab, I analyze network traffic to and from Azure virtual machines using Wireshark and experiment with network security groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 20.04

<h2>The Set-Up</h2>

In Azure, I created two VMs within the same virtual network for intercommunication: one running Windows 10 Pro and the other Ubuntu. The Windows VM will connect to the Ubuntu VM using the command line/PowerShell. 

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DutP0XT.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Using Remote Desktop Connection, I connect to the Windows VM using its public IP address. From there, I installed Wireshark in order to begin inspecting traffic. 
</p>
<br />

<p>
<img src="https://i.imgur.com/2JQawfN.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

In Wireshark, I filtered for ICMP traffic and used PowerShell to execute the ping command, which utilizes ICMP for network communication issues. I tested connectivity with the Ubuntu VM's private IP address and with google.com. Then, I performed a perpetual ping to the Ubuntu VM using the command: ping -t (IP address) to observe how network security groups function.
</p>
<br />

<p>
<img src="https://i.imgur.com/HTga2Iq.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
In the Azure portal, I accessed the networking settings for the Ubuntu VM and added an inbound security rule to block ICMP traffic, setting its priority higher than SSH (300) to ensure it applies first.







</p>
<br />

<p>
<img src="https://i.imgur.com/i3BC2LW.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

Back on the Windows VM, I confirmed that ICMP traffic is blocked due to the inbound security rule. After changing the rule to allow traffic again, the perpetual ping successfully resolves without timing out. 
</p>
<br />

<p>
<img src="https://i.imgur.com/fDtuLo9.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Next, I examined SSH traffic by logging into the Ubuntu server via PowerShell using the ssh command. In Wireshark, I filtered the traffic with tcp.port == 22. My session is recorded in Wireshark, capturing each command I execute while logged into the Ubuntu server.

</p>
<br />

<p>
<img src="https://i.imgur.com/mptFClI.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
After examining SSH traffic, I exited the Ubuntu server to filter for DHCP traffic. I issued a new IP address from my VM using the command ipconfig /renew, which temporarily disconnected me for a few seconds. Upon reconnecting, the resulting traffic was captured in Wireshark.

</p>
<br />

<p>
<img src="https://i.imgur.com/gtiupfH.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
To observe DNS traffic, I used the filter udp.port == 53 and the command nslookup. I wanted to see the results that are from looking up google.com and disney.com, two very popular sites. 
</p>
<br />

<p>
<img src="https://i.imgur.com/N7voXYU.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
To finish my lab, I decided to observe RDP traffic. The filter for Wireshark is tcp.port == 3389. There is non-stop traffic because RDP is constantly showing me a live stream from one computer to another (in my case, my computer accessing the VM that is hosted on Azure) and thus traffic is always transmitted. 
</p>
<br />

<h2>What we learned </h2>

The purpose of this lab is to observe how different protocols and ports are used in network communication. Although it doesn’t directly enable troubleshooting, it helps gather valuable information. Successful troubleshooting requires tools like Wireshark and the command line to understand network traffic flow.
