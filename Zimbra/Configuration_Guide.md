# Zimbra Installation and Configuration

## 1. Installing Zimbra

1.1. Install Zimbra using the `install.sh` script, as illustrated in Figure 1.3.1.

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/b05238fb-e6a2-41c9-ba1e-8730a81dad23)

Figure 1.3.1: Installing Zimbra

After system checks, agree to the license (press 'y') to proceed with the installation process.

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/b68ba743-1393-48c8-aedd-bb33fe00c2e9)

Figure 1.3.2: Installing Zimbra

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/ca165ec7-1224-4df7-89d7-880c3f3f06ec)

Figure 1.3.3: Installing Zimbra

Prompt 'y' for all install packages, including zimbra-ldap. Optionally, remove the dnscache package later. 
Here I gave ‘y’ to dnscache too. But, I had to remove that package later (steps are shown in this report)  

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/0c6898cc-4dc7-4532-8bad-a081b5ba8002)

Figure 1.3.4: Installing Zimbra

Continue by pressing 'y' for all, as shown in Figure 1.3.5.

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/599d6200-bee7-4a21-9935-0d0b7a3ce67b)

Finally, the Zimbra installation process begins.

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/afab8a32-05e3-4686-ae77-08cec3d30367)

Figure 1.3.6: Installing Zimbra

Access the Store Configuration by pressing '7' (Figure 1.3.7).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/efac2c73-2382-42b1-8186-b4c1fea6726b)

Figure 1.3.7: Installing Zimbra

Set the Admin Password for the admin account (Figure 1.3.8). 

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/5c7cd8fa-4e54-40e9-a7cd-ca3392dde39c)

Figure 1.3.8: Installing Zimbra

Ensure the password status is 'Set' and press 'r' to return to the main menu (Figure 1.3.9).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/118f07f0-ac2c-4b5c-8273-47f7e712e301)

Figure 1.3.9: Installing Zimbra

To apply and save the configuration, follow the steps in Figure 1.3.10. 

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/4193ff0a-41ef-4d3b-a9ad-32b2fc7cca7c)

Figure 1.3.10: Installing Zimbra

Press Enter to complete the Zimbra configuration (Figure 1.3.11).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/89016874-aa29-4621-861c-8cd50a0ad848)

Figure 1.3.11: Installing Zimbra

## 2. Zimbra Initial Configuration

### 2.1. Set TCP port number

Set the TCP port number for the Zimbra mail server (Figure 2.1).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/b92e53d3-4186-4c87-b0a2-5ec7fb2534bf)

Figure 2.1: Set TCP port number

### 2.2. Login to Zimbra Admin Account Using Client Machine

On the client Fedora VM, open Firefox and go to https://10.0.1.2:7071 (Figure 2.2.1). If it says 'Your connection is not secure,' click 'Advanced.'

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/c518145f-b42d-4ad1-b05c-f22ad33cc9cb)

Figure 2.2.1: Login to Zimbra admin account using client machine

Click 'Add Exception' (Figure 2.2.2), then 'Confirm Security Exception' (Figure 2.2.3). You will be redirected to the Zimbra admin login page (Figure 2.2.4).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/79dfadde-9367-4936-a1fa-1de0d183b28e)

Figure 2.2.2: Login to Zimbra admin account using client machine

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/87706733-f57b-4ba8-94b0-7c636b922e5b)

Figure 2.2.3: Login to Zimbra admin account using client machine

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/03493b2d-93b5-4490-bcf2-895ab8bc7ad6)

Figure 2.2.4: Login to Zimbra admin account using client machine

Login with the username 'admin' and the password given during the Zimbra installation to access the admin account (Figure 2.2.5).

![image](https://github.com/rv0fficial/CSA-Server-Client-Configuration/assets/147927710/0120f0cc-08de-46c3-a78a-2ce7cfe34179)

Figure 2.2.5: Login to Zimbra admin account using client machine
