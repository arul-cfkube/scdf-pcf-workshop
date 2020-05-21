= HTTP to MySQL Demo

In this demonstration, you will learn how to build a data pipeline using Spring Cloud Data Flow to consume data from an http endpoint and write to MySQL database using jdbc sink.

== Requirements
. Mysql Tile installed on TAS

. create a mysql service
+
[source, bash]
---------------------------------------------------------------------
$ cf cs p.mysql small mysql-db

---------------------------------------------------------------------
. Create the test database with a names table (in MySQL) using:

+
[source, bash]
---------------------------------------------------------------------
CREATE DATABASE test;
USE test;
CREATE TABLE names
(
	name varchar(255)
);

---------------------------------------------------------------------
