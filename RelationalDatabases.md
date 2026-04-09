## Introduction to Relational Databases

## Step 1: Create Database
To create a new database we must log in as the root MySQL user as the account I previously made doesn’t have permission to create new databases. I log it with the command,
```
sudo mysql -u root
```
Then I create a new database called DinnerDB and allow my ‘opener’ account privileges with the command
```
create database DinnerDB;
grant all privileges on DinnerDB.* to ‘opener’@’localhost’;
```
Then I exit the root MySQL account
```
\q
```
---
## Step 2: Create Tables
At this point I logged in as ‘opacuser’@’localhost’ with 
```
mysql -u opener -p
```
Then I checked that DinnerDB was created and in the right spot with
```
show databases;
```
Which showed that I had three databases, one of which was DinnerDB. Then to access DinnerDB I used the command of 
```
use DinnerDB;
```


—
## Step 3: Create Meals Table
And then entered the following code
```
create table Meals (
    meal_id int auto_increment primary key,
    meal_name varchar(100) not null,
    cuisine varchar(50),
    cooking_time int not null default 1 check (cooking_time > 0),
    vegetarian boolean
);
```
—
## Step 4: Create Ingredients Table
The proceeded to create another table in DinnerDB called Ingredients rather than the above Meals. 
```
create table Ingredients (
    ingredient_id int auto_increment primary key,
    meal_id int not null,
    ingredient_name varchar(100) not null,
    quantity varchar(50),
    foreign key (meal_id) references Meals(meal_id) on delete cascade
);
```
—
## Step 5: Insert Data
From here I began entering information on what meal and what nationality of meal. 
```
insert into Meals (meal_name, cuisine, cooking_time, vegetarian) values
    ('Spaghetti Bolognese', 'Italian', 45, FALSE),
    ('Vegetable Stir Fry', 'Chinese', 20, TRUE),
    ('Chicken Curry', 'Indian', 50, FALSE),
    ('Mushroom Risotto', 'Italian', 35, TRUE);
```
Then entered what ingredient and how much for each of the meals. 
```
insert into Ingredients (meal_id, ingredient_name, quantity) values
    (1, 'Spaghetti', '200g'),
    (1, 'Ground Beef', '250g'),
    (1, 'Tomato Sauce', '1 cup'),
    (2, 'Broccoli', '100g'),
    (2, 'Carrots', '50g'),
    (2, 'Soy Sauce', '2T'),
    (3, 'Chicken Breast', '300g'),
    (3, 'Curry Powder', '2T'),
    (3, 'Coconut Milk', '1 cup'),
    (4, 'Arborio Rice', '1 cup'),
    (4, 'Mushrooms', '1 cup'),
    (4, 'Parmesan Cheese', '1/2 cup');
```
—
## Step 6: Querying Data
Now that all of the information is entered it is now usable and can be searched.
```
Select * from Meals;
```
The results showed all of the meals that were entered.
```
Select * from Meals where vegetarian =TRUE;
```
Which returned vegetable stir fry and mushroom risotto, both of which are apparently vegetarian.
I can also sort the recipes is descending and ascending order by changing the last word of the next command from desc to asc
```
select * from Meals order by cooking_time desc;
```
Then to combine the two tables so that they are more easily readable we rename three rows and combine the tables with
```
select Meals.meal_name as Meals,
    Ingredients.ingredient_name as Ingredients,
    Ingredients.quantity as Quantity
    from Meals
    join Ingredients on Meals.meal_id = Ingredients.meal_id;
```
Then to show how this is useful we search for the ingredients of chicken curry with
```
select ingredient_name as Ingredients,
    quantity as Quantity
    from Ingredients 
    where meal_id = (select meal_id from Meals where meal_name = 'Chicken Curry');
```
Despite this, I can still conduct data analysis on what nationality all of the meals are
```
select cuisine, count(*) as meal_count 
    from Meals
    group by cuisine;
```
Then I sort everything by what meal is under 45 minutes to make
```
select meal_name, cooking_time 
    from Meals 
    where cooking_time <= 45
    order by cooking_time asc;
```
And then leave mysql with
```
\q
```
—
## Step 7: Database Management
Then I want to learn how to revoke privileges to accounts so I start on that by logging into the root account
```
Sudo mysql -u root
```
Then to be able to revoke privileges I need to be able to see what privileges an account has with
```
Show grants for ‘opener’@’localhost’;
```
This shows three different lines  of results of things that have been granted to this account. From here I begin to revoke privileges.
```
revoke all privileges on DinnerDB.* from ‘opener’@’localhost’;
```
Which I then checked with 
```
show grants for ‘opener’@’localhost’;
```
Which shows that I did actually remove privileges on it. Then I get curious to see if my failure of an opacuser account is still around so I run 
```
Select user, host from mysql.user;
```
My failure opacuser account is still there so I’m going to drop it. 
```
Drop user ‘opacuser’@’localhost’;
```
No one else should do this but I’m using a different account name so it works for me. While I’m at it I go ahead and drop the DinnerDB tables as well. 
```
drop database DinnerDB;
```
—
## Reflection
Everything worked well other than some trouble that I experienced with signing into my ‘opener’@’localhost’ account, something which I knew was likely to happen again. This is also part of the reason why I deleted the opacuser account even though it was wasn’t doing anything. I’d rather just get rid of it entirely than to have my failure lingering around. 


