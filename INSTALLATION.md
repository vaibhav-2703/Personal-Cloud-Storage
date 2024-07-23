# Installation Instructions

## Prerequisites

- An old PC or server
- Ubuntu Server 24.04 LTS ISO
- Internet connection

## Steps

1. **Install Ubuntu Server**:
   - Boot from the Ubuntu Server ISO and follow the installation instructions (Make sure to set a static ip while installing Ubuntu Server).

2. **Update Packages**:
   ```bash
   sudo apt update
   sudo apt dist-upgrade
   sudo apt autoremove
3. **Set the Hostname**:
```bash
  sudo nano /etc/hostname
  sudo nano /etc/hosts
  sudo reboot
```
4. **Download Nextcloud**:
```bash
wget https://download.nextcloud.com/server/releases/latest.zip
```
5. **MariaDB Setup**:
  - Install MariaDB:
  ```bash
  sudo apt install mariadb-server
  ```
  - Secure MariaDB:
  ```bash
  sudo mysql_secure_installation
  ```
  - Create Nextcloud Database:
  ```bash
  sudo mariadb
CREATE DATABASE nextcloud;
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
FLUSH PRIVILEGES;
```
6. **Apache Webserver Setup**:

- Install Apache and PHP Packages:
```bash
sudo apt install libmagickcore-6.q16-6-extra php php-apcu php-bcmath php-cli php-common php-curl php-gd php-gmp php-imagick php-intl php-mbstring php-mysql php-zip php-xml
```
- Install and Unzip Nextcloud:
```bash
sudo apt install unzip
unzip latest.zip
mv nextcloud nc.learnlinux.tv
sudo chown -R www-data:www-data nc.learnlinux.tv
sudo mv nc.learnlinux.tv /var/www/
sudo a2dissite 000-default.conf
```
- Create Apache Config for Nextcloud:
```bash
sudo nano /etc/apache2/sites-available/nc.learnlinux.tv.conf
```
Add the following content:
```apache
<VirtualHost *:80>
    DocumentRoot "/var/www/nc.learnlinux.tv"
    ServerName nc.learnlinux.tv

    <Directory "/var/www/nc.learnlinux.tv/">
        Options MultiViews FollowSymlinks
        AllowOverride All
        Order allow,deny
        Allow from all
   </Directory>

   TransferLog /var/log/apache2/nc.learnlinux.tv_access.log
   ErrorLog /var/log/apache2/nc.learnlinux.tv_error.log
</VirtualHost>
```
- Enable Site and Modules:
```bash
sudo a2ensite nc.learnlinux.tv.conf
sudo a2enmod dir env headers mime rewrite ssl
sudo systemctl restart apache2
```
7. **Configure PHP**:
 - Edit php.ini:
```bash
sudo nano /etc/php/8.3/apache2/php.ini
```
- Ensure the following settings:
```ini
Copy code
memory_limit = 512M
upload_max_filesize = 200M
max_execution_time = 360
post_max_size = 200M
date.timezone = America/Detroit
opcache.enable=1
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=10000
opcache.memory_consumption=128
opcache.save_comments=1
opcache.revalidate_freq=1
```
8. **Setting Up Cloudflare Tunnel**:

 - Prerequisites

    - **Domain Name**: Purchase a domain from a registrar of your choice. Look for affordable options.
    - **Cloudflare Account**: Sign up for a free account at [Cloudflare](https://www.cloudflare.com/).

-  Steps to Set Up Cloudflare Tunnel

    - Purchase a Domain

   - Choose and purchase a domain from a domain registrar. 

-  Configure DNS with Cloudflare

  - Log in to your Cloudflare account.
  - **Add your site**: Click on "Add a Site" and follow the prompts to add your domain.
  - Cloudflare will provide you with nameserver (DNS) information. 
  - Log in to your domain registrar’s dashboard.
  - **Update DNS**: Replace your existing nameservers with those provided by Cloudflare. This process might take anywhere from 5 to 24 hours to propagate.

- Create a Cloudflare Tunnel

  - Once DNS changes have propagated (Cloudflare will notify you by email), go back to the Cloudflare dashboard.
  - Click on **"Zero Trust"** from the menu.
  - Navigate to **"Network"** -> **"Tunnels"** -> **"Create a Tunnel"**.
  - **Name your Tunnel**: You can choose any name that helps you identify it.
  - **Choose Environment**: Select **"Docker"** if using Docker or **"Debian"** for Ubuntu.

- Configure Public Hostname

  - Click on **"Public Hostname"** -> **"Add a Public Hostname"**.
  - Enter your chosen subdomain and domain. For example, `subdomain.yourdomain.com`.
  - **Type**: Select **"HTTP"**.
  - **URL**: Enter the static IP address and port of your Ubuntu server. For example, if your server’s IP is `192.168.1.100` and your web server is running on port `80`, enter `192.168.1.100:80`.

- Adjust TLS Settings

  - Under **"Advanced Application Settings"**, go to **"TLS Settings"**.
  - Disable **"TLS Verify"** to avoid issues with SSL/TLS certificates.

- Final Steps

  - **Review and Save**: Make sure all details are correct and save the configuration.
  - **Test Access**: Try accessing your cloud storage server through the domain you set up to ensure everything is working correctly.

9. **(Optional) Docker Setup**:

  - Install Docker:
  ```bash
  sudo apt install docker.io docker-compose
  ```
  - Nextcloud Docker Setup:
  ```bash
  curl -LO https://github.com/nextcloud/all-in-one/blob/main/compose.yaml
  docker-compose -f compose.yaml up -d
  ```
10. **(Optional) TrueNAS Setup**:

  - Follow the TrueNAS documentation for installation and configuration.
## Troubleshooting

For troubleshooting tips, refer to the [STATUS.md](https://github.com/vaibhav-2703/Personal-Cloud-Storage/blob/main/STATUS.md).
