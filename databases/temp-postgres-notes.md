**how to connect to a postgres Server**
- postgres runs on port 5432 of localhost.
- postgres use postgres protocol, so we cant connect to the localhost:5432 with http/https throught browser.
- we can connect to the postgres server only through the postgres client. 
- psql client in terminal, pgadmin GUI, database driver in application server.
```bash
psql -h localhost -p 5432 -U vidysagar
```
- Connection establishes between client and `localhost:5432` after authentication of user vidyasagar successful.


**DATABASE Level Commands**
|command|description|
|-------|-----------|
|`\l` or `\list`|Show all databases|
|`CREATE DATABASE SCHOOL;`|creates new database school|
|`ALTER DATABASE SCHOOL RENAME TO COLLEGE`|rename a database|
|`DROP DATABASE COLLEGE`|Delete a database|
|`\c COLLEGE` OR `\connect COLLEGE`|Connection to a database(use)|
|`SHOW CURRENT_DATABASE();`|Display current database|

**SCHEMA Level Commands**
|command|description|
|-------|-----------|
|`CREATE SCHEMA AUTH;`|creates a schema|
|`ALTER SCHEMA AUTH RENAME TO SALES;`|rename the schema|
|`DROP SCHEMA AUTH CASCADE;`|delete the schema and inside objects|
|`ALTER TABLE SALES.EMPLOYEE SET SCHEMA AUTH;`|move the employee table from sales to auth schema|
|`ALTER VIEW SALES.EMPLOYEE_VIEW SET SCHEMA AUTH;`|move the employee_view from sales to auth schema|
|`SET SEARCH_PATH TO SALES,AUTH,PUBLIC;`|default path for object acess/creation|

**TABLE structure Level Commands**
| command | description |
|---------|------------|
|<code>CREATE TABLE AUTH.USERS(<br>ID INT PRIMARY KEY,<br>USER_NAME VARCHAR(50),<br>EMAIL VARCHAR(100),<br>CREATED_AT DATE);<code>| creates a table users in the auth schema|
|`DROP TABLE AUTH.USERS CASCADE;`|delete the table auth.users|
|`TRUNCATE TABLE AUTH.USERS;`|empty up the table auth.users|
|`ALTER TABLE AUTH.USERS RENAME TO USERS_TABLE;`|renamae table|
|`ALTER TABLE AUTH.USERS ADD COLUMN ACTIVE_STATUS BOOL;`|add a column|
|`ALTER TABLE AUTH.USERS DROP COLUMN ACTIVE_STATUS;`|drop a column|
|`ALTER TABLE AUTH.USERS RENAME COLUMN ACTIVE_STATUS TO STATUS;`|rename a column|
|`ALTER TABLE AUTH.USERS ALTER COLUMN ACTIVE_STATUS TYPE INT;`|change column type|

**DATA TYPES in postgres**
| type | description |
|---------|------------|
|`uuid`|universal unique id (efficent in distributed systems)|
|`smallint`|-32768 to +32767|
|`integer`|-2147483648 to +2147483647|
|`bigint`|-9223372036854775808 to +9223372036854775807|
|`numeric(s,p)` or `decimal(s,p)`|float number length `s` with `p` digits after decimal|
|`smallserial`|autoincrement from 1 to 32767|
|`serial`|autoincrement from 1 to 2147483647|
|`bigserial`|autoincrement from 1 to 9223372036854775807|
|`char(50)`|fixed-length, blank-padded|
|`varchar(50)`|variable-length with limit|
|`text`|unlimited length|
|`date`|'2020-02-23'|
|`time`|'23:30:43'|
|`timestamp`|'2020-02-23 23:30:43'|
|`timestamptz`|'2020-02-23 23:30:43+02'|
|`boolean`|true or false|
|`create type transaction_status as enum("pending","processing","sent","completed");`|enum creation|
|`alter type transaction_status add value "processed" before/after "sent";`|add value to enum|
|`alter type transaction_status drop value "processed";`|delete value to enum|
|`alter type transaction_status rename value "a" to "b";`|rename value in enum|
|`drop type transaction_status`|delete data type|

**data constraints in postgres**
| type | description |
|---------|------------|
|`primary key`|table identifier. only one. `unique` + `not null`.|
|`foreign key`|check weather if the value is present in the other table.|
|`not null`|ensures the values is not null|
|`unique`|ensure the value is unique in the entire column|
|`default`|specifies what the default value should be when no values is inserted.|
|`check`|ensure the rule is not voilating|

***primary key***
| command | description |
|---------|------------|
|`id serial primary key`| while creation of a table|
|`primary key (order_id,product_id)`|composite primary key while creation of a table (in new line)|
|`ALTER TABLE USERS ADD CONSTRAINT PkEY_CONSTRAINT PRIMARY KEY(USER_ID)`| add constraint later on to a table|
|`ALTER TABLE USERS DROP CONSTRAINT PKEY_CONSTRAINT;`|drop contraint `PKEY_CONSTRAINT`|

***foreign key***
| command | description |
|---------|------------|
|`order_id integer references sales.orders(order_id)`|creation of foreign key|
|`foreign key(order_id) references sales.orders(order_id)`|creation of foreigin key|
|`foreign key(order_id,customer_id) references sales.orders(order_id,customer_id)`|composite foreigin key|
|`alter table shipments add constraint Fkey_shipments foreign key(order_id) references sales.orders(order_id);`|add constraint|
|`alter table shipments drop constraint Fkey_shipments;`|drop constraint|
| `order_id integer references sales.orders(order_id) on delete cascade on update cascade`|cascade on delete and update of the parent table|

***not null***
| command | description |
|---------|------------|
|`name varchar(50) not null`|creation|

***unique***
| command | description |
|---------|------------|
|`username varchar(50) unique`|creation|
|`unique (order_id,product_id)`|composite unique|
|`ALTER TABLE USERS ADD CONSTRAINT check_constraint UNIQUE(USERNAME)`| add constraint later on to a table|
|`ALTER TABLE USERS DROP CONSTRAINT UNIQUE_CONSTRAINT;`|drop contraint `UNIQUE_CONSTRAINT`|

***default***
| command | description |
|---------|------------|
|`created_at date not null default now()`|creation|
- note: now() can be used for any timestamp related types. example: date, time, timestamp,timestamptz. it will auto set correct type.

***check***
| command | description |
|---------|------------|
|`check(price>30)`|creation|
|`check(price>30 and age>20)`|composite check|
|`ALTER TABLE USERS ADD CONSTRAINT check_constraint check(price>30 and age>20)`| add constraint later on to a table|
|`ALTER TABLE USERS DROP CONSTRAINT check_constraint;`|drop contraint `check_constraint`|
- note: and,or,not,>,<,>=,<=,=,<>(not equals) are used in check

**TABLE operational Level Commands**
