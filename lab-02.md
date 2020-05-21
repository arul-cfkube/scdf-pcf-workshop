= Lab-01

== Creating a Data Pipeline Using the Dashboard

. https://docs.pivotal.io/scdf/1-6/getting-started.html#creating-a-data-pipeline-using-the-dashboard

==  Running an data pipeline from SCDF shell

. Access SCDF Shell
+
[source, bash]
---------------------------------------------------------------------
$ cf dataflow-shell my-dataflow
Attaching shell to dataflow service my-dataflow in org myorg / space dev as user...
Welcome to the Spring Cloud Data Flow shell. For assistance hit TAB or type "help".
Successfully targeted https://dataflow-9f45f80b-c6b6-43dd-a7d4-e43f14990ffd.apps.example.com
dataflow:>
---------------------------------------------------------------------


. Import the stream app starters using the Data Flow shell’s app import command:
+
[source, bash]
---------------------------------------------------------------------
dataflow:>app import https://dataflow.spring.io/rabbitmq-maven-latest
Successfully registered 65 applications from [source.sftp.metadata,
sink.throughput.metadata, ... sink.router.metadata, sink.mongodb]
---------------------------------------------------------------------

. Create the stream using the Data Flow shell’s stream create command:
+
[source, bash]
---------------------------------------------------------------------
dataflow:>stream create --name words --definition "http | splitter --expression=payload.split(' ') | log"
Created new stream 'words'
---------------------------------------------------------------------

. Deploy the stream, using the stream deploy command:
+
[source, bash]
---------------------------------------------------------------------
dataflow:>stream deploy words
Deployment request has been sent for stream 'words'
---------------------------------------------------------------------


. Login to cf cli and check the deployed apps.
+
[source, bash]
---------------------------------------------------------------------
$ cf apps
Getting apps in org myorg / space dev as user...
OK

name                        requested state   instances   memory   disk   urls
RWSDZgk-words-http-v1       started           1/1         1G       1G     RWSDZgk-words-http-v1.apps.example.com
RWSDZgk-words-log-v1        started           1/1         1G       1G     RWSDZgk-words-log-v1.apps.example.com
RWSDZgk-words-splitter-v1   started           1/1         1G       1G     RWSDZgk-words-splitter-v1.apps.example.com

---------------------------------------------------------------------

. Do a post form scdf shell
+
[source, bash]
---------------------------------------------------------------------

$ cf dataflow-shell dataflow
...
Welcome to the Spring Cloud Data Flow shell. For assistance hit TAB or type "help".
dataflow:>http post --target https://RWSDZgk-words-http-v1.apps.example.com --data "This is a test"
> POST (text/plain) https://RWSDZgk-words-http-v1.apps.example.com This is a test
> 202 ACCEPTED
---------------------------------------------------------------------

. Watch for the processed data in the RWSDZgk-words-log-v1 application’s logs:
+
[source, bash]
---------------------------------------------------------------------
2018-06-07T16:47:08.80-0500 [APP/PROC/WEB/0] OUT 2018-06-07 21:47:08.808  INFO 16 --- [plitter.words-1] RWSDZgk-words-log-v1                     : This
2018-06-07T16:47:08.81-0500 [APP/PROC/WEB/0] OUT 2018-06-07 21:47:08.810  INFO 16 --- [plitter.words-1] RWSDZgk-words-log-v1                     : is
2018-06-07T16:47:08.82-0500 [APP/PROC/WEB/0] OUT 2018-06-07 21:47:08.820  INFO 16 --- [plitter.words-1] RWSDZgk-words-log-v1                     : a
2018-06-07T16:47:08.82-0500 [APP/PROC/WEB/0] OUT 2018-06-07 21:47:08.822  INFO 16 --- [plitter.words-1] RWSDZgk-words-log-v1                     : test
---------------------------------------------------------------------
