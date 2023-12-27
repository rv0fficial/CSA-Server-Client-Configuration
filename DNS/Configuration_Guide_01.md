Main objective of this lab is to equip the students with knowledge on how to install and configure a 
DNS server in a Linux environment. In parallel students will gain knowledge on using yum install 
to install a package in Linux.

# DNS

DNS is the naming service provided by the Internet for TCP/IP networks. It was developed so that 
machines on the network could be identified with common names instead of Internet addresses. 
DNS performs naming between hosts within your local administrative domain and across domain 
boundaries.

The collection of networked machines that use DNS are referred to as the DNS namespace. The 
DNS namespace can be divided into a hierarchy of domains. A DNS domain is a group of 
machines. Each domain is supported by two or more name servers, a principal server and one or 
more secondary servers. Each server implements DNS by running a daemon called in.named. 
On the client's side, DNS is implemented through the “resolver.” The resolver's function is to 
resolve users' queries. It queries a name server, which then returns either the requested information 
or a referral to another server.

---

# DNS Server Configuration README

## Step 01:

1. Stop any running services:
    ```bash
    service dhcpd stop
    ```
    or
    ```bash
    service <service_name> stop
    ```

2. Set VMware network settings to `vmnet2-Host_only` and configure the server IP settings manually:
    - IP Address: 10.0.1.2
    - Netmask: 255.255.255.0
    - Gateway: 10.0.1.1

3. Restart the network service.

## Step 02:

4. Open a terminal.

5. Provide root privileges to the session.

6. Use the `hostname` command to view the machine's hostname:
    ```bash
    hostname
    ```

7. Use the `hostname -f` or `hostname --fqdn` command to view the Fully Qualified Domain Name of the machine:
    ```bash
    hostname -f
    hostname --fqdn
    ```
    
    The above commands will show the default hostname; It should change as below.
   
9. Change the current machine (hostname) domain to `mlb-dc1-centos7.csa.lk`.

10. Use the `nmtui` tool to set the static hostname in the `/etc/hostname` file.

11. Set the hostname.

12. Restart the `systemd-hostnamed` to notice the change in the static hostname:
    ```bash
    systemctl restart systemd-hostnamed
    ```

13. Verify the change in hostname:
    ```bash
    hostname
    cat /etc/hostname
    cat /etc/sysconfig/network
    ```

14. Use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name of the machine.

15. Open the `/etc/hosts` file:
    ```bash
    vi /etc/hosts
    ```

16. Edit the hosts file to add:
    ```
    10.0.1.2 mlb-dc1-centos7.csa.lk
    ```
    
    Add the above line to that file, and alignments should be match.

17. Again, use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name.

18. Reboot your machine to apply changes.

## Step 03:

18. Stop any running services.

19. Set VMware network settings to `vmnet8-NAT` and configure the server IP settings to obtain IP address automatically (Not required).

20. Restart the network service.

21. Again, use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name. Has it changed?

    Outputs:
    
    `hostname –s` --> mlb-dc1-centos7
    
    `hostname –-fqdn` --> mlb-dc1-centos7.csa.lk
    
23. Open the `resolv.conf` file under `/etc` folder and look inside:
    ```bash
    cat /etc/resolv.conf
    ```
    
    Output:

    ![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/d263c601-a043-483e-94fe-f6e0b98f431c)

### Note:

The resolver is a set of routines that provide access to the Internet Domain Name System. See resolver(3RESOLV).resolv.conf is a configuration file that contains the information that is read by the resolver routines the first time they are invoked by a process. The file is designed to be human readable and contains a list of keywords with values that provide various types of resolver information. 

The resolv.conf file contains the following configuration directives: 

Nameserver 

Specifies the Internet address in dot-notation format of a name server that the resolver is to query. Up to MAXNS name servers may be listed, one per keyword. See <resolv.h>. If there are multiple servers, the resolver library queries them in the order listed. If no name server entries are present, the resolver library queries the name server on the local machine. The resolver library follows the algorithm to try a name server until the query times out. It then tries the the name servers that follow, until each query times out. It repeats all the name servers until a maximum number of retries are made. 
    
Domain 

Specifies the local domain name. Most queries for names within this domain can use short names relative to the local domain. If no domain entry is present, the domain is determined from sysinfo(2) or from gethostname(3C). (Everything after the first `.' is presumed to be the domain name.) If the host name does not contain a domain part, the root domain is assumed. You can use the LOCALDOMAIN environment variable to override the domain name. 

Search

The search list for host name lookup. The search list is normally determined from the local domain name. By default, it contains only the local domain name. You can change the default behavior by listing the desired domain search path following the search keyword, with spaces or tabs separating the names. Most resolver queries will be attempted using each component of the search path in turn until a match is found. This process may be slow and will generate a lot of network traffic if the servers for the listed domains are not local. Queries will time out if no server is available for one of the domains. 
    
The search list is currently limited to six domains and a total of 256 characters. 

## Step 04:

23. Open a terminal.

24. Provide root privileges to the session.

25. Use the `ping` command to query existing internet domains:
    ```bash
    ping google.lk
    ping yahoo.com
    ```

26. To install DNS server on CentOS 6.5, enter the following command:
    ```bash
    yum install –y bind*
    ```

## Step 05:

27. Set VMware network settings to `vmnet2-Host_only` and configure the server IP settings to obtain IP address manually:
    - IP Address: 10.0.1.2
    - Netmask: 255.255.255.0
    - Gateway: 10.0.1.1

28. Restart the network service.

    This step is already completed at the being of this implementation, before the DHCP part.

## Step 06:

29. `named` is the Domain Name Service name.

30. Check the status of the `named` service:
    ```bash
    systemctl status named
    ```

31. Start the service up:
    ```bash
    systemctl start named
    ```

32. Check the status again.

## Step 07:

33. Open the configuration files of the DNS server:
    ```bash
    vi /etc/named.conf
    ```

34. Read the file and try to understand the coding.

The named.conf file is a collection of statements using nested options surrounded by opening and closing ellipse characters, { }. 
Administrators must be careful when editing named.conf to avoid syntactical errors as many seemingly minor errors will prevent the named service from starting 
 
`listen-on port 53 { 127.0.0.1; };   # PRIMARY Bind DNS IP Address and listing port` 
 
 
### Note:  
Specifies the network interface on which named listens for queries. By default, all interfaces are used.
Using this directive on a DNS server which also acts a gateway, BIND can be configured to only answer queries that originate from one of the networks.  
 
What is the listening port number of the dns server? 

Why use this port number? 

Why does the DNS server need a listening port in the first place? 

Do you think the IP address should change? 

`listen-on-v6 port 53 { ::1; };` 
 
### Note:  
What is the difference between the previous listening declaration and this one? 
 
`directory     "/var/named";` 
 
### Note:  
Specifies the named working directory if different from the default value, /var/named/. 
 
`allow-query     { localhost; };`       
           
### Note:  
Specifies which hosts are allowed to query this nameserver. By default, all hosts are allowed to query. An access control list, or collection of IP addresses or networks may be used here to only allow particular hosts to query the nameserver. 
Does anything needed to be changed? 
How are you going to allow query from your network? 
 
`dnssec` 
 
### Note:  
The Domain Name System Security Extensions (DNSSEC) is a suite of Internet Engineering Task Force (IETF) specifications for securing certain kinds of information provided by the Domain Name System (DNS) as used on Internet Protocol (IP) networks. It is a set of extensions to DNS which provide to DNS clients (resolvers) origin authentication of DNS data, authenticated denial of existence, and data integrity, but not availability or confidentiality. 
 
` zone "." IN {     type hint;     file "named.ca"; 
};` 
 
### Note:  
A zone statement defines the characteristics of a zone such as the location of its configuration file and zone-specific options. This statement can be used to override the global options statements. 

a.	allow-query — Specifies the clients that are allowed to request information about this zone. The default is to allow all query requests. 

b.	file — Specifies the name of the file in the named working directory that contains the zone's configuration data. 

c.	type — Defines the type of zone.Below is a list of valid options: 

       i.	forward — Forwards all requests for information about this zone to other nameservers. 
   
       ii.	hint — A special type of zone used to point to the root nameservers which resolve queries when a zone is not otherwise known. No configuration beyond the default is necessary with a hint zone. 
   
       iii.	master — Designates the nameserver as authoritative for this zone. A zone should be set as the master if the zone's configuration files reside on the system. 
   
       iv.	slave — Designates the nameserver as a slave server for this zone. Also specifies the IP address of the master nameserver for the zone. 

## Step 08 – Do This by Yourself:

35. Specify two zones: one forward lookup zone and one reverse lookup zone.

36. Try to start a service using the GUI options available.

--- 

Feel free to adjust the formatting or add any additional information that may be relevant to your users.
