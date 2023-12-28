# DNS Server Configuration Lab Readme

## Overview

This lab is designed to provide students with hands-on experience in installing and configuring a DNS (Domain Name System) server in a Linux environment. The lab also covers the usage of `yum install` to install packages in Linux. It is essential to have completed the previous labs to proceed with this one.

## Step 01 - Configuring DNS Server

1. Open the DNS server configuration file for editing with root privileges:
   ```bash
   vi /etc/named.conf
   ```
2. Add two zones to the `named.conf` file: one forward lookup zone and one reverse lookup zone.

   ```bind
   options {

   [...]
   
	listen-on port 53 { 127.0.0.1; 10.0.1.2; };

   [...]
   
	allow-query     { localhost; 10.0.1.0/24; };

   [...]
   
   };
   ```

   In the 'option' section, the lines mentioned above should only be changed. The following part should be directly added to this file.  

   ```bind
   //forward look up zone
   zone "ns" IN {
   type master;
   file "forward.csa.lk";
   allow-update { none; };
   };

   //reverse look up zone
   zone "1.0.10.in-addr.arpa" IN { 
   type master;
   file "reverse.csa.lk";
   allow-update { none; };
   };
   ```
   The correct config file is stored in this path: DNS/Config_Files/named.conf

3. Save and close the file.

## Step 02 - Creating Zone Files

4. Change to the named directory to create zone files:
   ```bash
   cd /var/named
   ```

5. Create the forward zone file (`forward.csa.lk`):
   ```bash
   vi forward.csa.lk
   ```
   Add the following lines to the file:

   ```bind
   $TTL 86400
   @   IN  SOA     mlb-dc1-centos7.csa.lk. root.csa.lk. (
           2011071001  ;Serial
           3600        ;Refresh
           1800        ;Retry
           604800      ;Expire
           86400       ;Minimum TTL
   )
   @       IN  NS          mlb-dc1-centos7.csa.lk.
   @       IN  A           10.0.1.2  <- Provide your server IP address
   @       IN  A           10.0.1.3 <- Provide your client IP address
   mlb-dc1-centos7  IN  A   10.0.1.2 <- Provide your server IP address
   csa-cli-fedora29    IN  A   10.0.1.3 <- Provide your client IP address
   ```
   
   The correct config file is stored in this path: DNS/Config_Files/forward.csa.lk

6. Save and close the file.

7. Create the reverse zone file (`reverse.csa.lk`):
   ```bash
   vi reverse.csa.lk
   ```
   Add the following lines to the file:

   ```bind
   $TTL 86400
   @   IN  SOA     mlb-dc1-centos7.csa.lk. root.csa.lk. (
           2011071001  ;Serial
           3600        ;Refresh
           1800        ;Retry
           604800      ;Expire
           86400      ) ;Minimum TTL

   @       IN  NS          mlb-dc1-centos7.csa.lk.
   @       IN  PTR         csa.lk.
   mlb-dc1-centos7       IN  A   10.0.1.2  <- Provide your server IP address
   csa-cli-fedora29      IN  A   10.0.1.3 <- Provide your server IP address
   2     IN  PTR         mlb-dc1-centos7.csa.lk.
   3    IN  PTR         csa-cli-fedora29.csa.lk.
   ```

   The correct config file is stored in this path: DNS/Config_Files/reverse.csa.lk

8. Save and close the file.

9. Read the provided information to understand the commands and configurations in the zone files.

## Step 03 - Stop Name Server and DHCP Service

10. Stop the name server and DHCP service if running:
    ```bash
    service dhcpd stop
    service named stop
    ```

11. Open and edit `dhcpd.conf` file:
    ```bash
    vi /etc/dhcp/dhcpd.conf
    ```

12. Make the necessary changes to set the domain name and domain-name servers.

    ```dhcp
    # ...
    option domain-name "csa.sub";
    option domain-name-servers mlb-dc1-centos7.csa.lk;
    # ...
    subnet 10.0.1.0 netmask 255.255.255.0 {
        option domain-name-servers mlb-dc1-centos7.csa.lk;
        # ...
    }
    # ...
    ```

13. Save and close the file.

## Step 04 - Configure Firewall and Test DNS

14. Allow DNS communication via the CentOS firewall:
    ```bash
    firewall-cmd --permanent --add-port=53/udp
    ```

15. Load the new firewall rules:
    ```bash
    firewall-cmd --reload
    ```

16. Test DNS configuration and zone files for syntax errors:
    ```bash
    named-checkconf /etc/named.conf
    named-checkzone csa.lk /var/named/forward.csa.lk
    named-checkzone csa.lk /var/named/reverse.csa.lk
    ```

17. Restart DHCP and DNS services:
    ```bash
    service dhcpd start
    service named start
    # OR
    service dhcpd restart
    service named restart
    ```

## Step 05 - Test DNS Server

18. Test DNS Server using the following commands:
    ```bash
    dig csa.lk
    dig mlb-dc1-centos7.csa.lk
    nslookup mlb-dc1-centos7.csa.lk
    nslookup csa.lk
    ```

## Step 06 - Configuration on Fedora Client

19. Go to the Fedora client (ensure root privileges).

20. Restart its network services:
    ```bash
    [root@server ~]# service network restart
    ```

21. Use the `hostname` command to check the machine's hostname:
    ```bash
    [root@localhost ~]# hostname
    ```

22. Follow the procedure from DNS-Part 01 in the lab sheet to change the Fedora machine's hostname.

23. After changing the machine name, add DNS server details to the client's `resolv.conf` file:
    ```bash
    [root@server ~]# vi /etc/resolv.conf
    ```
    Add the following lines:
    ```bind
    # Generated by NetworkManager
    search mlb-dc1-centos7.csa.lk
    nameserver 10.0.1.5
    ```

24. Restart its network services:
    ```bash
    [root@server ~]# service network restart
    ```

25. Test DNS Server from the client side using the commands mentioned in Step 05.

## Step 08 - Explore Further (Self-Exploration)

26. Try specifying new zones (forward and reverse lookup).

27. Attempt to start a service using available GUI options.

**Note:** Ensure that there are no errors in the zone files and the DHCP and DNS configurations for smooth operations.
