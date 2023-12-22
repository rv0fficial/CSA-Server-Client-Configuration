# DHCP Installation and Configuration Guide

## Introduction

The Dynamic Host Configuration Protocol (DHCP) is a network management protocol used on UDP/IP networks. A DHCP server dynamically assigns IP addresses and other network configuration parameters to devices on a network, facilitating communication with other IP networks. This guide outlines the installation and configuration of DHCP in a CentOS environment, with subsequent testing using Fedora.

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

Edit the DHCP configuration file to specify the DHCP interface. Use the following commands:

```bash
vi /etc/sysconfig/dhcpd
```

Assign the network interface using the `DHCPDARGS` line, e.g., `DHCPDARGS=eth0`. Save and close the file.

Copy the sample DHCP configuration file to the designated directory:

```bash
cp /usr/share/doc/dhcp-4.1.1/dhcpd.conf.sample /etc/dhcp/dhcpd.conf
```

Edit the `dhcpd.conf` file:

```bash
vi /etc/dhcp/dhcpd.conf
```

Make necessary changes, such as setting the domain name, domain-name servers, defining the subnet, IP address range, and other parameters. Ensure to uncomment the line `authoritative;` if this DHCP server is official for the local network.

After editing, save and close the file. Start the DHCP server service and set it to start automatically on every reboot:

```bash
service dhcpd start
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

**Note:** If there are unused entries in `dhcpd.conf`, consider commenting them to prevent issues during DHCP service startup.
