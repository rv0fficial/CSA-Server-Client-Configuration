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

### Network Configurations:

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

### Network Configurations:

1. Configure the Network Adapter connected to VMnet2 using the following details:

   ```
   IP address: 10.0.1.3/24
   Gateway: 10.0.1.1
   ```

2. Activate the connection using the GUI.
