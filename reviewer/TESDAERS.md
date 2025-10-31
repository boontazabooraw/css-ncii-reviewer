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
- To ensure that both ends are working, try creating file/folder for each using your **SERVER-PC**

## TIME TO REMOT KONTOL
- On your **SERVER-PC** Go to Remote Desktop Connection settings.
- Remot Kontol your **CLIENT-PC** using it's IP address `192.168.x.4` and the Password `Password1` (since it is User1)
- Check your **CLIENT-PC** for the Remote confirmation then accept.
    - Restart the **Client-PC** using the **SERVER-PC** via Remot Kontol
- Once done, do the same for Laptop(`User2:Password2`)

### Moving forward...
- Restart the **SERVER-PC** using your **CLIENT-PC**:
    - More Choices
        - Use a different Account
            - ``Administrator:Css@12345``

## PRINTER LETSGO
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


## BACKUP

---

# Written Examination

## SET 1

1. **Blue** is the common color for the USB 3.0 connector for Standard A receptacles and plugs.

2. **Flash memory** is the type of memory used in the Solid state Drive (SSD) as a storage.

3. **Read only Memory (ROM)** is originally, where the BIOS stored in a standard PC.

4. **ISO image** is a complete copy of everything stored on a physical optical disc.

5. **16 GB for 32-bit OS or 20 GB for 64-bit OS** is the minimum requirement for hard disk drive capacity in Windows 10.

6. **Computer** is where Windows device drivers are stored.

7. **+5V** is the volt rating of the RED wire in a power supply.

8. **Cross-over** is a network cable where one end is T568-A while the other is T568-B configuration.

9. In Windows 7, when LAN connection has a cross and red indicator, it signifies that there is a **problem with the LAN driver.**

10. **Network Interface Card** is the hardware component that allows connection over a computer network.

11. **Router** is a networking device that forwards data packets between computer networks.

12. Wi-Fi is a trademarked phrase, which means **IEEE.802.11**

13. **Local Area Network (LAN)** is a collection of devices connected together in one physical location.

14. **Metropolitan Area Network** is a type of network that interconnects multiple local area network.

15. **Anti-virus software** is a computer program used to prevent, detect, and remove possible threats to the system.

16. **Peer to peer network** is a two or more PCs are connected and snare resources without going through a separate server computer.

17. **Provider address** is the IP address used by the virtual machine to communicate over the physical network.

18. **WPA2-Personal** is a type of version that protects unauthorized network access by utilizing a setup password.

19. **Access control list** consists of the user matrices, and capability tables that govern the rights and privileges of users.

20. **Information Extortion** occurs when an attacker or trusted insider steals data from a computer system and demands compensation for its return.

21. **Continuous Assessment** monitors the initial security accreditation of an information system for tracking of changes.

22. **Remote Storage Service** is a service in Windows Server that enables administrators to migrate data to a lower-cost and tape media on file servers.

23. **Virtual cluster server** is an Application group that contains a Client Access Point and with at least one application specific resource.

24. **Ping** is a utility tool that test whether a particular host is reachable across an IP Network.

25. **F5** is not an appropriate key to enter BIOS setup

26. **Safe mode** enables users of Windows to correct any problems preventing from booting up normally.

27. **Repair Computer** is a Start up recovery option in a computer that essentially boot into the Recovery partition of the main hard drive.

28. A single physical hard drive can divide into multiple **Logical** hard drives.

29. **Advanced Technology Attachment (ATA) and Serial Advanced Technology Attachment (SATA)** are the two types of interface used to communicate between the hard drive and the computer motherboard.

30. **World Wide Web** is the spider-like interconnection in millions of pieces of information located on computers around the cyber space.


## SET 3

1. **3 meters** is the maximum cable leegnth for USB 3.0 when using a non-twisted pair wire.

2. M.2 is a form factor for **Storage drive.**

3. **Real Time Clock/Non-Volatile Random-Access Memory (RTC/NVROM) chip** is the kind of chip stores the settings in the BIOS.

4. **ISO file** is used as a substitute for actual disc, allowing users to run software without having to load a CD or DVD.

5. **Windows Ultimate edition** has a program capable of restricting other users to access certain programs in a computer.

6. System32 is the folder that the device driver can be found.

7. **Red = 5V, Yellow = 12V** is the ideal voltage range on the hotwires of a Molex connector.

8. **Straight-Thru** ethernet cable used to connect computers to hubs & switches?

9. **LAN Card** is also known as a network interface controller.

10. 192.168.0.1 is a **Class C** IP Address.

11. **Core** is a router that is designed to operate in the Internet Backbone.

12. **MAC Address Filtering** allows to define a list of devices and only allow those devices on Wi-Fi network.

13. **Wide Area Network** is a type of network that connects through signals, e.g. infrared?

14. **Wireless Local Area Network** allows users to move around the coverage area in a line of sight while maintaining a network connection.

15. **NetBackup** is an enterprise-level heterogeneous backup and recovery suite.

16. **Directory server** is a type of server that allows the central administration and management of network users and network resources.

17. **Loss** is an information asset suffering damage, unintended modification or disclosure.

18. **WPS** is the network security standard that tries to make connections between a router and wireless devices faster and easier.

19. **File Transfer Protocol** is the most efficient in transferring larger files.

20. **Spoofing** is a technique used to gain unauthorized access to computers, wherein the intruder sends message with a source IP address.

21. **File Screening** enables administrators to define the types of file that can save within a Windows Volume and folder.

22. **Network Load Balancing** is a second clustering technology that provides by corresponding client requests across several servers with replicated configurations.

23. Circular Logging log files that conserves disk space by ensuring that the performance log file will not continue growing over certain limits.

24. A **Network Card** should be upgraded to receive and transmit strong signals to and from other computer.

25. **Dynamic Host Control Protocol (DHCP)** is a protocol that provides quick, automatic, and central management for the distribution of IP addresses within a network.

26. **nslookup** is a command-line tool used to find the IP address that corresponds to a host to domain name.

27. **CMOS Battery failure** the problem of a computer if the time and date keep on resetting even after fixing in the BIOS.

28. **Crystal Disk info** is a tool used for checking the condition of the hard drive with signs like "Good", "Caution" or "Bad."

29. A task to do after planning to fix the network is **Implenting the solution**.

30. **Debugging** is a process of scanning, identifying, diagnosing and resolving problems, errors and bugs in software.



## SET 4

1. **Universal Serial Bus (USB) C Thunderbolt** is a 24 pin USE connector system with a rotationally symmetrical connector.

2. **Chipset** facilitates communication between the CPU and other devices in the system.

3. **Voltage Basic Input Output System (BIOS)** is the option in the BIOS/UEFI used to overclock computer.

4. **8GB** is the minimum byte capacity of a bootable flash drive for Windows 10.

5. **512MB** is the minimum RAM requirement of Server 2012 R2.

6. In Windows 7, Windows update and security is located in **Settings**.

7. ATX 12V P I connector provides **3.3V, 5V, 12V** voltages for the motherboard.

8. **ANSI/TIA-568** defined as a structured network cabling system standard for commercial buildings, and between buildings in campus environments.

9. In networking, **Connection** through a network refers to pieces of related information that are transferred.

10. 172.168.10.1 is a **Class B** IP address.

11. **Edge** is atype of router that enables an internal network to connect to an external network.

12. **Bandwidth** pertains to the maximum data transfer rate of a network for internet connection.

13. **Storage Area Network** is a type of high-speed interconnects using a variety of technology/topology.

14. **Customer Premises Equipment (CPE)** acts like a border router to connect corporate sites to wired access lines.

15. **Authentication** is a process of recognizing a user's identity.

16. **Directory server** is a type of server that occupies a large portion of computing territory between the database server and end user.

17. In the context of security, **Access** is a privilege or assigned permission for the use of computer data or resource.

18. **10BASE-36** are media types of 10-megabit Ethernet that is composed of broadband coaxial cable carrying multiple baseband channels for a maximum length of 3,600 meters.

19. **Firewall** is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules.

20. **Downtime** is the term used to refer when a system is unavailable.

21. **NSLOOKUP** is a command used to certify the peer name resolution in Active Directory.

22. **Lightweight Directory Access Protocol (LDAP)** is an active directory protocol used to access data from a database.

23. **Current Activity** displays the measures and various real-time statistics on the system's perforamnce.

24. **Network Interface Card** is a type of peripheral card having media access control address.

25. **Vertical cross-connect** is a term for the main patch panel that links to every telecommunication room inside the building?

26. **Raceway** is an enclosed or semi-enclosed channel that protects, routes and hides cables and wires.

27. **Boot** is a material that slid before crimping both ends of a patch cable and acts to reduce strain in a cable.

28. **Loop Pack Test** verifies the operation of communication device by sending and receiving data from the same port.

29. **Packet analyzer** is a networking tool that monitors and intercepts traffic over the computer network.

30. Resistance reading of an open wire is **Infinite ohms**.



## Set 5

1. **Firewire** is an interface standard for a serial bus for high-speed communications also referred as Institute of Electrical and Electronics Engineers (IEEE) 1394 interface.

2. **Input/Output Controller Hub** is a microchip to manage data communication between a CPU and a motherboard.

3. **LoJack** is a setup option in Unified Extensible Firmware Interface (UEFI)/Basic Input Output System (BIOS) that allows security settings to perform such tasks as locating the device.

4. **Diskpart** is an exit based command used for creating bootable flash drive.

5. **512MB** is the minimum Random-Access Memory (RAM) requirement of Server 2008 R2.

6. In Windows 7, **Only standard video is displayed** is an indicator tells that a video driver has not installed.

8. **Cable splicing** is the process of joining two cable ends together.

9. **Terminal Server** is a hardware device that enables to connect serial devices across a network.

10. 10.10.10.1 is a **Class A** IP address.

11. **250** devices can connect to a wireless router.

12. A device that acts as a bridge between wireless clients and wired network is **Hub**.

13. **Virtual Local Area Network** is a logical subnetwork that groups collection of devices from the same network switch.

14. **Latency** is a tire time lapse between data send and when received, it can have a big impact on performance.

15. **Stress Test** means pushing a component to its limits and extremes.

16. **Real-Time Communication Server** is a type of server that enables large numbers users to exchange information near instantaneously and formerly known as Internet Relay Chat (IRC.)

17. **Integrity** ensures that information and programs are changing only in a specified and authorized manner.

18. **Netstat** is a command-line network utility that displays network connections for TCP and routing tables.

19. **Communications protocol** are set of rules that describe how software and hardware should interact within a network.

20. **Malicious Code** attack includes the execution of viruses, worms, Trojan horses, and active Web scripts with the intent to destroy or steal information.

21. **Hypertext Transfer Protocol (HTTP) are set of rules for transferring files such as text, graphic images, sound, video and other multimedia files on the World Wide Web (WWW.)

22. **System Management Tools** are traditional methods used to access server data using the Server Message Block (SMB) protocol over TCP/IP.

23. **Ldifde.exe and csvde.exe** are two command-line utilities used to export and import Active Directory object information.

24. **Gateway** is a node in a computer network that allows data to flow from one discrete network to another.

25. **Resistance** are electrical quantities measured when performing continuity testing of twisted pair cable.

26. **Local Backup** is a type of backup used in hard drive, disc, flash drive and external drives that are housed site.

27. **Startup Repair** is a recovery tool in Windows fixes and scans computer problem that prevents computer from booting repeatedly.

28. **Windows Recovery Environment** is used to run commands for troubleshooting unbootable operating systems.

29. **Capacitor** is an electronic components need to discharge eletrically before the replacement of parts.

30. **802.11n** is a current wireless networking standard that can communicate with older network standards.

...
