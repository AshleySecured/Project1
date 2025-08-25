# Active Directory in VirtualBox with Automated User Provisioning (PowerShell)

## About The Project 💡

Welcome to Project 1 of my System & Network Security series. In this project, I demonstrate how to build and secure a network from the ground up—showcasing the full process from initial setup to a hardened environment, documenting my progress and difficulties. 

## 🔧 Built With
#### Oracle VirtualBox  
– for creating and managing the virtualized lab environment

#### Windows Server 2019 ISO 
– to deploy and configure Active Directory

#### Windows 11 (host machine) 
– base operating system running the virtual environment

#### PowerShell 
– for automating user creation and administrative tasks

#### Networking Configurations (DHCP, DNS, RAS, NAT) 
– for setting up secure network services

## 🚀Getting Started

This project was originally inspired by <b>Josh Madakor</b>, who created an excellent walkthrough video on building a virtual Active Directory environment from start to finish. To gain a strong foundation, I first followed his tutorial step-by-step to understand the process.

After completing it once, I challenged myself to recreate the <b>entire project from scratch—without referencing the walkthrough</b>. In my YouTube video (TBA), you’ll see me apply what I learned, troubleshoot in real-time, and demonstrate my ability to independently build and secure the environment. Please feel free to check it out if you're interested.

## ⚙️Installation & Setup

To get started, download the required programs to set up your virtual environment. If you don’t already have Windows, install <b>Windows 10 or 11</b>—either works fine.

Next, install <b>Oracle VirtualBox</b> to create a safe, sandboxed environment where you can experiment without affecting your main system. Don’t forget the <b>Oracle VirtualBox Extension Pack</b>, which adds extra features we’ll need later.

You will also need to download and install the <b>Windows Server 2019 ISO</b>. This will serve as the backbone of your <b>Active Directory Domain Controller</b>. Be prepared for long load times throughout this process—patience is important as VirtualBox can take a while to fully spin up environments.

For more detailed instructions—such as what to check, uncheck, or type in the setup screens—please reference [<b>Josh Madakor’s YouTube tutorial here</b>](https://youtu.be/MHsI8hJmggI?si=rVuRlcRrg_Vgz-mS). He provides an excellent walkthrough with clear visuals.

For reference, I recreated my own notes from Josh's video of the Network Diagram to setup a successful environment. Please note, this diagram I drew from my own computer as reference for myself in the future. You can see Josh's original diagram in his video. <img width="856" height="415" alt="Screenshot 2025-08-19 120649" src="https://github.com/user-attachments/assets/36343fda-3946-49af-8350-9b02a9f504d0" />


## Setting Up NAT and Intranet in VirtualBox

<b>Open VirtualBox Application.</b>

*Go to New → Vitrual Machine Name → Type 'DC' → Windows Version: Windows 10 64-bit.*

*Click the Ok button to establish the DC Server*

*Next, go to Settings -> General -> Under 'Features' choose 'Bidirectional' for Shared Clipboard & Drag-n-Drop*

*Then, on the same Settings page, go to Network and checkbox Adapter 2*
*Since we already have our built in NAT Network, we need to have an Internal Network as well*
*Once checked, name this network INTRANet*

## Installing Windows Server 2019 ISO

<b>Double click on your DC Server to run it:</b>

*Now that our DC Server is up and running, we can upload Windows Server 2019*
*Open the DC Server and select the 'upload' icon*
*Upload -> Server 2019 ISO file -> START*

<b>Windows Server 2019 will pop up. Give this time to load.</b>
*Once uploaded, click Next -> Install -> DESKTOP EXPERIENCE (Standard)*
*Accept Terms -> CUSTOM: Install Windows Only -> Next -> SERVER WILL RESTART.*

<b>Windows Server 2019 ISO is now installed</b>

## Setting up Default Admin Account

<b>Once installed, it's time to set up our Default Admin account</b>

*Give a generic password -> PASSWORD1 -> Finish*

<b>Server is now up and running.</b>

*Use the input feature at the top menu bar of your VM -> Keyboard -> CTRL + ALT + DEL -> Login with password*

## Setting up the IP Addresses

<b>Once we get to the Server Manager page, go to the bottom right of your VM -> Click computer icon -> Ethernet -> Change adapter options</b>

*Make sure you have 2 Ethernets*

*On your NAT Network, which is your HOME network -> right-click -> Status -> Details*

<b>Input the IP Address below for your NAT Network:</b>

*IP address: 10.0.2.15*
  **Note: When you see an IP Address beginning with '10', it is your home network.** 

*Do the same for your Internal Network*

*Right-click -> Status -> Details*

*IP Address: 169.254.196.79*
  **Note: When you see an IP Address beginning with '169', it is your internal network.**

<b>Click OK and Restart the VM to confirm configurations</b>

## Network Connectivity

Login again after your restart.

Go back to the Ethernet connections.
*Right-click -> Internal Internet -> Properties -> (Double-Click) -> Internet Protocol Version 4 IPv4 -> Use this IP Address:*
*IP Address: 172.16.0.1*
*Subnet Mask: 255.255.255.0*

<b>On the same pop-up:</b>
*Preferred DNS Server: 127.0.0.1*
  - This IP Address is a generic address that will redirect the connection back to your own computer

Click OK and close the window.

## Installing Domain/AD DS/Create Domain

**On the server manager:**
*Click -> Add roles and features -> Next -> Next -> Choose the checkbox: Active Directory Domain Services -> Add features -> Next -> Install*

**Back on the server manager screen:**

You'll notice on the top right, there is a yellow triangle with an exclamation point in the center.
*Click the triangle and select -> Promote this server to a DC*

**On the deployment configuration page:**
*Select -> Add new forest -> Root Domain Name: mydomain.com*
*Click Next -> Enter password -> Next -> Next all the way -> Install.
**It will restart again.**

Now, when you log back on, you'll notice the screen say MYDOMAIN\Admin. This is what we want.

Next, we have to create a dedicated domain admin account.

**Go to:**
1. Start
2. Windows Administrative Tools
3. AD Users and Computers
   *Right-click on mydomain.com:*
     - New
     - Organizational Unit
     - Name: _ADMINS
   *Inside of _ADMINS:*
      - Right-click
      - New
      - User
      - Input name info
      - User login name: a-(your first initial and full last name)
      - Set password
      - Uncheck the box: USER MUST CHANGE PASSWORD AT NEXT LOGON
      - **Finish**
  
<b>Notice on your AD Users and Computers area, we now have an account - which is us!</b>

*Next, we right-click on the account we just made -> Properties -> Member of -> Add:*
Under *Select Groups*, type in DOMAIN ADMINS in the section 'Enter object names to select'.
Click 'Check names'
'Apply'
'OK'

Now that we have our Domain account, let's test it.
<b>To Test this:</b>
*Logout*
*Select 'Other User'*
*Use your domain account:* a-asomereville
                          Password1
<b>This should pull your name from the Active Directory</b>

Domain installation is done.

## Installing RAS/NAT

We use RAS/NAT Networks to allow clients to be on a private virtual network of their own, but can still access the internet through the DC.

<b>To do this:</b>
1. Go back to server manager -> Add roles and features
2. Click Next 3x
3. At Roles -> Select the box that says REMOTE ACCESS
4. Next
5. At Role Services -> Select ROUTING
6. Next 3x
7. Install
8. Close

*The REMOTE ACCESS option allows clients to use the internet from the main domain. Even though they are not necessarily 'remote' as the name suggests, they are logging on remotely from their computer as a user and not an admin. Same network just different computers, thats why it's called remote.*

After we'eve installed these features, we need to configure our DC Server to update these changes.

<b>To do this:</b>
1. On your Server Manager screen, on the top-right select -> Tools
2. Scroll down to where it says ROUTING AND REMOTE ACCESS
3. Select -> DC Server -> Right-click this
4. Configure and Enable
5. Next

Now, we will be at a configuration screen. At the screen:
1. Select -> Network Address Translation (NAT)
2. Next
3. Under NAT Connection, fill this in:
   - 'Use the public interface to connect to the internet'
   - Select the NAT or HOME internet
   - Next
   - Finish
  
Going back to DHCP -> Tools -> DHCP
*Now you should see a GREEN arrow next to your DC Server, letting you know your server is up and running and totally configured.*

<b>So far, we've set up our Domain and the RAS/NAT servers.</b>

Before moving onto the Client Server, we have to set up the DHCP Server on our DC.
  *-The DHCP Server lets our clients obtain and IP Address when connected to the internet so they can freely browse the internet within the company's limits.*

<b>To set up DHCP:</b>
1. Go to -> Add roles and features
2. Next
3. Next
4. Server roles -> DHCP -> Add features -> Next 3x -> Install -> Close

<b>Next, to go:</b>
1. Tools -> DHCP to set up a scope

<b>Once open, click on your domain server:</b> *Notice how both servers are down*
1. Right-click the first server: IPv4
2. 'New Scope' -> Next
3. *Name the scope what the IP Range is*

*IP Range is: 172.16.0.100-200*

4. Next
5. No exclusions
6. Next
7. Select 'yes' for configurations
8. Next

For Router (Default Gateway), use the DC IP Address:
*IP Address: 172.16.0.1*

9. Next
10. Next
11. 'Now WINS Servers'
12. Next

<b>Activate Scope</b>
*Select 'Yes, I want to activate this scope now*

13. Next
14. Finish
15. Now, right-click on your server -> Select Authorize -> Refresh

<b>Both of your servers should now be GREEN.</b>

Before we create the Client Server and join our Domain, we need to create our PowerShell script to create users in our AD. Thanks to Josh Madakor, the PS Script has already been created for us but I would review the lines of code to ensure we understand <b>why</b> were using this script and <b>what</b> it does.

To get to Powershell:
1. Windows Start
2. Windows Powershell
3. Windows Powershell ISE -> Right-click -> More -> 'Run as Administrator'

Now we can upload our script. For reference, I typed out the script below:


              


### *[Project is not yet complete, will be adding additional information]*




## 🗺️ Roadmap

### ✅ Step 1: Set Up Host Environment

* 💻 Ensure **Windows 10 or 11** is installed
* 🛠️ Download and install **Oracle VirtualBox** and the **Extension Pack**

### ✅ Step 2: Create Virtual Machines

* 🖥️ Set up **Windows Server 2019 VM** for Active Directory
* 🖥️ Set up **Windows 11 VM** for client access and testing

### ✅ Step 3: Configure Networking

* 🌐 Establish internal network settings in VirtualBox
* 🔧 Configure DHCP, DNS, and other services for Active Directory

### ✅ Step 4: Install & Configure Active Directory

* 🏢 Promote the Windows Server VM to a **Domain Controller**
* 👤 Create and manage users and organizational units

### ✅ Step 5: Automate User Management

* ⚡ Use **PowerShell scripts** to add and manage users efficiently

### ✅ Step 6: Test & Validate Environment

* 🔍 Verify connectivity between VMs
* ✅ Test user logins, permissions, and AD policies
* 🛠️ Troubleshoot any configuration issues

### ✅ Step 7: Document & Demonstrate

* 🎥 Record a **YouTube walkthrough** recreating the environment from scratch
* 📝 Ensure all steps are clear, reproducible, and well-documented

---

## 🤝 Contributing

Thank you for making it through my notes! I welcome contributions, suggestions, and feedback from the community! If you have ideas for better implementations, optimizations, or alternative approaches, please feel free to open an issue or submit a pull request.

Whether it’s improving scripts, enhancing the lab setup, or suggesting new security practices, your input is greatly appreciated. Let’s learn and improve together!

*Please note: This project is primarily for educational purposes, so all contributions should respect safe and ethical cybersecurity practices.*

---
## 📬 Contact

If you have questions, suggestions, or want to connect, feel free to reach out:

Email: ashleymetadata@gmail.com

LinkedIn: linkedin.com/in/ashleysomereville

GitHub: github.com/ashleysecured

I’m always happy to discuss projects, exchange ideas, or collaborate on learning cybersecurity skills!

---
## 🙏 Acknowledgements

A special thanks to <b>Josh Madakor</b>, whose original walkthrough video inspired this project. His tutorial provided a clear foundation for building a virtual Active Directory environment, which I first followed step-by-step before recreating it independently. Check out the original video here: [How to Setup a Basic Home Lab Running Active Directory (Oracle VirtualBox) | Add Users w/PowerShell](https://www.youtube.com/watch?v=MHsI8hJmggI)

---
## 🏷️ Tags

#Cybersecurity #ActiveDirectory #Virtualization #VirtualBox #PowerShell #WindowsServer #SystemAdministration #HomeLab #NetworkSecurity #LearningProject






