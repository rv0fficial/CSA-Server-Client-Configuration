# DNS Server Configuration Lab Readme

## Overview

This lab is designed to provide students with hands-on experience in installing and configuring a DNS (Domain Name System) server in a Linux environment. The lab also covers the usage of `yum install` to install packages in Linux. It is essential to have completed the previous labs to proceed with this one.

## Step 01 - Configuring DNS Server

1. Open the DNS server configuration file for editing with root privileges:
   ```bash
   [root@server ~]# vi /etc/named.conf
   ```
2. Add two zones to the `named.conf` file: one forward lookup zone and one reverse lookup zone.

   ```bind
   // ... (existing configurations)

   ##### Forward Lookup Zone #####
   zone "ns" IN {
       type master;
       file "forward.csa.lk";
       allow-update { none; };
   };

   ##### Reverse Lookup Zone #####
   zone "1.0.10.in-addr.arpa" IN {
       type master;
       file "reverse.csa.lk";
       allow-update { none; };
   };
   ```

3. Save and close the file.

## Step 02 - Creating Zone Files

4. Change to the named directory to create zone files:
   ```bash
   [root@server ~]# cd /var/named
   ```

5. Create the forward zone file (`forward.csa.lk`):
   ```bash
   [root@server named]# vi forward.csa.lk
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
   @       IN  A           10.0.1.5  <- Provide your server IP address
   @       IN  A           10.0.1.10 <- Provide your client IP address
   mlb-dc1-centos7  IN  A   10.0.1.5 <- Provide your server IP address
   <client hostname>    IN  A   10.0.1.6 <- Provide your client IP address
   ```

6. Save and close the file.

7. Create the reverse zone file (`reverse.csa.lk`):
   ```bash
   [root@server named]# vi reverse.csa.lk
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
   server       IN  A   10.0.1.5  <- Provide your server IP address
   client       IN  A   10.0.1.10 <- Provide your server IP address
   5     IN  PTR         mlb-dc1-centos7.csa.lk.
   10    IN  PTR         <client hostname>.
   ```

8. Save and close the file.

9. Read the provided information to understand the commands and configurations in the zone files.

## Step 03 - Stop Name Server and DHCP Service

10. Stop the name server and DHCP service if running:
    ```bash
    [root@server ~]# service dhcpd stop
    [root@server ~]# service named stop
    ```

11. Open and edit `dhcpd.conf` file:
    ```bash
    [root@server ~]# vi /etc/dhcp/dhcpd.conf
    ```

12. Make the necessary changes to set the domain name and domain-name servers.

    ```dhcp
    # ...
    option domain-name "csa.lk";
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
    [root@server ~]# firewall-cmd --permanent --add-port=53/udp
    ```

15. Load the new firewall rules:
    ```bash
    [root@server ~]# firewall-cmd --reload
    ```

16. Test DNS configuration and zone files for syntax errors:
    ```bash
    [root@server ~]# named-checkconf /etc/named.conf
    [root@server ~]# named-checkzone csa.lk /var/named/forward.csa.lk
    [root@server ~]# named-checkzone csa.lk /var/named/reverse.csa.lk
    ```

17. Restart DHCP and DNS services:
    ```bash
    [root@server ~]# service dhcpd start
    [root@server ~]# service named start
    # OR
    [root@server ~]# service dhcpd restart
    [root@server ~]# service named restart
    ```

## Step 05 - Test DNS Server

18. Test DNS Server using the following commands:
    ```bash
    [root@server ~]# dig csa.lk
    [root@server ~]# dig mlb-dc1-centos7.csa.lk
    [root@server ~]# nslookup mlb-dc1-centos7.csa.lk
    [root@server ~]# nslookup csa.lk
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
