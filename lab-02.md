= Lab-01

==  Running an  app from SCDF shell

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

. Next, deploy the stream, using the stream deploy command:
+
[source, bash]
---------------------------------------------------------------------
dataflow:>stream deploy words
Deployment request has been sent for stream 'words'
---------------------------------------------------------------------
