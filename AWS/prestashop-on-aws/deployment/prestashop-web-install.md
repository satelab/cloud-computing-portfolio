PART 3: INSTALL PRESTASHOP VIA WEB INTERFACE 

Step 16: Access PrestaShop Installer 
1. Open web browser 
2. Navigate to http://16.171.226.187
3. You should see PrestaShop installation wizard
 

Complete Installation Wizard 

Step 1: Choose Language
● Select your language 
● Click Next

License Agreements 
● Check "I agree to the above terms and conditions" 
● Click Next
 

System Compatibility 
● Verify all checks are green ✅
 ● If any are red, fix the issues first 
● Click Next

Step 4: Store Information Fill in: 
● Shop name: My PrestaShop Store 
● Main activity: Computer Hardware and Software 
● Country: Nigeria
● First name: michael 
● Last name: isonguyo 
● E-mail address: signofambition@gmail.com
● Shop password: Mike@1010.com 
● Re-type to confirm: Mike@1010.com 
● Click Next

 

Follow the prompts to complete the setup to select the content of your store.
Click on Next
 

System Configuration (DATABASE) 
⚠ CRITICAL STEP - Use your database server private IP 
Fill in: 
● Database server address: 172.31.33.30
○ DO NOT use localhost or 127.0.0.1 
● Database name: prestashop 
● Database login: michael.isonguyo 
● Database password: Mike@1010com 
● Tables prefix: ps_ (leave default) 
 
Click "Test your database connection now!"
You should see: ✅ "Database is connected" 
Click Next


Installation Progress 
● Wait for installation to complete (3-5 minutes) 
● Progress bars will show each step 
Installation in progress 
 

Installation Complete 
● You'll see success message 


● IMPORTANT: Note the random admin folder name shown 
Installation complete screen 

Security Configuration (REQUIRED) 

# Return to SSH terminal on application server 
# Remove installation directory 
sudo rm -rf /var/www/html/install 
 
# Rename admin directory 
sudo mv /var/www/html/admin /var/www/html/admin_secure 
 
# Verify
ls /var/www/html/
 

Open a web browser to access the backend and the store front end
#Front End Access
Type: http://16.171.266.187
 

#Backend Access
Type: http://16.171.266.187/admin_secure
 

An error has occurred. We need to resolve the issue.

Steps To Resolve the issues:

#Disable SSL Requirement

Step 1: Edit PrestaShop Configuration

# SSH into your application server
ssh -i prestashop-key.pem ubuntu@16.171.226.187 -p 22

# Edit the configuration file
sudo nano /var/www/html/app/config/parameters.php
 
Step 2: Find and Change SSL Settings
Look for these lines (around line 20-30):
'cookie_key' => 'xxxxxxxxxxxxx',
'cookie_iv' => 'xxxxxxxxxxxxx',
'ps_caching' => 'CacheMemcache',
'ps_cache_enable' => false,
'ps_creation_date' => 'xxxx-xx-xx',

Add this line (or change if it exists):
'PS_SSL_ENABLED' => false,
 
Also look for:
'PS_SSL_ENABLED_EVERYWHERE' => 1,

Change it to:
'PS_SSL_ENABLED_EVERYWHERE' => 0,
 
Step 3: Save and Exit
•	Press Ctrl + X
•	Press Y
•	Press Enter

Step 4: Clear Cache

# Clear PrestaShop cache
sudo rm -rf /var/www/html/var/cache/*

# Restart Apache
sudo systemctl restart apache2

Update Database Settings (Alternative Method)

If the above doesn't work, update directly in the database:

# Connect to database from application server
mysql -h 172.31.33.30 -u michael.isonguyo -p
 
Enter password: Mike@1010com
Then run these SQL commands:
USE prestashop;
UPDATE ps_configuration SET value = '0' WHERE name = 'PS_SSL_ENABLED';
UPDATE ps_configuration SET value = '0' WHERE name = 'PS_SSL_ENABLED_EVERYWHERE';
EXIT; 
Step 6: Try Accessing Again
Now try:
•	Frontend: http://<application-public-ip>
•	Admin: http://<application-public-ip>/admin_secure

Try Accessing Again
Now try:
•	Frontend: http://16.171.226.187
 

•	Admin: http://16.171.226.187/admin_secure
 

Frontend Registration Page:
 


Contact Page:
 

Successful Login to the store backend
 
 
And now you can navigate and perform administrative tasks from the backend.
