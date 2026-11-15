<h1>SIEM Home Lab and XDR solution in Wazuh</h1>

<h2>Description</h2>
(Documentation in progress) 

In this project, I installed and experimented with Wazuh on a home computer. Wazuh is a free and open source security platform that unifies XDR and SIEM capabilities. It protects workloads across on-premises, virtualized, containerized, and cloud-based environments. The goal of this project is to gain hands-on experience with risk assessment and monitor vulnerabilities. Before getting this up and running, there are a few things I'll need!

<br />


<h2>Utilities Used</h2>

- <b>Oracle Virtual Box</b> 

<h2>Environments Used </h2>

- <b>Windows 11 on Main PC</b>
- <b>Ubuntu Server on Virtual Box VM</b>

<h2>Project Architecture:</h2>
<p align="center">
<img src="https://imgur.com/7IWkuFw.png" height="80%" width="80%" alt="Home Lab Architecture"/>

<p align="center">

<h2>Project walk-through:</h2>

<p align="center">
Install and Launch Virtual Box: <br/>
<img src="https://imgur.com/eXOCy4f.png" height="50%" width="50%" alt="Virtualbox Installer"/>
<br />
<br />
Create a new Ubuntu Server Virtual Machine and configure settings:  <br/>
<img src="https://imgur.com/xpfy3z2.png" height="60%" width="60%" alt="VM Creation Prompt"/>
<img src="https://imgur.com/t1zJpNT.png" height="60%" width="60%" alt="VM Creation Prompt"/>

- Important Settings: 4 Cores, 50GB Storage, 4GB Ram, 1 Bridged Adapter
<br />
<br />
<p align="center">
Start the machine, Follow Ubunter Server Installation Prompts, Sign In: <br/>
<img src="https://imgur.com/A8kVgcK.png" height="50%" width="50%" alt="Ubuntu Server Install"/>

- Once signed in, be sure to run the update commands!
- sudo apt update
- sudo apt upgrade
- Installation of Wazuh can officially begin!
<br />
<br />
<p align="center">
Add the Wazuh GPG Key:<br/>
<img src="https://imgur.com/HKhl6zj.png" height="90%" width="90%" alt="Internal Network IPV4 Properties Config"/>
  
- Via a terminal window [ curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg ] 
- This adds the GPG key to verify Wazuh packages.
<br />
<br />
<p align="center">
Download and Execute Wazuh Installation Script:<br/>
<img src="https://imgur.com/gCKoKxs.png" height="90%" width="90%" alt="Internal Network IPV4 Properties Config"/>
  
- Via a terminal window [ curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash./wazuh-install.sh -a -i ]
- -a: Installs all components (manager, indexer.)
- -i: Runs in interactive mode
- The script installs all required services and configures them automatically.
- Once installed, the login details will be shown in the terminal. Be sure to save this login for later!
- Now we can access the Dashboard!
<br />
<br />
<p align="center">
Check your Ubuntu VM’s IP address with ifconfig:<br/>
<img src="https://imgur.com/T2Zd9xn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
On your Ubuntu server, open a browser and go to https://< ubuntu-vm-ip >:<br/>
<img src="https://imgur.com/46MwpMj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p align="center">
Now we can see the Dashboard!
<img src="https://imgur.com/TpvO02R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
Download and install the latest Wazuh agent on Host Machine:<br/>
<img src="https://imgur.com/5RXpLED.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
<p align="center">
On the Ubuntu Server, Generate Agent Key on Ubuntu Manager:<br/>
<img src="https://imgur.com/0v7ocN7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
- sudo /var/ossec/bin/manage_agents
- Select A to add an agent.
- Assign a name (e.g., WindowsHost).
- Leave IP address blank unless static assignment is needed.
- After creation, select E to extract the key.
- Copy the key output.
<br />
<br />
<p align="center">
On the Windows Host:<br/>
<img src="https://imgur.com/kFa7wkv.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

- Open Wazuh Agent Manager GUI from the Start Menu.
- Paste the copied key into the appropriate field.
- Save and apply the key.
- Add the manager's IP address (IP address of your Ubuntu manager).
- Restart the agent service.
<br />
<br />
<p align="center">
We can then go to the WAZUH dashboard and see the agent on-boarded:<br/>
<img src="https://imgur.com/8MZn4xT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<h2>File Integrity Monitoring:</h2>
<p align="center">
Wazuh supports real-time monitoring of file and folder changes using Syscheck.:<br/>
<br />
<br />
<p align="center">
On the host machine, Open the following configuration file:<br/>
<p align="center">
<img src="https://imgur.com/oUafrxL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
- Open the following configuration file as admin: C:\Program Files (x86)\ossec-agent\ossec.conf
- Add the following entry inside the Directory block: <directories realtime="yes">C:\Users\abc\Test</directories>
- Make sure to create the test folder first, then copy the path to the directory entry
- This monitors the specified folder in real-time.
- Save changes and restart the agent
<br />
<br />
<p align="center">
Verify the Integrity monitoring:<br/>
<p align="center">
<img src="https://imgur.com/j9zvrcf.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/DXoL8Sp.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

- Open the Wazuh Dashboard in your browser.
- Navigate to Agents → ensure the Windows agent is listed and status is Active.
- Go to the Integrity Monitoring section.
- Perform actions (create/modify/delete files) in the monitored folder.
- Confirm that alerts appear in the dashboard.
<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
