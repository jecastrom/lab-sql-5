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

#### Answer:
```sql
ALTER TABLE
    staff DROP COLUMN picture;
```

<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>


### Query 2
A new person is hired to help Jon. Her name is TAMMY SANDERS, and she is a customer. Update the database accordingly.

#### Answer:
```sql
INSERT INTO
    staff
VALUES
    (
        DEFAULT,
        'Tammy',
        'Sanders',
        79,
        'Tammy.Sanders@sakilastaff.com',
        2,
        1,
        'Tammy',
        'P@ssw0rd',
        NOW()
    );
```
<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>


### Query 3
Add rental for movie "Academy Dinosaur" by Charlotte Hunter from Mike Hillyer at Store 1. You can use current date for the `rental_date` column in the `rental` table.
   **Hint**: Check the columns in the table rental and see what information you would need to add there. You can query those pieces of information. For eg., you would notice that you need `customer_id` information as well. To get that you can use the following query:

 ```sql
    select customer_id from sakila.customer
    where first_name = 'CHARLOTTE' and last_name = 'HUNTER';
```
    
    Use similar method to get `inventory_id`, `film_id`, and `staff_id`.

#### Answer:
```sql
INSERT INTO
    rental
VALUES
    (DEFAULT, NOW(), 1, 130, NULL, 1, NOW());
```
<img src="" alt="Drawing" style="width: 200px;"/>
<a href="#Lab-SQL-Queries-5">Go to Top</a>

   

### Query 4
Delete non-active users, but first, create a _backup table_ `deleted_users` to store `customer_id`, `email`, and the `date` for the users that would be deleted. Follow these steps:

- #### Query 4a
   Check if there are any non-active users
#### Answer:
```sql
SELECT
    count(*) AS non_active_users
FROM
    customer
WHERE
    `active` = (0);
```
<img src="" alt="Drawing" style="width: 200px;"/>
 or with a bit more visivility 
 and to store the query 
 on a view for later use:         
<br></br>

```sql
CREATE VIEW users_status AS
SELECT
    (
        SELECT
            count(*)
        FROM
            customer
        WHERE
            `active` = 1
    ) AS active_users,
    (
        SELECT
            count(*)
        FROM
            customer
        WHERE
            `active` = 0
    ) AS non_active_users
FROM
    customer
GROUP BY
    1,
    2;
```
<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>


- #### Query 4b
   Create a table _backup table_ as suggested
#### Answer:
```sql
CREATE TABLE deleted_users
SELECT
    customer_id,
    email,
    last_update
FROM
    customer
LIMIT
    0;
```
<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>


- #### Query 4c
  Insert the non active users in the table _backup table_
#### Answer:
```sql
INSERT INTO
    deleted_users (customer_id, email)
SELECT
    customer_id,
    email
FROM
    customer
WHERE
    `active` = (0);
```
 I verify that the data was actually copied to the new table:
 

 ```sql
 SELECT
    count(*) AS deleted_users_backup
FROM
    deleted_users
WHERE
    date(last_update) = '2021-12-26';
```
<img src="" alt="Drawing" style="width: 200px;"/>

<a href="#Lab-SQL-Queries-5">Go to Top</a>

 
- #### Query 4d
  Delete the non active users from the table _customer_

#### Answer:
```sql
DELETE FROM
    customer
WHERE
    `active` = 0;
```
I am unable to delete the non-active users from the custome table. This table has 2 
foreign keys: at table "payment" and "rental" that references the "customer_id" key with the foreign 
key constrain "ON DELETE RESTRICT", which is the default constrain when adding a foreign key
in MySQL.
 
Asuming that the business agrees and there are not compliece restrictions on deleting this recors,
for me the best solution would be to change the constrain from "ON DELETE RESTRICT ON UPDATE CASCADE"
to "ON DELETE CASCADE ON UPDATE CASCADE". This way the non-active costomers along with their payment 
history would be deleted.

To alter the table and drop the previous constrait, we need the constraint name:

```sql
SHOW CREATE TABLE payment;
SHOW CREATE TABLE rental;
```

Now having the constraint name (fk_payment_customer) and (fk_rental_customer) I can proceed
to drop the old constraint, and add the new constraint ON DELETE CASCADE ON UPDATE CASCADE.

```sql
ALTER TABLE
    payment DROP FOREIGN KEY fk_payment_customer,
ADD
    FOREIGN KEY(customer_id) REFERENCES customer(customer_id) ON DELETE CASCADE ON UPDATE CASCADE;
ALTER TABLE
    rental DROP FOREIGN KEY fk_rental_customer,
ADD
    FOREIGN KEY(customer_id) REFERENCES customer(customer_id) ON DELETE CASCADE ON UPDATE CASCADE;
```
Now I can delete all non-active users:

```sql
DELETE FROM
    customer
WHERE
    `active` = 0;
```
To check if the non-users delete went as expected
I do a select from the view I created previously to see 
the count of total users statuses:

```sql
SELECT
    *
FROM
    users_status;
```
<img src="" alt="Drawing" style="width: 200px;"/>


<a href="">CLICK HERE for the SQL Lab 5 script only on .sql file</a>

<a href="#Lab-SQL-Queries-5">Go to Top</a>

