![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab SQL Queries 5
### Jorge Castro DAPT NOV2021

In this lab, you will be using the [Sakila](https://dev.mysql.com/doc/sakila/en/) database of movie rentals. You have been using this database for a couple labs already, but if you need to get the data again, refer to the official [installation link](https://dev.mysql.com/doc/sakila/en/sakila-installation.html).

Go to:   

- <a href="#Query-1">Query 1</a>
- <a href="#Query-2">Query 2</a>
- <a href="#Query-3">Query 3</a>
- <a href="#Query-4">Query 4</a>
  - <a href="#Query-4a">Query 4a</a>
  - <a href="#Query-4b">Query 4b</a>
  - <a href="#Query-4c">Query 4c</a>
  - <a href="#Query-4d">Query 4d</a>







The database is structured as follows:

![dvdrental](https://user-images.githubusercontent.com/63274055/147394200-2fdeec2e-a41a-4094-b544-dede2b263d96.png)

<br><br>

## Instructions

### Query 1
Drop column `picture` from `staff`. 


<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>


### Query 2
A new person is hired to help Jon. Her name is TAMMY SANDERS, and she is a customer. Update the database accordingly.           

<a href="#Lab-SQL-Queries-5">Go to Top</a>


### Query 3
Add rental for movie "Academy Dinosaur" by Charlotte Hunter from Mike Hillyer at Store 1. You can use current date for the `rental_date` column in the `rental` table.
   **Hint**: Check the columns in the table rental and see what information you would need to add there. You can query those pieces of information. For eg., you would notice that you need `customer_id` information as well. To get that you can use the following query:

 ```sql
    select customer_id from sakila.customer
    where first_name = 'CHARLOTTE' and last_name = 'HUNTER';
```
    
    Use similar method to get `inventory_id`, `film_id`, and `staff_id`.


<a href="#Lab-SQL-Queries-5">Go to Top</a>

   

### Query 4
Delete non-active users, but first, create a _backup table_ `deleted_users` to store `customer_id`, `email`, and the `date` for the users that would be deleted. Follow these steps:

- #### Query 4a
   Check if there are any non-active users

<a href="#Lab-SQL-Queries-5">Go to Top</a>


- #### Query 4b
   Create a table _backup table_ as suggested

<a href="#Lab-SQL-Queries-5">Go to Top</a>


- #### Query 4c
  Insert the non active users in the table _backup table_

<a href="#Lab-SQL-Queries-5">Go to Top</a>

 
- #### Query 4d
  Delete the non active users from the table _customer_

<a href="#Lab-SQL-Queries-5">Go to Top</a>

