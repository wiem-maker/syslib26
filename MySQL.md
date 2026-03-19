This document records my process installing and configuring MYSQL on my Ubuntu VM hosted on Google Cloud.  
The goal was to install, configure, and run MYSQL.
 
## Environment


- VM Provider: Google Cloud
- OS: Ubuntu 22.04 LTS
- Web Server: Apache 2.4.52
- PHP Version: 8.1
- Browser Used for Testing: Firefox (Linux)


## Step 1: Update Package Lists


First, I updated the package list to ensure I was installing the latest available versions.


```
sudo apt update
sudo apt upgrade
sudo apt autoremove
sudo apt clean
```
I had 8 packages that needed to be updated and were  without an issue.


---
## Step 2: Install MYSQL
Then I installed the MySQL Community Server package.
```
sudo apt install mysql-server


```
Then I ran the following code to verify that it was the correct version
```
mysql —version
```
The version was 8.0.45.


Then I checked that the database server was working with the following code
```
systemctl status mysql
```
Then I ran a security script
```
Sudo mysql_secure_installation
```
Which prompted a number of responses where I needed to answer either y or n.


    Validate passwords: Y
    Password validation policy: 0 (zero) for LOW
    Remove anonymous users: Y
    Disallow root login remotely: Y
    Remove test database and access to it: Y
    Reload privilege tables now: Y


Then I logged into the database with 
```
Sudo mysql -u root
```
This initially failed until I realized it was due to a mistype and corrected it.
Some of the more important messages from this login would be
```
Welcome to the mySQL monitor. Commands end with ; or \g.
```
As well as 
```
Type ‘help;’ or ‘\h’ for help. Type ‘\c’ to clear the current input statement.
```
After this I access the databases with
```
mysql> show databases;
```
 This returned the information that the two databases present are information_schemamysql 
 and performance_schemasys
To go back to the regular bash shell the command in 
```
mysql> \q
```
---
## Step 3: Create and Set Up a Regular User Account
To start creating a regular user account I signed back into the MySQL server with
```
Sudo mysql -u root
```
Then proceeded to make a new user with 
```
create user ‘opacuser’@’localhost’ identified by  ‘******’;
```
This initial password of only numbers was refused by MySQL so a different password
 was chosen which was accepted. 
---
## Step 4: Create a Practice Database
We begin this step by creating a database for the new user
```
Create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;
```
At this point I had to close my VM and needed to re-log into my new user account opacuser with 
```
Mysql -u upacuser -p
```
At which point I realized I had forgotten my hastily made account andneeded to retrieve
 the password from a different computer. From here I faced a number of problems, the first 
 of which was that mysql wouldn’t allow me to log into the newly created account with the 
 password, as in I couldn’t type anything in at all and the only key working was the ‘enter’
  key. This was such an issue that the entire client was actually removed and reinstalled in
   the hopes of fixing the issue. At this point, all of the above commands relating to Mysql
    were run again with the exception of creating a new account which was instead given the 
    name of ‘opener’@’localhost’ due to some issues with the first name. 
Moving On. 
From here the following command was run
```
show databases;
```
Which returned the databases of information_schemamysql and performance_schemasys.
Then all prililedges were granted to the new account with 
```
grant all privileges on opacdb.* to 'opener'@'localhost';
```
Then I logged out with
```
\q
```
---
## Step 5: Logging in as a Regular User and Creating Tables
First I open my bashrc file with edit as that is the editor I have
```
edit ~\.bashrc
```
And add “export MYSQL_PS1="[\d]> " to the bottom of the file.
Then I save and leave the file. 
From here I log into mysql as the account I just made, which in my case use the command of
```
Musql -u opener -p
```
And then the password. A trippy part is that it doesn’t show any indication that the password
 is being typed. I don’t like that but can’t do anything to fix it. 
From here I check the databases again with 
```
show databases;
``` 
 And then the additional command of 
```
Use opacdb;
```
From here I insert the following code
```
create table books (
        id int unsigned not null auto_increment,
        author varchar(150) not null,
        title varchar(150) not null,
        copyright year not null,
        primary key (id)
);
```
From here I use the command of 
```
Show tables;
```
To display what tables are in opacdb and then
```
Describe books;
```
To open the table and view it.
From here I inserted information into the table with 
```
insert into books (author, title, copyright) values
('Jennifer Egan', 'The Candy House', '2022'),
('Imbolo Mbue', 'How Beautiful We Were', '2021'),
('Lydia Millet', 'A Children\'s Bible', '2020'),
('Julia Phillips', 'Disappearing Earth', '2019');
```
And the viewed the new information in the table with 
```
Select * from books;
```

---

## Step 6: Testing Commands
From here I tested commands one at a time to see their effects
```mysql> select author from books;
mysql> select copyright from books;
mysql> select author, title from books;
mysql> select author from books where author like '%millet%';
mysql> select title from books where author like '%mbue%';
mysql> select author, title from books where title not like '%e';
mysql> select * from books;
mysql> alter table books add publisher varchar(75) after title;
mysql> describe books;
mysql> update books set publisher='Simon & Schuster' where id='1';
mysql> update books set publisher='Penguin Random House' where id='2';
mysql> update books set publisher='W. W. Norton & Company' where id='3';
mysql> update books set publisher='Knopf' where id='4';
mysql> select * from books;
mysql> delete from books where author='Julia Phillips';
mysql> insert into books
       (author, title, publisher, copyright) values
       ('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
       ('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');
mysql> select * from books;
mysql> select author, publisher from books where copyright < '2011';
mysql> select author from books order by copyright;
mysql> \q
```
This ultimately completely replaced the data initially inteted into the table into different
 datasets. AKA it switched the first set of books for different books.

---

## Step 7: Install PHP and MySql Support
The entirety of this step is simply installing php-musql and then restarting apache2 and mysql with
```
Sudo apt install php-mysql
Sudo stemctl restart apache2
Sudo systemctl restart mysql
```

## Step 8: Create PHP Scripts
First I change directory and create a new file with the touch command
```
Vd /var/www
sudo tough login.php
```
From here I used chmod to change the file’s permissions and then chown to file’s ownership.
```
sudo chmod 640 login.php
sudo chown :www-data login.php
```
I then use the following command to check the file owner and group owner
```
Ls -l login.php
```
That -l is the letter L rather than a 1 for reference of future me. 
Then I used edit to add the following credentials
```
Sudo edit login.php
<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opener";
$db_password = "XXXXXXXXX";
?>
```
The XXX’s are going to stay, I’m not sharing my password. 

From here I change directories and then edit our opac.php file
```
Cd /var/www/html
Sudo edit opac.php
```
Then I add the following PHP
```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MySQL Server Example</title>
</head>
<body>

    <h1>A Basic OPAC</h1>
    <p>We can retrieve all the data from our database and book table using a couple of
     different queries.</p>

    <?php
    // Load MySQL credentials securely
    require_once '/var/www/login.php';

    // Enable detailed MySQL error reporting
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

    // Establish database connection
    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);

    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    echo "<h2>Query 1: Retrieving Publisher and Author Data</h2>";

    // Query using prepared statement
    $stmt = $conn->prepare("SELECT publisher, author FROM books");
    $stmt->execute();
    $result = $stmt->get_result();

    while ($row = $result->fetch_assoc()) {
        echo "<p>Publisher " . htmlspecialchars($row["publisher"]) .
             " published a book by " . htmlspecialchars($row["author"]) . ".</p>";
    }

    $stmt->close();

    echo "<h2>Query 2: Retrieving Author, Title, and Date Published Data</h2>";

    $stmt2 = $conn->prepare("SELECT author, title, copyright FROM books");
    $stmt2->execute();
    $result2 = $stmt2->get_result();

    while ($row = $result2->fetch_assoc()) {
        echo "<p>A book by " . htmlspecialchars($row["author"]) .
             " titled <em>" . htmlspecialchars($row["title"]) .
             "</em> was released in " . htmlspecialchars($row["copyright"]) . ".</p>";
    }

    $stmt2->close();
    $conn->close();
    ?>

</body>
</html>
```
From here I checked that everything was alright with 
```
Sudo php -f /var/www/login.php
Sudo pgp -f /var/www/html/opac.php
```
Then I went to my External IP address site at
```
MySQL Server Example
```
And got to see what I had created appear.

###Reflection
This was probably one of the hardest lessons we have had so far. While several fo the
 other ones were difficult for various reasons, this was the first one where my usual curse
  with technology really seemed to appear. I spent a solid hour of deleting and reinstalling
   MySQL as I attempted to create a user account. I never found out what the issue actually was,
    only that it finally let me create one after many many tries. Seeing the end result for these
     lessons are usually pretty satisfying and reaffirm that taking this class was a good idea.
      This one just made me glad it was over. 


