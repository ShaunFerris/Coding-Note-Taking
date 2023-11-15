#webdevSpecific #SQL 

The content of this and related notes is heavily based on the content of the book "Learning SQL" by Alan Beulieu.

## Introduction to databases
<span style="color: cyan; font-style: italic;">A database is nothing more than a set of related information.</span>

A telephone book is a classic example of a database but it suffers from several problems. It is only indexed by first and last name, so finding a number by address is hard. From moment it is printed it gets less accurate, and searching it is time consuming. 

All paper/manual data storage systems suffer from these drawbacks, and that is why some of the first in demand applications for computer systems were electronic database systems.

### The relational model
Prior to the 70's databases used heirarchical or network models. These data models will not be covered in depth here, but see the book refferenced at the top of this note for a summary of them.

In 1970, IBM published a paper titled: <span style="color: cyan; font-style: italic;">"A relational model of data for large shared data banks"</span> which proposed that data be represented as sets of *tables*. Rather than using opinters to navigate between related entities redundant data is used to link records id different tables.
![[Pasted image 20231114123849.png]]
Each table in a relational database includes information that uniquely identifies a row in that table (known as the primary key), along with additional information needed to describe the entity completely. Looking again at the customer table, the cust_id column holds a different number for each customer; George Blake, for example, can be uniquely identified by customer ID #1. No other customer will ever be assigned that identifier, and no other information is needed to locate George Blake’s data in the customer table.

Some of the tables also include information used to navigate to another table; this is where the “redundant data” mentioned earlier comes in. For example, the account table includes a column called cust_id, which contains the unique identifier of the customer who opened the account, along with a column called product_cd, which contains the unique identifier of the product to which the account will conform. These columns are known as foreign keys, and they serve the same purpose as the lines that connect the entities in the hierarchical and network versions of the account information. If you are looking at a particular account record and want to know more information about the customer who opened the account, you would take the value of the cust_id column and use it to find the appropriate row in the customer table (this process is known, in relational database lingo, as a join.

It might seem wasteful to store the same data many times, but the relational model is quite clear on what redundant data may be stored. For example, it is proper for the account table to include a column for the unique identifier of the customer who opened the account, but it is not proper to include the customer’s first and last names in the account table as well. If a customer were to change her name, for example, you want to make sure that there is only one place in the database that holds the customer’s name; otherwise, the data might be changed in one place but not another, causing the data in the database to be unreliable. The proper place for this data is the customer table, and only the cust_id values should be included in other tables. It is also not
proper for a single column to contain multiple pieces of information, such as a name column that contains both a person’s first and last names, or an address column that contains street, city, state, and zip code information. The process of refining a database design to ensure that each independent piece of information is in only one place (except for foreign keys) is known as normalization.

## What is SQL?
SQL was originally called SEQUEL, and is a language for manipulating data in relational tables. SQL goes hand in hand with the relational model of data because the result of an SQL query is a table, also called in this context a *result set*.
What this means in practice is that a new, _permanent_ table can be created in a relational databse simply by storing the result set of a query. Similarly, a query can use both permanent tables and the result sets from other queries as inputs. SQL is sometimes said to stand for Structured Query Language, but this is not actually true (according to the author of the textbook I am using for these notes.)

### SQL statement classes
SQL the language is divided into several distinct parts, including <span style="color: cyan; font-weight: bold; font-style: italic;"></span <span style="color: cyan; font-weight: bold; font-style: italic;"></span> <span style="color: cyan; font-weight: bold; font-style: italic;">schema statements, data statements and transaction statements.</span>

Schema statements define data structures stored in the database, data statements manipulate that data and transaction statements are used to begin end and roll back transaction statements. For example to create a new table in your database you would uste the SQL schema statement `create table` whereas the process of populating your new table with data would require the SQL data statemnet `insert`.

 Here is an example statement that creates a table called `corporation`:
 ```sql
CREATE TABLE corporation
	(corp_id SMALLILNT,
	name VARCHAR(30)
	CONSTRAINT pk_corporation PRIMARY KEY (corp_id)
);
```

This statement creates a table with two columns, `corp_id` and `name` with `corp_id` as the primary key.