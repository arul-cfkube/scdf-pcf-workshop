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

== Steps

. Create a stream
+
[source, bash]
---------------------------------------------------------------------
dataflow:>stream create --name mysqlstream --definition "http | jdbc --tableName=names --columns=name"
---------------------------------------------------------------------

. Deploy the stream
+
[source, bash]
---------------------------------------------------------------------
stream deploy --name mysqlstream --properties "deployer.jdbc.cloudfoundry.services=mysql-db"
---------------------------------------------------------------------

. Verify the stream is successfully deployed
+
[source, bash]
---------------------------------------------------------------------
$cf apps | grep mysql
pivotal-mysqlweb                                started           1/1         1G       1G     pivotal-mysqlweb-grouchy-fossa-yq.cfapps.haas-438.pez.pivotal.io
YIOpPBA-mysqlstream-jdbc-v1                     started           1/1         2G       1G     YIOpPBA-mysqlstream-jdbc-v1.cfapps.haas-438.pez.pivotal.io
YIOpPBA-mysqlstream-http-v1                     started           1/1         2G       1G     YIOpPBA-mysqlstream-http-v1.cfapps.haas-438.pez.pivotal.io
---------------------------------------------------------------------

. Run a curl POST cmd to test
+
[source, bash]
---------------------------------------------------------------------
$ curl -d '{"name":"user1"}' -H 'Content-Type: application/json' https://YIOpPBA-mysqlstream-http-v1.cfapps.haas-438.pez.pivotal.io -k

Eg:
curl -d '{"name":"user1"}' -H 'Content-Type: application/json' <<http-app-url>> -k

---------------------------------------------------------------------

. The name should be updated in mysql db. Check names tables to confirm . We could use mysql web app to do this. https://github.com/pivotal-cf/PivotalMySQLWeb
+
[source, bash]
---------------------------------------------------------------------
select * from names;
---------------------------------------------------------------------
