## Koha Installation 

--- 

## Step 1: Creating a new Virtual Machine 

Before anything can be done with Koha, I need to create a new VM due to creating one with too-small storage. To fix this I need make a new VM that has a larger storage so that it can run Koha. I went with default region and zone, E2 (Low cost, day-to-day computing), e2-medium (2 vCPU, 1 core, 4 GB memory), Ubuntu 24.04 LTS, 20GB Hard Disk, and allow HTTP traffic. From here I need to wait for the machine to actually finish setting up. Once the new VM is create I go ahead and update it with the following commands. 

``` 

Sudo apt update 

Sudo apt upgrade 

Sudo apt autoremove 

Sudo apt clean 

exit 

``` 

From here I move onto working on the Google Cloud firewall. 

--- 

## Step 2: Google Cloud Firewall 

To access the area where the firewall creation is I simply navigate tot he page where I would usuall open my VM and instead scroll down until I see the icon labeled ‘Set Up Firewall Rules” with a brick wall icon and click on it. Then I click the button at the top of the page that is labeled “create firewall rule” the rule part of that is important so make sure it is the correct one. This opens a new page where I can begin to create the new rules.  

``` 

Name: koha-staff-8080 

Description: Open port 8080 for the Koha staff interface 

Target tags: koha-staff-8080 

Source filter: IPv4 

Source IPv4 Ranges: 0.0.0.0/0 

TCP: click the box to activate it 

Ports (immediately under TCP): 8080 

Create 

``` 

From here I then make another rule 

``` 

Name: koha-opac-8081 

Description: Open port 8081 for the Koha opac interface 

Target tags: koha-opac-8081 

Source filter: IPv4 

Source IPv4 Ranges: 0.0.0.0/0 

TCP: click the box to activate it 

Ports (immediately under TCP): 8081 

Create 

``` 

Then I move onto tmux 

--- 

## Step 3: TMUX 

Tmux can be used to reconnect to where we were is for some reason the session is disconnected. I wish I had known about it earlier as my internet has been a nightmare lately. I check that it is installed by running it name as a command. 

``` 

Tmux 

``` 

This opened what seems to be a new area, I’m going to trust it for now. 

If something did happen and I needed to reconnect I would use 

``` 

Tmux attach 

``` 

--- 

## Step 4: Installing Koha’s Repostiory 

To begin I use a series of signing keys to make sure that I’m installing the real koha and not some kind of malware. 

``` 

 sudo apt install apt-transport-https ca-certificates curl 

sudo mkdir -p --mode=0755 /etc/apt/keyrings 

sudo curl -fsSL https://debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc 

``` 

Then I turn myself into the root user with  

``` 

Sudo su 

``` 

Which is much easier than MySQL’s root user commands. 

Then I continue and paste the following command into the command line. 

``` 

tee /etc/apt/sources.list.d/koha.sources <<EOF 

Types: deb 

URIs: https://debian.koha-community.org/koha/ 

Suites: 25.05 

Components: main 

Signed-By: /etc/apt/keyrings/koha.asc 

EOF 

``` 

Then check it with 

``` 

cat /etc/apt/sources.list.d/koha.sources 

``` 

To make sure that everything is correct. 

Then I exit the root account with 

``` 

Exit 

``` 

--- 

## Step 5: Installing MariaDB 

Koha can work with both MariaDB and MySQL but it defaults to MariaDB so that’s the one that I’ll install. 

``` 

Sudo apt update 

Sudo apt install mariadb-server 

``` 

--- 

## Step 6: Installing Koha 

``` 

Sudo apt update 

``` 

Then check that the file I’m about to install is the real Koha again with 

``` 

Apt show koha-common 

``` 

Then I run the command that is the reason I am doing this in a tmux session. Koha is apparently huge and likely to crash my VM. It’s been working on installing the software for a few minutes already so I don’t doubt it.  

``` 

Sudo apt install koha-common 

``` 

--- 

## Step 7: Opening Ports 

Then I need to edit koha-sites.conf but since it’s such an important file I’m going to make a copy first. 

``` 

sudo cp /etc/koha/koha-sites.conf /etc/koha/koha-sites.conf.backup 

``` 

Then I acutually edit it by opening it in Edit and adding “8081” after OPACPORT= 

``` 

edit /etc/koha/koha-sites.conf 

``` 

Once I tried to execute this command my VM informed me that I have not installed editor so I need to do that first. The command for doing so is as follows 

``` 

wget https://github.com/microsoft/edit/releases/download/v1.2.1/edit-1.2.0-x86_64-linux-gnu.tar.zst 

tar -xf  edit-1.2.0-x86_64-linux-gnu.tar.zst 

sudo install -m 0755 edit /usr/local/bin/edit 

``` 

With that done I can run the previous command again and actually edit the config file.  

``` 

edit /etc/koha/koha-sites.conf 

``` 

I ran into a problem as soon as I tried to save my changes with the notification that I didn’t have the authority. Luckily, I know how to circumvent this problem. I just add sudo to the start of the command. 

``` 

Sudo edit /etc/koha/koha-sites.conf 

``` 

The changes I made were simply adding a few numbers until the two lines being changed look like the following 

``` 

INTRAPORT="8080" 

OPACPORT="8081" 

``` 

The sections that need to be changed can be easily identified by their titles of intraport and opacport. The only parts being changed are the numbers. Then save and exit Edit. 

``` 

Ctrl+q 

``` 

--- 

## Step 7: Setting up Apache 

From here I need to enable a few Apache modules with 

``` 

sudo a2enmod rewrite cgi headers proxy_http 

sudo systemctl restart apache2 

``` 

I then check that everything in apche2 restarted well with 

``` 

Sudo systemmctl status apache2 

``` 

Everything is back up and running so now I can move on.  From here I need to make another copy of an important file before I mess with it. 

``` 

sudo cp /etc/apache2/ports.conf /etc/apache2/ports.conf.backup 

``` 

Then I edit the file and exit my Edit program 

``` 

Sudo edit /etc/apache2/ports.conf 

Ctrl+q 

``` 

The parts I changed withing the file was to add  

``` 

Listen 8080 

Listen 8081 

``` 

Under Listen 80 and then save and exit. 

--- 

## Step 8: Creating a Koha Instance AKA Starting Koha Up 

Koha needs to actually be activated which I do with the following command. 

``` 

sudo koha-create --create-db bibliolib 

``` 

Then I need to restart apache and check that is is running again after the restart. 

``` 

sudo systemctl restart apache2 

Sudo systemctl status apache2 

``` 

--- 

## Step 9: More Apache2 Configurations 

``` 

sudo a2dissite 000-default 

sudo a2enmod deflate 

sudo a2ensite bibliolib 

``` 

Then reboot and reboot apache2 before checking that its running 

``` 

Sudo system reload apache2 

sudo systemctl restart apache2 

Sudo systemctl status apache2 

``` 

--- 

## Step 10: Koha Website Set-up 

Koha apparently sets up its own username and password so I need to use the following command to retrieve that information. 

``` 

Sudo koha-passwd bibliolib 

``` 

This provides me with the username and password which I will not be sharing here.  

From here I go to the actual website where I will finish setting Koha up.  

http://136.116.34.103:8080 

The website isn’t loading so I’m going to go through and make sure I did everything correctly. 

I have discovered that network tags are a thing that need to be associated with a VM in order for this project to work. You can access the area needed to do so by either setting it up while creating a VM or by adding the tags to an existing VM. Since I have a VM I did it that way. It involved clicking on the VM name in Google Cloud, clicking edit at the top of the new page, scrolling down until I saw “Network Tags” and then adding my two tags into it, seperating them with ,’s. The two tags are koha-staff-8080, and koha-staff-8080.  

http://136.116.34.103:8081
