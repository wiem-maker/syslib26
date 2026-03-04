Update Commands 

Sudo apt update 

Sudo apt upgrade 

Sudo apt autoremove 

Sudo apt clean 

 

Apt search updatefilename 

/\ shows what kinds of updated are available 

 

Apt show updatefilename 

/\ shows more information about the updates 

 

Sudo apt install updatefilename 

/\ exactly what it says 

 

REMOVING PROGRAMS 

Sudo apt --purge remove updatefilename 

Purge doesn’t get rid of the configurations that were downloaded so another command needs to be run 

Sudo apt autoremove 

Removes the extre things that were downloaded 

Sudo apt clean 

Gets rid of the cache on the machine as well 
