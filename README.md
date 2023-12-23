# CSA-Server-Client-Configuration
![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/50824973-8956-4a25-baba-46562e55875b)

## 1. Directory Setup

Create a directory structure on your E drive as follows:

```
E:\
|-- CSA
    |-- CSA-Server
    |-- CSA-Client
```

## 2. Server Setup - MLB-DC1-CentOs7

### Specifications:

- **Configuration Type:** Typical
- **VM Name/Machine Name:** MLB-DC1-CentOs7
- **Location:** E:\CSA\CSA-Server
- **ISO Image:** CentOS-7-x86_64-DVD
- **RAM:** 1.5 GB
- **HDD:** 15 GB
- **Processors:** 1
- **Partitioning:** Automatic (CentOs 7 Installation Process)
- **Root Password:** root
- **Username/Password:** svr1/svr1
- **Hostname (Under Network Settings):** mlb-dc1-centos7.csa.lk
- **Network Adapter 1:** VMnet2
- **Network Adapter 2:** NAT

## 3. Client Setup - CSA-CLI-Fedora28

### Specifications:

- **Configuration Type:** Typical
- **VM Name/Machine Name:** CSA-CLI-Fedora28
- **Location:** E:\CSA\CSA-Client
- **ISO Image:** Fedora-LXDE-Live-x86_64-28-1.1
- **RAM:** 1.5 GB
- **HDD:** 15 GB
- **Processors:** 1
- **Partitioning:** Automatic (Fedora Installation Process)
- **Root Password:** root
- **Username/Password:** client1/client1
- **Hostname (Under Network Settings):** csa-cli-fedora28.csa.lk
- **Network Adapter 1:** VMnet2
- **Network Adapter 2:** NAT

The first network adapter created for the client and server was used to connect the server and the client using a specific virtual network (or specific virtual interface) called VMnet2. 
It means the first created network adapter acts as a virtual interface to both the client and server to make a connection between them.
The second network adapter created was used to connect to the internet using NAT in both the server and the client.

## 4. Creating a virtual switch (or VMnet2 switch)

Even though we created the network interfaces on both sides(client and server), there is no established connection between the client and the server. 
To establish the connection, we have to have a virtual switch (or VMnet2 switch) in the VMware. 

1. **Open Virtual Network Editor:**
   - Launch VMware Workstation on your host machine.
   - In the menu bar, navigate to "Edit" and select "Virtual Network Editor."

2. **Add a Custom Network (VMnet):**
   - In the Virtual Network Editor window, click on the "Change Settings" button.
   - Click on the "Add Network..." button.

3. **Select VMnet 2:**
   - In the "Add Network" window, select "VMnet 2" from the drop-down menu.

4. **Configure VMnet 2:**
   - You can configure the settings for VMnet 2, including the subnet IP address range, subnet mask.

   ```
   IP address: 10.0.1.0
   Subnet mask: 255.255.255.0
   ```
    
   - Uncheck the box that says "Use local DHCP service to distribute IP address to VMs." (This ensures that the DHCP service is disabled for VMnet 2.)
   - Keep the remaining box checked.

6. **Save Changes:**
   - Click on the "Apply" or "OK" button to save the changes.

7. **Close Virtual Network Editor:**
   - Close the Virtual Network Editor.

## 5. Network Configurations:

### Find the MAC address of a vm's network interfaces 

Now, we have the created virtual interfaces on both sides, and we need to assign IP addresses to those interfaces.
To do that, we must know the MAC address of those interfaces (VMnet2).

1. **Ensure VM is Powered Off:**
   - Make sure the virtual machine is powered off.

2. **Open VM Settings:**
   - In VMware Workstation, select the virtual machine you're interested in.
   - Click on "Edit virtual machine settings" or simply right-click and choose "Settings."

3. **View Network Adapter Settings:**
   - In the Virtual Machine Settings window, you'll see a list of hardware components. Find the network adapters in the list.

4. **Find MAC Address:**
   - For each network adapter (e.g., Network Adapter 1, Network Adapter 2), you can find the MAC address displayed in the settings.
   - The MAC address is often labeled as "MAC Address" or similar.

5. **Take Note of VMnet2 Connection:**
   - If you're specifically looking for the MAC address of the network adapter connected to VMnet2, ensure that the appropriate network adapter is set to use VMnet2.

### Server:

1. Use the `ifconfig` command to list all installed network adapters on the VM. Identify adapters used by NAT and VMnet2.
   - Hint: Adapter used by NAT may already have an IP in the 192.168 range.

2. Server Network Configurations (using `nmtui`):

   - Open a terminal and run the command `nmtui`.
   - Navigate to "Edit Connection."
   - Select the adapter used in VMnet2 and edit it.
   - In IPv4 settings, select the type as Manual and enter the following IP configurations:

     ```
     IP address: 10.0.1.2/24
     Gateway: 10.0.1.1
     ```

   - Save the configurations (Select OK).

3. Connection Activation (using `nmtui`):

   - Run the command `nmtui`.
   - Select "Activate Connection."
   - Choose the adapter to be activated.

### Client:

1. Configure the Network Adapter connected to VMnet2 using the following details:

   ```
   IP address: 10.0.1.3/24
   Gateway: 10.0.1.1
   ```

2. Activate the connection using the GUI.

