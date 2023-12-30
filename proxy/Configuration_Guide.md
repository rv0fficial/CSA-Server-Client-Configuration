# Squid Proxy Server Configuration

## 1. Configure Squid Proxy Server

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

9. Install Command for NTP

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

Start Squid

```bash
# Command to start Squid
systemctl start squid
```

Squid.conf File Command

```bash
# Command to view squid.conf before editing
cat /etc/squid/squid.conf
```

### 1.11 Edited squid.conf

```bash
# Command to edit squid.conf
vi /etc/squid/squid.conf
```

### 1.12 Copy Squid Original File to Another File

```bash
# Command to copy squid.conf to a backup file
cp /etc/squid/squid.conf /etc/squid/squid.conf.backup
```

### 1.13 Checking squid.conf Errors

```bash
# Command to check squid.conf for errors
squid -k parse
```

### 1.14 Restarting Squid

```bash
# Command to restart Squid
systemctl restart squid
```

### 1.15 Change Firefox Connection Settings

Update Firefox connection settings to use the proxy.

### 1.16 Allowing TCP & UDP Ports

```bash
# Commands to allow TCP and UDP ports
firewall-cmd --permanent --add-port=3128/tcp
firewall-cmd --permanent --add-port=3128/udp
firewall-cmd --reload
```

## 2. Check Internet Access through the Proxy

### 2.1 Checking Internet Access through the Proxy

```bash
# Command to check internet access through the proxy
curl -x http://<proxy_ip>:3128 http://www.google.com
```

## 3. Enable the Appropriate Cache Replacement Policy

### 3.1 Adding a Cache Replacement Policy

Edit the Squid.conf file to add a cache replacement policy.

## 4. Create a Resource Record for the Proxy in Your DNS Server

### 4.1 Command for Open forward.csa.lk File

```bash
# Command to open forward.csa.lk file
vi /var/named/forward.csa.lk
```

### 4.2 Forward.csa.lk

Edit the forward.csa.lk file to add resource records for the proxy.

### 4.3 Command for Open reverse.csa.lk File

```bash
# Command to open reverse.csa.lk file
vi /var/named/reverse.csa.lk
```

### 4.4 Reverse.csa.lk

Edit the reverse.csa.lk file to add resource records for the proxy.

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

## 7. Set the Size of the Cache Folder

### 7.1 Changing the Cache Size

```bash
# Command to change the cache size
vi /etc/squid/squid.conf
```

Update the cache size in the squid.conf file.
