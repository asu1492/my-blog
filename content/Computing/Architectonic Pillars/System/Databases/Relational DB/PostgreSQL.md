---
source: https://btholt.github.io/complete-intro-to-databases/complex-sql-queries
---
Sources

1. <https://btholt.github.io/complete-intro-to-databases/>

POSTGRES STACKOVERFLOW

* <https://stackoverflow.com/questions/40865564/why-command-dt-gives-no-relations-found>
	
* <https://serverfault.com/questions/115051/how-to-restore-postgresql-database-from-tar-file>
	
	

History

1. UCLA POSTGRES Project 

What is PostgreSQL 

1. Open Source object relational DBMS that has genesis in UCLA POSTGRES Project 
2. open source means that you can use, modify, and distribute the Postgres source code to meet your business requirements. 
3. like **object oriented programming, supports inheritance and overloading**. Use these technique to simplify design and reuse database objects. 
4. can use all the std relational db constructs, such as keys, transactions, views, functions, and stored procedures
5. can also use some of NoSQL functionality such as JSON for structured data and HSTORE for non - hierarchical data 
6. supports two node synchronous replication
7. also supports multi node asynchronous replication.   
	1. Master node distributes changes to multiple read only replicas for scalability purposes 
8. Partitioning and Sharding(store horizontal partitions across multiple remote servers) 

Restore in Postgres

* `pg_restore -c -U postgres -d client03 -v "/tmp/client03.tar" -W`
		
	

* * *

* * *

docker run --name my-postgres -e POSTGRES\_PASSWORD\=mysecretpassword -p 5432:5432 -d --rm postgres:13.0

\-p is port
\-d is detach
\-rm is remove the logs once we are done

* * *

docker exec \-it -u postgres my-postgres psql

	psql is Command line client
	

Note:

1. Capitalization is only for own benefit. 

* * *

* * *

|     |     |
| --- | --- |
| Creating Databases | CREATE DATABASE message\_boards; |
| Connecting to a database | \\c message\_boards; |
| quit database | \\q |
| List of databases | \\l |
| List of tables | \\d |
| List of commands with \\ | \\? |
| List of Queries | \\h |
| Commenting in SQL | \--hi |
| Create a table | CREATE TABLE users (<br>user\_id INTEGER  PRIMARY KEY GENERATED ALWAYS AS IDENTITY, <br>username VARCHAR ( 25 ) UNIQUE NOT NULL,<br>email VARCHAR (50) UNIQUE NOT NULL, <br>full\_name VARCHAR (100) NOT NULL,<br>last\_login TIMESTAMP, <br>created\_on TIMESTAMP NOT NUL<br>); <br><br>Note: <br>alternate to VARCHAR is TEXT w/o any limit -- 65000 character <br>MongoDB has \_id |
| Insert into | INSERT INTO users(username, email, full\_name, created\_on) VALUES ('Abhis', '[aujjain@gmail.com](mailto:aujjain@gmail.com)', 'Abhishek Singh', NOW()); |
| Read from database | Select \* from users <br><br>projections : <br>Select username, full\_name from users; <br><br>!!! \* is wildcard means select everything. |
| WHERE | SELECT username, email, user\_id FROM users where user\_id\=150;<br><br>SELECT username, email, user\_id FROM users where last\_login IS NULL AND created\_on <  NOW() - interval '6 months' limit 10; |
| ORDER BY | SELECT created\_on, email, user\_id FROM users ORDER BY created\_on LIMIT 10; (ASC is implied) <br><br>SELECT created\_on, email, user\_id FROM users ORDER BY DESC created\_on LIMIT 10; |
| Count | SELECT COUNT(\*) FROM users; |
| Update | UPDATE users SET last\_login=NOW() WHERE user\_id=1 RETURNING \*<br><br>UPDATE users SET full\_name='Abhishek Singh', email ='lol@gmail.com' WHERE user\_id=2 RETURNING \*; <br><br>!!! Returning \* brings back updated record. |
| Foreign Keys | user\_id INT REFERENCES users(user\_id) ON DELETE CASCADE,  <br><br>ON DELETE CASCADE <br><br>* when user get deleted, go into comment table and delete all their comments. <br>* thus, easier clean up <br><br>ON DELETE SET NULL<br><br>* when user get deleted, comment remains and username is shown as NULL |

|     |
| --- |
| CREATE TABLE users (<br>user\_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,<br>username VARCHAR ( 25 ) UNIQUE NOT NULL,<br>email VARCHAR ( 50 ) UNIQUE NOT NULL,<br>full\_name VARCHAR ( 100 ) NOT NULL,<br>last\_login TIMESTAMP,<br>created\_on TIMESTAMP NOT NULL);<br><br>CREATE TABLE boards (<br>board\_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,<br>board\_name VARCHAR ( 50 ) UNIQUE NOT NULL,<br>board\_description TEXT NOT NULL);<br><br>CREATE TABLE comments (<br>comment\_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,<br>user\_id INT REFERENCES users(user\_id) ON DELETE CASCADE,<br>board\_id INT REFERENCES boards(board\_id) ON DELETE CASCADE,<br>comment TEXT NOT NULL,<br>time TIMESTAMP); |
| INT REFERENCES -- references another row in other tables -- its a superpower |

* * *

JOINS 

* * *

![[Computer Science/Databases/_resources/PostgreSQL.resources/unknown_filename.2.png]]

|     |     |
| --- | --- |
|     | SELECT comment\_id, user\_id, LEFT(comment, 20) AS preview FROM comments WHERE board\_id = 39; <br><br>Note: <br><br>* Left will give first 20 char of comments |
|     | SELECT comment\_id, user\_id, RIGHT(comment, 20) AS preview FROM comments WHERE board\_id = 39;<br><br>NOte: we are using user\_id as foreign key and not username as username may be changed by user, compromising data integrity |
| Inner Join | SELECT comment\_id, comments.user\_id, username, time, LEFT(comment, 20) AS preview <br>FROM comments<br>INNER JOIN users ON comments.user\_id = users.user\_id<br>WHERE board\_id=39;<br>![[Computer Science/Databases/_resources/PostgreSQL.resources/Image.png]]<br>\| Note: we have used comments.user\_id to remove ambiguity as it exist in multiple tables<br><br>Note: POSTgreSQL has natural inner join that intelligently matches up across without referencing foreign key... e.g we can re write above query as <br><br>SELECT comment\_id, comments.user\_id, username, time, LEFT(comment, 20) AS preview<br>FROM comments<br>NATURAL INNER JOIN users <br>WHERE board\_id=39; |

* * *

GROUP BY

* * *

Sub Queries using () 

1. SELECT
	1. comment\_id, user\_id, LEFT(comment, 20)
	2. FROM comments
	3. WHERE user\_id =(SELECT user\_id FROM users WHERE full\_name ='Maynord Simonich');

Group by  (boards with most comments)

1. SELECT  boards.board\_name, COUNT(\*) AS comment\_count
2. FROM comments
3. NATURAL INNER JOIN boards
4. GROUP BY boards.board\_name
5. ORDER BY comment\_count DESC
6. LIMIT 10;
7. ![[Computer Science/Databases/_resources/PostgreSQL.resources/unknown_filename.png]]

* * *

JSON in PostgreSQL 

* * *

1. PostgreSQL is multi paradigm database
2. can store schema and schema less data together -- blur line b/w document and records
3. SELECT content -> 'type' FROM rich\_content;
	1. SELECT DISTINCT CAST(content -> 'type'AS TEXT) AS content\_type FROM rich\_content;
	2. SELECT DISTINCT content ->> 'type'AS content\_type FROM rich\_content;
4. SELECT content -> 'dimensions' AS content\_type FROM rich\_content;
	1. SELECT content -> 'dimensions' AS content\_type FROM rich\_content WHERE content ->> 'dimensions'IS NOT NULL;
	2. ![[Computer Science/Databases/_resources/PostgreSQL.resources/unknown_filename.1.png]]

* * *

Indexes and Performance of Queries

* * *

[Indexes and Performance profile of Query.txt](https://drive.google.com/open?id=1--Bd7bcgJOx9JEdXOdrFyrDVtkY-QsNA&authuser=abhility2021%40gmail.com&usp=drive_fs)

* * *

SQL Injections

* * *

1. Ned

![[Computer Science/Databases/_resources/PostgreSQL.resources/instructions.md.pdf]]

* * *

* * *

* Regarding public in PostgreSQL, public is defined as the default schema name when no schema name is specified. However, this **can changed in the postgresql.conf file** **on the search\_path = xxx line.** To see what your current default schemas are set to issue the following SQL command:
	* SHOW\_ search\_path;
* If you want to change your default schema path in your open query session, issue the following SQL command:
	* SET search\_path = new\_path;

However, in the example you posted I believe that the naming conflict you are having problems with is not with the schema name but with the **function parameter name account\_category and the table name account\_category. You could rename your parameter name to avoid this conflict.** **==In databases with many schemas, for clarities sake I often explicitly specify public at the start of database object names.==**

Regarding your second question, I don't think PostgreSQL is unique in its usage of public, but I do know that many other databases do their schemas in a different way.
