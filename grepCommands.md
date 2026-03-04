Grep commands from the lesson and notes for myself to help my understanding

Grep –i “search term” filename 

The –i ignores case sensitivity 


If I used grep –v it would search for things that don’t have that search term 


Grep –ic “search term” filename 

The above command would search for and count how results contained the search term 


Grep –iw “complete word term we want” filename 

Above would search for the complete search term rather than simply resources containing part of the term searching “end” and only getting “end” rather than “endometriosis” for example 

The “less” command shows everthing in a file, it also freezes my machine and I have to restart it 


Using –o return only results that match 

“grep -oi “cotton” savesdrecs.bib | sort | uniq –c" 

8 COTTON 

7 Cotton 

83 cotton 


grep -i "^journal =" savedrecs.bib | cut -d"=" -f2 |\ 
    sed 's/ {//' | sed 's/},//' | \ 
    sort | uniq -c | sort –nr 

Returned the journals within the results and how many there were 