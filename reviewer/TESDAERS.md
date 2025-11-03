# COC1

## Reformat the Bootable USB:
- ``diskpart``
- ``listdisk`` to see all drives
- ``select disk 1``
- ``clean``
- ``create partition primary``
- ``format fs=ntfs quick``
- once done, copy the OS installer into the Flash Drive

### WIN10 PRO - Client

## Install the OS 
- BIOS or Bootmanager, then boot to the Flash Drive
- Wipe all existing partitions
- Create new partition and then format
(Note: Partition values will depend based on instruction)

## Configuring Installed OSs
- Network & Sharing Setting >> Sharing & Network >> Changed Advanced Sharing
- Turn on every thing except for Password Sharing
- About This PC >> Remote Desktop
- Turn on Remote Desktop option
- Rename the PCs "CLIENT-PC" and "SERVER-PC"

## If instructed to do partition after installation
- Disk Management
- Shrink the corresponding drive
(Note: Partition values will depend based on instruction)

---

# COC2

>cdrking **(router)** - 192.168.1.1

>tplink **(wap)** - 192.168.0.254

>Make sure both are reset

- Router - 192.168.x.101
- WAP - 192.168.x.102
- SERVER - 192.168.x.103
- Client - 192.168.x.104
- Laptop - 192.168.x.105

## Configuring the router and WAP
### Configuring the Router
- Connect the Router's cable, disconnect the WAP's cable.
- Go to the Router's GUI by accessing the default IP.
- Rename the SSID: ``[LAST_NAME]-ROUTER`` 
- Put a WPA2 password: ``css@12345``
- Configure the IP Address: ``192.168.x.101``
- Once done, try pinging and connecting to the GUI once again.
- Disconnect the router's cable and proceed to WAP configuration.
### Configuring the WAP
- Connect the WAP's cable, disconnect the Router's
- Go to the WAP's GUI by accessing the default IP.
- Rename the SSID: ``[LAST_NAME]-WAP`` 
- Put a WPA2 password: ``css@12345``
- Configure the IP Address: ``192.168.x.102``
- Once done, connect the Router's cable, try pinging and connecting to the GUI once again.
## Configuring the remaining devices
### Set the IP Addresses for the Server, Client, and WLAN Device (Laptop) using the information above.


### T-568 A:
```WG  G  WO  B  WB  O  WBr  Br```

### T-568 B:
```WO  O  WG  B  WB  G  WBr  Br```

---



# COC3

## 1. Launch Server Manager
- Launch Server Manager
- Add roles and features

---

## 2. Add Roles and Features Wizard

### 1. Before you begin
- [Next]

### 2. Select installation type
- Role-based or feature-based installation

### 3. Select destination server
- Select a server from the server pool

### 4. Select server roles
Add/check the following features:

#### Roles
- *Active Directory Domain Service*
- *DHCP Server*
- *DNS Server*
- *File and Storage Services*
- *File Server Resource Manager*
- *Print and Document Services*
- *Remote Desktop Services*

#### Features
- Windows Server Backup

### 5. Active Directory Domain Services
- [Next]

### 6. DHCP Server
- [Next]

### 7. DNS Server
- [Next]

### 8. Print and Document Services
- [Next]

### 9. Select Role Services
- **Remote Desktop Connection Broker**
  - **Add Features**: [Next] [Install]
  - Finish then [Close]

---

##  Post-Installation Configuration

### Promote this server to a domain controller
- Click the **Flag Icon** on the Top Navigation
- Select: `Promote this server to a domain controller`

#### Deployment Configuration
- **Add a new forest**:
	- Root domain name
		- `[surname.local]`
#### Domain Controller Options
- Password
	- `[Css@12345]` >>> [Next]
##### DNS Options
	- [Next]
#### Additional Options
- **NetBIOS domain Name**:
	- `[Auto-filled]`
#### Additional Options/Paths
	- [Next]
#### Review Option
	- [Next]
#### Pre-requisites check
- [Install] >>> `You will be prompted to restart the Server PC`

###  Complete DHCP configuration
- Click the **Flag Icon** on the Top Navigation
	- Description:
		- [Next]
	- Authorization:
		- [Commit] [Close]
- Click **DHCP** on Side Navigation
	- Right click your ``[Server_Name]`` then select **DHCP Manager**
		- Locate your IPv4 and add a New Scope
			- Scope Name: [surname-scope]
				- [Next]
			- IP Address Range
				- Start IP Address: `192.168.x.1`
				- End IP  Address: `192.168.x.10`
				- Length: `24`
				- Subnet mask: `255.255.255.0`
			- Add Exclusions and Delay
				- [Next]
			- Lease Duration
				- [Next]
			- Router (Default Gateway)
				- `192.168.x.1` [Add] | [Next]
			- Domain Name and DNS Servers
				- [Next]
			- WINS Servers
				- [Next]
			- Activate Scope
				- *Yes* [Finish]

###  AD Users & Computers Configuration

#### [surname].local
- Create new Organization Unit
    -  Name: [Folder Redirection]
- Create (two) new Users
    - Username:User1;Password:[Password1]
    - Checklist: User cannot change password, Password never expires

###  Creating the Shared Folder for Users in Domain
- Create a new folder in your Desktop and name it 'File Sharing'
- Go to Properties > Sharing > Advanced Sharing
    - ===Share this folder===
        - Permissions
            -  Add 'au' with Full Control
            -  Do the same for ``Everyone``
        -  Once done, copy the Network Path **(IMPORTNANT)**

###  Configuring the Group Policy Management
- Forest: [surname].local > Domain > [surname].local > `Folder Redirection`
- Create a new GPO for `Folder Redirection`
    - Edit the newly created GPO
        - User Configuration > Policies > Windows Settings > Folder Redirection > ===Desktop===
        - Desktop folder's Properties
            - Basic Redirect Everyone's folder to the same location
            - Root Path: [the Copied Route Path]
            - Next tab then uncheck everything
            - Save the changes
        - Document folder's properties
            - Do the same

###  Joining CLIENT-PC to the Domain
- Go to the `Rename this PC (Advanced)` settings
- Join your domain ([surname].local) using User1 credentials
    - Username: Administrator;Password: [Css@12345]
- You will be prompted to restart.
- 
###  Joining Laptop to the Domain
- Go to the `Rename this PC (Advanced)` settings
- Join your domain ([surname].local) using User2 credentials
    - Username: Administrator;Password: [Css@12345]
- You will be prompted to restart.

### Now both EndUsers will generate folders on your SERVER-PC's Shared Folder
- To ensure that both ends are working, try creating a file/folder for each using your **SERVER-PC**

## REMOTE DESKTOP CONNECTION
- On your **SERVER-PC** Go to Remote Desktop Connection settings.
- Establish Remote connection with **CLIENT-PC** using it's IP address `192.168.x.4` and the Password `Password1` (since it is User1)
- Check your **CLIENT-PC** for the Remote confirmation then accept.
    - Restart the **Client-PC** using the **SERVER-PC** via Remot Kontol
- Once done, do the same for Laptop(`User2:Password2`)

### Moving forward...
- Restart the **SERVER-PC** using your **CLIENT-PC**:
    - More Choices
        - Use a different Account
            - `[Use the Administrator Credentials]`

## Implementing File Screening
- On your Server Manager, navigate to the File and Storage Services inside the Sidebar, then Shares
	- Tasks
		- **SMB Share - Quick**
		- `[Next]`
		- **Type a custom path:** and select the folder to do file screening
		- `[Next]`
		- **Enable access-based enumeration** and **Allow caching of share**
		- **Customize permissions...**
			- **_Remove everybody EXCEPT Admin and System_**
			- Add a new User (Accounts) and give it *Full Control* on the Basic Permissions
			- `[Ok]` and `[Apply]`
		- `[Create]` then `[Close]`
- Navigate to Server Manager's *Tools* and select **File Server Resource Manager**
	- Inside *File Screening Management*, you can **implement** and **create templates** for your own File Screening Ruleset.
 		- When *creating a template*, just **give it a name** and then select the **File groups to block.**
		- To implement the File Screening rule, go to **File Screens** then click **Create File Screen...**
  			- Specify the *File Screen Path* folder
     		- On the *Derive properties . . .* you can select the File Screen Template of your liking.
       		- `[Create]`
## Assigning a Quota
- On the File server resource Manager window, navigate to **Quota Management**
	- Inside **Quotas**, **Create a Quota...**
		- Specify the folder *(Same folder from File Screening)* and the *Size Limit* of your liking.

## PRINTER
- Install the `EPSON-L360` as Admin on **CLIENT-PC**
- Install `EPSON-L360`

### Print Management
- Go to Server Manager's **Print Management**
- Print Servers > **SERVER-PC** (Local) > Printers
- Right click `EPSON L360 Series` then Deploy with Group Policy
    - Browse (folder) > Folder Redirection > [surname].local > New GPO
    - Add the Checklist: The Users, The Computers
    - Save changes
    - Right Click `EPSON L360 Series`
        - Properties > Sharing
        - Checklist: Share this Printer, Render Print Jobs, List in the Directory
        - Save changes
    - Go to Print Servers on **SERVER-PC**.[surname].local
        - Apply, Accept then Print

# NOTE: EPSON L360 SERIES COPY

#### On your CLIENT-PC
- Launch Printer & Scanners
- Add a Printer or Scanner (Try refreshing if not loading properly.)

#### Find EPSON L360 Series on SERVER-PC
- Add Device
- Navigate to Desktop and make a new text file
- Print the text file that you created

#### On your Laptop (User2)
- Make a new text file, type CSS NSII and then Print using the Printer.
- Revert back to original state:
    - `WORKGROUP`
    - IPv4 (Automatic IP Assignement)
    - IPv6 (Enabled)



---

# Written Examination

## SET A

1. **_Blue_** is the common color for the USB 3.0 connector for Standard A receptacles and plugs.
2. **_Flash memory_** is the type of memory used in the Solid state Drive (SSD) as a storage.
3. **_Read only Memory (ROM)_** is originally, where the BIOS stored in a standard PC.
4. **_ISO image_** is a complete copy of everything stored on a physical optical disc.
5. **_16 GB for 32-bit OS or 20 GB for 64-bit OS_** is the minimum requirement for hard disk drive capacity in Windows 10.
6. **_Computer_** is where Windows device drivers are stored.
7. **_+5V_** is the volt rating of the RED wire in a power supply.
8. **_Cross-over_** is a network cable where one end is T568-A while the other is T568-B configuration.
9. In Windows 7, when LAN connection has a cross and red indicator, it signifies that there is a **_problem with the LAN driver._**
10. **_Network Interface Card_** is the hardware component that allows connection over a computer network.
11. **_Router_** is a networking device that forwards data packets between computer networks.
12. Wi-Fi is a trademarked phrase, which means **_IEEE.802.11_**
13. **_Local Area Network (LAN)_** is a collection of devices connected together in one physical location.
14. **_Metropolitan Area Network_** is a type of network that interconnects multiple local area network.
15. **_Anti-virus software_** is a computer program used to prevent, detect, and remove possible threats to the system.
16. **_Peer to peer network_** is a two or more PCs are connected and snare resources without going through a separate server computer.
17. **_Provider address_** is the IP address used by the virtual machine to communicate over the physical network.
18. **_WPA2-Personal_** is a type of version that protects unauthorized network access by utilizing a setup password.
19. **_Access control list_** consists of the user matrices, and capability tables that govern the rights and privileges of users.
20. **_Information Extortion_** occurs when an attacker or trusted insider steals data from a computer system and demands compensation for its return.
21. **_Continuous Assessment_** monitors the initial security accreditation of an information system for tracking of changes.
22. **_Remote Storage Service_** is a service in Windows Server that enables administrators to migrate data to a lower-cost and tape media on file servers.
23. **_Virtual cluster server_** is an Application group that contains a Client Access Point and with at least one application specific resource.
24. **_Ping_** is a utility tool that test whether a particular host is reachable across an IP Network.
25. **_F5_** is not an appropriate key to enter BIOS setup
26. **_Safe mode_** enables users of Windows to correct any problems preventing from booting up normally.
27. **_Repair Computer_** is a Start up recovery option in a computer that essentially boot into the Recovery partition of the main hard drive.
28. A single physical hard drive can divide into multiple **_Logical_** hard drives.
29. **_Advanced Technology Attachment (ATA) and Serial Advanced Technology Attachment (SATA)_** are the two types of interface used to communicate between the hard drive and the computer motherboard.
30. **_World Wide Web_** is the spider-like interconnection in millions of pieces of information located on computers around the cyber space.


## SET B

1. **_Thermal Paste_** is a compound used to prevent overheating of CPU.
2. The maximum cable legnth for USB 2.0 is **_5 meters._**
3. **_Firmware_** is the software program stored into a Read Only Memory (ROM.)
4. Disc image is also known as **_ISO Image._**
5. **_1 gigabyte (GB) for 32-bit or 2 GB for 64-bit_** is the minimum hardware requirements of the computer RAM for Windows 10.
6. **_Device Manager_** is an extension of Microsoft management console used to manage hardware devices including device drivers.
7. **_+12V_** is the volt rating of the YELLOW wire in a power supply.
8. The Yost cable, Cisco cable, and Console cable are also known as **_Roll-over._**
9. In Server 2008, Local Area Network (LAN) connection icon in a yellow color indicator signifies **_no connectivity._**
10. Light Emitting Diode (LED) in a Network Interface Card (NIC) has **_2_** lights.
11. Port **_Forwarding_** is an application of network address translation (NAT) that redirects a communication request from one address and port number combination to another.
12. The alternative for wired connection that uses radio frequencies to send signals between devices and a router is called **_Wireless Fidelity._**
13. **_Campus Area Network_** is a computer network made up of an interconnection of local area network (LANs) within a limited geographical area, e.g corporate buildings.
14. **_Metropolitan Area Network_** is the term that applied to the interconnection of LAN in a city into 5 to 50 kilometers.
15. **_Network Administrator_** is assigned to monitor the network security, operation and network users.
16. **_File Server_** is used to store the user documents with a large amount of memory and storage space.
17. **_Provider address_** is the IP address used by the virtual machine to communicate over the physical network.
18. **_WPA2-Enterprise_** is a type of version that verifies network users through a server and requires a more complicated setup, but provides additional security.
19. **_Port number_** is a way to identify a specific process to which an Internet or other network message forwarded when it arrives at a server.
20. An automated software program that executes certain commands when it receives a specific input is **_Bot._**
21. **_File System Quota_** enables administrators to configure storage thresholds on particular sets of data stored on server NTFS volumes.
22. **_Failover_** is a feature in Windows server that provides the ability to migrate workloads between a source and target cluster.
23. **_Counter Logs_** are log files that record performance statistics based on the various performance objects and instances available in Performance Monitor.
24. **_Chkdsk_** is the Recovery Console Command that examines the volume for errors and repairs it.
25. The computer system is functional if it is in **_Normal booting._**
26. **_Fully automatic_** is a Windows update that downloads and installs the latest version.
27. **_Bad Sector_** is the physically damaged cluster of storage on the hard drive.
28. **_IP address conflict_** occurs when two or more devices connected with the same IP address in a computer network.
29. **_Hard Disk_** should be replaced if frequent error messages appear while moving files and booting up the operating system.
30. **_fixmbr_** is a Recovery Console Command that writes a new master boot record on the hard disk drive.


## SET C

1. **_3 meters_** is the maximum cable leegnth for USB 3.0 when using a non-twisted pair wire.
2. M.2 is a form factor for **_Storage drive._**
3. **_Real Time Clock/Non-Volatile Random-Access Memory (RTC/NVROM) chip_** is the kind of chip stores the settings in the BIOS.
4. **_ISO file_** is used as a substitute for actual disc, allowing users to run software without having to load a CD or DVD.
5. **_Windows Ultimate edition_** has a program capable of restricting other users to access certain programs in a computer.
6. **_System32_** is the folder that the device driver can be found.
7. **_Red = 5V, Yellow = 12V_** is the ideal voltage range on the hotwires of a Molex connector.
8. **_Straight-Thru_** ethernet cable used to connect computers to hubs & switches?
9. **_LAN Card_** is also known as a network interface controller.
10. 192.168.0.1 is a **_Class C_** IP Address.
11. **_Core_** is a router that is designed to operate in the Internet Backbone.
12. **_MAC Address Filtering_** allows to define a list of devices and only allow those devices on Wi-Fi network.
13. **_Wide Area Network_** is a type of network that connects through signals, e.g. infrared?
14. **_Wireless Local Area Network_** allows users to move around the coverage area in a line of sight while maintaining a network connection.
15. **_NetBackup_** is an enterprise-level heterogeneous backup and recovery suite.
16. **_Directory server_** is a type of server that allows the central administration and management of network users and network resources.
17. **_Loss_** is an information asset suffering damage, unintended modification or disclosure.
18. **_WPS_** is the network security standard that tries to make connections between a router and wireless devices faster and easier.
19. **_File Transfer Protocol_** is the most efficient in transferring larger files.
20. **_Spoofing_** is a technique used to gain unauthorized access to computers, wherein the intruder sends message with a source IP address.
21. **_File Screening_** enables administrators to define the types of file that can save within a Windows Volume and folder.
22. **_Network Load Balancing_** is a second clustering technology that provides by corresponding client requests across several servers with replicated configurations.
23. **_Circular Logging_** log files that conserves disk space by ensuring that the performance log file will not continue growing over certain limits.
24. A **_Network Card_** should be upgraded to receive and transmit strong signals to and from other computer.
25. **_Dynamic Host Control Protocol (DHCP)_** is a protocol that provides quick, automatic, and central management for the distribution of IP addresses within a network.
26. **_nslookup_** is a command-line tool used to find the IP address that corresponds to a host to domain name.
27. **_CMOS Battery failure_** the problem of a computer if the time and date keep on resetting even after fixing in the BIOS.
28. **_Crystal Disk info_** is a tool used for checking the condition of the hard drive with signs like "Good", "Caution" or "Bad."
29. A task to do after planning to fix the network is **_Implenting the solution_**.
30. **_Debugging_** is a process of scanning, identifying, diagnosing and resolving problems, errors and bugs in software.


## SET D

1. **_Universal Serial Bus (USB) C Thunderbolt_** is a 24 pin USE connector system with a rotationally symmetrical connector.
2. **_Chipset_** facilitates communication between the CPU and other devices in the system.
3. **_Voltage Basic Input Output System (BIOS)_** is the option in the BIOS/UEFI used to overclock computer.
4. **_8GB_** is the minimum byte capacity of a bootable flash drive for Windows 10.
5. **_512MB_** is the minimum RAM requirement of Server 2012 R2.
6. In Windows 7, Windows update and security is located in **_Settings_**.
7. ATX 12V P I connector provides **_3.3V, 5V, 12V_** voltages for the motherboard.
8. **_ANSI/TIA-568_** defined as a structured network cabling system standard for commercial buildings, and between buildings in campus environments.
9. In networking, **_Connection_** through a network refers to pieces of related information that are transferred.
10. 172.168.10.1 is a **_Class B_** IP address.
11. **_Edge_** is atype of router that enables an internal network to connect to an external network.
12. **_Bandwidth_** pertains to the maximum data transfer rate of a network for internet connection.
13. **_Storage Area Network_** is a type of high-speed interconnects using a variety of technology/topology.
14. **_Customer Premises Equipment (CPE)_** acts like a border router to connect corporate sites to wired access lines.
15. **_Authentication_** is a process of recognizing a user's identity.
16. **_Directory server_** is a type of server that occupies a large portion of computing territory between the database server and end user.
17. In the context of security, **_Access_** is a privilege or assigned permission for the use of computer data or resource.
18. **_10BASE-36_** are media types of 10-megabit Ethernet that is composed of broadband coaxial cable carrying multiple baseband channels for a maximum length of 3,600 meters.
19. **_Firewall_** is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules.
20. **_Downtime_** is the term used to refer when a system is unavailable.
21. **_NSLOOKUP_** is a command used to certify the peer name resolution in Active Directory.
22. **_Lightweight Directory Access Protocol (LDAP)_** is an active directory protocol used to access data from a database.
23. **_Current Activity_** displays the measures and various real-time statistics on the system's perforamnce.
24. **_Network Interface Card_** is a type of peripheral card having media access control address.
25. **_Vertical cross-connect_** is a term for the main patch panel that links to every telecommunication room inside the building?
26. **_Raceway_** is an enclosed or semi-enclosed channel that protects, routes and hides cables and wires.
27. **_Boot_** is a material that slid before crimping both ends of a patch cable and acts to reduce strain in a cable.
28. **_Loop Pack Test_** verifies the operation of communication device by sending and receiving data from the same port.
29. **_Packet analyzer_** is a networking tool that monitors and intercepts traffic over the computer network.
30. Resistance reading of an open wire is **_Infinite ohms._**


## Set E

1. **_Firewire_** is an interface standard for a serial bus for high-speed communications also referred as Institute of Electrical and Electronics Engineers (IEEE) 1394 interface.
2. **_Input/Output Controller Hub_** is a microchip to manage data communication between a CPU and a motherboard.
3. **_LoJack_** is a setup option in Unified Extensible Firmware Interface (UEFI)/Basic Input Output System (BIOS) that allows security settings to perform such tasks as locating the device.
4. **_Diskpart_** is an exit based command used for creating bootable flash drive.
5. **_512MB_** is the minimum Random-Access Memory (RAM) requirement of Server 2008 R2.
6. In Windows 7, **_Only standard video is displayed_** is an indicator tells that a video driver has not installed.
8. **_Cable splicing_** is the process of joining two cable ends together.
9. **_Terminal Server_** is a hardware device that enables to connect serial devices across a network.
10. 10.10.10.1 is a **_Class A_** IP address.
11. **_250_** devices can connect to a wireless router.
12. A device that acts as a bridge between wireless clients and wired network is **_Hub._**
13. **_Virtual Local Area Network_** is a logical subnetwork that groups collection of devices from the same network switch.
14. **_Latency_** is a tire time lapse between data send and when received, it can have a big impact on performance.
15. **_Stress Test_** means pushing a component to its limits and extremes.
16. **_Real-Time Communication Server_** is a type of server that enables large numbers users to exchange information near instantaneously and formerly known as Internet Relay Chat (IRC.)
17. **_Integrity_** ensures that information and programs are changing only in a specified and authorized manner.
18. **_Netstat_** is a command-line network utility that displays network connections for TCP and routing tables.
19. **_Communications protocol_** are set of rules that describe how software and hardware should interact within a network.
20. **_Malicious Code_** attack includes the execution of viruses, worms, Trojan horses, and active Web scripts with the intent to destroy or steal information.
21. **_Hypertext Transfer Protocol (HTTP)_** are set of rules for transferring files such as text, graphic images, sound, video and other multimedia files on the World Wide Web (WWW.)
22. **_System Management Tools_** are traditional methods used to access server data using the Server Message Block (SMB) protocol over TCP/IP.
23. **_Ldifde.exe and csvde.exe_** are two command-line utilities used to export and import Active Directory object information.
24. **_Gateway_** is a node in a computer network that allows data to flow from one discrete network to another.
25. **_Resistance_** are electrical quantities measured when performing continuity testing of twisted pair cable.
26. **_Local Backup_** is a type of backup used in hard drive, disc, flash drive and external drives that are housed site.
27. **_Startup Repair_** is a recovery tool in Windows fixes and scans computer problem that prevents computer from booting repeatedly.
28. **_Windows Recovery Environment_** is used to run commands for troubleshooting unbootable operating systems.
29. **_Capacitor_** is an electronic components need to discharge eletrically before the replacement of parts.
30. **_802.11n_** is a current wireless networking standard that can communicate with older network standards.

...
