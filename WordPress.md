
## Installing Wordpress

## Step 1: Requirements
In order to install Wordpress I need to check that my machine has PHP version 8.3 or higher and MySQL version 8.0 or higher. It also needs https support. To do this I run the following commands for each requirement.
```
Php --version
mysql --version
```
Which returned the respective results that my machine is running PHP 8.3.6 and MySQL 8.0.45.
From here I install various PHP modules to help WordPress run smoothly.
```
sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl
```
Then restarted apache2 and mysql.
```
Sudo systemctl restart apache2
Sudo systemctl restart mysql
```
—---
## Step 2: Download and Extract
Moving on from prepping the system for the download, it time for the actual WordPress download. Which is begun by moving to the /var/www/html directory.
```
cd  var/www/html
```
And then downloading the latest zip file from wordpress with wget.
```
Sudo wget https://wordpress.org/latest.zip
```
Then actually unzipping the zip file which needs me to download the unzip command.
```
sudo apt install unzip
```
And then continuing to actually unzip the latest.zip file.
```
Sudo unzip latest.zip
```
Then to save some space in my VM I delete the latest.ziip file after it has been unzipped.
—---
## Step 3: Create the Database and a User
Then I create a new database and user in MySQL
```
sudo mysql -u root
create user ‘wordpress’@’localhost’ identified by ‘********’;
```
Then I created a new database and granted the newly created account privileges to it.
```
Create database wordpress;
Grant all privileges on wordpress.* to ‘wordpress’@’localhost’;
```
Then I finished it off by checking that the database was actually created and exiting mysql.
```
Show databases;
\q
```
—---
## Step 4: Set up wp-config.php
Changing directory again I go to 
```
cd /var/www/html/wordpress
```
Where I create a copy of and rename a file
```
Sudo cp wp-config-sample.php wp-config.php
```
Then I enter the newly created file and change some things
```
Sudo edit wp-config.php
```
In the file I change some of the database settings, changing the DB_User to ‘wordpress’ the username to ‘wordpress’ and putting in my password for the DB_Password.
—---
## Step 5: Run the Install Script
Opening the site in a web browser let me set up the site with WordPress the site and run the installation script.

—---
## Reflection
Overall, this was one of our easier lessons. It was much easier and I had fewer issues than usual as I went through everything. All of the issue I experienced were so minor that I didn’t document them as they resulted from my own mistyping of commands rather than the system having it out for me. 
