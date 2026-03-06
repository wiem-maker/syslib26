# Commands for maintenance of my VM as well as installing and removing programs


#Update Commands 

Sudo apt update 

Sudo apt upgrade 

Sudo apt autoremove 

Sudo apt clean 

 ##
'''
Apt search updatefilename 
'''
Shows what programs are available

 
'''
Apt show updatefilename 
'''
Shows inforamtion about the program so that we can check that it is the right program

'''
Sudo apt install updatefilename 
'''
Installing the new program

 

#REMOVING PROGRAMS 
'''
Sudo apt --purge remove updatefilename 
'''
Purge doesn’t get rid of the configurations that were downloaded so another command needs to be run 

'''
Sudo apt autoremove 
'''
Removes the extre things that were downloaded 

'''
Sudo apt clean 
'''
Gets rid of the cache on the machine as well 


##Reflection
I'll be real, it's been a bit since I did this lesson but I've already 
used the commands from it in a few lessons since and as I continue to fight
with my documentation for this course in Github. From what I remember the only
issues I has were my fear of accidentally deleting something important with 
autoremove, purge, or clean.
