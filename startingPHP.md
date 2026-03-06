*Installing and Configuring PHP

**
Module 4 Notes 
We installed libapache2-mod-php with the command of 
“sudo apt install php libapache2-mod-php" 
Then we restarted apache2 with “sudo systemctl restart apache2”  
 ***
Then checked that apache2 had restarted and was working with 
“systemctl status apache” 
 ***
Then to check that everything went well we made a file in
 /var/www/html/ with the command of “sudo edit info.php” and then wrote  
“<?php 
Phpinfo(); 
?>” into the file before saving and checking out our browser which
 displayed a website for php 
 
Then we got rid of the file with “sudo rm /var/www/html/info.php” 
 **
 
We changed directory to /etc/apache2/mods-available/ and then copied 
our dir.conf file with “sudo cp dir.conf dir.conf.bak” before editing the dir.conf file with “sudo edit dir.conf” 
From there we changed the order of things in the file so that index.php 
was first in the order instead of index.html 
***
We checked the new config with “apachectl configtest” which returned the
 message of “Syntax Ok” 
 ***
After that we reloaded apache2 with “sudo systemctl reload apache2” and 
checked it with “systemctl status apache 2” 
 **
 
We changed directory again which is more than we usually do for this 
class and moved to ?ver/www/html/ and ran the command of
 “sudo edit index.php” where I then copied and pasted the following 
 code because I’m not interested in coding and knew I would mistype 
 '''
“<!DOCTYPE html> 
<html lang="en"> 
<head> 
	<meta charset="UTF-8"> 
	<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
	<title>Browser Detector</title> 
</head> 
<body> 
	<h1>Browser & OS Detection</h1> 
	<p>You are using the following browser to view this site:</p> 
  
	<?php 
	$user_agent = $_SERVER['HTTP_USER_AGENT']; 
  
	// Browser Detection 
	if (stripos($user_agent, 'Edg') !== false || stripos($user_agent, 'Edge') !== false) { 
    	$browser = 'Microsoft Edge'; 
	} elseif (stripos($user_agent, 'Firefox') !== false) { 
    	$browser = 'Mozilla Firefox'; 
	} elseif (stripos($user_agent, 'Chrome') !== false && stripos($user_agent, 'Chromium') === false) { 
    	$browser = 'Google Chrome'; 
	} elseif (stripos($user_agent, 'Chromium') !== false) { 
    	$browser = 'Chromium'; 
	} elseif (stripos($user_agent, 'Opera Mini') !== false) { 
    	$browser = 'Opera Mini'; 
	} elseif (stripos($user_agent, 'Opera') !== false || stripos($user_agent, 'OPR') !== false) { 
    	$browser = 'Opera'; 
	} elseif (stripos($user_agent, 'Safari') !== false && stripos($user_agent, 'Chrome') === false) { 
    	$browser = 'Safari'; 
	} else { 
    	$browser = 'Unknown Browser'; 
	} 
  
	// OS Detection 
	if (stripos($user_agent, 'Windows') !== false) { 
    	$os = 'Windows'; 
	} elseif (stripos($user_agent, 'iOS') !== false || stripos($user_agent, 'iPhone') !== false || stripos($user_agent, 'iPad') !== false) { 
    	$os = 'iOS'; 
	} elseif (stripos($user_agent, 'Android') !== false) { 
    	$os = 'Android'; 
	} elseif (stripos($user_agent, 'Mac') !== false || stripos($user_agent, 'Macintosh') !== false) { 
    	$os = 'Mac'; 
	} elseif (stripos($user_agent, 'Linux') !== false) { 
    	$os = 'Linux'; 
	} else { 
    	$os = 'Unknown OS'; 
	} 
  
	// Output Result 
	echo "<p>Your browser is <strong>$browser</strong> and your operating system is <strong>$os</strong>.</p>"; 
	?> 
  
</body> 
</html>” 
'''
From there we checked our website again and it said that I was using Microsoft Edge ad my browser with my operating system being Windows. Which was very cool and could be useful in a couple different situations. 

**Reflection
I'm still a bit confused about PHP but I'm willing to go along
with it for now until I understand more. It was interesting to install
what I understand is essentially a mod or apache2 if I'm understanding correctly
The only issue I ran into was when I launched the website after putting 
in the coding for a browser detector but it was only because I hadn't reloaded
the page so it could catch up. I've made that mistake a couple of times already
and am not enjoying the experience of doing so. I also hadn't realized we had
so many different directories on our virtual machines until this lesson.
Overall, I didn't have any real issues just a minor problem that I blame myself
for forgetting how to dix rather than anything important.

