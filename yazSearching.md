

#YAZ 

#Downloading Yaz

To use yaz we need to run “yaz-client” which opens up the system and our command sline starts with Z> 

Use the command 

'''
open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY
'''

 to actually access the UK library database 

 ##

'''
Z> find @and @attr 1=4 "information" @attr 1=21 "library science" 
'''

“find” is the search request 

“@and” is using Boolean 

“@attr 1=4” is to search for our search term in the Title Field of the search engine 

“information” and “library science” are the terms we are searching for 

“@attr 1=22 is to search for ou search term in the subject heading field 
 

“show 1” shows the first result “show 2” the second and so on 

“attr 1=1016 “search term”” is to search for the search term in the Any field of the engine 


#EXAMPLES 

'''
find @and 1=4 "Fiber" @attr 1=21 "textiles"
'''

 which returned 4 hits and 0 records.  

“Show 1” was a treaty between Us and China 


'''
find @attr 1=4 “victory garden”
'''

 returned 27 hits. “Show 1” was a book in the UK library that I went to go find in the actual UK Libraries database 

 

#End yaz with quit 


Change the files from .marc to json with
'''
yaz-marcdump -o json records.marc > records.json
'''

Then make it more readable with 
'''jq”  “jq . records.json > records-formatted.json''' 


##Reflection
I still don't understand changing the files from one form to another but I know
how to use the commands to do so. Overall, the issues I had with this lesson came 
me thinking that we were searching in the Library of Congress rather than the UK Libraries
the only reason I think this happened is that two of my other classes were both
looking at the Library of Congress rather intently at the same time as this. 
Once I got everything correct in my head everything went well and with a few 
search terms everything went great.
