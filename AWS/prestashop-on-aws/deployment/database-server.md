PART 1: CREATE DATABASE SERVER

Step 1: Launch Database EC2 Instance 
1.	Login to AWS Management Console 
2.	Go to EC2 Dashboard 
3.	On the search box, type EC2. Select it and click "Launch Instance"
   
Configure Instance: 
● Name and tags: PrestaShop-Database 
 
● Application and OS Images (AMI): Ubuntu Server 22.04 LTS (Free tier eligible) 
 
● Instance type: t2.micro 

 
● Key pair: Click "Create new key pair" 
○ Key pair name: prestashop-key 
○ Key pair type: RSA 
○ Private key file format: .pem 
○ Click "Create key pair" (file will download - save it safely!)
 

● Network settings: 
○ Click "Edit" 
○ Security group name: database-sg 
○ Description: Database security group  
○ Inbound security group rules: 
■ Rule 1: SSH, Port 22, Source: My IP 
■ Rule 2: MySQL/Aurora, Port 3306, Source: application-server


● Configure storage: 8 GB gp3 (default) 
 
4.Click "Launch instance" 
 
  o	Click on view all instances
 
5. Wait until Instance State = Running
 
6. Select your instance and note: 
○ Instance ID – i-0166ae076bd335fb
○ Public IPv4 address (for SSH access) – 13.60.65.51
○ Private IPv4 address (for database connection) – 172.31.33.30
 

Step 2: Connect to Database Server 
 

o	EC2 Instance Connect
 
o	Session Manager
 
o	SSH Client
 
On Windows:
o	Tab on your Windows key and type in cmd in the popup search box and hit the ENTER key. 
o	Alternatively, Press the Windows key + R together and type in cmd in run box and hit the ENTER key.
o	Type in the cmd window:
ssh -i prestashop-key.pem ubuntu@13.60.65.51 -p 22
 
 
On Mac/Linux: 
Locatethe terminal and run:
o	chmod 400 prestashop-key.pem 
o	ssh -i prestashop-key.pem ubuntu@13.60.65.51 -p 22
If you see "Are you sure you want to continue connecting?" type yes
Step 3: Install MySQL on Database Server 
# Update package list 
sudo apt update 
 
# Upgrade existing packages 
sudo apt upgrade -y 

# Install MySQL Server 
sudo apt install mysql-server -y 
 
# Verify MySQL is running 
sudo systemctl status mysql 
 

Step 4: Secure MySQL Installation 
sudo mysql_secure_installation 

Answer the prompts: 
● Validate Password Component? N (or Y if you want strong password enforcement) 
 
● Set root password? Y 
○ Enter password: Root@2026.com
○ Re-enter password: Root@2026.com 
● Remove anonymous users? Y 
● Disallow root login remotely? N (we need remote access) 
 
● Remove test database? Y
  
● Reload privilege tables? Y 
 

Step 5: Configure MySQL for Remote Access

# Edit MySQL configuration 
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf 
 
Find the line: bind-address = 127.0.0.1 
Change it to: bind-address = 0.0.0.0 
 
Save and exit: 

● Press Ctrl + X 
● Press Y 
● Press Enter 

# Restart MySQL 
sudo systemctl restart mysql 
 
# Verify it's running 
sudo systemctl status mysql
 
Press q to exit the status view.


Step 6: Create Database and User for PrestaShop 

# Login to MySQL 
sudo mysql -u root -p 
 
Enter password: Root@2026.com
Run these SQL commands: 
CREATE DATABASE prestashop; 
CREATE USER 'prestashop_user'@'%' IDENTIFIED BY 'PrestaShop@2024'; 
GRANT ALL PRIVILEGES ON prestashop.* TO 'michael isonguyo@'%'; 
FLUSH PRIVILEGES; 
SHOW DATABASES; 


Modified Username:
DROP USER IF EXISTS ‘michael isonguyo’@’localhost’;
DROP USER IF EXISTS ‘michael isonguyo’@’%’;
FLUSH PRIVILEGES;
CREATE USER 'michael.isonguyo’@'%' IDENTIFIED BY 'Mike@1010com’; 
GRANT ALL PRIVILEGES ON prestashop.* TO 'michael.isonguyo@'%' WITH GRANT OPTION; 
FLUSH PRIVILEGES;  
SHOW DATABSES;
EXIT;
 
DATABASE SERVER SETUP COMPLETE
