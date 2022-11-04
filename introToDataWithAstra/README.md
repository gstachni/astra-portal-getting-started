# Overview
Get up and running with the **CRUD** operations you’ll need for building a PoC or working with your app.

**In this guide, we will**
- Create a database
- Create tables
- Execute (Create, Read, Update, Delete) CRUD operations

## 1 Create a database
First off, create a database with the following database and keyspace name with the button below. Once you’ve created a database and it becomes active, continue to the next section.

**Database Name:** 
```shell 
workshops
```

**Keyspace Name:** 
```shell 
chatsandra
```

<<createDatabase>>

This will only take a couple minutes. Once your database is **_ACTIVE_**, continue on to the next section.

## 2 Create tables
Ok, now that you have a database created, the next step is to create tables to work with. 

### 2a Launch the CQL Console and login to the database
Open the CQL Console using the button below. 

_You may want to put the console and this guide side by side for easy copying._

<<launchCQLConsole>>


### 2b Describe keyspaces and set keypsace context to **chatsandra**
Ok, now we're ready to rock. Creating tables is easy, but before we create one, we need to tell the database which keyspace to use.

First, let's **_DESCRIBE_** all of the keyspaces that are in the database. This will give us a list of the available keyspaces.

📘 **Command to execute**
```sql
DESC KEYSPACES;
```
_"DESC" is short for "DESCRIBE"; either is valid._

> CQL commands usually end with a semicolon '**_;_**'. If you hit 'Enter' and nothing happens (and you don't even get your prompt back), it's often because your command wasn't closed with a '**_;_**'. If in trouble, you can always get back to the prompt with **_Ctrl-C_**. Then, you can start typing a new command.

The one we care about for now is **_chatsandra_**. From here, execute the **_USE_** command with the **_chatsandra_** keyspace to tell the database our context is within **_chatsandra_**.

> _Pro-tip: Take advantage of the TAB-completion in the CQL Console. Try typing **_USE cha_** and then pressing TAB, for example._

📘 **Command to execute**
```sql
USE chatsandra;
```

Notice how the prompt displays **_<...@cqlsh:chatsandra>_**, informing us we are **_using_** the **_chatsandra_** keyspace. Now we are ready to create our table.

### 2c Create the **_users_** table
At this point, we can execute a command to create a new table. We’ll be creating a table to contain user information. Just copy/paste the following command into your CQL console and hit **_enter_**.

📘 **Command to execute**

```sql
CREATE TABLE IF NOT EXISTS users ( 
  email       TEXT,
  name        TEXT,
  password    TEXT,
  user_id     UUID,
  PRIMARY KEY (( email ))
);
```

Then **_DESCRIBE_** your keyspace tables to ensure it is there.

📘 **Command to execute**
```sql
DESC TABLES;
```

Aaaand **BOOM!**, you created a table in your database. That's it.
Now let's start working with data.

## 3 Execute CRUD operations
CRUD stands for "**create, read, update, and delete**". Simply put, they are the basic types of commands you need to work with ANY database in order to maintain data for your applications.

### 3a **(C)RUD** = create = insert data
We created the **_users_** table a moment ago, now let's put some data in it. This is done with the **INSERT** statement. We'll begin by inserting three rows into the **_users_** table.

Copy and paste the following in your CQL Console:
_(We provided a few variations to get a feel for the syntax needed.)_

📘 **Commands to execute**

```sql
INSERT INTO users (
  email,    // TEXT
  name,     // TEXT
  password, // TEXT
  user_id   // UUID: id of a user
)
VALUES (
  'otzi@mail.com',
  'Otzi Oney',
  '123456',
  11111111-1111-1111-1111-111111111111
);

INSERT INTO users (email, name, password, user_id) VALUES (
  'fred@qmail.net', 'Fred Fivey', 'qwerty',
  55555555-5555-5555-5555-555555555555
);
INSERT INTO users (email, name, password, user_id) VALUES (
  'nina@zmail.org', 'Nina Niney', 's3cr3t',
  99999999-9999-9999-9999-999999999999
);
```

### 3b **C(R)UD** = read = read data
Now that we've inserted a few rows, let's take a look at how to read the data. This is done with a **SELECT** statement.

📘 **Command to execute**

```sql
SELECT email, name, password FROM users 
  WHERE email = 'otzi@mail.com';
```

### 3d **CR(U)D** = update = update data
At this point we've **_CREATED_** and **_READ_** some data. Let’s now figure out how to change some existing data to a new value. That’s where **_UPDATE_** comes into play.

📘 **Commands to execute**

```sql
UPDATE users 
  SET password = '78910' 
    WHERE email = 'otzi@mail.com';

SELECT email, name, password FROM users 
  WHERE email = 'otzi@mail.com';
```

That's it! We successfully edited a post (on both tables).
All that's left now is to **DELETE** some data.

### 3e **CRU(D)** = delete = remove data

The final operation from our **CRUD** acronym is **DELETE**. This is the operation we use when we want to remove data from the database.

In Apache Cassandra, you can **_DELETE_** from the cell level all the way up to the partition using the same **_DELETE_** command. This means you can remove a single column in a single row, or you can remove a whole partition. 

_Generally speaking, it's best to perform as few delete operations as possible on the largest amount of data. Think of it this way: if you want to delete ALL data in a table, don't delete each individual cell, just **TRUNCATE** the table. If you need to delete all the rows in a partition, don't delete each row, **DELETE** the partition, and so on._

📘 **Commands to execute**

```sql
SELECT email, name, password FROM users;

DELETE FROM users
  WHERE email = 'otzi@mail.com';

SELECT email, name, password FROM users;
```

Notice now that we have one less user. It's as simple as that!

## 4 Summary 
In summary, we learned how to execute basic CRUD operations using CQL along with some CQL basics. 

This will get you going, but there's a lot more we can do.

If you'd like to go deeper, take a look at the following free workshop [Intro to Apache Cassandra for Developers](https://github.com/datastaxdevs/workshop-intro-to-cassandra). Or you can click below to explore how to load data into Astra and try your hand at an quickstart app using Python.
