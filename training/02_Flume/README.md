# Flume

```html
http://flume.apache.org
```

- [Brief Introduction](#brief-introduction)
  - [Architecture](#architecture)
    - [Basic Flow](#basic-flow)
    - [Multi-Agent Flow](#multi-agent-flow)
    - [Consolidation](#consolidation)
    - [Multiplexing](#multiplexing)
    - [Bin Structure](#bin-structure)
    - [Start Agent](#start-agent)
  - [Setup](#setup)
- [Demo](#demo)
  - [A Simple Sample](#a-simple-sample)
    - [Prepare](#prepare)
    - [Sample Config](#sample-config)
    - [Start Sample Agent](#start-sample-agent)
    - [Test](#test)
  - [ZooKeeper Integration](#zookeeper-integration)
  - [Plugins](#plugins)
  - [Data Ingestion](#data-ingestion)
    - [RPC](#rpc)
    - [Executing Commands](#executing-commands)
    - [Network Streaming](#network-streaming)
- [Components](#components)
  - [Sources](#sources)
    - [Avro Source](#avro-source)
    - [Thift Source](#thrift-source)
    - [Exec Source](#exec-source)
    - [JMS Source](#jms-source)
    - [Spooling Directory Source](#spooling-directory-source)
    - [Taildir Source](#taildir-source)
    - [Kafka Source](#kafka-source)
    - [NetCat TCP Source](#netcat-tcp-source)
    - [NetCat UDP Source](#netcat-udp-source)
    - [Syslog TCP Source](#syslog-tcp-source)
    - [Multiport Syslog TCP Source](#multiport-syslog-tcp-source)
    - [Syslog UDP Source](#syslog-udp-source)
    - [HTTP Source](#http-source)
    - [Stress Source](#stress-source)
    - [Custom Source](#custom-source)
    - [Scribe Source](#scribe-source)
  - [Sinks](#sinks)
    - [HDFS Sink](#hdfs-sink)
    - [Hive Sink](#hive-sink)
    - [Logger Sink](#logger-sink)
    - [Avro Sink](#avro-sink)
    - [Thrift Sink](#thrift-sink)
    - [IRC Sink](#irc-sink)
    - [File Roll Sink](#file-roll-sink)
    - [Null Sink](#null-sink)
    - [HBase Sinks](#hbase-sinks)
      - [HBaseSink](#hbasesink)
      - [HBase2Sink](#hbase2sink)
      - [AsyncHBaseSink](#asynchbasesink)
    - [MorphlineSolrSink](#morphlinesolrsink)
    - [ElasticSearchSink](#elasticsearchsink)
    - [Kite Dataset Sink](#kite-dataset-sink)
    - [Kafka Sink](#kafka-sink)
    - [HTTP Sink](#http-sink)
    - [Custom Sink](#custom-sink)
  - [Channels](#channels)
    - [Memory Channel](#memory-channel)
    - [JDBC Channel](#jdbc-channel)
    - [Kafka Channel](#kafka-channel)
    - [File Channel](#file-channel)
    - [Spillable Memory Channel](#spillable-memory-channel)
    - [Pseudo Transaction Channel](#pseudo-transaction-channel)
    - [Custom Channel](#custom-channel)
  - [Channel Selectors](#channel-selectors)
    - [Replicating Channel Selector](#replicating-channel-selector)
    - [Multiplexing Channel Selector](#multiplexing-channel-selector)
    - [Custom Channel Selector](#custom-channel-selector)
  - [Sink Processors](#sink-processors)
    - [Default Sink Processor](#default-sink-processor)
    - [Failover Sink Processor](#failover-sink-processor)
    - [Load balancing Sink Processor](#load-balancing-sink-processor)
    - [Custom Sink Processor](#custom-sink-processor)
  - [Event Serializers](#event-serializers)
    - [Body Text Serializer](#body-text-serializer)
    - ["Flume Event" Avro Event Serializer](#"flume-event"-avro-event-serializer)
    - [Avro Event Serializer](#avro-event-serializer)
  - [Interceptors](#interceptors)
    - [Timestamp Interceptor](#timestamp-interceptor)
    - [Host Interceptor](#host-interceptor)
    - [Static Interceptor](#static-interceptor)
    - [Remove Header Interceptor](#remove-header-interceptor)
    - [UUPD Interceptor](#uuid-interceptor)
    - [Morphline Interceptor](#morphline-interceptor)
    - [Search and Replace Interceptor](#search-and-replace-interceptor)
    - [Regex Filtering Interceptor](#regex-filtering-interceptor)
  - [Alias Conventions](#alias-conventions)

## Brief Introduction

### Architecture

#### Basic Flow

![Flume Basic Flow](http://flume.apache.org/_images/DevGuide_image00.png "Basic Flow")

#### Multi-Agent Flow

![Flume Multi-Agent Flow](http://flume.apache.org/_images/UserGuide_image03.png "Multi-Agent Flow")

#### Consolidation

![Flume Consolidation](http://flume.apache.org/_images/UserGuide_image02.png "Consolidation")

#### Multiplexing

![Flume Multiplexing](http://flume.apache.org/_images/UserGuide_image01.png "Multiplexing")

#### Bin Structure

```bash
$  tree -I *doc*
.
├── CHANGELOG
├── DEVNOTES
├── LICENSE
├── NOTICE
├── README.md
├── RELEASE-NOTES
├── bin
│   ├── flume-ng
│   ├── flume-ng.cmd
│   └── flume-ng.ps1
├── conf
│   ├── flume-conf.properties.template
│   ├── flume-env.ps1.template
│   ├── flume-env.sh.template
│   └── log4j.properties
├── doap_Flume.rdf
├── lib
│   ├── apache-log4j-extras-1.1.jar
│   ├── async-1.4.0.jar
│   ├── asynchbase-1.7.0.jar
│   ├── avro-1.7.4.jar
│   ├── avro-ipc-1.7.4.jar
│   ├── commons-cli-1.2.jar
│   ├── commons-codec-1.8.jar
│   ├── commons-collections-3.2.2.jar
│   ├── commons-compress-1.4.1.jar
│   ├── commons-dbcp-1.4.jar
│   ├── commons-io-2.1.jar
│   ├── commons-jexl-2.1.1.jar
│   ├── commons-lang-2.5.jar
│   ├── commons-logging-1.2.jar
│   ├── commons-pool-1.5.4.jar
│   ├── curator-client-2.6.0.jar
│   ├── curator-framework-2.6.0.jar
│   ├── curator-recipes-2.6.0.jar
│   ├── derby-10.14.1.0.jar
│   ├── flume-avro-source-1.9.0.jar
│   ├── flume-dataset-sink-1.9.0.jar
│   ├── flume-file-channel-1.9.0.jar
│   ├── flume-hdfs-sink-1.9.0.jar
│   ├── flume-hive-sink-1.9.0.jar
│   ├── flume-http-sink-1.9.0.jar
│   ├── flume-irc-sink-1.9.0.jar
│   ├── flume-jdbc-channel-1.9.0.jar
│   ├── flume-jms-source-1.9.0.jar
│   ├── flume-kafka-channel-1.9.0.jar
│   ├── flume-kafka-source-1.9.0.jar
│   ├── flume-ng-auth-1.9.0.jar
│   ├── flume-ng-config-filter-api-1.9.0.jar
│   ├── flume-ng-configuration-1.9.0.jar
│   ├── flume-ng-core-1.9.0.jar
│   ├── flume-ng-elasticsearch-sink-1.9.0.jar
│   ├── flume-ng-embedded-agent-1.9.0.jar
│   ├── flume-ng-environment-variable-config-filter-1.9.0.jar
│   ├── flume-ng-external-process-config-filter-1.9.0.jar
│   ├── flume-ng-hadoop-credential-store-config-filter-1.9.0.jar
│   ├── flume-ng-hbase-sink-1.9.0.jar
│   ├── flume-ng-hbase2-sink-1.9.0.jar
│   ├── flume-ng-kafka-sink-1.9.0.jar
│   ├── flume-ng-log4jappender-1.9.0.jar
│   ├── flume-ng-morphline-solr-sink-1.9.0.jar
│   ├── flume-ng-node-1.9.0.jar
│   ├── flume-ng-sdk-1.9.0.jar
│   ├── flume-scribe-source-1.9.0.jar
│   ├── flume-shared-kafka-1.9.0.jar
│   ├── flume-spillable-memory-channel-1.9.0.jar
│   ├── flume-taildir-source-1.9.0.jar
│   ├── flume-thrift-source-1.9.0.jar
│   ├── flume-tools-1.9.0.jar
│   ├── flume-twitter-source-1.9.0.jar
│   ├── geronimo-jms_1.1_spec-1.1.1.jar
│   ├── gson-2.2.2.jar
│   ├── guava-11.0.2.jar
│   ├── httpclient-4.5.3.jar
│   ├── httpcore-4.4.6.jar
│   ├── irclib-1.10.jar
│   ├── jackson-annotations-2.9.7.jar
│   ├── jackson-core-2.9.7.jar
│   ├── jackson-core-asl-1.9.3.jar
│   ├── jackson-databind-2.9.7.jar
│   ├── jackson-mapper-asl-1.9.3.jar
│   ├── javax.servlet-api-3.1.0.jar
│   ├── jetty-6.1.26.jar
│   ├── jetty-http-9.4.6.v20170531.jar
│   ├── jetty-io-9.4.6.v20170531.jar
│   ├── jetty-jmx-9.4.6.v20170531.jar
│   ├── jetty-security-9.4.6.v20170531.jar
│   ├── jetty-server-9.4.6.v20170531.jar
│   ├── jetty-servlet-9.4.6.v20170531.jar
│   ├── jetty-util-6.1.26.jar
│   ├── jetty-util-9.4.6.v20170531.jar
│   ├── joda-time-2.9.9.jar
│   ├── jopt-simple-5.0.4.jar
│   ├── jsr305-1.3.9.jar
│   ├── kafka-clients-2.0.1.jar
│   ├── kafka_2.11-2.0.1.jar
│   ├── kite-data-core-1.0.0.jar
│   ├── kite-data-hbase-1.0.0.jar
│   ├── kite-data-hive-1.0.0.jar
│   ├── kite-hadoop-compatibility-1.0.0.jar
│   ├── libthrift-0.9.3.jar
│   ├── log4j-1.2.17.jar
│   ├── lz4-java-1.4.1.jar
│   ├── mapdb-0.9.9.jar
│   ├── metrics-core-2.2.0.jar
│   ├── mina-core-2.0.4.jar
│   ├── netty-3.10.6.Final.jar
│   ├── opencsv-2.3.jar
│   ├── paranamer-2.3.jar
│   ├── parquet-avro-1.4.1.jar
│   ├── parquet-column-1.4.1.jar
│   ├── parquet-common-1.4.1.jar
│   ├── parquet-encoding-1.4.1.jar
│   ├── parquet-format-2.0.0.jar
│   ├── parquet-generator-1.4.1.jar
│   ├── parquet-hadoop-1.4.1.jar
│   ├── parquet-hive-bundle-1.4.1.jar
│   ├── parquet-jackson-1.4.1.jar
│   ├── protobuf-java-2.5.0.jar
│   ├── scala-library-2.11.12.jar
│   ├── scala-logging_2.11-3.9.0.jar
│   ├── scala-reflect-2.11.12.jar
│   ├── serializer-2.7.2.jar
│   ├── slf4j-api-1.7.25.jar
│   ├── slf4j-log4j12-1.7.25.jar
│   ├── snappy-java-1.1.4.jar
│   ├── twitter4j-core-3.0.3.jar
│   ├── twitter4j-media-support-3.0.3.jar
│   ├── twitter4j-stream-3.0.3.jar
│   ├── velocity-1.7.jar
│   ├── xalan-2.7.2.jar
│   ├── xercesImpl-2.9.1.jar
│   ├── xml-apis-1.3.04.jar
│   ├── xz-1.0.jar
│   └── zkclient-0.10.jar
└── tools
    └── flume-ng-log4jappender-1.9.0-jar-with-dependencies.jar

```

#### Start Agent

```bash
bin/flume-ng agent -n $agent_name -c conf -f conf/flume-conf.properties.template
```

### Setup

## Demo

### A Simple Sample

#### Prepare

```bash
wget http://www.apache.org/dyn/closer.lua/flume/1.9.0/apache-flume-1.9.0-bin.tar.gz

tar zxf apache-flume-1.9.0-bin.tar.gz
```

#### Sample Config

```properties
# conf/example.properties: A single-node Flume configuration

# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
a1.sources.r1.type = netcat
a1.sources.r1.bind = localhost
a1.sources.r1.port = 44444

# Describe the sink
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

#### Start Sample Agent

```bash
# Quick Start
$ bin/flume-ng agent --conf conf --conf-file conf/example.properties --name a1 -Dflume.root.logger=INFO,console

# Print Config and Log RawData
bin/flume-ng agent --conf conf --conf-file conf/example.properties --name a1 -Dflume.root.logger=DEBUG,console -Dorg.apache.flume.log.printconfig=true -Dorg.apache.flume.log.rawdata=true

# JSON Reporting
bin/flume-ng agent --conf-file example.conf --name a1 -Dflume.monitoring.type=http -Dflume.monitoring.port=34545

```

#### Test

```bash
telnet localhost 44444
```

### Zookeeper Integration

```bash
# Zookeeper Path
- /flume
 |- /a1 [Agent config file]
 |- /a2 [Agent config file]

 # Start Agent
 bin/flume-ng agent –conf conf -z zkhost:2181,zkhost1:2181 -p /flume –name a1 -Dflume.root.logger=INFO,console
```

### Plugins

```plain
$FLUME_HOME/plugins.d

plugins.d/
plugins.d/custom-source-1/
plugins.d/custom-source-1/lib/my-source.jar
plugins.d/custom-source-1/libext/spring-core-2.5.6.jar
plugins.d/custom-source-2/
plugins.d/custom-source-2/lib/custom.jar
plugins.d/custom-source-2/native/gettext.so

-lib - the plugin’s jar(s)
-libext - the plugin’s dependency jar(s)
-native - any required native libraries, such as .so files
```

### Data Ingestion

#### RPC

```bash
bin/flume-ng avro-client -H localhost -p 41414 -F /usr/logs/log.10
```

#### Executing Commands

#### Network Streaming

- Avro
- Thrift
- Syslog
- Netcat

## Components

### Sources

#### Avro Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = avro
a1.sources.r1.channels = c1
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 4141
```

#### Thrift Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = thrift
a1.sources.r1.channels = c1
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 4141
```

#### Exec Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = exec
a1.sources.r1.command = tail -F /var/log/secure
a1.sources.r1.channels = c1

a1.sources.tailsource-1.type = exec
a1.sources.tailsource-1.shell = /bin/bash -c
a1.sources.tailsource-1.command = for i in /path/*.txt; do cat $i; done
```

#### JMS Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = jms
a1.sources.r1.channels = c1
a1.sources.r1.initialContextFactory = org.apache.activemq.jndi.ActiveMQInitialContextFactory
a1.sources.r1.connectionFactory = GenericConnectionFactory
a1.sources.r1.providerURL = tcp://mqserver:61616
a1.sources.r1.destinationName = BUSINESS_DATA
a1.sources.r1.destinationType = QUEUE
```

#### Spooling Directory Source

```properties
a1.channels = ch-1
a1.sources = src-1

a1.sources.src-1.type = spooldir
a1.sources.src-1.channels = ch-1
a1.sources.src-1.spoolDir = /var/log/apache/flumeSpool
a1.sources.src-1.fileHeader = true
```

#### Taildir Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = TAILDIR
a1.sources.r1.channels = c1
a1.sources.r1.positionFile = /var/log/flume/taildir_position.json
a1.sources.r1.filegroups = f1 f2
a1.sources.r1.filegroups.f1 = /var/log/test1/example.log
a1.sources.r1.headers.f1.headerKey1 = value1
a1.sources.r1.filegroups.f2 = /var/log/test2/.*log.*
a1.sources.r1.headers.f2.headerKey1 = value2
a1.sources.r1.headers.f2.headerKey2 = value2-2
a1.sources.r1.fileHeader = true
a1.sources.ri.maxBatchCount = 1000
```

#### Kafka Source

```properties
# Example for topic subscription by comma-separated topic list
tier1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
tier1.sources.source1.channels = channel1
tier1.sources.source1.batchSize = 5000
tier1.sources.source1.batchDurationMillis = 2000
tier1.sources.source1.kafka.bootstrap.servers = localhost:9092
tier1.sources.source1.kafka.topics = test1, test2
tier1.sources.source1.kafka.consumer.group.id = custom.g.id

# Example for topic subscription by regex
tier1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
tier1.sources.source1.channels = channel1
tier1.sources.source1.kafka.bootstrap.servers = localhost:9092
tier1.sources.source1.kafka.topics.regex = ^topic[0-9]$
# the default kafka.consumer.group.id=flume is used
```

#### NetCat TCP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = netcat
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 6666
a1.sources.r1.channels = c1
```

#### NetCat UDP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = netcatudp
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 6666
a1.sources.r1.channels = c1
```

#### Syslog TCP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = syslogtcp
a1.sources.r1.port = 5140
a1.sources.r1.host = localhost
a1.sources.r1.channels = c1
```

#### Multiport Syslog TCP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = multiport_syslogtcp
a1.sources.r1.channels = c1
a1.sources.r1.host = 0.0.0.0
a1.sources.r1.ports = 10001 10002 10003
a1.sources.r1.portHeader = port
```

#### Syslog UDP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = syslogudp
a1.sources.r1.port = 5140
a1.sources.r1.host = localhost
a1.sources.r1.channels = c1
```

#### HTTP Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = http
a1.sources.r1.port = 5140
a1.sources.r1.channels = c1
a1.sources.r1.handler = org.example.rest.RestHandler
a1.sources.r1.handler.nickname = random props
a1.sources.r1.HttpConfiguration.sendServerVersion = false
a1.sources.r1.ServerConnector.idleTimeout = 300
```

#### Stress Source

```properties
a1.sources = stresssource-1
a1.channels = memoryChannel-1
a1.sources.stresssource-1.type = org.apache.flume.source.StressSource
a1.sources.stresssource-1.size = 10240
a1.sources.stresssource-1.maxTotalEvents = 1000000
a1.sources.stresssource-1.channels = memoryChannel-1
```

#### Custom Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = org.example.MySource
a1.sources.r1.channels = c1
```

#### Scribe Source

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.type = org.apache.flume.source.scribe.ScribeSource
a1.sources.r1.port = 1463
a1.sources.r1.workerThreads = 5
a1.sources.r1.channels = c1
```

### Sinks

#### HDFS Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = hdfs
a1.sinks.k1.channel = c1
a1.sinks.k1.hdfs.path = /flume/events/%y-%m-%d/%H%M/%S
a1.sinks.k1.hdfs.filePrefix = events-
a1.sinks.k1.hdfs.round = true
a1.sinks.k1.hdfs.roundValue = 10
a1.sinks.k1.hdfs.roundUnit = minute
```

#### Hive Sink

```properties
# Example Hive Table
create table weblogs ( id int , msg string )
    partitioned by (continent string, country string, time string)
    clustered by (id) into 5 buckets
    stored as orc;

# Example Agent
a1.channels = c1
a1.channels.c1.type = memory
a1.sinks = k1
a1.sinks.k1.type = hive
a1.sinks.k1.channel = c1
a1.sinks.k1.hive.metastore = thrift://127.0.0.1:9083
a1.sinks.k1.hive.database = logsdb
a1.sinks.k1.hive.table = weblogs
a1.sinks.k1.hive.partition = asia,%{country},%y-%m-%d-%H-%M
a1.sinks.k1.useLocalTimeStamp = false
a1.sinks.k1.round = true
a1.sinks.k1.roundValue = 10
a1.sinks.k1.roundUnit = minute
a1.sinks.k1.serializer = DELIMITED
a1.sinks.k1.serializer.delimiter = "\t"
a1.sinks.k1.serializer.serdeSeparator = '\t'
a1.sinks.k1.serializer.fieldnames =id,,msg
```

#### Logger Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = logger
a1.sinks.k1.channel = c1
```

#### Avro Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = avro
a1.sinks.k1.channel = c1
a1.sinks.k1.hostname = 10.10.10.10
a1.sinks.k1.port = 4545
```

#### Thrift Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = thrift
a1.sinks.k1.channel = c1
a1.sinks.k1.hostname = 10.10.10.10
a1.sinks.k1.port = 4545
```

#### IRC Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = irc
a1.sinks.k1.channel = c1
a1.sinks.k1.hostname = irc.yourdomain.com
a1.sinks.k1.nick = flume
a1.sinks.k1.chan = #flume
```

#### File Roll Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = file_roll
a1.sinks.k1.channel = c1
a1.sinks.k1.sink.directory = /var/log/flume
```

#### Null Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = null
a1.sinks.k1.channel = c1
```

#### HBase Sinks

##### HBaseSink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = hbase
a1.sinks.k1.table = foo_table
a1.sinks.k1.columnFamily = bar_cf
a1.sinks.k1.serializer = org.apache.flume.sink.hbase.RegexHbaseEventSerializer
a1.sinks.k1.channel = c1
```

##### HBase2Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = hbase2
a1.sinks.k1.table = foo_table
a1.sinks.k1.columnFamily = bar_cf
a1.sinks.k1.serializer = org.apache.flume.sink.hbase2.RegexHBase2EventSerializer
a1.sinks.k1.channel = c1
```

##### AsyncHBaseSink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = asynchbase
a1.sinks.k1.table = foo_table
a1.sinks.k1.columnFamily = bar_cf
a1.sinks.k1.serializer = org.apache.flume.sink.hbase.SimpleAsyncHbaseEventSerializer
a1.sinks.k1.channel = c1
```

#### MorphlineSolrSink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = org.apache.flume.sink.solr.morphline.MorphlineSolrSink
a1.sinks.k1.channel = c1
a1.sinks.k1.morphlineFile = /etc/flume-ng/conf/morphline.conf
# a1.sinks.k1.morphlineId = morphline1
# a1.sinks.k1.batchSize = 1000
# a1.sinks.k1.batchDurationMillis = 1000
```

#### ElasticSearchSink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = elasticsearch
a1.sinks.k1.hostNames = 127.0.0.1:9200,127.0.0.2:9300
a1.sinks.k1.indexName = foo_index
a1.sinks.k1.indexType = bar_type
a1.sinks.k1.clusterName = foobar_cluster
a1.sinks.k1.batchSize = 500
a1.sinks.k1.ttl = 5d
a1.sinks.k1.serializer = org.apache.flume.sink.elasticsearch.ElasticSearchDynamicSerializer
a1.sinks.k1.channel = c1
```

#### Kite Dataset Sink

```properties

```

#### Kafka Sink

```properties
a1.sinks.k1.channel = c1
a1.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink
a1.sinks.k1.kafka.topic = mytopic
a1.sinks.k1.kafka.bootstrap.servers = localhost:9092
a1.sinks.k1.kafka.flumeBatchSize = 20
a1.sinks.k1.kafka.producer.acks = 1
a1.sinks.k1.kafka.producer.linger.ms = 1
a1.sinks.k1.kafka.producer.compression.type = snappy
```

#### HTTP Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = http
a1.sinks.k1.channel = c1
a1.sinks.k1.endpoint = http://localhost:8080/someuri
a1.sinks.k1.connectTimeout = 2000
a1.sinks.k1.requestTimeout = 2000
a1.sinks.k1.acceptHeader = application/json
a1.sinks.k1.contentTypeHeader = application/json
a1.sinks.k1.defaultBackoff = true
a1.sinks.k1.defaultRollback = true
a1.sinks.k1.defaultIncrementMetrics = false
a1.sinks.k1.backoff.4XX = false
a1.sinks.k1.rollback.4XX = false
a1.sinks.k1.incrementMetrics.4XX = true
a1.sinks.k1.backoff.200 = false
a1.sinks.k1.rollback.200 = false
a1.sinks.k1.incrementMetrics.200 = true
```

#### Custom Sink

```properties
a1.channels = c1
a1.sinks = k1
a1.sinks.k1.type = org.example.MySink
a1.sinks.k1.channel = c1
```

### Channels

#### Memory Channel

```properties
a1.channels = c1
a1.channels.c1.type = memory
a1.channels.c1.capacity = 10000
a1.channels.c1.transactionCapacity = 10000
a1.channels.c1.byteCapacityBufferPercentage = 20
a1.channels.c1.byteCapacity = 800000
```

#### JDBC Channel

```properties
a1.channels = c1
a1.channels.c1.type = jdbc
```

#### Kafka Channel

```properties
a1.channels.channel1.type = org.apache.flume.channel.kafka.KafkaChannel
a1.channels.channel1.kafka.bootstrap.servers = kafka-1:9092,kafka-2:9092,kafka-3:9092
a1.channels.channel1.kafka.topic = channel1
a1.channels.channel1.kafka.consumer.group.id = flume-consumer
```

#### File Channel

```properties
a1.channels = c1
a1.channels.c1.type = file
a1.channels.c1.checkpointDir = /mnt/flume/checkpoint
a1.channels.c1.dataDirs = /mnt/flume/data
```

#### Spillable Memory Channel

```properties
# Example of Agent
a1.channels = c1
a1.channels.c1.type = SPILLABLEMEMORY
a1.channels.c1.memoryCapacity = 10000
a1.channels.c1.overflowCapacity = 1000000
a1.channels.c1.byteCapacity = 800000
a1.channels.c1.checkpointDir = /mnt/flume/checkpoint
a1.channels.c1.dataDirs = /mnt/flume/data

# Example To disable the use of the in-memory queue and function like a file channel:
a1.channels = c1
a1.channels.c1.type = SPILLABLEMEMORY
a1.channels.c1.memoryCapacity = 0
a1.channels.c1.overflowCapacity = 1000000
a1.channels.c1.checkpointDir = /mnt/flume/checkpoint
a1.channels.c1.dataDirs = /mnt/flume/data

# Example To disable the use of overflow disk and function purely as a in-memory channel:
a1.channels = c1
a1.channels.c1.type = SPILLABLEMEMORY
a1.channels.c1.memoryCapacity = 100000
a1.channels.c1.overflowCapacity = 0
```

#### Pseudo Transaction Channel

```properties
```

#### Custom Channel

```properties
a1.channels = c1
a1.channels.c1.type = org.example.MyChannel
```

### Channel Selectors

#### Replicating Channel Selector

```properties
a1.sources = r1
a1.channels = c1 c2 c3
a1.sources.r1.selector.type = replicating
a1.sources.r1.channels = c1 c2 c3
a1.sources.r1.selector.optional = c3
```

#### Multiplexing Channel Selector

```properties
a1.sources = r1
a1.channels = c1 c2 c3 c4
a1.sources.r1.selector.type = multiplexing
a1.sources.r1.selector.header = state
a1.sources.r1.selector.mapping.CZ = c1
a1.sources.r1.selector.mapping.US = c2 c3
a1.sources.r1.selector.default = c4
```

#### Custom Channel Selector

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.selector.type = org.example.MyChannelSelector
```

### Sink Processors

#### Default Sink Processors

```properties
```

#### Failover Sink Processor

```properties
a1.sinkgroups = g1
a1.sinkgroups.g1.sinks = k1 k2
a1.sinkgroups.g1.processor.type = failover
a1.sinkgroups.g1.processor.priority.k1 = 5
a1.sinkgroups.g1.processor.priority.k2 = 10
a1.sinkgroups.g1.processor.maxpenalty = 10000
```

#### Load balancing Sink Processor

```properties
a1.sinkgroups = g1
a1.sinkgroups.g1.sinks = k1 k2
a1.sinkgroups.g1.processor.type = load_balance
a1.sinkgroups.g1.processor.backoff = true
a1.sinkgroups.g1.processor.selector = random
```

#### Custom Sink Processor

```properties
```

### Event Serializers

#### Body Text Serializer

```properties
a1.sinks = k1
a1.sinks.k1.type = file_roll
a1.sinks.k1.channel = c1
a1.sinks.k1.sink.directory = /var/log/flume
a1.sinks.k1.sink.serializer = text
a1.sinks.k1.sink.serializer.appendNewline = false
```

#### "Flume Event" Avro Event Serializer

```properties
a1.sinks.k1.type = hdfs
a1.sinks.k1.channel = c1
a1.sinks.k1.hdfs.path = /flume/events/%y-%m-%d/%H%M/%S
a1.sinks.k1.serializer = avro_event
a1.sinks.k1.serializer.compressionCodec = snappy
```

#### Avro Event Serializer

```properties
a1.sinks.k1.type = hdfs
a1.sinks.k1.channel = c1
a1.sinks.k1.hdfs.path = /flume/events/%y-%m-%d/%H%M/%S
a1.sinks.k1.serializer = org.apache.flume.sink.hdfs.AvroEventSerializer$Builder
a1.sinks.k1.serializer.compressionCodec = snappy
a1.sinks.k1.serializer.schemaURL = hdfs://namenode/path/to/schema.avsc
```

### Interceptors

#### Timestamp Interceptor

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.channels =  c1
a1.sources.r1.type = seq
a1.sources.r1.interceptors = i1
a1.sources.r1.interceptors.i1.type = timestamp
```

#### Host Interceptor

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.interceptors = i1
a1.sources.r1.interceptors.i1.type = host
```

#### Static Interceptor

```properties
a1.sources = r1
a1.channels = c1
a1.sources.r1.channels =  c1
a1.sources.r1.type = seq
a1.sources.r1.interceptors = i1
a1.sources.r1.interceptors.i1.type = static
a1.sources.r1.interceptors.i1.key = datacenter
a1.sources.r1.interceptors.i1.value = NEW_YORK
```

#### Remove Header Interceptor

```properties
```

#### UUID Interceptor

```properties
```

#### Morphline Interceptor

```properties
a1.sources.avroSrc.interceptors = morphlineinterceptor
a1.sources.avroSrc.interceptors.morphlineinterceptor.type = org.apache.flume.sink.solr.morphline.MorphlineInterceptor$Builder
a1.sources.avroSrc.interceptors.morphlineinterceptor.morphlineFile = /etc/flume-ng/conf/morphline.conf
a1.sources.avroSrc.interceptors.morphlineinterceptor.morphlineId = morphline1
```

#### Search and Replace Interceptor

```properties
# Example 1
a1.sources.avroSrc.interceptors = search-replace
a1.sources.avroSrc.interceptors.search-replace.type = search_replace

# Remove leading alphanumeric characters in an event body.
a1.sources.avroSrc.interceptors.search-replace.searchPattern = ^[A-Za-z0-9_]+
a1.sources.avroSrc.interceptors.search-replace.replaceString =

# Example 2
a1.sources.avroSrc.interceptors = search-replace
a1.sources.avroSrc.interceptors.search-replace.type = search_replace

# Use grouping operators to reorder and munge words on a line.
a1.sources.avroSrc.interceptors.search-replace.searchPattern = The quick brown ([a-z]+) jumped over the lazy ([a-z]+)
a1.sources.avroSrc.interceptors.search-replace.replaceString = The hungry $2 ate the careless $1

```

#### Regex Filtering Interceptor

```properties
# Example 1
a1.sources.r1.interceptors.i1.regex = (\\d):(\\d):(\\d)
a1.sources.r1.interceptors.i1.serializers = s1 s2 s3
a1.sources.r1.interceptors.i1.serializers.s1.name = one
a1.sources.r1.interceptors.i1.serializers.s2.name = two
a1.sources.r1.interceptors.i1.serializers.s3.name = three

# Example 2
a1.sources.r1.interceptors.i1.regex = ^(?:\\n)?(\\d\\d\\d\\d-\\d\\d-\\d\\d\\s\\d\\d:\\d\\d)
a1.sources.r1.interceptors.i1.serializers = s1
a1.sources.r1.interceptors.i1.serializers.s1.type = org.apache.flume.interceptor.RegexExtractorInterceptorMillisSerializer
a1.sources.r1.interceptors.i1.serializers.s1.name = timestamp
a1.sources.r1.interceptors.i1.serializers.s1.pattern = yyyy-MM-dd HH:mm

```

### Alias Conventions

| Alias Name| Alias Type
|--- | ---
|a | **a**gent
|c | **c**hannel
|r | sou**r**ce
|k | sin**k**
|g | sink **g**roup
|i | **i**nterceptor
|y | ke**y**
|h | **h**ost
|s | **s**erializer
