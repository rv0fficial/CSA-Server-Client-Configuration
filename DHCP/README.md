# DHCP Installation and Configuration Guide

## Introduction

The Dynamic Host Configuration Protocol (DHCP) is a network management protocol used on UDP/IP networks. 
A DHCP server dynamically assigns IP addresses and other network configuration parameters to devices on a network, facilitating communication with other IP networks. 
This guide outlines the installation and configuration of DHCP in a CentOS environment, with subsequent testing using Fedora.

## Lab Setup Overview

In Lab 4, students are tasked with installing and configuring a DHCP server in CentOS and verifying its functionality using Fedora.

## Step 1: Disable DHCP Settings in VMnet 2

Ensure that DHCP settings are disabled in VMnet 2 to avoid conflicts during the DHCP server setup.

## Step 2: Installing DHCP in CentOS

To install the DHCP server on CentOS, execute the following command:

```bash
$ yum install -y dhcp
```

## Step 3: Configuring DHCP Settings

Now we need to mention the interface details, which is going to be the DHCP interface. 
Edit the following DHCP configuration file to specify the DHCP interface. Use the following commands:

```bash
vi /etc/sysconfig/dhcpd
```

Assign the network interface using the `DHCPDARGS` line, e.g., `DHCPDARGS=eno335...`. Save and close the file.


```bash
DHCPDARGS=eno335XXXXXXX
```

Go to the mentioned file and paste the above-mentioned line into that file. 
Always after the equal sign, the line should contain the device name of the VMnet2 interface of the centOS. 
You can also use the profile name, but there shouldn't be any spaces in that name. DHCPDARGS means dhcp arguments.

After that, copy the sample DHCP configuration file to the designated directory:

```bash
cp /usr/share/doc/dhcp-4.1.1/dhcpd.conf.sample /etc/dhcp/dhcpd.conf
```
The DHCP version might vary depending on the time. Therefore, it is better to press the `Tab` key on the keyboard to auto-fill the above-line version part. 

Edit the `dhcpd.conf` file:

```bash
vi /etc/dhcp/dhcpd.conf
```

Make the changes as shown below (Set the domain name and domain-name servers).

```bash
option domain-name dsnm.sub
option domain-name-servers server.unixmen.local
```

And, ensure to uncomment the line `authoritative;` if this DHCP server is official for the local network.

When it comes to the bottom part, define the sunbet, range of ip addresses, domain and domain name servers like 
below:

```bash
# A slightly different configuration for an internal subnet.
subnet 10.0.1.0 netmask 255.255.255.0 {
  range 10.0.1.20 10.0.1.30;
  option domain-name-servers server.unixmen.local;
  2 option domain-name "dsnm.sub";
  option routers 10.0.1.1;
  option broadcast-address 10.0.1.255;
  default-lease-time 600;
  max-lease-time 7200;
}
```
After making all the changes you want, save and close the file. Be mindful that if 
you have another unused entries on the dhcpd.conf file, comment them. Otherwise, 
you’ll have issues while starting dhcpd service.

Start the DHCP server service and set it to start automatically on every reboot:

```bash
service dhcpd start
```
If you want to start up the DHCP server at logon to the server session use;

```bash
chkconfig dhcpd on
```

## Step 4: Checking DHCP Server and Client Status

Verify the DHCP server status using:

```bash
systemctl status dhcpd
```

Ensure that DHCP is active and running. On the client side, change the IP settings to Automatic (DHCP). Use the following command to check the IP address assigned:

```bash
ifconfig
```

The DHCP server setup is complete, and successful IP assignment can be confirmed on the client side.

**Note:** To check whether the DHCP is performing, the NAT adapter should be turned off in Centos and Fedora. 
