## Creating a Bare Bones Cataloging Module

—
## Step 1: Creating the HTML Page and a PHP Cataloging Page
Before anything else I change my directory
```
cd /var/www/html
```
Then I create a directory within a directory
```
Sudo mkdir cataloging
```
I checked that it was made with
```
Ls
```
And then changed directory to cataloging
```
Cd cataloging
```
Then created a new file named index.html and added some html coding
```
Sudo edit index.html
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>

    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>

    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>

        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>

        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>

        <label for="copyright">Copyright:</label>
        <input type="date" name="copyright" id="copyright" required>

        <input type="submit" value="Submit">
    </form>
</body>
</html>
```
Then I exit the file with
```
ctrl+q
```
—
## Step 2: PHP Insert Script
Then I create a new file called insert.php and added some php coding
```
Sudo edit insert.php
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cataloging: Data Entry</title>
</head>
<body>

<h1>Cataloging: Data Entry</h1>

<?php

// Load MySQL credentials
require_once '/var/www/login.php';

// Enable MySQL error reporting
mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

// Establish connection
$conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $author = trim($_POST["author"] ?? "");
    $title = trim($_POST["title"] ?? "");
    $publisher = trim($_POST["publisher"] ?? "");
    $copyright = $_POST["copyright"] ?? "";

    if ($author === "" || $title === "" || $publisher === "" || $copyright === "") {
        echo "All fields are required.";
    } elseif (!preg_match('/^\d{4}-\d{2}-\d{2}$/', $copyright)) {
        echo "Copyright date must use YYYY-MM-DD format.";
    } else {
        // Prepare and bind SQL statement
        $stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
        $stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

        if ($stmt->execute() === TRUE) {
            echo "New record created successfully";
        } else {
            echo "Error: " . $stmt->error;
        }
        $stmt->close();
    }
} else {
    echo "Please submit records using the cataloging form.";
}

// Close connection
$conn->close();
?>

<p><a href='index.html'>Return to Cataloging Page</a></p>
<p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
</body>
</html>
```
And then exited the file with 
```
Ctrl+q
```
—
## Step 3:Security
Then move over to another directory to start making a password for those allowed to enter data.
```
Cd /etc/apache2
```
Then set up the password, which I’m not going to publish here
```
Sudo htpasswd -c ‘etc’apache2’.htpasswd libcat
```
Then I entered the password a few times as prompted, this application also doesn’t show actually typing in the password but it’s happening.
If I were to add users later I would leave out the -c.
Then I open the apache2.config file
```
Sudo edit /etc/apache2/apache2.conf
```
Then scrolled through the file until line 170 and change a few things until it looks like
```
<Directory /var/www/html/cataloging/>
  Options Indexes FollowSymLinks
  AllowOverride AuthConfig
  Require all granted
</Directory>
```
Changed directory again and added some things
```
Cd /var/www/html/cataloging
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```
Closed out of the file with 
```
Ctrl+q
```
Ran a check to make sure everything was okay
```
Sudo apachectl configtest
```
Receives a result of Syntex Ok and then restarted apache2 and checked that it was running okay
```
Sudo systemctl restart apache2
Sudo systemctl status apache2
```
Everything was running well.


—
## Step 4: Permissions and Ownership
Then I change the group ownership of /var/www/html
```
Sudo chown :www-data /var/www/html
```
And then make sure that anything created in this directory will be under the same group ownership
```
Sudo find /ver/www/html -type d -exec chmod g+s { } +
```

—
## Step 5: Get Cataloging
From here I went to the webpage 
```
http://34.46.34.80/cataloging/index.html

and got an access denied notice, at which point I began my search for my error. It turned out that while I was messing with the apache2.conf file, that what I was trying to put in was supposed to go below the existing /var/www/ block of configuration. When I fixed that, rebooted apache2, and then reloaded the website everything was working fine. 
—
## Reflection
While this was very interesting and relatively painfree, for my database it is ultimately useless. I mentioned it in the section before this one that my books3 table doesn’t accept YYYY-MM-DD format information, instead only using YYYY format for some reason. I’m likely missing something in some of the coding for html or php but it’s interesting to see and execute the database module anyway. 
