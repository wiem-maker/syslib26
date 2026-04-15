## Step 1: The plan and what I think is wrong
I think that something went wrong with the “books” database in MySQL and that it has been messing with the programming that has been inserted at various points. To try and counteract this I plan to completely delete the “books” database and then go through everything that may need access to that database and re-do them. 
---
## Step 2: Logging into MySQL and seeing what is there
To begin this I will need to log into MySQL and access the opacdb where my “books” table is stored.
```
Mysql -u opener -p
Show databases;
Open opacdb;
Show tables;
```
From here I can see that there are three different tables in “opacdb” with all of them being names “books” but with the addition of a number at the end from when I oirigionally set these up and ran into issues. After looking around the internet some I learned how to use the “DROP” command and as such will delete all of the books tables so that I can start fresh.
```
Drop table books;
Drop table books2;
Drop table books3;
```
—
## Step 3: Recreating the Books table
Now that everything is gone I will start a new table with the name of books and re-do the various commands I ran in the “MySQL.md” file in GitHub.
```
create table books (
        id int unsigned not null auto_increment,
        author varchar(150) not null,
        title varchar(150) not null,
        copyright year not null,
        primary key (id)
);
Show tables;
```
Now that the table is created again I checked that it existed in “opacdb” and will now go through some of the other commands from the original lesson. 
```
insert into books (author, title, copyright) values
('Jennifer Egan', 'The Candy House', '2022'),
('Imbolo Mbue', 'How Beautiful We Were', '2021'),
('Lydia Millet', 'A Children\'s Bible', '2020'),
('Julia Phillips', 'Disappearing Earth', '2019');
alter table books add publisher varchar(75) after title;
update books set publisher='Simon & Schuster' where id='1';
update books set publisher='W. W. Norton & Company' where id='3';
update books set publisher='Knopf' where id='4';
delete from books where author='Julia Phillips';
insert into books
       (author, title, publisher, copyright) values
       ('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
       ('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010-01-01'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-01');
```
—
## Step 4: Actually Fixing the “books” Table
Now that that is complete it is time to run some of the lines that I believe have created this whole issue with the “books” table.
```
alter table books add publication_date date;
update books set publication_date = str_to_date(concat(copyright, '-01-01'), '%Y-%m-%d');
drop column copyright;
change publication_date copyright date not null;
```
After doing this I went ahead and checked if the Barebones OPAC (MySQL Server Example) was working right. It actually was, as far as I can tell the issue that I’ve been having regarding both the “books” table and the DD-MM-YYYY format has been fixed. 
—
## Reflection
This has been an issue bothering me for several weeks and through sever assignments. I finally realized that I could just completely delete the problem table of “books” and redo it to fix everything. It has been an issue that I keep having as we move through this class where my mind believes that everything is more interconnected than it actually is, making any fixes that need to be made seem to be mountains rather than molehills. I am very pleased that this is now fixed and will go through some of the other things we have done to make sure everything else is working well. 
