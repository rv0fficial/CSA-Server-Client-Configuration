# Squid Proxy Server Configuration

## 1. Configure Squid Proxy Server

After configuring the Squid® proxy server check whether you can access internet through the proxy.

1. **Fedora Client (VMnet2) to CentOS Server:**
   - The Fedora client sends its HTTP requests to the CentOS server's IP address associated with the VMnet2 adapter (e.g., 10.0.1.2).

2. **CentOS Server (Squid Proxy) Processes the Request:**
   - Squid proxy running on the CentOS server (listening on the VMnet2 IP address) processes the incoming HTTP requests from the Fedora client.

3. **CentOS Server to Internet (NAT Adapter):**
   - After processing, if the request requires internet access, Squid forwards the request to the internet using the CentOS server's NAT adapter.

4. **Internet Response to CentOS Server (NAT Adapter):**
   - The internet responds to the request, and the response is sent back to the CentOS server via the NAT adapter.

5. **CentOS Server (Squid Proxy) to Fedora Client (VMnet2):**
   - Squid receives the internet response and sends it back to the Fedora client over the VMnet2 adapter.

6. **Fedora Client Processes the Response:**
   - The Fedora client processes the response received from the CentOS server and displays the content.

In summary, the CentOS server acts as an intermediary (proxy) between the Fedora client and the internet. The Fedora client sends its requests to the CentOS server, which, in turn, forwards them to the internet and returns the responses to the Fedora client. The NAT adapter in the CentOS server enables internet connectivity for the Squid proxy.

---

### Centos Settings

1. Stop DNS and DHCP

    ```bash
    service dhcpd stop
    service named stop
    ```

2. Checking Squid Availability

    ```bash
    squid -v
    ```

    This command will display the version of Squid if it's installed. 
    If Squid is not installed, you will likely see a message indicating that the command is not found or not recognized. 

3. Checking the Internet Connection

    ```bash
    ping google.com
    ```

4. Installing Squid

    ```bash
    yum install squid
    ```

5. Checking Squid

    ```bash
    systemctl status squid
    ```

6. Before following the below steps, make sure to stop the squid. (IMPORTANT)

    ```bash
    systemctl stop squid
    ```
    
7. Check whether the date, time and time zone in CentOS is correct or not.

    ```bash
    date
    timedatectl
    ```

    If it is incorrect according to the actual dates, follow the steps below.

8. Change the false time zone to the real one. 

    ```bash
    timedatectl set-timezone Asia/Colombo
    ```
    
    Turn off your vmnat adapter and turn on your NAT adapter

10. Install Command for NTP

    ```bash
    yum install ntp
    ```
    
    if that didn’t work try running `yum install ntpd`
   
11. Enabling NTPD

    ```bash
    systemctl enable ntpd
    ```

12. Run systemctl start ntpd

    ```bash
    systemctl start ntpd
    ```

    **Note:**
   The Network Time Protocol (NTP) is not a strict requirement for configuring Squid,
   but having accurate time settings on your server is generally a good practice.
   NTP ensures that the system time is synchronized with a reliable time source,
   which can be important for various system processes, log entries, and SSL/TLS certificates (if used).

14. Squid.conf File Command

    ```bash
    cat /etc/squid/squid.conf
    ```

15. Go to the editing mode of squid.conf

    ```bash
    vi /etc/squid/squid.conf
    ```

16. Open the Squid configuration file (`/etc/squid/squid.conf`) and verify the `http_port` directive.
    Ensure that it is correctly configured with the desired IP address and port.

    ```conf
    http_port 10.0.1.2:3128
    ```

18. Check the firewall settings to ensure that the port used by Squid is allowed (Allowing TCP Ports). 

    ```bash
    firewall-cmd --permanent --add-port=3128/tcp
    firewall-cmd --reload
    ```

19. Checking squid.conf Errors

    ```bash 
    squid -k parse
    ```

20. Restarting Squid

    ```bash
    systemctl restart squid
    ```
    
21. Examine the Squid logs for more detailed error messages. Check both the `cache.log` and `access.log` files.

    ```bash
    tail -f /var/log/squid/cache.log
    tail -f /var/log/squid/access.log
    ```

**Note:** Remember to turn on both adapters on the centos side while testing.

If NAT is disabled, Squid is unable to bind to the specified IP address and port. 
(Error message --> "Cannot bind socket FD 11 to 10.0.1.2:3128: (99) Cannot assign requested address")

### Fedora Settings

22. Access Internet through Proxy from Fedora:
    
   - Open a web browser on your Fedora client (Typically, when testing a proxy, you would configure the proxy
     settings directly in the web browser on your Fedora client).
     
     Go to the proxy settings and configure it to use the IP address and port where Squid is listening on your CentOS server.
     This should be the IP address of the vmnet2 adapter on your CentOS server `10.0.1.2`, and the default port for Squid is `3128`.
    
   - Disable the NAT adapter on your Fedora machine to ensure that it goes through the Squid proxy.
   - Try to access a website, and the request should go through the Squid proxy on your CentOS server.
   - Monitor Squid logs on your CentOS server to verify that requests are being processed.

---

## 2. Squid Cache Directory Setup

Enable caching (set a cache folder and the size of the cache)

1. Change to the Squid directory:

    ```bash
    cd /var/spool/squid
    ```

2. Create a cache directory:

    ```bash
    mkdir cache
    ```
    This creates a directory named `cache` within the Squid spool directory.
    
3. Set ownership to the Squid user and group:

    ```bash
    chown squid:squid cache/
    ```

   This changes the ownership of the `cache` directory to the `squid` user and group. 
   It's important for Squid to have the necessary permissions to write to the cache.

4. Set permissions recursively:

    ```bash
    chmod -R 750 cache/
    ```

    This sets the permissions for the `cache` directory, allowing the `squid` user to read, write, and execute, while restricting other users.

5. After creating and configuring the cache directory, you need to update the Squid configuration file to specify this directory for caching.

    ```bash
    vi /etc/squid/squid.conf
    ```

   ```conf
   cache_dir ufs /var/spool/squid/cache 100 16 256
   ```
   Ensure that this line reflects the correct path to your cache directory.

6. Set Coredump Directory

   Set the coredump directory to the cache directory, you would typically do this with the `coredump_dir` directive:
   
   ```conf
   coredump_dir /var/spool/squid/cache
   ```

7. Remember to restart Squid after making changes to the configuration:

   ```bash
   systemctl restart squid
   ```

---

## 2. Set the Size of the Cache Folder

1. Changing the Cache Size

   ```bash
   vi /etc/squid/squid.conf
   ```

The size of the cache folder can be set as follows,

   ```conf
   cache_dir ufs /var/spool/squid/cache 2000 16 256
   ```

1. **Type of Cache (`ufs`):**
   - `ufs` stands for "Unix File System," which is a type of cache storage on the local file system.

2. **Path to Cache Directory (`/var/spool/squid/cache`):**
   - This is the location on your file system where Squid will store its cache.

3. **Directory Size (`2000`):**
   - This value represents the total size of the cache directory in megabytes. In this example, it's set to 2000 MB, which is equivalent to 2 GB.

4. **Minimum Space Required (`16`):**
   - This is the minimum amount of disk space that Squid requires to be available in the cache directory before it considers storing more objects.
     In this case, it's set to 16 MB.

5. **Maximum Object Size (`256`):**
   - This is the maximum size of an individual object that Squid will store in the cache. In this example, it's set to 256 KB.

These values help control the behavior of the cache. For instance, Squid won't store objects larger than the specified maximum object size (`256 KB`), 
and it won't consider storing objects if the available disk space falls below the minimum space required (`16 MB`).

---

## 3. Enable the Appropriate Cache Replacement Policy

To enable the appropriate cache replacement policy in Squid, you can use the `cache_replacement_policy` directive in your Squid configuration file. The cache replacement policy determines how Squid selects objects to remove from the cache when it reaches its storage limits. Common cache replacement policies include LRU (Least Recently Used), LFUDA (Least Frequently Used with Dynamic Aging), and others.

1. Open the Squid configuration file in a text editor. The file is typically located at `/etc/squid/squid.conf`:

   ```bash
   vi /etc/squid/squid.conf
   ```

2. Locate or add the `cache_replacement_policy` directive. Uncomment the line if necessary, and set the desired cache replacement policy.
   For example, to use the LRU policy:

   ```conf
   cache_replacement_policy lru
   ```

   Replace `lru` with the cache replacement policy you want to use.

4. Save the changes and exit the text editor.

5. Restart Squid to apply the changes:

   ```bash
   sudo systemctl restart squid
   ```

---

## 4. Create a Resource Record for the Proxy in Your DNS Server

1. Command for Open forward.csa.lk File

    ```bash
    vi /var/named/forward.csa.lk
    ```

2. Edit the forward.csa.lk file to add resource records for the proxy.

    ```bash
    proxy-centos7		IN	A	10.0.1.2
    ```
    
    The config file is stored in this path: proxy/Config_Files/forward.csa.lk
    
3. Command for Open reverse.csa.lk File

    ```bash
    vi /var/named/reverse.csa.lk
    ```

4. Edit the reverse.csa.lk file to add resource records for the proxy.

    ```bash
    proxy-centos7 		IN	A	10.0.1.2
    [...]
    2	IN	PTR	proxy-centos7.csa.lk.
    ```

    The config file is stored in this path: proxy/Config_Files/reverse.csa.lk

---

## 5. Implement SSL in the Proxy Environment

### 5.6 Checking the Date and Time

```bash
# Command to check the date and time
date
```

### 5.7 Install Command for mod_ssl

```bash
# Command to install mod_ssl
yum install mod_ssl
```

### 5.8 Installed mod_ssl

```bash
# Command to verify the installation of mod_ssl
ls /etc/httpd/modules | grep mod_ssl
```

### 5.9 Changing the Lines in squid.conf

```bash
# Command to edit squid.conf
vi /etc/squid/squid.conf
```

### 5.10 Changing the Lines in squid.conf

Update specific lines in the squid.conf file.

### 5.11 Command for Create a Self-signed SSL Certificate

```bash
# Command to create a self-signed SSL certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/squid/squid.key -out /etc/squid/squid.crt
```

### 5.12 Creating Self-signed SSL Certificate

```bash
# Command to create a self-signed SSL certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/squid/squid.key -out /etc/squid/squid.crt
```

### 5.13

Make a trustworthy certificate that may be used in a browser.

### 5.14 Configure the Permissions

```bash
# Command to configure permissions for SSL certificate
chmod 600 /etc/squid/squid.*
chown squid:squid /etc/squid/squid.*
```

### 5.15 Create a Folder for New Certificates

```bash
# Command to create a folder for new certificates
mkdir /etc/squid/cert
```

### 5.16 Changing Squid Service and Extra Line

```bash
# Command to edit squid.conf
vi /etc/squid/squid.conf
```

### 5.17 Starting Squid

```bash
# Command to start Squid
systemctl start squid
```

### 5.18

Update the Squid service to use SSL.

### 5.19 Running Commands in Fedora Terminal

Run the necessary commands in the Fedora terminal.

### 5.20 Import the Certificates

```bash
# Command to import the certificates
cp /etc/squid/squid.crt /usr/share/pki/ca-trust-source/anchors/
update-ca-trust
```

### 5.21 Created Certificate

Certificates have been created successfully.

## 6. Select Leaven Web Pages for the Simulation Test

### 6.1 Clear History in Firefox

```bash
# Command to clear history in Firefox
Ctrl + Shift + Del
```

### 6.2 Loading 10 Webpages



Open 10 webpages in Firefox.

### 6.3 Command for Check the Cache Size

```bash
# Command to check the cache size
du -sh /var/spool/squid
```

### 6.4 Checking nmtui

```bash
# Command to check nmtui for network settings
nmtui
```

### 6.5 Clear History in Firefox

```bash
# Command to clear history in Firefox
Ctrl + Shift + Del
```

### 6.6 Loading  Websites Again

Visit the same 10 webpages again without internet connection.

### 6.7 Checking nmtui

```bash
# Command to check nmtui for network settings
nmtui
```

### 6.8 Loading the Last Web Page

Load the last webpage after loading the initial 10 pages.

### 6.9 Command for Check the Cache Size

```bash
# Command to check the cache size
du -sh /var/spool/squid
```



Update the cache size in the squid.conf file.
