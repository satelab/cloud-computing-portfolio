PART 2: CREATE APPLICATION SERVER 

Step 7: Launch Application EC2 Instance 
1. Return to EC2 Dashboard 

2. Click "Launch Instance" Configure Instance: 
● Name and tags: PrestaShop-Application 
 
● Application and OS Images (AMI): Ubuntu Server 22.04 LTS 
 
● Instance type: t2.micro 
 
● Key pair: Select prestashop-key (same key as database) 
 

● Network settings: 
○ Click "Edit" 
 
○ Security group name: application-sg 
 
○ Description: Application security group 
 
○ Inbound security group rules: 
■ Rule 1: SSH, Port 22, Source: My IP 
■ Rule 2: HTTP, Port 80, Source: Anywhere (0.0.0.0/0) 
■ Rule 3: HTTPS, Port 443, Source: Anywhere (0.0.0.0/0)
 
Modified Security Group:
 
● Configure storage: 8 GB gp3 

 
3. Click "Launch instance" 
 
4. Wait until Instance State = Running
 
5. Note the Public IPv4 address - 16.171.266.187


Step 8: Update Security Groups Configure 

Database Security Group: 
1. Go to EC2 → Security Groups 
2. Select database-sg 
3. Click "Edit inbound rules" 
 

4. Delete the rule allowing MySQL from "Anywhere" 
5. Click "Add rule" 
○ Type: MySQL/Aurora 
○ Port Range: 3306 
○ Source: Custom → Select application-sg 
6. Click "Save rules"
 

Updated Inbound security rule:
 

NOTE: This ensures only my application server can access the database.


Step 9: Connect to Application Server 
On Windows:
o	Tab on your Windows key and type in cmd in the popup search box and hit the ENTER key. 
o	Alternatively, Press the Windows key + R together and type in cmd in run box and hit the ENTER key.
o	Type in the cmd window:
ssh -i prestashop-key.pem ubuntu@16.171.226.187 -p 22
 
 
On Mac/Linux: 
Locatethe terminal and run:
o	chmod 400 prestashop-key.pem 
o	ssh -i prestashop-key.pem ubuntu@13.60.65.51 -p 22
If you see "Are you sure you want to continue connecting?" type yes

Step 10: Install Apache Web Server 

# Update packages 
sudo apt update 
 
sudo apt upgrade -y 
 

# Install Apache 
sudo apt install apache2 -y 
 

# Enable and start Apache 
sudo systemctl enable apache2 
sudo systemctl start apache2
 

# Check status 
sudo systemctl status apache2 
 

Test Apache: 
Open browser and go to http://application -public-ip You should see the Apache default page.
 

Click Continue to continue.
 

Step 11: Install PHP and Required Extensions 
# Install PHP and all required extensions 
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd \
php-intl php-mbstring php-xml php-zip php-bcmath php-json php-soap unzip -y 

# Verify PHP installation 
php -v 
The PHP version output (should be 8.1 or higher) as shown below.
 

Step 12: Configure Apache for PrestaShop 

# Enable Apache rewrite module 
sudo a2enmod rewrite 
 
# Restart Apache 
sudo systemctl restart apache2	 

Step 13: Test Database Connection 
# Install MySQL client 
sudo apt install mysql-client -y 
 

# Test connection (replace with your database private IP) 
mysql -h 172.31.33.30 -u michael.isonguyo -p 
 
Enter password: Mike@1010com 

If successful, you'll see MySQL prompt. 
 

Run: SHOW DATABASES; EXIT;

Step 14: Download PrestaShop 

# Go to web directory 
cd /var/www/html 
 

# Download PrestaShop 
sudo wget https://github.com/PrestaShop/PrestaShop/releases/download/8.1.7/prestashop_8.1.7.zip 
 
# Unzip main file 
sudo unzip prestashop_8.1.7.zip 
 

# Unzip inner prestashop.zip 
sudo unzip prestashop.zip 
# When prompted: replace index.php? [y]es, [n]o, [A]ll, [N]one, [r]ename: A # Type A and press Enter
 

# Remove zip files 
sudo rm prestashop_8.1.7.zip prestashop.zip 
 
# Remove Apache default page 
sudo rm index.html
 

 # Set correct permissions 
sudo chown -R www-data:www-data /var/www/html 
 
sudo chmod -R 755 /var/www/html 
 

# Verify files 
ls -la	
 

Step 15: Configure Apache Virtual Host 

# Create virtual host configuration 
sudo nano /etc/apache2/sites-available/prestashop.conf 
 

Copy and paste this configuration:
<VirtualHost *:80>
        ServerAdmin admin@localhost 
        DocumentRoot /var/www/html

       <Directory /var/www/html>
       Options FollowSymLinks 
       AllowOverride All
       Require all granted 
</Directory> 
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined 
</VirtualHost>
 
Save: Ctrl + X, then Y, then Enter 

# Disable default site 
sudo a2dissite 000-default.conf 
 
# Enable PrestaShop site 
sudo a2ensite prestashop.conf 

# Test configuration 
sudo apache2ctl configtest 

# Restart Apache 
sudo systemctl restart apache2
 
