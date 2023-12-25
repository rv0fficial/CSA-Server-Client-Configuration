
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
    - IP Address: 10.0.1.5
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

8. Change the current machine domain to `csa.lk`.

9. Use the `nmtui` tool to set the static hostname in the `/etc/hostname` file.

10. Set the hostname.

11. Restart the `systemd-hostnamed` to notice the change in the static hostname:
    ```bash
    systemctl restart systemd-hostnamed
    ```

12. Verify the change in hostname:
    ```bash
    hostname
    cat /etc/hostname
    cat /etc/sysconfig/network
    ```

13. Use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name of the machine.

14. Open the `/etc/hosts` file:
    ```bash
    vi /etc/hosts
    ```

15. Edit the hosts file to add:
    ```
    127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
    10.0.1.5 mlb-dc1-centos7.csa.lk
    ```

16. Again, use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name.

17. Reboot your machine to apply changes.

## Step 03:

18. Stop any running services.

19. Set VMware network settings to `vmnet8-NAT` and configure the server IP settings to obtain IP address automatically.

20. Restart the network service.

21. Again, use `hostname -f` or `hostname --fqdn` to view the Fully Qualified Domain Name. Has it changed?

22. Open the `resolv.conf` file under `/etc` folder and look inside:
    ```bash
    cat /etc/resolv.conf
    ```

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
    - IP Address: 10.0.1.5
    - Netmask: 255.255.255.0
    - Gateway: 10.0.1.1

28. Restart the network service.

## Step 06:

29. `named` is the Domain Name Service name.

30. Check the status of the `named` service:
    ```bash
    service named status
    ```

31. Start the service up:
    ```bash
    service named start
    ```

32. Check the status again.

## Step 07:

33. Open the configuration files of the DNS server:
    ```bash
    vi /etc/named.conf
    ```

34. Read the file and try to understand the coding.

### Note:

- Follow the instructions in the provided code comments for explanations and additional information.

## Step 08 – Do This by Yourself:

35. Specify two zones: one forward lookup zone and one reverse lookup zone.

36. Try to start a service using the GUI options available.

--- 

Feel free to adjust the formatting or add any additional information that may be relevant to your users.
