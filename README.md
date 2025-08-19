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


## Step 1: Setting Up NAT in VirtualBox

<b>Open VirtualBox Manager.</b>

*Go to File → Preferences → Network → NAT Networks.*

*Click the Add (+) button to create a new NAT network.*

*Give it a name (e.g., NATNetwork).*

*Check Enable DHCP so VirtualBox can automatically assign IPs.*

<b>Attach your VMs to this network:</b>

*Right-click a VM → Settings → Network.*

*Set Adapter 1 to NAT Network → Select your NATNetwork.*

<b>Start your VM and test network connectivity with:</b>

*ping google.com*

## Step 2: Installing and Configuring RAS (Routing and Remote Access Services) on Server 2019

<b>Inside your Windows Server 2019 VM, open Server Manager.</b>

*Click Add Roles and Features.*

<b>In the wizard:</b>

*Role-based or feature-based installation → Next.*

*Select your server → Next.*

<b>Under Server Roles, check Remote Access.</b>

*Expand it and select Routing → Next.*

*Add required features → Next → Install.*

<b>After installation, open Routing and Remote Access from the Tools menu.</b>

*Right-click your server name → Configure and Enable Routing and Remote Access.*

*Select NAT option in the wizard.*

*Choose the network interface connected to the internet (NAT).*

*Apply and finish setup.*

<b>Verify NAT is working by joining your client VM to the internet through this server.</b>

## Step 3: Verify Connectivity

Test that your Server VM can access the internet.

Test that your Client VM (e.g., Windows 10) can access the internet through the Server (via NAT + RAS).

At this point, your basic networking is complete.

## Final Network Topology

Once installed and configured correctly, your virtual machine network should look like this: <img width="956" height="799" alt="Screenshot 2025-08-18 174832" src="https://github.com/user-attachments/assets/1568e686-94d4-443b-945d-2a36b9c88563" />



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






