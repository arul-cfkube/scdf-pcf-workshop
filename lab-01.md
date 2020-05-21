



# Creating a Data Pipeline Using the Shell

Start the Data Flow shell using the cf dataflow-shell command added by the Spring Cloud Data Flow for VMware Tanzu cf CLI plugin:

$ cf dataflow-shell my-dataflow
Attaching shell to dataflow service my-dataflow in org myorg / space dev as user...
Welcome to the Spring Cloud Data Flow shell. For assistance hit TAB or type "help".
Successfully targeted https://dataflow-9f45f80b-c6b6-43dd-a7d4-e43f14990ffd.apps.example.com
dataflow:>
Import the stream app starters using the Data Flow shell’s app import command:

```
dataflow:>app import https://dataflow.spring.io/rabbitmq-maven-latest
Successfully registered 65 applications from [source.sftp.metadata,
sink.throughput.metadata, ... sink.router.metadata, sink.mongodb]
With the app starters imported, you can use three–the http source, the split processor, and the log sink–to create a stream that consumes data via an HTTP POST request, processes it by splitting it into words, and outputs the results in logs.
```
Create the stream using the Data Flow shell’s stream create command:

```
dataflow:>stream create --name words --definition "http | splitter --expression=payload.split(' ') | log"
Created new stream 'words'
```
Next, deploy the stream, using the stream deploy command:
```
dataflow:>stream deploy words
Deployment request has been sent for stream 'words'
```


= Lab 01

==  Requirements for lab-01

. SCDF tile installed on TAS
+
[source, bash]
---------------------------------------------------------------------
$ cf create-service p-dataflow standard scdf-service

$ cf service scdf-service
Showing info of service scdf-service in org arul / space test as admin...

name:             scdf-service
service:          p-dataflow
tags:
plan:             standard
description:      Deploys Spring Cloud Data Flow servers to orchestrate data pipelines
documentation:    http://cloud.spring.io/spring-cloud-dataflow/
dashboard:        https://p-dataflow.cfapps.haas-438.pez.pivotal.io/instances/ee360a67-9af3-4ce6-a5ae-e8a621d6365c/dashboard
service broker:   p-dataflow

Showing status of last operation from service scdf-service...

status:    create succeeded
message:   Created
started:   2020-05-16T21:13:32Z
updated:   2020-05-16T21:32:19Z
---------------------------------------------------------------------

. Install SCDF cf cli plugin
+
[source,bash]
---------------------------------------------------------------------
 $ cf install-plugin -r CF-Community "spring-cloud-dataflow-for-pcf"
---------------------------------------------------------------------

. Creating a Data Pipeline Using the Shell
+
[source,bash]
---------------------------------------------------------------------
 $ cf install-plugin -r CF-Community "spring-cloud-dataflow-for-pcf"
---------------------------------------------------------------------

. Creating a Data Pipeline Using the Shell
+
[source,bash]
---------------------------------------------------------------------

cf dfsh scdf-service
Attaching shell to dataflow service scdf-service in org arul / space test as admin...
Launching dataflow shell JAR
  ____                              ____ _                __
 / ___| _ __  _ __(_)_ __   __ _   / ___| | ___  _   _  __| |
 \___ \| '_ \| '__| | '_ \ / _` | | |   | |/ _ \| | | |/ _` |
  ___) | |_) | |  | | | | | (_| | | |___| | (_) | |_| | (_| |
 |____/| .__/|_|  |_|_| |_|\__, |  \____|_|\___/ \__,_|\__,_|
  ____ |_|    _          __|___/                 __________
 |  _ \  __ _| |_ __ _  |  ___| | _____      __  \ \ \ \ \ \
 | | | |/ _` | __/ _` | | |_  | |/ _ \ \ /\ / /   \ \ \ \ \ \
 | |_| | (_| | || (_| | |  _| | | (_) \ V  V /    / / / / / /
 |____/ \__,_|\__\__,_| |_|   |_|\___/ \_/\_/    /_/_/_/_/_/

2.4.2.RELEASE

Welcome to the Spring Cloud Data Flow shell. For assistance hit TAB or type "help".
Successfully targeted https://dataflow-ee360a67-9af3-4ce6-a5ae-e8a621d6365c.cfapps.haas-438.pez.pivotal.io
dataflow:>
---------------------------------------------------------------------
