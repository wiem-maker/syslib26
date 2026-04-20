##Installing Omeka
---
## Step 1: Prerequisites
Before anything else can be done, I need to update my system to its most recent version
```
Sudo apt update
Sudo apt upgrade
Sudo apt autoremove
Sudo apt clean
```
From here I check that the versions of MySQL and PHP are ones that meet Omekas requirements by both viewing Omeka’s requirements and checking to make sure what versions of MySQL and PHP are on my machine.
```
Php –version
Mysql –version
```
From here I make sure that I have both “mysqi” and “exif” PHP extensions are installed and working. I used the following command to check this by reading the results of the command.
```
Php –ini
```
By reading the last section of the results I discovered that mysqli and exif are both installed on my machine. 
---
## Step 2: Installing Prerequisites
From here I needed to begin installing software such as imagemagick.
```
Sudo apt install imaggemagick
```
Here I discovered that I was running an outdate kernel and needed to reboot my machine.
```
Sudo systemctl –no-wall reboot
```
Then I needed to enable an Apache module
```
sudo a2enmod rewrite
```
When I do this I recieve a message that I need to restart apache2 and it provides the command to do so.
```
Systemctl restart apache2
```
This in itself returned a message asking for an Ubuntu password. Seeing as I don’t remember ever making an Ubuntu password, I’m going to instead rerun the command as a sudo. 
```
Sudo systemctl restart apache2
```
To check that this worked I’m going to run the following command as well
```
Systemctl status apache2
```
This showed the result of active (running) and meant that the sudo command had worked. 
---
## Step 3: Setting up MySQL for a New Account
Before moving on I need to create a new database and MySQL account
```
Create user ‘reader’@’localhost’ identified by ‘password’;
Grant all privileges on Omeka1.* to ‘reader’@’localhost’;
Create databaske Omeka1;
\q;
```
---
## Step 4: Downloading Omeka
To download the Omeka zip file I first need to change directories
```
Cd /var/www/html
```
Then I need to actually download and unzip the zip file.
```
Sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip
Sudo unzip omekka-3.2.zip
```
---
## Step 5: File Work
From here I rename my new file from omeka-3.2 to simply omeka and move it to a different directory.
```
Mv omeka-3.2 omeka
Sudo mv omeka .var.www.html
Cd /var/www/html/omeka
```
Once I change my directory to /var/www/html/omeka I make a copy of the db.ini file and rename one as the backup so that I can edit one of them without worry. I also go ahead and fill some database credentials into db.ini.
```
Sudo cp db.ini db.ini.backup
Sudo edit db.ini
```
From here I need to change directory and edit a file.
```
Cd /etc/apache2
Sudo edit apache2.conf
<Directory /var/www/html/omeka>
	Options Indexes FollowSymLinks
	AllowOverride ALL
	Require all granted
</Directory>
```
From here I reset the apache2 server so that the changes I just made would go into effect and checked once I had that everything was back up with the apache2 server.
```
Sudo systemctl restart apache2
Systemctl status apache2
```
---
## Step 6: File Ownership
Now that everything finally has all of the information it needs it is time to change file ownership some.
```
Sudo chown -R :www-data /var/www/html/omeka
Sudo chmod -R 774 /var/www/html/omeka
```
---
## Reflection
This was an interesting assignment. I didn’t run into any major stumbling blocks as I installed Omeka other than an issue with finding the right download. I was in the process of figuring it out myself through brute force when I realized that if I was having so many issues then other people might have as well and went to look at our classes announcements where I found your instructions. I am confident that I would have been able to figure it out myself eventually. Other than that minor issue that was quickly resolved, everything else went well with this lesson and I enjoyed creating a quick Dublin Core entry in Omeka for a recently read book and seeing it appear. 
