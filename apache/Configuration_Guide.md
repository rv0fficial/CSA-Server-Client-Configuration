# Apache Web Server Installation and SSL Configuration

## Installation of Apache Web Server

1. Install the Apache Web Server on your CentOS 7 server using the following command:

   ```bash
   yum install httpd*
   ```

2. Allow both HTTP and HTTPS ports through the firewall:

   ```bash
   firewall-cmd --permanent --add-port=80/tcp
   firewall-cmd --permanent --add-port=443/tcp
   ```

3. Reload the firewall configurations:

   ```bash
   firewall-cmd --reload
   ```

4. Default root directory for the Apache Web Server is "/var/www/html". Create a simple .html file:

   ```bash
   cd /var/www/html
   vi index.html
   ```

5. Start the Apache Server:

   ```bash
   systemctl start httpd
   ```

6. If you want to enable the initiation of Apache Server at system boot time, use the following command:

   ```bash
   systemctl enable httpd
   ```

7. Open up the client and use the IP address of the Web Server as the URL in the client's web browser.

## Enabling HTTPS Connection for the Apache Web Server

**Note:** By default, the web server is loaded using HTTP protocol, which is not secure. 
To use HTTPS, you need to sign your website with a Certificate Authority (CA). 
For this practical, we will self-sign our website and add the CA certificate to the client's web browser.

1. Install SSL in the CentOS 7 Server:

   ```bash
   yum install mod_ssl openssl
   ```

2. Create a private key for the website:

   ```bash
   openssl genrsa -out ca.key 2048
   ```

3. Create a signing request for the website:

   ```bash
   openssl req -new -key ca.key -out ca.csr
   ```

   Follow the prompts to fill in the required information during the creation of the signing request.

4. Create the self-signed certificate, valid for 365 days:

   ```bash
   openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
   ```

5. Copy all the necessary files to the relevant locations:

   ```bash
   cp ca.crt /etc/pki/tls/certs/
   cp ca.key /etc/pki/tls/private/
   cp ca.csr /etc/pki/tls/private/
   ```

6. Set the Apache Web Server to use the created certificates by editing relevant lines in the ssl.conf file:

   ```bash
   vi /etc/httpd/conf.d/ssl.conf
   ```

   Uncomment the following lines and change the required values:

   ```apache
   DocumentRoot "/var/www/html"
   ServerName 10.0.1.2:443
   SSLEngine on
   SSLCertificateFile /etc/pki/tls/certs/ca.crt
   SSLCertificateKeyFile /etc/pki/tls/private/ca.key
   ```

7. Restart the Apache Web Server to apply the changes:

   ```bash
   systemctl restart httpd
   ```

8. Open the website using the client web browser. If required, add the certificate manually to the web browser's trusted certificates list.
