Apache Web Server Setup Guide

Introduction

This guide explains how to set up an Apache web server on an Ubuntu Server for local network access. It also covers deploying multiple websites using Virtual Hosts and configuring essential settings.

Prerequisites

Ubuntu Server installed

Apache2 installed (sudo apt install apache2)

Static IP address assigned to your server

Step 1: Directory Structure

Create separate directories for each website inside /var/www/html/.

sudo mkdir /var/www/html/site1
sudo mkdir /var/www/html/site2

Add sample content to each site:

echo "<h1>Welcome to Site 1</h1>" | sudo tee /var/www/html/site1/index.html
echo "<h1>Welcome to Site 2</h1>" | sudo tee /var/www/html/site2/index.html

Step 2: Configure Virtual Hosts

Create separate configuration files for each site.

For Site 1

sudo nano /etc/apache2/sites-available/site1.conf

Add the following content:

<VirtualHost *:80>
    ServerAdmin admin@site1.local
    DocumentRoot /var/www/html/site1
    ServerName site1.local
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

For Site 2

sudo nano /etc/apache2/sites-available/site2.conf

Add the following content:

<VirtualHost *:80>
    ServerAdmin admin@site2.local
    DocumentRoot /var/www/html/site2
    ServerName site2.local
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Step 3: Enable the Virtual Hosts

Run these commands:

sudo a2ensite site1.conf
sudo a2ensite site2.conf
sudo systemctl reload apache2

Step 4: Update /etc/hosts (For Local Access)

Add these lines to your /etc/hosts file:

192.168.0.109 site1.local
192.168.0.109 site2.local

Replace 192.168.0.109 with your server's static IP.

Step 5: Firewall Configuration

Ensure Apache is allowed in UFW:

sudo ufw allow 'Apache'
sudo systemctl reload apache2

Step 6: Access the Websites

For Site 1: http://site1.local

For Site 2: http://site2.local

Troubleshooting Tips

✅ Ensure Apache service is running: sudo systemctl status apache2
✅ Verify Apache is listening on port 80: sudo netstat -tuln | grep ':80'
✅ Restart Apache after config changes: sudo systemctl restart apache2
