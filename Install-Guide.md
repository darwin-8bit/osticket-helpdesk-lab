# osTicket on Debian Server — Install Guide

## Prerequisites 
### **Hardware** 
  Server: HP EliteDesk 800 G4, 8GB RAM, 128GB SSD, i5 Core
  
### **Software**
  - **Proxmox VE:** Hosts and manages the Debian LXC container.
  - **Debian 12:** Provides the Linux operating environment.
  - **Apache:** Serves the osTicket web application.
  - **PHP:** Executes application code.
  - **MariaDB:** Stores ticket and application data.
  - **osTicket:** Provides the help desk interface for managing support requests.
### **LXC Container**
  New VM → 2 vCPU, 4 GB RAM, 128 GB disk 

----

## 1. Verify the Debian Environment
```bash
cat /etc/os-release
ip -br address
ip route
df -h /
```
These commands verify the operating system, network configuration, routing, and available disk space.

Update the system and install basic utilities:
```bash
apt update
apt full-upgrade -y
apt install -y sudo curl wget unzip ca-certificates
```
## 2.Install Apache, PHP, and MariaDB
```bash
apt install -y \
  apache2 mariadb-server \
  libapache2-mod-php php-cli php-common \
  php-mysql php-gd php-imap php-intl \
  php-mbstring php-xml php-zip php-curl \
  php-apcu php-ldap
```
Enable Apache and MariaDB
```bash
systemctl enable --now apache2
systemctl enable --now mariadb
```
Verify the installation:
```bash
php -v
php -m
systemctl status apache2 --no-pager
systemctl status mariadb --no-pager
```
## 3. Create osTicket Database

Open the MariaDB console:
```bash
mariadb
```
Create the database and dedicated application user:
```sql
CREATE DATABASE osticket
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

CREATE USER 'osticketuser'@'localhost'
  IDENTIFIED BY 'REPLACE_WITH_A_STRONG_PASSWORD';

GRANT ALL PRIVILEGES ON osticket.*
  TO 'osticketuser'@'localhost';

EXIT;
```
## 4. Download and Unzip osTicket
> Check https://osticket.com/download for the latest release number, then:
```bash
cd /tmp
wget https://github.com/osTicket/osTicket/releases/download/v1.17.1/osTicket-v1.17.1.zip
unzip osTicket-v1.17.1.zip -d osticket
sudo cp -r osticket/upload/* /var/www/html/
sudo rm /var/www/html/index.html               # remove Apache's default page
sudo cp /var/www/html/include/ost-sampleconfig.php /var/www/html/include/ost-config.php
sudo chown -R www-data:www-data /var/www/html/
sudo systemctl restart apache2
```

## 5. Configure Apache
Create a dedicated Apache virtual host:
```bash

nano /etc/apache2/sites-available/osticket.conf
```
Add the following configuration:
```apache

<VirtualHost *:80>
    ServerName osticket.local
    DocumentRoot /var/www/osticket

    <Directory /var/www/osticket>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/osticket-error.log
    CustomLog ${APACHE_LOG_DIR}/osticket-access.log combined
</VirtualHost>
```
Enable the required Apache module and site:
```bash

a2enmod rewrite
a2ensite osticket.conf
a2dissite 000-default.conf
apache2ctl configtest
systemctl reload apache2
```
The configuration test should return:
```text

Syntax OK
```
## 6. Configure Application Permissions

Set the initial ownership and permissions:
```bash

chown -R root:www-data /var/www/osticket

find /var/www/osticket -type d -exec chmod 755 {} \;
find /var/www/osticket -type f -exec chmod 644 {} \;
```

Locate the osTicket configuration template:
```bash

ls -l /var/www/osticket/include/ost-sampleconfig.php
```

If the template exists, create the configuration file:

```bash

cp /var/www/osticket/include/ost-sampleconfig.php \
  /var/www/osticket/include/ost-config.php
```

The installer may require temporary write access to the configuration file. If so, grant only the necessary access for installation and restore restrictive permissions afterward.

Do not make the entire application directory writable by the Apache service account.

## 7. Complete the Web Installer
Retrieve the container's IP address:
```bash

hostname -I
```

From a computer on the same network, open:
```text

http://YOUR-LXC-IP/
```

Complete the installation wizard using the following database settings:

Setting	Value
**Database Host**	localhost
**Database Name**	osticket
**Database Username**	osticketuser
**Database Password**	The password created in Step 3

Configure the help desk name and administrator account.

After installation **VERY IMPORTANT**

    Remove or disable the setup directory as directed by the installer.
    Restrict write access to include/ost-config.php.
    Verify that the administrator can log in.
    Create a test ticket and verify that it can be assigned and resolved.

## 8. Validate the Deployment
Step 8: Validate the Deployment

Run the following commands to verify the main services:
```bash

systemctl is-active apache2
systemctl is-active mariadb
php -v
apache2ctl configtest
```
Check the Apache error log if the application fails to load:
```bash

tail -n 50 /var/log/apache2/osticket-error.log
```
Test the database credentials:
```bash

mariadb -u osticketuser -p osticket
```

Enter the database password when prompted. Type exit to leave the database console.
