# Question 4

> 4.	Create a database(internship) and few tables in database.

To Create database (internship) simple postgres SQL query can be written as 

```
~ CREATE DATABASE internship;
```

![Create Database](screenshots/Screenshot%202021-11-30%20024510.png) 

To create table, change into the database created and run the following query:

```
# \c internship
# CREATE TABLE members (
	id int,
	name varchar(255)
);
```

![Create Table](screenshots/Screenshot%202021-11-30%20024959.png)